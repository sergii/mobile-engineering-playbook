# AGENTS.md

This repository defines the shared baseline for React Native / Expo mobile engineering.

## Required reading

Before making implementation or architectural decisions, read:

1. `MOBILE_ENGINEERING_PLAYBOOK.md`
2. `guides/decision-ladder.md`
3. The target application's own `AGENTS.md`, README, architecture notes, and domain documentation

Project-specific documented requirements override this generic playbook.

## Default operating mode

Use the simplest implementation that correctly expresses the product requirement.

Start from:

- current stable Expo SDK;
- the React Native version supported by that Expo SDK;
- TypeScript;
- Expo Router for real application navigation;
- React Native primitives;
- `StyleSheet`;
- Expo platform APIs where appropriate.

Do not install a UI framework, styling framework, state-management library, animation framework, server-state library, or testing framework merely because it is commonly used.

Every dependency must solve a concrete problem that exists now.

## Build vertically

Prefer a complete thin user workflow over a broad unfinished architecture.

Good:

```text
open scanner
→ detect code
→ resolve item
→ show result
→ confirm
```

Bad:

```text
build the complete camera architecture
build the complete design system
build the complete state layer
```

before a user can complete a useful task.

## Complexity order

Escalate in this order:

```text
React Native primitive
→ Expo API
→ small local abstraction
→ focused library
→ larger framework
→ custom native implementation
```

Stop as soon as the problem is solved well enough.

## UI rules

- Domain components are preferred over speculative generic abstractions.
- Start with `StyleSheet` and small token files.
- Extract tokens from repeated decisions, not imagined future requirements.
- Prefer semantic tokens as the product matures.
- Use native controls when they are appropriate.
- Use `@expo/ui` selectively, not as a mandatory application-wide UI system.
- Add Reanimated or Gesture Handler only when interaction complexity requires them.
- Motion must explain state, causality, continuity, or spatial relationships. Avoid decorative motion by default.
- Accessibility is part of correctness.

## Verification rules

Do not consider a UI task complete because the code compiles.

For meaningful UI changes:

1. run the application;
2. reach the changed flow;
3. exercise the primary interaction;
4. inspect the actual rendered result;
5. verify important states and failure behavior.

Use deterministic tests for deterministic behavior.

Use Maestro for critical end-to-end user journeys when E2E coverage is justified.

Use agent-device or an equivalent device-control tool for exploratory, visual, and agent-driven verification after a runnable UI exists.

Do not use agent-device as a substitute for deterministic E2E tests.

## Dependency rule

Before adding a dependency, answer:

1. What exact problem exists today?
2. Can React Native solve it?
3. Can Expo solve it?
4. Can a small local abstraction solve it?
5. Why is this dependency the smallest appropriate solution?
6. What runtime, build, upgrade, or maintenance cost does it introduce?

If these questions do not have convincing answers, do not add the dependency.

## Change discipline

- Keep changes proportional to the requested task.
- Do not refactor unrelated code without a clear reason.
- Do not introduce architecture for hypothetical future requirements.
- Prefer reversible decisions early.
- Record significant cross-cutting dependency or framework decisions briefly in an ADR or pull request description.

## Definition of done

At minimum, a completed vertical slice should:

- boot successfully;
- be reachable through the intended navigation;
- allow the primary user action;
- produce a visible expected result;
- handle important loading, disabled, empty, and failure states where relevant;
- pass TypeScript and relevant checks;
- provide basic accessibility for custom interactive controls;
- avoid unnecessary dependencies;
- have been inspected in a running application.

For critical flows, add deterministic E2E coverage.

For important visual flows, perform device or simulator visual verification.

Complexity must be earned.