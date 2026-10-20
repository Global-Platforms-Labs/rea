---
type: documentation
entity: fork-maintenance
version: 1.0
updated: 2026-10-20
---

# Maintaining this REA fork

## Repository and distribution ownership

| Resource                                                                  | Owner and role                                                                        |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| [Global-Platforms-Labs/rea](https://github.com/Global-Platforms-Labs/rea) | Organization fork; review and integrate fork changes here                             |
| [morluto/rea](https://github.com/morluto/rea)                             | Upstream project, contribution policy and release history                             |
| `rea-agents` on npm, upstream site and release links                      | Existing upstream distribution; the package name does not select this fork's checkout |
| Local source build                                                        | Uses the selected checkout and its locked dependencies                                |

- Preserve upstream license, notices, attribution, package identity and useful documentation.
- A change merged into this fork does not establish an upstream merge or published package release.
- Keep documentation public and reusable. Project credentials, customer evidence, internal service addresses and access instructions belong in their authorized private location.
- Follow [contribution scope](https://github.com/Global-Platforms-Labs/rea/blob/main/CONTRIBUTING.md#contribution-scope) and [PR ownership/readiness](https://github.com/Global-Platforms-Labs/rea/blob/main/CONTRIBUTING.md#pull-request-ownership-and-readiness).

## Build the selected source revision

```sh
git clone https://github.com/Global-Platforms-Labs/rea.git
cd rea
git rev-parse HEAD
```

- Use `.nvmrc` and `package.json#packageManager` from that revision.
- Install the locked dependencies and build the runtime:

```sh
npm ci
npm run build:cached
node scripts/rea.mjs --help
node scripts/rea.mjs mcp
```

- `npx rea-agents` resolves the npm distribution. Use the local dispatcher to inspect this checkout's implementation.
- For client registration, follow [installation](installation.md) and [client configuration](client-configuration.md); review setup changes before applying them.
- Bring only the engines required by the selected workflow. Source compilation does not install or verify Ghidra, IDA, JEB or other optional engines.

## Synchronize upstream

1. Record the current branch, revision and working-tree state. Preserve local work before integration.
2. Inspect existing remotes. In a conventional fork checkout, `origin` points to this fork and `upstream` points to `morluto/rea`.
3. If the upstream remote is missing, add it once. Do not overwrite a caller's existing remote without resolving its intended role.
4. Fetch current references and compare the actual revisions before choosing an integration.

```sh
git status --short --branch
git remote -v
git fetch origin
git fetch upstream
git log --oneline --left-right origin/main...upstream/main
git diff --stat origin/main...upstream/main
```

Missing upstream remote:

```sh
git remote add upstream https://github.com/morluto/rea.git
```

5. Prepare integration on a dedicated project branch based on the fork's current main. Resolve documentation changes against the new source and contracts.
6. Preserve new upstream guides and behavior. Recompute [documentation coverage](documentation-coverage.md) instead of reusing stale counts.
7. Run checks appropriate to the affected files, then review the complete diff.
8. Open the integration PR against `Global-Platforms-Labs/rea`. A contribution to upstream is a separate PR and scope decision.

- Do not force-push the shared main branch to match upstream.
- Keep human ownership/readiness declarations with the human owner. Preserve any unresolved scope or verification limits in the PR.

## Verification

| Change                         | Checks and evidence                                                                           |
| ------------------------------ | --------------------------------------------------------------------------------------------- |
| Documentation                  | Relative links and anchors, `git diff --check`, `npm run docs:check`, `npm run docs:build`    |
| Source or contracts            | Relevant tests and `npm run check:fast`; add broader checks when affected scope requires them |
| Provider or lifecycle behavior | [Testing guide](testing.md), real-provider prerequisites and actual host/target coverage      |
| Release/package behavior       | [Release guide](releasing.md), package verification and explicit publishing authority         |

- Check source links, generated-file ownership and the package scripts in the selected revision.
- Documentation success establishes document consistency. Report runtime acceptance and provider verification separately.
- See [architecture](overview.md) and [planning/AI workflow](ai-workflow.md) before extending a capability.
