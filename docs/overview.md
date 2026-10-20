---
type: documentation
entity: project-overview
version: 1.1
updated: 2026-10-20
---

# REA project overview

## Purpose

- REA is a local reverse-engineering toolkit exposed through a CLI and an MCP server.
- An analyst or coding agent selects a question and target. REA inspects supported artifacts, calls analysis engines, records observations, and compares retained evidence.
- The coding agent owns planning, interpretation, implementation, and the choice of acceptance tests. REA provides analysis and verification operations; it does not supply a model or automatically implement a replacement application.
- Start with [installation](installation.md), then follow [planning and AI-assisted work](ai-workflow.md). This overview maps those workflows to their implementation.
- This repository is a fork of the upstream REA project. [Fork maintenance](fork-maintenance.md) distinguishes local source builds, published packages and upstream synchronization.

## Architecture

### System Diagram

```text
Analyst or coding agent
          |
    CLI or MCP adapter
          |
  Named contracts and input validation
          |
  Shared application workflows
     |           |             |
  Sessions    Analysis       Evidence and
  and routing providers      Unknown records
     |           |             |
     +-- domain semantics -----+
                 |
      Observation or comparison
      with identity and limitations
```

- **Composition:** `src/composition/` constructs providers and connects application ports to concrete adapters. Construction does not itself authorize target execution.
- **Application:** `src/application/` owns workflows, target/session lifecycle, provider selection, evidence retention, and setup planning.
- **Domain and contracts:** `src/domain/` owns evidence identities, graphs, comparison rules, and expected failures. `src/contracts/` binds named operations to request/result schemas and effect declarations.
- **Adapters:** provider directories own engine protocols, filesystem reads, and runtime observation. CLI/MCP adapters translate requests and results.
- **Shared provider lifetime:** `src/providers/` owns deferred creation, inspection scope and retryable cleanup. Engine-specific adapters retain their protocol and target authority.
- **Process foundation:** `src/process/`, `src/filesystem/`, and `src/windows/` own process identity, stable files, bounded I/O, cancellation, and cleanup.
- The detailed [architecture diagram](https://github.com/Global-Platforms-Labs/rea/blob/main/docs/architecture.mermaid) maps runtime calls and ownership, including calls through application ports. It is not a literal import graph. [Tool design](tool-design.md) explains how to extend the layers.

### Tech Stack

| Area                | Implementation                                                                                                                       |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Runtime             | TypeScript, Node.js ESM, npm; development versions are pinned in `.nvmrc` and `package.json#packageManager`                          |
| Public interfaces   | CLI dispatcher and command registration; MCP SDK over stdio by default, opt-in streamable HTTP                                       |
| Validation          | Zod contracts, JSON Schema validation and canonical JSON commitments                                                                 |
| JavaScript analysis | Babel parsing, ASAR/ZIP readers, source-map decoding, optional Wakaru recovery                                                       |
| Runtime observation | CDP, Playwright, V8 Inspector, process capture, platform-specific native helpers                                                     |
| External engines    | Caller-supplied tools selected for a supported format, including Hopper, Ghidra, IDA, Kuna, JEB, JADX, Apktool, ADB, WABT and Tshark |
| Verification        | Vitest, production-boundary fixtures, explicit real-provider lanes, package verification                                             |
| Documentation       | Markdown/VitePress in `docs/`; separately authored public site in `website/`                                                         |

- The [HTTP transport](mcp-contracts.md#transports) has its own binding, client and security constraints. Client setup continues to register stdio.
- Optional engines load independently. Tool installation or presence does not grant permission to execute or mutate a target.

## Modules

| Module                              | Description                                                                                                                                  | Documentation                                                                                                                                                                        |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Entry points and adapters           | `scripts/rea.mjs`, `src/cli*`, `src/main*`, `src/server/`; command routing, MCP registration, progress and result delivery                   | [CLI](cli.md), [MCP contracts](mcp-contracts.md)                                                                                                                                     |
| Composition and configuration       | `src/composition/`, `src/config/`; fresh providers, independent optional loading, caller-selected environment                                | [Installation](installation.md), [architecture](https://github.com/Global-Platforms-Labs/rea/blob/main/docs/architecture.mermaid)                                                    |
| Sessions and records                | `src/application/binary/`, `src/application/investigation/`; target binding, snapshots, authenticated Evidence and Unknown revisions         | [Session and evidence flow](#session-and-evidence-flow)                                                                                                                              |
| Domain and contracts                | `src/domain/`, `src/contracts/`; pure models, named operation schemas, effects, failure semantics                                            | [Tool design](tool-design.md), [MCP contracts](mcp-contracts.md)                                                                                                                     |
| Artifact and JavaScript analysis    | `src/artifacts/`, `src/javascript/`, `src/application/javascript/`; inventory, extraction, application/semantic graphs, recovery and tracing | [Artifact reconstruction](javascript-artifact-reconstruction.md), [workflows](javascript-application-workflows.md)                                                                   |
| Native engines and platform readers | `src/hopper/`, `src/ghidra/`, `src/ida/`, `src/kuna/`, `src/native/`; static analysis, metadata and selected native observations             | [Native investigation](native-investigation.md), [Kuna](kuna-provider.md), [binary diagnostics](binary-diagnostics.md)                                                               |
| Managed and mobile engines          | `src/dotnet/`, `src/android/`, `src/apktool/`, `src/adb/`, `src/jeb/`; distinct format/tool boundaries                                       | [.NET](managed-code-analysis.md), [Android](android-analysis.md), [JEB](jeb-analysis.md)                                                                                             |
| Browser and runtime observations    | `src/browser/`, `src/inspector/`, `src/process/capture/`; website, Electron, Node and process evidence                                       | [Browser](browser-observation.md), [JavaScript runtime](javascript-runtime-observation.md), [process capture](process-capture.md)                                                    |
| Language and package metadata       | `src/go/`, `src/rust/`, `src/flutter/`, `src/il2cpp/`, `src/electron/`; selected build, dependency, AOT and fuse facts                       | [Go](go-binaries.md), [Rust](rust-binaries.md), [Flutter](flutter-analysis.md), [IL2CPP](il2cpp-metadata.md), [Electron fuses](electron-fuses.md)                                    |
| Explicit tool execution             | `src/objdump/`, `src/gdb/`, `src/rizin/`, `src/cutter/`, `src/frida/`; command profiles, debugger sessions and caller scripts                | [Tool integrations](reverse-engineering-tools.md), [Frida](frida-instrumentation.md)                                                                                                 |
| Other artifact engines              | `src/firmware/`, `src/evm/`, `src/wasm/`, `src/reference/`; firmware, bytecode and historical source input                                   | [Firmware](firmware-analysis.md), [EVM](evm-bytecode.md), [WASM](wasm-artifacts.md), [historical source](javascript-application-workflows.md#historical-source-to-bundle-comparison) |
| Recorded data                       | `src/captures/`, `src/pcap/`, `src/signals/`, `src/sqlite/`; bounded inspection of selected capture/database inputs                          | [Packet fields](packet-captures.md), [PCAP summaries](pcap-analysis.md), [signals](signal-recordings.md), [SQLite](sqlite-databases.md)                                              |
| Recorded agent environments         | `src/application/environment/`; serve or export retained web-capture behavior                                                                | [Agent environments](agent-environments.md)                                                                                                                                          |
| Resource ownership                  | `src/providers/`, `src/process/`, `src/filesystem/`, `src/windows/`, `native/windows/`; creation, identity, deadlines and cleanup            | [Resource ownership](#resource-ownership), [Windows boundary](windows-ghidra-p0.md)                                                                                                  |
| Evaluation and distribution         | `src/evaluation/`, `tests/`, `scripts/`, `.github/`, `.agents/`; fixtures, agent evaluation, generated metadata and releases                 | [Testing](testing.md), [releasing](releasing.md), [coverage map](documentation-coverage.md)                                                                                          |

- [GGUF/safetensors inspection](model-artifacts.md) has dedicated `src/model/` and `src/application/model/` owners. It reads declarations and derived totals without model loading or inference.
- Its default header digest identifies the inspected prefix. Only explicit full-file hashing commits to every byte.

### Session and evidence flow

1. **Receive a named request.** CLI or MCP validates the operation's inputs. CLI one-shot operations and MCP requests reuse application services.
2. **Choose the applicable route.** Native analysis binds one selected deep provider, Hopper, Ghidra, IDA or Kuna, to a target. Artifact, managed, browser, JEB and other workflows have their own declared capabilities; a target does not gain support merely from its extension.
3. **Acquire evidence.** The provider returns observations and explicit limitations. Application workflows preserve artifact/provider identity and distinguish observed, derived, and inferred results.
4. **Retain records.** `InvestigationRecords` owns the `EvidenceLedger`. Imports validate commitments and references before merging. Unknowns retain their revision history and mutation evidence.
5. **Compose follow-up questions.** Traces, graph projections and comparisons cite their input Evidence. Saved snapshots and bundle exports allow later work to reuse those records.
6. **Verify a finite claim.** Comparison and reconstruction tools evaluate declared evidence and scope. A valid Evidence hash establishes record integrity, not that a supplied interpretation is true.
7. **Settle owned resources.** One-shot commands close their owned sessions. MCP owns a longer-lived session and closes resources on shutdown. Cleanup failure remains an explicit failure.

Implementation anchors:

- [Session composition](https://github.com/Global-Platforms-Labs/rea/blob/main/src/composition/binary.ts), [session router](https://github.com/Global-Platforms-Labs/rea/blob/main/src/application/binary/SessionProviderRouter.ts).
- [InvestigationRecords](https://github.com/Global-Platforms-Labs/rea/blob/main/src/application/investigation/InvestigationRecords.ts), [EvidenceLedger](https://github.com/Global-Platforms-Labs/rea/blob/main/src/application/investigation/EvidenceLedger.ts), [Evidence model](https://github.com/Global-Platforms-Labs/rea/blob/main/src/domain/evidence.ts).
- [MCP lifecycle](https://github.com/Global-Platforms-Labs/rea/blob/main/src/main.ts), [one-shot execution](https://github.com/Global-Platforms-Labs/rea/blob/main/src/application/DirectAnalysis.ts).

### Resource ownership

| Resource                           | Ownership rule                                                                                                                                           |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Owned process                      | Retain lineage and start identity through cancellation and cleanup. A reused PID alone is insufficient.                                                  |
| Caller-owned engine or application | Preserve the declared attachment boundary. Closing an REA connection does not grant authority to close an attached GUI application or save its database. |
| Artifact/file                      | Preserve caller selection and stable byte identity. Paths, URLs and display names retain distinct semantics.                                             |
| Output or extraction directory     | Track created resources and validate paths. A failed cleanup does not release ownership.                                                                 |
| Runtime observation                | Use the requested passive/active mode and declared effects. Static analysis does not authorize execution or device mutation.                             |
| Debugger or injected script        | Preserve target identity and explicit execution/mutation authority. Console commands and Frida scripts can affect files, memory or network.              |
| Configuration                      | Setup presents its changes before applying them. Existing configuration and unrelated tools remain under caller control.                                 |

Relevant owners: [provider lifetime](https://github.com/Global-Platforms-Labs/rea/tree/main/src/providers), [process primitives](https://github.com/Global-Platforms-Labs/rea/tree/main/src/process), [stable filesystem reads](https://github.com/Global-Platforms-Labs/rea/tree/main/src/filesystem), [tool effects](https://github.com/Global-Platforms-Labs/rea/blob/main/src/contracts/toolEffects.ts).

## Key Features

| Feature                           | Description                                                                            | Documentation                                                                                                                         |
| --------------------------------- | -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Artifact inventory                | Identify components and preserve their byte identities before choosing deeper analysis | [Apple applications](apple-application-analysis.md), [artifact reconstruction](javascript-artifact-reconstruction.md)                 |
| Native investigation              | Functions, instructions, references, types, UI/dispatch and selected runtime calls     | [Native guide](native-investigation.md), [Ghidra](ghidra-provider.md), [IDA](ida-provider.md), [Kuna](kuna-provider.md)               |
| JavaScript reconstruction         | Recover static modules, Electron boundaries and cross-layer relationships              | [Graph](javascript-application-graph.md), [recovery](javascript-recovery.md), [workflows](javascript-application-workflows.md)        |
| Managed inspection                | PE/CLI metadata and CIL, member comparison and declared native boundaries              | [.NET guide](managed-code-analysis.md)                                                                                                |
| Android and JEB                   | APK code, resources, device acquisition, and caller-started JEB inspection             | [Android](android-analysis.md), [Apktool](apktool-resource-analysis.md), [ADB](adb-device-analysis.md), [JEB](jeb-analysis.md)        |
| Language-specific metadata        | Build identities, auditable dependencies and selected AOT/runtime mappings             | [Go](go-binaries.md), [Rust](rust-binaries.md), [Flutter](flutter-analysis.md), [IL2CPP](il2cpp-metadata.md)                          |
| Explicit debugger/instrumentation | Selected commands, target state and caller-supplied scripts                            | [Tool integrations](reverse-engineering-tools.md), [Frida](frida-instrumentation.md)                                                  |
| Web and runtime correlation       | Capture scripts and execution, map source positions, reconcile static/runtime findings | [Script export](website-script-export.md), [runtime](web-runtime.md), [reconciliation](javascript-runtime-reconciliation.md)          |
| Historical and offline evidence   | Network archives, binary layout, recorded crashes, firmware and EVM bytecode           | [Network captures](web-network-captures.md), [crashes](recorded-crashes.md), [firmware](firmware-analysis.md), [EVM](evm-bytecode.md) |
| Recorded data and replay          | Packet/signal/database inspection and retained web behavior for agent environments     | [PCAP](pcap-analysis.md), [signals](signal-recordings.md), [SQLite](sqlite-databases.md), [environments](agent-environments.md)       |
| Reconstruction verification       | Compare selected scenarios, track obligations, and evaluate a named coverage boundary  | [Obligation ledgers](reconstruction-obligation-ledgers.md), [readiness](reconstruction-readiness.md), [AI workflow](ai-workflow.md)   |

## Development

### Setup

- Use the pinned development toolchain, then `npm ci`.
- Follow [CONTRIBUTING.md](https://github.com/Global-Platforms-Labs/rea/blob/main/CONTRIBUTING.md) for hooks, configuration and generated-file ownership.
- Supply only the external engines needed by the intended analysis lane. See [installation](installation.md) and each provider guide.

### Build & Run

```sh
npm run build:cached
node scripts/rea.mjs --help
node scripts/rea.mjs mcp
```

- `build:cached` compiles the runtime and packages the authored investigation skill.
- `docs:generate` creates documentation projections; generated catalogs are not authored source.
- Edit `.agents/skills/reverse-engineer-anything/` for the packaged skill. `skills/` is a generated output.
- `docs/` is the VitePress reference site. `website/` has its own static HTML workflow and publishing rules in the [website README](https://github.com/Global-Platforms-Labs/rea/blob/main/website/README.md).

### Testing

| Change                                       | Relevant check                                                 |
| -------------------------------------------- | -------------------------------------------------------------- |
| Source semantics                             | `npm run test:local -- PATH...`                                |
| Workflow needing built artifacts             | `npm run test:focused -- PATH...`                              |
| Types, lint and import boundaries            | `npm run check:fast`                                           |
| Documentation facts and generated references | `npm run docs:check`                                           |
| Documentation rendering and local links      | `npm run docs:build`                                           |
| External provider behavior                   | The matching `verify:*` lane with its documented prerequisites |
| Package/setup/distribution                   | `npm run verify:package` and `npm pack --dry-run`              |

- [Testing](testing.md) distinguishes public CLI/MCP acceptance, production-boundary integration, producer goldens and real-engine verification.
- [Client verification](client-verification.md) documents fixture-based agent checks and their acceptance limits.
- A successful documentation build does not verify provider availability, arbitrary target behavior, or reconstruction completeness.

## References

- [Planning and AI-assisted work](ai-workflow.md).
- [Documentation coverage and remaining gaps](documentation-coverage.md).
- [Detailed architecture](https://github.com/Global-Platforms-Labs/rea/blob/main/docs/architecture.mermaid).
- [Tool design](tool-design.md), [contracts](mcp-contracts.md), [testing](testing.md), [release process](releasing.md).
