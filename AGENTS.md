# AGENTS.md

This repository defines the shared baseline for React Native / Expo mobile engineering.

## Context loading

Do not load every playbook document for every task. Use the smallest relevant context.

### Always read

1. the target product repository's local agent instructions (`AGENTS.md`, `CLAUDE.md`, Cursor/Copilot rules, or equivalent);
2. the product/domain documentation directly relevant to the current task.

Project-specific documented requirements override generic examples in this playbook.

### Read when needed

- `MOBILE_ENGINEERING_PLAYBOOK.md` - when making architectural decisions, starting a project, or resolving an unfamiliar mobile-engineering question;
- `guides/decision-ladder.md` - before adding or adopting a cross-cutting dependency or framework;
- `guides/agent-failure-modes.md` - when generated code looks web-shaped, overbuilt, dependency-heavy, or otherwise suspicious;
- `guides/platform-native-rules.md` - when touching native configuration, `ios/` / `android/`, auth routing/security boundaries, storage, safe areas, edge-to-edge behavior, keyboard behavior, OTA/native compatibility, or lifecycle-sensitive behavior;
- `guides/vertical-slice-checklist.md` - before calling a meaningful feature slice complete;
- an archetype - only when the product or user explicitly selected it and the current task materially relates to it.

Do **not** silently choose an archetype because a project merely resembles one.

Context budget is also a resource: load guidance because it is relevant, not because it exists.

## Default operating mode

Use the simplest implementation that correctly expresses the current product requirement.

Start from:

- current stable Expo SDK;
- the React Native and React versions supported by that Expo SDK;
- React Native New Architecture as the modern baseline;
- strict TypeScript;
- Expo Router for real application navigation;
- React Native primitives;
- `StyleSheet`;
- Expo platform APIs where appropriate.

Do not install a UI framework, styling framework, state library, animation framework, server-state library, storage framework, auth platform, or testing framework merely because it is common in starter projects.

Every dependency must solve a concrete problem that exists now.

## Build vertically

Prefer one complete thin user workflow over broad unfinished infrastructure.

```text
one user goal
→ real interaction
→ state change
→ visible result
→ verification
```

Do not build the complete API layer, design system, state layer, or test architecture before a user can accomplish a useful task.

## Complexity order

Escalate in this order:

```text
React Native primitive
→ Expo API
→ small local abstraction
→ focused dependency
→ larger framework
→ custom native implementation
```

Stop when the problem is solved well enough.

## Archetype rule

Archetypes under `archetypes/` describe recurring product forces, not mandatory architectures.

When one is explicitly selected:

- use its vocabulary, states, questions, and architectural biases as context;
- do not interpret bias values as priorities, package requirements, or installation instructions;
- verify every tool against the actual product;
- let product-specific rules override archetype guidance.

## Agent guardrails

- Do not use DOM elements, browser storage, browser navigation, CSS, or `className` in native code unless the project explicitly adopted a cross-platform system that provides them.
- Do not add a styling/state/query/auth/storage library "for speed" without a demonstrated product need.
- Installed or transitive does not mean adopted. Unused direct imports do not prove a package is removable.
- Keep strict TypeScript. Do not use `any`, unsafe assertions, or ignore directives merely to make generated code compile.
- Do not create generic wrappers, barrel files, or architecture layers before real repetition or ownership boundaries justify them.
- Do not treat Expo Go as proof of native behavior once native configuration or native libraries matter.
- Before editing `ios/` or `android/`, determine whether the project uses CNG/Prebuild as source of truth or explicitly owns native projects.
- In CNG-owned projects, persistent native changes belong in app config, config plugins, Expo modules, or another reproducible mechanism - not only in generated native files.
- Prefer Expo Router Protected Routes for route-level access rules instead of scattered `useEffect` + `router.replace()` guards.
- Never treat Protected Routes or hidden screens as server-side authorization. Protected backend operations must authenticate and authorize independently.
- Use route/layout Error Boundaries for unexpected React render/lifecycle failures, not expected product states.
- Do not assume Error Boundaries catch ordinary event-handler or async-operation failures; handle those explicitly.
- Do not blindly wrap every screen in a safe-area container. Avoid double insets.
- Do not consider text-entry UI verified until it has been exercised with the keyboard open on targeted platforms.
- When OTA delivery is adopted, do not publish updates across incompatible native runtimes.

## Storage baseline

Choose persistence by sensitivity and data shape:

```text
small sensitive key-value
→ expo-secure-store

small non-sensitive key-value
→ AsyncStorage

SQLite already adopted + simple key-value
→ consider expo-sqlite/kv-store

structured/queryable local data
→ expo-sqlite when justified

offline synchronization
→ define the offline model first, then choose sync tooling
```

Do not invent a global persisted session store merely because authentication exists.

## Auth/session baseline

Keep these concerns separate:

```text
authentication identity
≠ credential/token storage
≠ route protection
≠ server authorization
≠ user profile/server data
≠ application state
```

Do not add an auth platform until the product actually has account/identity requirements that justify one.

Use secure platform storage for sensitive client credentials when such credentials must exist on-device. Prefer Expo Router Protected Routes for client-side access control. Enforce protected data access independently on the server.

Model restoration/expiration deliberately:

```text
unknown / restoring
→ authenticated
→ unauthenticated
→ expired / revoked
```

Do not flash protected UI before restoration completes. On logout or account/session change, remove credentials and invalidate user-scoped cached/persisted data where retaining it could expose the previous user's information.

## UI and platform rules

- Domain components are preferred over speculative generic abstractions.
- Start with `StyleSheet` and small token files.
- Use native controls when appropriate; use `@expo/ui` selectively.
- Add Gesture Handler or Reanimated only when interaction complexity requires them.
- Motion and haptics must communicate meaningful state or causality.
- Accessibility is part of correctness.
- Treat edge-to-edge and safe-area insets as normal platform layout inputs.
- Use `react-native-safe-area-context` when the application owns an inset; do not double-apply navigator-managed insets.

## Verification rules

A UI/native task is not complete because TypeScript compiles.

For meaningful changes:

1. run the application in the correct runtime;
2. reach the changed flow;
3. exercise the primary interaction;
4. inspect the actual rendered result;
5. verify realistic loading/error/permission/empty states;
6. verify keyboard and safe-area behavior when relevant;
7. verify cold-start/background-resume/process-restart behavior when the workflow depends on it;
8. verify OTA/native runtime compatibility when the product uses OTA updates.

Use Maestro for deterministic critical E2E paths when justified.

Use agent-device or equivalent tooling for exploratory and visual verification after a runnable UI exists.

Do not use device-driving agents as a substitute for deterministic regression coverage.

## Dependency rule

Before adding or adopting a dependency, answer:

1. What exact problem exists today?
2. Can React Native solve it?
3. Can Expo solve it?
4. Can a small local abstraction solve it?
5. Is there already an adopted solution in this project?
6. Why is this dependency the smallest appropriate solution?
7. What runtime, native-build, bundle, migration, and maintenance cost does it introduce?
8. Does it support the current Expo / React Native New Architecture and runtime model?

If the answers are weak, do not add it.

## Definition of done

At minimum, a meaningful vertical slice should:

- boot in the correct runtime;
- be reachable;
- allow the primary user action;
- produce a visible expected result;
- handle realistic failure/permission/loading states;
- pass strict TypeScript and relevant checks;
- provide basic accessibility for custom controls;
- avoid unnecessary dependencies;
- have been exercised in the running app;
- respect server authorization boundaries where protected data is involved;
- survive relevant lifecycle interruptions when the product requires it;
- respect OTA/native compatibility when OTA delivery is used.

Use `guides/vertical-slice-checklist.md` for the fuller checklist.

Complexity must be earned.
