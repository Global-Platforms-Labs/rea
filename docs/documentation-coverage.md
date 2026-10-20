---
type: documentation
entity: coverage
version: 1.1
updated: 2026-10-20
---

# Documentation coverage

## Scope and method

- Baseline: `7d6f90485ee078834ce64656770a34947bb81803` in the Global-Platforms-Labs fork.
- Inventory: `git ls-files`, grouped by source directory and supporting area. Every tracked `src/` path belongs to one row below.
- Source reconciliation: entry points, provider composition, named contracts, session/Evidence owners, reconstruction authority checks and package scripts.
- Existing detailed guides retain ownership of provider behavior, supported host/format profiles and historical real-tool evidence.
- The 2,373 tracked source files include 683 colocated `*.test.ts` files. They are not 2,373 production modules.
- The repository baseline has 4,850 tracked paths in total. The source map and supporting-area table account for every path once.
- This is a complete directory-level inventory with targeted source review, not an exhaustive method audit or fresh runtime certification.
- New documentation added here: `overview.md`, `ai-workflow.md`, `documentation-coverage.md`, and `fork-maintenance.md`. Baseline counts exclude these additions.
- Ignored generated outputs, local plans, checkpoints and untracked investigation artifacts are excluded.

```sh
git ls-tree -r --name-only 7d6f90485ee078834ce64656770a34947bb81803 -- src
git ls-tree -r --name-only 7d6f90485ee078834ce64656770a34947bb81803 -- docs tests scripts bridge native website .agents .github experiments licenses third_party
```

## Source ownership and guide map

| Source area        | Tracked files | Responsibility                                                                       | Guide                                                                               |
| ------------------ | ------------: | ------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------- |
| `src/` root files  |            27 | CLI/MCP entry points, command groups, logging, identities and diagnostics            | [Guide](cli.md)                                                                     |
| `src/adb/`         |            14 | ADB device discovery, acquisition and explicit mutation                              | [Guide](adb-device-analysis.md)                                                     |
| `src/android/`     |            13 | Caller-supplied headless JADX APK inspection                                         | [Guide](android-analysis.md)                                                        |
| `src/apktool/`     |            16 | Caller-supplied resource decoding and tree inspection                                | [Guide](apktool-resource-analysis.md)                                               |
| `src/application/` |           392 | Shared workflow services, ports, setup, sessions, Evidence and reconstruction        | [Guide](overview.md)                                                                |
| `src/artifacts/`   |            81 | Inventory, desktop triage, archive readers/extraction, Apple and JavaScript inputs   | [Guide](desktop-applications.md)                                                    |
| `src/browser/`     |           164 | CDP/Playwright, scripts, source/runtime attribution and network history              | [Guide](browser-observation.md)                                                     |
| `src/captures/`    |             3 | Tshark selected packet fields and provider discovery                                 | [Guide](packet-captures.md)                                                         |
| `src/cli/`         |            56 | Command families, setup interaction and one-shot adapters                            | [Guide](cli.md)                                                                     |
| `src/composition/` |            37 | Fresh factories, named inspection descriptors, optional loading and provider wiring  | [Guide](overview.md#architecture)                                                   |
| `src/config/`      |            13 | Configuration/environment parsing, origin, provider and delivery settings            | [Guide](installation.md)                                                            |
| `src/contracts/`   |            95 | Named tool/prompt schemas, examples, effects and error contracts                     | [Guide](mcp-contracts.md)                                                           |
| `src/cutter/`      |             2 | Authenticated local Cutter bridge and command lifecycle                              | [Guide](reverse-engineering-tools.md#cutter-bridge)                                 |
| `src/domain/`      |           759 | Pure evidence, graphs, comparisons, format results and failure semantics             | [Guide](overview.md)                                                                |
| `src/dotnet/`      |            44 | Static PE/CLI metadata, CIL and managed member facts                                 | [Guide](managed-code-analysis.md)                                                   |
| `src/electron/`    |             9 | Electron fuse byte inspection and owned reader cleanup                               | [Guide](electron-fuses.md)                                                          |
| `src/evaluation/`  |             4 | Agent fixture evaluation, authenticated claim checks and reports                     | [Guide](client-verification.md)                                                     |
| `src/evm/`         |            13 | Bounded offline EVMole interface inference                                           | [Guide](evm-bytecode.md)                                                            |
| `src/filesystem/`  |             9 | Stable regular-file reads, owned handles and hashing                                 | [Guide](overview.md#resource-ownership)                                             |
| `src/firmware/`    |            13 | Binwalk/Unblob execution, reports and private extraction                             | [Guide](firmware-analysis.md)                                                       |
| `src/flutter/`     |             8 | Flutter APK identity and selected Dart AOT facts                                     | [Guide](flutter-analysis.md)                                                        |
| `src/frida/`       |             2 | Device/process discovery, attach/spawn and caller-script lifecycle                   | [Guide](frida-instrumentation.md)                                                   |
| `src/gdb/`         |             1 | Persistent GDB/MI session ownership and console operations                           | [Guide](reverse-engineering-tools.md#gdb)                                           |
| `src/ghidra/`      |            80 | Headless provider profiles, bridges, analysis and lifecycle                          | [Guide](ghidra-provider.md)                                                         |
| `src/go/`          |             9 | Go binary containers, image reads and build/module metadata                          | [Guide](go-binaries.md)                                                             |
| `src/hopper/`      |            29 | Hopper discovery, launch, socket bridge and provider adaptation                      | [Guide](installation.md#hopper)                                                     |
| `src/ida/`         |            34 | Upstream MCP adapter, attached GUI and owned headless analysis                       | [Guide](ida-provider.md)                                                            |
| `src/il2cpp/`      |            13 | Metadata/image registration facts and explicit Ghidra seed output                    | [Guide](il2cpp-metadata.md)                                                         |
| `src/inspector/`   |            10 | Passive V8 Inspector discovery and observation                                       | [Guide](javascript-runtime-observation.md)                                          |
| `src/javascript/`  |            27 | Bounded analysis worker, source maps and Wakaru recovery                             | [Guide](javascript-artifact-reconstruction.md)                                      |
| `src/jeb/`         |             8 | Caller-started JEB MCP client/project/unit/decompilation                             | [Guide](jeb-analysis.md)                                                            |
| `src/kuna/`        |            25 | Caller-supplied deep provider, project cache/export and queries                      | [Guide](kuna-provider.md)                                                           |
| `src/main/`        |             7 | MCP startup, stdio/HTTP transport selection and shutdown                             | [Guide](mcp-contracts.md)                                                           |
| `src/model/`       |             6 | GGUF/safetensors header declarations, scoped identity and optional full-file hashing | [Guide](model-artifacts.md)                                                         |
| `src/native/`      |            71 | Apple dispatch/UI/calls, PE resources, pwntools and pwndbg adapters                  | [Guide](native-investigation.md)                                                    |
| `src/objdump/`     |             1 | GNU objdump command selection                                                        | [Guide](reverse-engineering-tools.md#gnu-objdump)                                   |
| `src/pcap/`        |             8 | Tshark/capinfos capture summaries with owned snapshot/resource policy                | [Guide](pcap-analysis.md)                                                           |
| `src/process/`     |           106 | Owned process trees, commands, bounded I/O, capture and cleanup                      | [Guide](process-capture.md)                                                         |
| `src/providers/`   |             6 | Shared deferred client/inspection lifetime and retryable cleanup owners              | [Guide](tool-design.md#own-the-lifecycle)                                           |
| `src/reference/`   |             9 | Stable historical source-tree traversal and validation                               | [Guide](javascript-application-workflows.md#historical-source-to-bundle-comparison) |
| `src/rizin/`       |             2 | Static command profiles and persistent debugger sessions                             | [Guide](reverse-engineering-tools.md#rizin)                                         |
| `src/rust/`        |             5 | Rust binary auditable dependency metadata                                            | [Guide](rust-binaries.md)                                                           |
| `src/server/`      |           111 | MCP named registration, prompts, result delivery, stdio/HTTP server boundaries       | [Guide](mcp-contracts.md)                                                           |
| `src/signals/`     |             2 | Caller-supplied sigrok recorded-signal decoding/discovery                            | [Guide](signal-recordings.md)                                                       |
| `src/sqlite/`      |             8 | Owned SQLite snapshots, bounded workers and inspection                               | [Guide](sqlite-databases.md)                                                        |
| `src/wasm/`        |            15 | WABT static artifact inspection, interfaces, WAT and glue                            | [Guide](wasm-artifacts.md)                                                          |
| `src/windows/`     |             6 | Native package loading, opaque host handles and runtime controls                     | [Guide](windows-ghidra-p0.md)                                                       |
| **Total**          |     **2,373** | All tracked source paths at the baseline                                             | [Architecture](overview.md)                                                         |

## Application, domain and contract groups

| Group                       | Owners and documentation                                                                                                                                                                                                                          |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Session and records         | `application/binary`, `application/investigation`, root Evidence/Unknown models. [Session flow](overview.md#session-and-evidence-flow)                                                                                                            |
| Setup and maintenance       | Root `application/Setup*`, client registration, doctor, update/uninstall; `cli/` adapters. [Installation](installation.md), [client configuration](client-configuration.md)                                                                       |
| Reconstruction              | Root application/domain ledger, comparison, closure and readiness owners. [Obligations](reconstruction-obligation-ledgers.md), [coverage](reconstruction-coverage.md), [readiness](reconstruction-readiness.md)                                   |
| JavaScript                  | Application/domain `javascript`, `analysisView`, artifact readers and worker adapters. [Graph](javascript-application-graph.md), [workflows](javascript-application-workflows.md), [reconciliation](javascript-runtime-reconciliation.md)         |
| Native and package analysis | `native`, `binaryDiagnostics`, `apple`, `artifacts`, `harmony`, `electron`, `go`, `rust`, `il2cpp`, `flutter`. [Native](native-investigation.md), [desktop](desktop-applications.md), matching provider guides above                              |
| Format and tool services    | `managed`, `android`, `apktool`, `adb`, `jeb`, `firmware`, `wasm`, `evm`, `model`, `frida`, `reverse`. Typed service/port owners connect the corresponding adapters                                                                               |
| Recorded/live observations  | `process`, `captures`, `pcap`, `signals`, `sqlite`, browser/web services. [Process](process-capture.md), [web history](web-network-captures.md), matching recorded-data guides above                                                              |
| Agent environment           | `application/environment`, server handlers and network-capture/domain projections. [Recorded agent environments](agent-environments.md)                                                                                                           |
| Contract families           | Root named catalogs plus `adb`, `android`, `apktool`, `captures`, `evm`, `firmware`, `flutter`, `go`, `il2cpp`, `javascript`, `jeb`, `managed`, `model`, `native`, `pcap`, `rust`, `signals`, `sqlite`, `wasm`. [MCP contracts](mcp-contracts.md) |
| Resource lifetime           | `providers`, `process`, `filesystem`, `windows` and provider-owned cleanup state. [Lifecycle rules](tool-design.md#own-the-lifecycle), [lifecycle regressions](testing.md#lifecycle-regressions)                                                  |

## Supporting repository areas

| Area                 | Baseline tracked files | Documentation and ownership                                                                                                                                                    |
| -------------------- | ---------------------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `bridge/`            |                     33 | Engine/language/platform bridges. [Architecture](https://github.com/Global-Platforms-Labs/rea/blob/main/docs/architecture.mermaid) and corresponding provider guides           |
| `native/`            |                      5 | Windows Node-API implementation. [Windows boundary](windows-ghidra-p0.md)                                                                                                      |
| `tests/`             |                  1,499 | Public workflow acceptance, boundaries, conformance, fixtures and support. [Testing](testing.md)                                                                               |
| `scripts/`           |                    425 | Dispatcher, metadata/build/fixture generation, verification and maintenance. [Testing](testing.md), [releasing](releasing.md)                                                  |
| `.github/`           |                     44 | CI/provider lanes, contribution/readiness workflows, release and site publishing. [Contribution guide](https://github.com/Global-Platforms-Labs/rea/blob/main/CONTRIBUTING.md) |
| `.agents/`           |                     15 | Authored investigation, tool-design, optimization and release-note skills/references. [AI workflow](ai-workflow.md)                                                            |
| `website/`           |                     94 | Static HTML, assets, templates and case-study evidence. [Website README](https://github.com/Global-Platforms-Labs/rea/blob/main/website/README.md)                             |
| `experiments/`       |                    132 | Analysis-efficiency experiments and evidence. Historical experiments do not certify current performance                                                                        |
| `licenses/`          |                     20 | Distributed license texts; retain upstream notices and attribution                                                                                                             |
| `third_party/`       |                     13 | Pinned integration sources/notices and submodule entries; upstream submodule internals are outside this path count                                                             |
| `docs/`              |                    162 | 138 Markdown files including guides, translations, specs and verification records; configuration/assets/metadata included in total                                             |
| Root files           |                     33 | Node package/lockfile, installer, runtime pins, TypeScript/Vitest/lint/Turbo/mutation settings, release metadata, notices and contributor guides                               |
| `.husky/`            |                      2 | Local pre-commit/pre-push hooks. [Contribution guide](https://github.com/Global-Platforms-Labs/rea/blob/main/CONTRIBUTING.md)                                                  |
| **Supporting total** |              **2,477** | Added to the 2,373 source paths: **4,850 tracked baseline paths**                                                                                                              |

## Reconciled boundaries

- Connected the project overview, source ownership, planning/AI workflow and fork maintenance without replacing canonical provider guides.
- Preserved newer upstream HTTP MCP, provider lifecycle, WASM, model declarations, debugger/instrumentation and recorded-data features.
- Clarified .NET PE/CLI versus JVM analysis and retained separate IL2CPP/native routes.
- Distinguished ledger construction, finite claim verification, named coverage evaluation and repository conformance tests.
- Documented the missing general public obligation-proof producer; the ledger does not execute its verifier command.
- Kept package/site identity, contributor requirements and historical evidence separate from this fork's documentation changes.

## Gaps and verification limits

| Limit                             | Meaning and next evidence                                                                                                     |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Directory-level source inventory  | Every tracked path is mapped; not every exported symbol or internal method was individually audited                           |
| Existing detailed provider claims | Their own evidence dates and scope remain authoritative; navigation updates do not repeat those experiments                   |
| Runtime and provider availability | No engine, debugger, device, browser, external service or application run was performed for this documentation inventory      |
| Mutation and instrumentation      | Explicit command/script effects remain governed by their contracts; an analysis request does not automatically authorize them |
| Obligation proof production       | Requires a reviewed project-specific evidence producer and admissible observations                                            |
| Agent evaluation                  | Configured fixture claims and routing/workflow gates do not establish unrestricted agent correctness                          |
| Experiments/performance           | Existing measurements are historical and task/host-specific; no new speed, token or memory result is claimed                  |
| Generated catalogs and packages   | Require the documented generation, consistency and package gates                                                              |
| Documentation mechanics           | Links, formatting, facts and rendering checks cannot establish arbitrary runtime behavior                                     |

## Maintenance

1. Recompute the tracked inventory when source areas move or are added.
2. Update the owning detailed guide, overview and navigation with behavior/ownership changes.
3. Preserve source/profile/authority limits and distinguish unknown from absent.
4. Follow [fork maintenance](fork-maintenance.md) when integrating upstream changes.
5. Run documentation checks/build; add real-provider/package verification only for the claims changed.
