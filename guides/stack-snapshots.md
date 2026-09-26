# Stack Snapshots

A stack snapshot is a dated, reproducible compatibility contract for the executable React Native / Expo starter.

It exists because "use the latest versions" is not a safe mobile dependency policy. Expo, React Native, React, native toolchains, and Expo-managed packages move on related but different release cadences. The correct unit is the compatible stack, not each package independently.

## Two snapshot states

### Compatibility snapshot

A compatibility snapshot is assembled from authoritative upstream sources and records a coherent stable stack.

It may be used as the input to create a candidate starter, but it is not yet called proven.

Required evidence:

- Expo SDK compatibility table;
- Expo release/changelog notes;
- current stable template/scaffolder release;
- Node LTS status;
- SDK-specific recommended package versions;
- known regressions and native-toolchain caveats.

### Verified snapshot

A snapshot becomes verified only after the generated starter proves the recorded stack.

Minimum proof:

- project generated from the exact snapshot;
- lockfile committed;
- `npx expo install --check` passes;
- `npx expo-doctor@latest` passes;
- typecheck, lint, and unit/integration tests pass;
- iOS simulator build/launch passes;
- Android emulator build/launch passes;
- at least one real iPhone run passes;
- at least one low-end/representative Android run passes;
- internal distribution path is exercised;
- the evidence is tied to the snapshot id and git commit.

A documentation-only snapshot must not claim those runtime proofs.

## Snapshot contents

Each snapshot should record:

- snapshot id and date;
- status: `candidate`, `verified`, or `superseded`;
- stable-only / prerelease policy;
- Node version and support status;
- exact scaffolder and template versions;
- Expo SDK family;
- React Native, React, and React Native Web versions owned by that SDK;
- important Expo-managed package versions;
- EAS CLI version used for release tooling;
- testing choices;
- native ownership model;
- known compatibility notes;
- explicit verification state;
- upstream source URLs.

The machine-readable YAML file is the authoritative snapshot input for agents. The generated project's lockfile becomes the authoritative exact dependency graph after implementation.

## Version selection rules

1. Prefer the latest stable Expo SDK, not a beta/canary, for the production starter.
2. Treat the Expo SDK as the compatibility anchor.
3. Do not independently bump React Native or React beyond the versions supported by the chosen Expo SDK.
4. Use Expo's package resolver for Expo-managed/native packages:
   ```text
   npx expo install <package>
   npx expo install --check
   ```
5. Use `expo-doctor` as a required compatibility check.
6. Prefer an Active LTS Node release supported by the chosen Expo SDK.
7. Pin the scaffolder/template that created the snapshot. Do not use an unbounded `@latest` during a reproducible bootstrap.
8. External optional packages are not added to the core snapshot merely because they are popular or newly released. Add them only when the production-starter specification says the concern is implemented/scaffolded and after compatibility proof.
9. Once a project is generated, commit the lockfile and do not reconstruct the dependency graph from prose.
10. Keep prerelease experimentation in a separate snapshot. Never silently mutate a stable snapshot into beta/canary dependencies.

## Refresh policy

Create a new snapshot instead of editing an old one in place when a meaningful compatibility boundary changes.

Refresh when:

- a new stable Expo SDK is released;
- Expo publishes a critical regression/fix affecting the current snapshot;
- React Native/Expo support status changes materially;
- Apple/Google native toolchain or store requirements force a change;
- a security issue requires a dependency/toolchain change;
- the current snapshot is being adopted for a new executable starter after a long gap.

A new snapshot may supersede an old snapshot, but old snapshots remain immutable historical evidence.

## Agent usage

Agents should receive a small project-local instruction that points to:

1. the exact stack snapshot;
2. the production-starter specification;
3. the shared Mobile Engineering Playbook;
4. Expo's SDK-versioned project context/skills.

Do not paste the entire playbook into every agent prompt. The snapshot answers "which versions and compatibility assumptions?" while the playbook answers "how should we engineer this app?"

## Prior art and why we still keep snapshots

This pattern has strong precedents:

- Expo's current scaffolder generates project-level agent context, and Expo provides Skills plus an MCP server for live SDK-aware guidance.
- Ignite is a long-running opinionated React Native boilerplate with generators and a preselected stack.
- Platform-engineering systems such as Backstage Software Templates formalize the broader "golden path" idea: a reviewed template plus controlled inputs and repeatable creation steps.

We should reuse those ideas rather than invent a giant custom prompt.

Our additional layer is intentionally small: an immutable dated compatibility snapshot plus a proof matrix. That answers questions those systems do not necessarily answer for our portfolio: exactly which stack did we choose on a given date, why did we reject newer prereleases, and what evidence proved this particular combination?

References:

- https://docs.expo.dev/agents/
- https://docs.expo.dev/skills/
- https://docs.infinite.red/ignite-cli/
- https://backstage.io/docs/features/software-templates/

## Relationship to Expo's agent tooling

Expo now scaffolds new projects with agent context files and provides official Expo Skills and an Expo MCP server. That is complementary to this approach.

Use Expo's tooling for live, SDK-aware framework guidance. Use our snapshot for reproducibility, portfolio policy, evidence, and the exact compatibility baseline we intentionally selected.

This makes the model:

```text
our production-starter contract
        +
dated stack snapshot
        +
generated lockfile
        +
Expo SDK-versioned AGENTS / Skills / MCP
        +
verification evidence
```

The snapshot is not a replacement for upstream docs. It is the pinned input to a reproducible build.
