# Executable Starter Repository Bootstrap

Target repository: `sergii/react-native-production-starter`

This file exists so repository creation is the only manual/bootstrap boundary. Once the empty repository exists, an implementation agent can populate it without re-deciding architecture.

## Repository role

The executable repository owns:

- generated Expo application code;
- `package.json` and lockfile;
- app configuration;
- CI workflows;
- EAS configuration;
- tests and Maestro flows;
- optional agent-device replay scripts/evidence;
- proof records;
- release/build metadata implementation.

It does not own portfolio policy. Policy remains in `sergii/mobile-engineering-playbook`.

## Required upstream pins

Playbook:

- version: `v0.5.0`
- release-content revision: `3deecbbf63d0f116dbdf7ff1fd904d5799e425e1`
- frozen documentation branch: `release/v0.5.0-docs`

Stack snapshot:

- `snapshots/expo57-2026-09-26.yaml`
- status at bootstrap: `candidate`

## Expected first repository files

```text
README.md
AGENTS.md
package.json
package-lock.json
app.config.ts
eas.json
tsconfig.json

app/
src/

tests/
.maestro/

docs/
  architecture/
  release/
  proofs/

.github/
  workflows/
```

Exact generated files may differ with the pinned Expo template. Do not manufacture directories that the template does not need.

## First commit sequence

1. Scaffold using the exact command from the snapshot.
2. Commit the untouched generated project and lockfile as `chore: scaffold from expo57 snapshot`.
3. Run compatibility checks and record output.
4. Add the product-local `AGENTS.md` pinned to the frozen playbook revision.
5. Implement Phase 1 as small reviewable commits.
6. Do not mark the snapshot verified merely because generation succeeds.

## Phase 1 implementation order

Prefer this order:

```text
scaffold + lockfile
→ compatibility checks
→ environment/build identity
→ strict static checks
→ navigation/deep-link contract
→ unit/component testing
→ GitHub Actions fast gate
→ secure credential boundary
→ network boundary
→ diagnostics/build metadata
→ crash/source-map integration
→ simulator/emulator runtime proof
```

## Testing boundary

Default:

- Jest + jest-expo;
- React Native Testing Library;
- Expo Router testing utilities;
- Maestro for deterministic E2E;
- agent-device for exploratory verification/evidence;
- XcodeBuildMCP only when deeper iOS-native debugging is needed.

Do not replace deterministic coverage with LLM-driven device exploration.

## Repository creation settings

Recommended initial settings:

- default branch: `main`;
- initialize as an empty repository so the pinned Expo scaffolder owns the first generated dependency graph;
- do not add a GitHub-provided README, .gitignore, or license before scaffolding if that would interfere with the deterministic first commit;
- visibility should follow the owner's intended distribution model and is deliberately not guessed here.

## Blocker

The currently connected GitHub action set does not expose repository creation.

Once the repository exists, this document is sufficient to continue directly with Step 2 and Phase 1.
