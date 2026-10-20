---
layout: home

hero:
  name: REA
  text: Reverse Engineer Anything
  tagline: Understand binaries, applications, and runtime behavior with one CLI and MCP server.
  actions:
    - theme: brand
      text: Get started
      link: /installation
    - theme: alt
      text: View on GitHub
      link: https://github.com/morluto/rea

features:
  - title: Investigate with your agent
    details: Connect REA to your coding agent, describe the app or feature you want to understand, and follow the evidence.
    link: /mcp-prompts
  - title: Inspect across formats
    details: Explore native binaries, JavaScript and Electron apps, .NET assemblies, Android packages, firmware, and websites.
    link: /installation
  - title: Keep the evidence
    details: Analysis runs locally and returns observations, limitations, and unknowns through the CLI and MCP.
    link: /mcp-contracts
---

## Start with one command

```bash
npx rea-agents setup
```

Choose your agent, review the planned changes, and approve setup. Native binary
analysis can use an existing Hopper, Ghidra, IDA, or Kuna installation; static
JavaScript analysis needs no native analysis engine.

For a first result from an extracted JavaScript application or ASAR:

```bash
npx -y rea-agents@latest analyze-javascript-application /absolute/path/to/app --json
```

See [installation and setup](./installation.md) for requirements and configuration.

For runtime instrumentation, see the [Frida guide](./frida-instrumentation.md).

## Understand the project

- [Overview and architecture](overview.md): layers, modules, session/Evidence flow and resource ownership.
- [Planning and AI-assisted work](ai-workflow.md): task briefs, phased plans, implementation, review, verification and resume.
- [Documentation coverage](documentation-coverage.md): source inventory, guide map and remaining limits.
- [Fork maintenance](fork-maintenance.md): upstream ownership, selected source builds and synchronization.

## Choose a guide

| Task                                                  | Guides                                                                                                                                                                                                                                                                                                                                                 |
| ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Identify a desktop installation                       | [Runtime candidates, evidence and follow-up files](desktop-applications.md)                                                                                                                                                                                                                                                                            |
| Inspect native functions, references, or types        | [Native binaries](native-investigation.md), [Go metadata](go-binaries.md), [Rust dependencies](rust-binaries.md), [offline ELF layout](binary-diagnostics.md), [recorded crashes](recorded-crashes.md)                                                                                                                                                 |
| Select a native provider or import format             | [Ghidra semantics](ghidra-provider.md), [Windows](windows-ghidra-p0.md), [DOS](ghidra-dos.md), [raw bytes](raw-binary.md), [MIPS](ghidra-mips.md), [PSP](ghidra-psp.md), [NativeAOT](ghidra-nativeaot.md), [IDA](ida-provider.md), [Kuna](kuna-provider.md), [JEB](jeb-analysis.md)                                                                    |
| Analyze JavaScript or Electron artifacts              | [Static analysis](javascript-artifact-reconstruction.md), [tracing and comparisons](javascript-application-workflows.md), [application graph](javascript-application-graph.md), [source recovery](javascript-recovery.md)                                                                                                                              |
| Compare static JavaScript with runtime observations   | [Runtime attribution](javascript-runtime-observation.md), [reconciliation](javascript-runtime-reconciliation.md), [Electron observation](electron-observation.md), [Electron fuses](electron-fuses.md)                                                                                                                                                 |
| Inspect assemblies, packages, or firmware             | [Godot PCK](godot-pck.md), [.NET](managed-code-analysis.md), [Android](android-analysis.md), [Apktool resources](apktool-resource-analysis.md), [ADB acquisition](adb-device-analysis.md), [Apple](apple-application-analysis.md), [Harmony](harmony-analysis.md), [firmware](firmware-analysis.md), [WASM](wasm-artifacts.md), [EVM](evm-bytecode.md) |
| Observe or compare a browser scenario                 | [CDP observation](browser-observation.md), [scenarios](browser-scenario-contract.md), [runtime evidence](web-runtime.md), [historical captures](web-network-captures.md)                                                                                                                                                                               |
| Serve recorded behavior as an agent environment       | [Agent environments](agent-environments.md)                                                                                                                                                                                                                                                                                                            |
| Trace captured website source                         | [Source locations](web-source-location.md), [module traces](website-module-trace.md), [script export](website-script-export.md)                                                                                                                                                                                                                        |
| Capture and compare a command                         | [Process capture](process-capture.md)                                                                                                                                                                                                                                                                                                                  |
| Inspect language-specific AOT metadata                | [Flutter](flutter-analysis.md), [IL2CPP](il2cpp-metadata.md)                                                                                                                                                                                                                                                                                           |
| Inspect recorded packets, signals or databases        | [Packet fields](packet-captures.md), [PCAP summaries](pcap-analysis.md), [signals](signal-recordings.md), [SQLite](sqlite-databases.md)                                                                                                                                                                                                                |
| Run an explicit debugger or instrumentation operation | [Objdump, GDB, Rizin and Cutter](reverse-engineering-tools.md), [Frida](frida-instrumentation.md)                                                                                                                                                                                                                                                      |
| Use CLI/MCP and retain evidence                       | [CLI](cli.md), [MCP contracts](mcp-contracts.md), [agent prompts](mcp-prompts.md), [obligation ledgers](reconstruction-obligation-ledgers.md)                                                                                                                                                                                                          |

For model-file declarations, use [GGUF and safetensors inspection](model-artifacts.md).

For development, use [testing](testing.md), [client verification](client-verification.md),
[tool design](tool-design.md), and [releasing](releasing.md).
[Reconstruction readiness](reconstruction-readiness.md) describes a development
conformance verifier. [The 6.2 migration](migration-6.2.md) records that release's
compatibility changes; [the roadmap](roadmap.md) tracks future work.

![REA inspecting a native binary in Hopper](./assets/rea-hopper-analysis.png)
