# AGENTS.md

This repository defines the shared baseline for React Native / Expo mobile engineering.

## Required reading

Before making implementation or architectural decisions, read:

1. `MOBILE_ENGINEERING_PLAYBOOK.md`
2. `guides/decision-ladder.md`
3. `guides/agent-failure-modes.md`
4. `guides/platform-native-rules.md`
5. the target application's own `AGENTS.md`, README, architecture notes, and domain documentation
6. any archetype explicitly referenced by the target project or the user

Project-specific documented requirements override this generic playbook.

Do **not** silently choose an archetype just because a project looks similar. Archetypes are optional context, not automatic policy.

## Default operating mode

Use the simplest implementation that correctly expresses the product requirement.

Start from:

- current stable Expo SDK;
- the React Native and React versions supported by that Expo SDK;
- React Native New Architecture as the modern baseline;
- TypeScript with strict checking;
- Expo Router for real application navigation;
- React Native primitives;
- `StyleSheet`;
- Expo platform APIs where appropriate.

Do not install a UI framework, styling framework, state-management library, animation framework, server-state library, storage framework, or testing framework merely because it is common in modern starter projects.

Every dependency must solve a concrete problem that exists now.

## Build vertically

Prefer a complete thin user workflow over a broad unfinished architecture.

Good examples:

```text
open checkout
→ choose delivery
→ choose payment
→ confirm order
→ show receipt
```

```text
open conversation
→ write reply
→ send
→ show delivered state
```

```text
select destination
→ confirm pickup
→ request ride
→ show driver-search state
```

Bad:

```text
build the complete API layer
build the complete design system
build the complete state layer
```

before a user can complete a useful task.

Use `guides/vertical-slice-checklist.md` when finishing a slice.

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

## Archetype rule

Archetypes live under `archetypes/` and describe product-class forces, not mandatory architectures.

When an archetype is explicitly selected:

- treat its vocabulary, likely states, and architectural biases as context;
- still verify each dependency against the actual product;
- do not copy every component, state, or tool from the archetype;
- let project-specific rules win when they differ;
- record meaningful divergence only when it helps future maintainers.

## Agent guardrails

AI coding agents commonly import web habits, infer architecture from installed packages, or patch generated native files. Do not do that.

- Do not use DOM elements such as `div`, `span`, or `button` in native application code.
- Do not introduce CSS or `className` unless the project has explicitly adopted a system that supports them.
- Do not install NativeWind, Tamagui, Unistyles, or another styling system "for speed" without a documented product need.
- Do not treat the presence of a direct or transitive package as permission to adopt it in application code.
- Do not remove packages required by Expo, Expo Router, config plugins, or another adopted tool merely because application code does not import them directly.
- Do not assume Expo Go is the normal runtime once native libraries or native configuration matter.
- Do not declare native behavior verified only because TypeScript passes or Metro starts.
- Do not add browser storage, DOM navigation, or other web-only assumptions to native code without an explicit cross-platform requirement.
- Do not weaken types to make generated code compile. Avoid `any`; when unavoidable, isolate and justify it.
- Do not create barrel files by habit. Add them only when they improve a real module boundary.
- Do not add a dependency before checking whether the project already has an adopted solution for the same problem.
- Before editing `ios/` or `android/`, determine whether the project uses CNG/Prebuild as the source of truth or explicitly owns native projects.
- In CNG-owned projects, do not leave persistent fixes only in generated native files. Move them into app config, config plugins, or an appropriate Expo module.
- Do not implement route-level auth primarily with `useEffect` + `router.replace()` when Expo Router Protected Routes express the access rule declaratively.
- Do not wrap every route in a safe-area component blindly. Avoid double insets.
- Do not consider text-entry UI verified until it has been exercised with the keyboard open on targeted platforms.

See `guides/agent-failure-modes.md` and `guides/platform-native-rules.md` for examples and recovery rules.

## Runtime and native ownership

Expo Go is acceptable for early experiments that fit entirely inside its bundled native capabilities.

Move to a development build when native runtime configuration matters, including native libraries, config plugins, app-specific permissions/entitlements, custom native modules, or capabilities not present in Expo Go.

Once the product depends on a development build, verify native features in that development build or a production-like build.

For native project ownership:

```text
CNG / Prebuild source of truth
→ ios/ and android/ are generated output
→ persistent native configuration belongs in app config / config plugins / modules

explicitly owned native projects
→ direct Xcode / Gradle / native-source edits are valid
```

EAS is an Expo-integrated build and delivery option, not a mandatory application architecture. Local native builds are valid when they fit the project.

## Navigation rules

- Prefer Expo Router for real application navigation.
- Prefer Protected Routes for route-level authentication and authorization in current Expo Router projects.
- Use route groups for organization, not as a substitute for explicit access rules.
- Use route/layout Error Boundaries for unexpected runtime/render failures.
- Model expected product states such as validation, offline, empty, denied, or payment failures explicitly in normal UI.

## Storage rules

Choose persistence based on sensitivity and data shape:

```text
small sensitive key-value
→ expo-secure-store

small non-sensitive key-value
→ AsyncStorage

SQLite already adopted + simple key-value need
→ consider expo-sqlite/kv-store

structured/queryable local data
→ expo-sqlite when justified

offline sync
→ define offline model first, then choose sync tooling
```

Do not use browser `localStorage` by habit in native code.

Do not add a database for a handful of preferences.

## UI rules

- Domain components are preferred over speculative generic abstractions.
- Start with `StyleSheet` and small token files.
- Extract tokens from repeated decisions, not imagined future requirements.
- Prefer semantic tokens as the product matures.
- Use native controls when they are appropriate.
- Use `@expo/ui` selectively, not as a mandatory application-wide UI system.
- Add Reanimated or Gesture Handler only when interaction complexity requires them.
- Motion must explain state, causality, continuity, or spatial relationships.
- Accessibility is part of correctness.
- Domain-heavy UI examples belong in an archetype or product documentation, not in the universal core.
- Treat edge-to-edge and safe-area insets as normal platform layout inputs.
- Use `react-native-safe-area-context` when the application owns an inset; do not double-apply navigator-managed insets.

## TypeScript rules

- Keep `strict` enabled in new projects unless a documented compatibility constraint prevents it.
- Prefer explicit domain types over broad object shapes.
- Avoid `any`. If an external boundary forces it, contain the unsafe value at that boundary and convert it to a validated type.
- Do not use type assertions merely to silence a design or data-model problem.

## Verification rules

Do not consider a UI task complete because the code compiles.

For meaningful UI changes:

1. run the application;
2. reach the changed flow;
3. exercise the primary interaction;
4. inspect the actual rendered result;
5. verify important states and failure behavior;
6. if text input exists, exercise the flow with the keyboard open;
7. verify safe areas/system UI on targeted platforms.

Use deterministic tests for deterministic behavior.

Use Maestro for critical end-to-end user journeys when E2E coverage is justified.

Use agent-device or an equivalent device-control tool for exploratory, visual, and agent-driven verification after a runnable UI exists.

Do not use device-driving agents as a substitute for deterministic regression tests.

## Dependency rule

Before adding or adopting a dependency, answer:

1. What exact problem exists today?
2. Can React Native solve it?
3. Can Expo solve it?
4. Can a small local abstraction solve it?
5. Is there already an adopted solution in this project?
6. Why is this dependency the smallest appropriate solution?
7. What runtime, native-build, upgrade, bundle, or maintenance cost does it introduce?

If these questions do not have convincing answers, do not add the dependency.

A package being present transitively is not the same as the project adopting that package's API.

## Change discipline

- Keep changes proportional to the requested task.
- Do not refactor unrelated code without a clear reason.
- Do not introduce architecture for hypothetical future requirements.
- Prefer reversible decisions early.
- Record significant cross-cutting dependency or framework decisions briefly in an ADR or pull request description.

## Definition of done

At minimum, a completed vertical slice should:

- boot successfully in the correct runtime;
- be reachable through the intended navigation;
- allow the primary user action;
- produce a visible expected result;
- handle important loading, disabled, empty, permission, and failure states where relevant;
- pass strict TypeScript and relevant checks;
- provide basic accessibility for custom interactive controls;
- avoid unnecessary dependencies;
- have been inspected in a running application;
- have relevant keyboard/safe-area behavior verified.

For critical flows, add deterministic E2E coverage.

For important visual flows, perform device or simulator visual verification.

Complexity must be earned.
