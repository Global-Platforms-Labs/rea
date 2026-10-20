# Reconstruction obligation ledgers

`build_reconstruction_obligation_ledger` turns authenticated Evidence records
into a deterministic list of reconstruction claims. The same workflow
is available as:

- MCP tool: `build_reconstruction_obligation_ledger`
- CLI command: `rea build-reconstruction-obligation-ledger`

The ledger is conservative by design. Static Application Graph facts create
candidate obligations; they do not prove runtime or process behavior. A required
obligation closes only when one manifest binding supplies a unique owner, any
required parser/schema/domain type, every required case fixture, and a passing
verifier whose authority is comparable to the original observation. The
verifier must enumerate the obligation ID and its result must be present in the
input Evidence bundle. Contradictions, duplicate definitions or owners,
residual unknowns, missing dependencies, and unavailable authority keep closure
open or failed.

## What the ledger does and what the project supplies

| Operation                                | Input and result                                                                                             | Boundary                                                                                   |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ |
| `build_reconstruction_obligation_ledger` | Authenticated Evidence bundle, reviewed obligations and manifest; returns per-obligation closure diagnostics | Does not run the manifest's verifier command or write implementation code                  |
| `verify_reconstruction`                  | Finite behavioral/function/artifact claims referencing retained comparison Evidence                          | Evaluates those comparison claims; does not emit obligation-proof records                  |
| `evaluate_reconstruction_coverage`       | Inline coverage data and one `boundary_id`                                                                   | Evaluates declared inventory, ownership, proof and unresolved gaps for that boundary       |
| `npm run verify:readiness`               | Repository-owned conformance fixtures                                                                        | Tests REA's own readiness behavior; does not verify an arbitrary reconstructed application |

The current ledger authority check requires reconstruction fixture and verifier
Evidence with this shape:

- `predicate_type`: `rea.reconstruction-proof`.
- `operation`: `verify_reconstruction_obligations`.
- Observation classification: `observed`, with an authority accepted for the required boundary.
- `normalized_result.passed`: `true`.
- Matching `obligation_ids`, plus the applicable `fixture_ids`, `case_kinds`,
  `verifier_ids` and `claim_ids`.

`verify_reconstruction_obligations` is an expected Evidence operation value, not
a public CLI command or MCP tool. The repository contains controlled test proof
producers, but no general public runner that executes a project's tests and emits
these proof records. A project must implement and review that evidence-producing
harness before claiming automatic ledger closure. An agent must not relabel an
ordinary passing test or a `verify_reconstruction` result as such a proof.

An Evidence commitment validates record integrity and linkage. It does not
independently attest the truth of a caller-supplied test result. Review the
producer, exact target/implementation identity, observed results and coverage.
Until that integration exists, retain test reports and comparisons, keep the
ledger obligation open, and describe the gap explicitly.

Implementation: [ledger service](https://github.com/Global-Platforms-Labs/rea/blob/main/src/application/ReconstructionObligationLedgerService.ts),
[proof authority checks](https://github.com/Global-Platforms-Labs/rea/blob/main/src/application/ReconstructionObligationAuthority.ts),
[finite verification schema](https://github.com/Global-Platforms-Labs/rea/blob/main/src/domain/reconstructionVerificationSchemas.ts).
See [planning and AI-assisted work](ai-workflow.md) for the surrounding project workflow.

## Complete CLI request

This empty request is useful for checking client integration. It is valid and
returns an `unknown` ledger with zero obligations; absence of source Evidence
never claims closure:

```sh
rea build-reconstruction-obligation-ledger \
  '{
    "evidence_bundle": {
      "artifacts": [],
      "providers": [],
      "environments": [],
      "scenarios": [],
      "captures": [],
      "unknowns": [],
      "records": []
    },
    "reviewed_obligations": [],
    "manifest": {
      "bindings": [],
      "contradictions": []
    }
  }' \
  --json
```

For a real investigation, export an Evidence bundle from the session and replace
the empty bundle. Application Graph and process capture records generate
conservative candidates automatically. Add reviewed obligations when a required
claim is not represented by those sources.

## Reviewed obligation examples

Each example below is a complete `reviewed_obligations` element. Replace the
sample Evidence ID with an authenticated review record already present in
`evidence_bundle.records`. Stable IDs and versions make changes reviewable.

### CLI parsing

```json
{
  "obligation_id": "cli.config.invalid-option",
  "obligation_version": 1,
  "title": "Reject an invalid configuration option with the supported CLI error",
  "application_layer": "cli",
  "family": "argument-parsing",
  "target": {
    "artifact_sha256": null,
    "application_node_id": null,
    "semantic_node_id": null,
    "location": "rea --unknown-option"
  },
  "required": true,
  "required_case_kinds": ["positive", "negative", "malformed"],
  "required_original_authority": "process",
  "required_fixture_authority": "packaged-process",
  "required_verifier_authority": "packaged-process",
  "requires_parser_type": true,
  "dependency_obligation_ids": [],
  "residual_unknown_ids": [],
  "unavailable_authority": [],
  "required_next_evidence": ["Capture the installed CLI for all three cases."],
  "disposition": "active",
  "review_evidence_ids": [
    "ev_aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
  ]
}
```

### Protocol handler

```json
{
  "obligation_id": "protocol.rpc.cancel",
  "obligation_version": 1,
  "title": "Cancel an in-flight request and emit one terminal response",
  "application_layer": "protocol",
  "family": "request-response",
  "target": {
    "artifact_sha256": null,
    "application_node_id": null,
    "semantic_node_id": null,
    "location": "JSON-RPC request cancellation"
  },
  "required": true,
  "required_case_kinds": ["positive", "negative", "malformed", "cancellation"],
  "required_original_authority": "external",
  "required_fixture_authority": "protocol",
  "required_verifier_authority": "protocol",
  "requires_parser_type": true,
  "dependency_obligation_ids": [],
  "residual_unknown_ids": [],
  "unavailable_authority": [],
  "required_next_evidence": [
    "Record request, cancellation, and terminal response IDs."
  ],
  "disposition": "active",
  "review_evidence_ids": [
    "ev_bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb"
  ]
}
```

### Electron startup and background behavior

```json
{
  "obligation_id": "electron.background.startup",
  "obligation_version": 1,
  "title": "Start the background process before exposing the renderer bridge",
  "application_layer": "electron",
  "family": "startup",
  "target": {
    "artifact_sha256": null,
    "application_node_id": null,
    "semantic_node_id": null,
    "location": "main process startup"
  },
  "required": true,
  "required_case_kinds": ["positive", "negative", "teardown"],
  "required_original_authority": "runtime",
  "required_fixture_authority": "renderer",
  "required_verifier_authority": "renderer",
  "requires_parser_type": false,
  "dependency_obligation_ids": [],
  "residual_unknown_ids": [],
  "unavailable_authority": [],
  "required_next_evidence": [
    "Observe main/renderer ordering and application shutdown."
  ],
  "disposition": "active",
  "review_evidence_ids": [
    "ev_cccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccccc"
  ]
}
```

### Persistence migration

```json
{
  "obligation_id": "persistence.profile.migrate-v2",
  "obligation_version": 1,
  "title": "Migrate a version-one profile without losing unknown fields",
  "application_layer": "persistence",
  "family": "migration",
  "target": {
    "artifact_sha256": null,
    "application_node_id": null,
    "semantic_node_id": null,
    "location": "profile schema v1 to v2"
  },
  "required": true,
  "required_case_kinds": ["positive", "negative", "malformed"],
  "required_original_authority": "runtime",
  "required_fixture_authority": "integration",
  "required_verifier_authority": "integration",
  "requires_parser_type": true,
  "dependency_obligation_ids": [],
  "residual_unknown_ids": [],
  "unavailable_authority": [],
  "required_next_evidence": [
    "Compare pre-migration and committed storage snapshots."
  ],
  "disposition": "active",
  "review_evidence_ids": [
    "ev_dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
  ]
}
```

### Native ABI

```json
{
  "obligation_id": "native-abi.helper.status",
  "obligation_version": 1,
  "title": "Preserve the helper status-code and output-buffer contract",
  "application_layer": "native-abi",
  "family": "ffi",
  "target": {
    "artifact_sha256": null,
    "application_node_id": null,
    "semantic_node_id": null,
    "location": "helper_status(const uint8_t *, size_t, uint8_t *, size_t *)"
  },
  "required": true,
  "required_case_kinds": ["positive", "negative", "malformed"],
  "required_original_authority": "process",
  "required_fixture_authority": "native-abi",
  "required_verifier_authority": "native-abi",
  "requires_parser_type": true,
  "dependency_obligation_ids": [],
  "residual_unknown_ids": [],
  "unavailable_authority": [],
  "required_next_evidence": [
    "Run the ABI harness against the shipped and reconstructed libraries."
  ],
  "disposition": "active",
  "review_evidence_ids": [
    "ev_eeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee"
  ]
}
```

## Manifest and closure

A manifest binding names one owner and its exact implementation digest. Both
`original_cases` and the reconstruction fixture list must cover every
`required_case_kinds` value with authenticated, comparable Evidence.
`parser_type` is mandatory when `requires_parser_type` is true.

```json
{
  "obligation_id": "protocol.rpc.cancel",
  "owner": {
    "module_path": "src/protocol/requestRouter.ts",
    "symbol": "cancelRequest",
    "owner_sha256": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
  },
  "parser_type": {
    "parser": "parseCancelRequest",
    "schema": "cancelRequestSchema",
    "domain_type": "CancelRequest"
  },
  "original_cases": [
    {
      "kind": "positive",
      "evidence_id": "ev_6666666666666666666666666666666666666666666666666666666666666666",
      "location": "/captures/positive"
    },
    {
      "kind": "negative",
      "evidence_id": "ev_7777777777777777777777777777777777777777777777777777777777777777",
      "location": "/captures/negative"
    },
    {
      "kind": "malformed",
      "evidence_id": "ev_8888888888888888888888888888888888888888888888888888888888888888",
      "location": "/captures/malformed"
    },
    {
      "kind": "cancellation",
      "evidence_id": "ev_9999999999999999999999999999999999999999999999999999999999999999",
      "location": "/captures/cancellation"
    }
  ],
  "fixtures": [
    {
      "fixture_id": "rpc.cancel.positive",
      "case_kind": "positive",
      "authority": "protocol",
      "evidence_ids": [
        "ev_1111111111111111111111111111111111111111111111111111111111111111"
      ]
    },
    {
      "fixture_id": "rpc.cancel.negative",
      "case_kind": "negative",
      "authority": "protocol",
      "evidence_ids": [
        "ev_2222222222222222222222222222222222222222222222222222222222222222"
      ]
    },
    {
      "fixture_id": "rpc.cancel.malformed",
      "case_kind": "malformed",
      "authority": "protocol",
      "evidence_ids": [
        "ev_3333333333333333333333333333333333333333333333333333333333333333"
      ]
    },
    {
      "fixture_id": "rpc.cancel.cancellation",
      "case_kind": "cancellation",
      "authority": "protocol",
      "evidence_ids": [
        "ev_4444444444444444444444444444444444444444444444444444444444444444"
      ]
    }
  ],
  "verifier": {
    "verifier_id": "rpc.cancel.conformance",
    "claim_id": "protocol.rpc.cancel.v1",
    "command": "npm run test:protocol -- cancel",
    "authority": "protocol",
    "status": "pass",
    "result_evidence_id": "ev_5555555555555555555555555555555555555555555555555555555555555555",
    "enumerated_obligation_ids": ["protocol.rpc.cancel"],
    "nondeterminism": {
      "mode": "partial-order",
      "specification": "cancel precedes the single terminal response"
    }
  }
}
```

All referenced Evidence IDs must exist in the bundle. A green unit test cannot
close a `packaged-process` obligation. A static-only source cannot close a
runtime or process claim. Marking a claim `blocked` or `out-of-scope` records the
disposition but does not silently count it as verified.

The tool returns every obligation inline. Its `ledger_id` and `closure_digest`
identify the complete result, including summaries, reports, ownership and
dependency graphs, Evidence links, and limitations.

For named boundaries with explicit verifier contracts and package-proof kinds,
see [reconstruction coverage](reconstruction-coverage.md).
