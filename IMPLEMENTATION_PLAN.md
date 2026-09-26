# Production Starter Implementation Plan

Status legend:

- `DONE` - completed and evidenced
- `IN PROGRESS` - active work
- `BLOCKED` - external capability or input required
- `PENDING` - intentionally not started yet

## 1. Freeze documentation baseline

Status: **DONE**

Deliverables:

- publish the current playbook iteration as `v0.5.0`;
- pin the release-content commit;
- create a frozen release ref;
- ensure product templates can point to an exact revision;
- include production starter, stack snapshots, and testing/agent strategy.

Exit condition:

- exact immutable/reviewable revision is recorded and no implementation work depends on moving `main`.

## 2. Create separate executable starter repository

Status: **DONE**

Target repository:

`sergii/react-native-production-starter`

The repository owns executable code, generated lockfile, CI, EAS configuration, tests, proof artifacts, and release evidence.

The playbook remains the policy source of truth.

Exit condition:

- separate repository exists and references the frozen playbook revision and exact stack snapshot.

## 3. Implement Phase 1 minimal runnable starter

Status: **IN PROGRESS**

Scope:

- scaffold from exact compatibility snapshot;
- strict TypeScript;
- dev/staging/production identity;
- deterministic build/release metadata;
- Expo Router navigation and deep-link contract;
- lint/typecheck/unit/component tests;
- GitHub Actions fast checks;
- crash-reporting adapter + source-map release path;
- secure credential boundary;
- network boundary;
- support-safe diagnostics surface;
- basic accessibility/testability semantics.

Exit condition:

- app boots on iOS and Android development runtimes and Phase 1 checks pass.

## 4. Verify the compatibility snapshot

Status: **PENDING**

Run and record:

- generated project + lockfile;
- `npx expo install --check`;
- `npx expo-doctor@latest`;
- typecheck;
- lint;
- unit/component tests;
- iOS simulator;
- Android emulator;
- real iPhone;
- representative/low-end Android;
- internal distribution where applicable.

Exit condition:

- snapshot status changes from `candidate` to `verified` only when required proof exists.

## 5. Add upgrade-path harness

Status: **PENDING**

Build:

```text
previous release
→ create realistic persisted/session state
→ install next release over it
→ launch
→ run migrations
→ verify auth/session
→ verify deep link
→ verify critical flow
```

Keep the previous production release as a first-class test fixture.

Exit condition:

- clean-install green and upgrade green are independently proven.

## 6. Implement Phase 2 release and lifecycle safety

Status: **PENDING**

Scope:

- EAS/native build path;
- TestFlight / Play internal distribution;
- OTA runtime compatibility;
- channels/environment separation;
- rollback procedure;
- minimum supported version policy;
- optional vs required update flow;
- foreground/background/resume/process-restart verification.

Exit condition:

- release lifecycle is executable, not merely documented.

## 7. Add opt-in production recipes

Status: **PENDING**

Prefer removable recipes/modules over mandatory dependencies:

```text
recipes/
  notifications/
  analytics/
  i18n/
  feature-flags/
  entitlements/
  review-prompt/
```

Each recipe must state:

- problem solved;
- dependencies;
- native/runtime impact;
- install/remove path;
- test requirements;
- migration/rollback implications.

Exit condition:

- optional concerns can be adopted without restructuring the core starter.

## 8. Build and prove a tiny reference app

Status: **PENDING**

The reference app should exercise:

- sign-in/session restoration;
- persisted state;
- primary mutation/workflow;
- deep links;
- logout/account switching;
- upgrade migration;
- OTA-compatible update path;
- deterministic Maestro flow;
- agent-device exploratory verification and evidence.

Exit condition:

- the starter has proven itself in a product-shaped application.

## 9. Package the developer experience

Status: **PENDING**

Only after the starter is proven, choose the lightest useful distribution mechanism:

- GitHub Template;
- exact bootstrap script;
- small generator such as `create-*`;
- project bootstrap recipe for agents.

Avoid creating a custom CLI before repeated manual bootstrap work justifies it.

Exit condition:

- creating a new product from the verified starter is reproducible and low-friction.

## 10. Automate snapshot freshness detection

Status: **PENDING**

A scheduled/manual check may detect:

- new stable Expo SDK;
- Node LTS changes;
- critical Expo/RN fixes;
- native/store/toolchain requirement changes;
- security advisories;
- template/scaffolder updates.

Automation may create a candidate issue/PR/snapshot.

It must **not** silently replace a verified production snapshot or auto-promote a new stack without the proof matrix.

Exit condition:

- freshness is monitored while compatibility decisions remain explicit and reviewable.
