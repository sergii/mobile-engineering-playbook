# Decision Ladder

This guide defines when to introduce common React Native / Expo tools.

```text
Do not add a tool because it is available.
Add it when the application has a concrete problem that the tool solves better than the existing stack.
```

Archetypes may indicate that a class of product is more likely to encounter a problem earlier, but they never override this ladder.

Before adopting any dependency with native/runtime implications, verify that it supports the current Expo / React Native New Architecture, including bridgeless operation where applicable. Avoid dependencies that require the legacy bridge, disabling the New Architecture, or obsolete compatibility hacks.

---

# 1. Styling

## Default

Use React Native `StyleSheet`, local component styles, and small token files.

## Consider a styling layer when

A demonstrated problem appears, such as:

- many runtime themes;
- substantial phone/tablet responsive rules;
- large-scale semantic token switching;
- repeated media-query-like behavior;
- difficult platform-specific branching;
- design-system scale that makes plain imports hard to maintain.

Do not migrate merely for cleaner syntax.

---

# 2. Expo Router

## Default

Use Expo Router for real application navigation. A tiny one-screen experiment may avoid it temporarily.

Use it once stacks, tabs, modals, deep links, notification destinations, or platform-correct back behavior become product behavior.

## Authentication and authorization

Prefer current Expo Router Protected Routes for route-level access rules.

Use route groups to organize areas of the application, not as the access-control mechanism itself.

Do not default to scattered `useEffect` + `router.replace()` guards when a declarative route guard expresses the rule directly.

Imperative redirects remain valid for explicit transitions that are not access guards.

Reference: https://docs.expo.dev/router/advanced/authentication/

## Error boundaries

Expected product states such as validation errors, payment declines, offline state, empty results, or permission denial belong in normal product UI.

Use route/layout Error Boundaries for unexpected render/runtime failures and recovery scopes.

Reference: https://docs.expo.dev/router/error-handling/

---

# 3. Auth and Session

Authentication is not one concern. Keep these separate:

```text
identity/authentication
credential or token storage
route protection
authorization rules
user profile/server data
application state
```

## No accounts yet

Do not add an auth platform, session store, or credential persistence "for later".

## Product has accounts

Start from the actual backend/identity contract.

- if the client must hold a small sensitive credential/token, use secure platform storage such as `expo-secure-store`;
- protect routes declaratively with Expo Router Protected Routes;
- fetch profile/server data as server state when appropriate;
- keep transient UI/workflow state separate from authentication state.

## Consider an external auth platform when

The product has concrete identity requirements that justify one, such as managed OAuth/social login, enterprise identity, passwordless flows, MFA, organization membership, or account lifecycle needs that would otherwise be expensive to own.

Evaluate the provider against the real identity requirements. Do not choose Clerk, Auth0, Firebase Auth, Supabase Auth, or another provider merely because authentication exists.

## Do not invent a global persisted session store by default

Persist only the minimum client-side material required by the authentication model.

A token in SecureStore does not imply that the entire user/session/application state should also be persisted.

---

# 4. React Native Gesture Handler

## Default

Start with `Pressable`, scrolling primitives, and simple native interactions.

Consider Gesture Handler when:

- gestures coordinate with scroll views;
- multiple gestures interact;
- swipe/pan/pinch/long-press behavior becomes non-trivial;
- cancellation or simultaneous recognition matters;
- custom interaction semantics need production-grade gesture handling.

A transitive installation does not mean application code has adopted its API.

---

# 5. Reanimated

Use simple state changes and platform transitions where sufficient.

Add Reanimated when:

- animation is continuously linked to gestures;
- high-frequency animation needs UI-thread execution;
- shared values materially simplify complex interaction state;
- coordinated motion or advanced transitions are part of the product.

Do not add it for ordinary pressed opacity, simple show/hide behavior, or static state changes.

---

# 6. `@expo/ui`

Use React Native controls and primitives first.

Consider `@expo/ui` when native SwiftUI/Compose behavior creates a real product advantage, for example native menus, pickers, sheets, settings interactions, accessibility, or platform fidelity.

Do not make it a mandatory application-wide component system.

---

# 7. External State Management

Start with:

```text
useState
useReducer
Context
```

Consider Zustand, Redux, Jotai, or another state library when:

- many distant components mutate the same long-lived application state;
- state ownership is no longer clear;
- Context creates excessive coupling/update churn;
- state transitions deserve explicit centralized modeling;
- debugging shared application state has become difficult.

Do not add global state because the app has many screens, an API, or future complexity.

---

# 8. Server State

Use straightforward service calls for simple request/response workflows.

Consider TanStack Query or another server-state library when the product needs several of:

- caching/staleness policy;
- background refresh;
- request deduplication;
- retries;
- pagination;
- optimistic mutations;
- invalidation after mutation;
- multiple consumers of the same remote data.

Do not install it merely because the app calls an API.

---

# 9. Local Storage and Persistence

Choose persistence by sensitivity, data shape, access pattern, and offline requirements.

## Sensitive small key-value

Examples: access/refresh tokens or small credential material.

Prefer `expo-secure-store`.

Do not use SecureStore as the only source of truth for large or irreplaceable application data.

Reference: https://docs.expo.dev/versions/latest/sdk/securestore/

## Small non-sensitive key-value

Examples: `hasSeenOnboarding`, selected theme, filters, small preferences.

Default to `@react-native-async-storage/async-storage` when asynchronous KV storage is sufficient.

It is not secret storage.

Reference: https://docs.expo.dev/versions/latest/sdk/async-storage/

## SQLite already adopted

If `expo-sqlite` is already part of the product, consider `expo-sqlite/kv-store` instead of adding another KV dependency.

Reference: https://docs.expo.dev/versions/latest/sdk/sqlite/

## Structured/queryable local data

Consider `expo-sqlite` when the product needs persistent structured data, queries, transactions, or a real local data model.

Do not introduce a database for a handful of preferences.

## Synchronous/high-performance KV

Consider a focused solution such as MMKV only when measured startup/access latency, required synchronous reads, or another demonstrated performance constraint makes the asynchronous baseline insufficient.

Do not choose it only because a benchmark is faster.

## Offline synchronization

Do not choose WatermelonDB, PowerSync, or another sync/data framework before defining:

- offline data scope;
- source of truth;
- staleness rules;
- queued mutations;
- conflict policy;
- retry semantics;
- user-visible sync state.

Design the offline model first, then choose tooling.

---

# 10. Forms and Validation

Use native inputs and local state for small forms.

Add a form/validation library when:

- many fields share validation/orchestration;
- nested/dynamic fields are hard to manage;
- reusable schemas are valuable;
- error mapping/submission logic repeats;
- measured form performance becomes a problem.

Do not add one merely because the app contains forms.

---

# 11. Storybook

Do not require Storybook for the first vertical slice.

Add it when shared components have multiple meaningful states, isolated visual iteration becomes valuable, agents repeatedly modify common components, or a real design system is emerging.

Stories should represent meaningful product states, not every trivial wrapper.

---

# 12. Component Testing

Do not require component tests for every component.

Use React Native Testing Library or another appropriate current tool when interaction/state behavior, accessibility behavior, or a shared component is valuable to protect without a full device flow.

Prefer user-observable behavior over implementation details.

---

# 13. Maestro

Manual verification is acceptable for the earliest prototype.

Add Maestro when a user journey becomes critical, regression risk is meaningful, repeated cross-change verification is needed, or CI should protect the core flow.

Prioritize a small number of high-value deterministic journeys.

Reference: https://maestro.mobile.dev/

---

# 14. agent-device

Use after runnable UI exists.

Use agent-device for exploratory interaction, accessibility-tree inspection, screenshots, visual verification, agent-driven navigation, and finding unexpected states.

Do not use it as a replacement for deterministic E2E regression tests.

```text
Maestro = known deterministic critical paths
agent-device = exploratory and visual verification
```

Reference: https://github.com/callstack/agent-device

---

# 15. Deeper Device Tooling

Start with normal simulator/device debugging and agent-device.

Consider tools such as Argent when black-box interaction is insufficient and you need component-tree inspection, network introspection, Hermes profiling, CPU/render investigation, or deeper platform diagnostics.

Reference: https://github.com/software-mansion/argent

---

# 16. Skia and SVG

## SVG

Use normal views/text/images where appropriate. Add `react-native-svg` when product-specific vector graphics, lightweight charts/dials, or scalable diagrams/icons are required.

SVG is often the middle ground before a custom graphics engine.

## Skia

Do not install Skia for ordinary UI.

Consider it when graphics-heavy rendering, drawing, advanced visualization/effects, or measured rendering requirements exceed normal views/SVG.

Do not use it to draw ordinary buttons/cards.

---

# 17. Images

Use React Native image primitives for simple local or straightforward image needs.

Consider `expo-image` when remote caching, placeholders, transitions, image-heavy performance, or richer loading behavior materially helps the product.

---

# 18. Large Lists

## Default

Start with `FlatList` / `SectionList`.

Before replacing them, profile the actual problem: cell cost, rerenders, keys, images, layout work, state propagation, and frame behavior.

## Consider a specialized list implementation

If measured list/render problems remain after correcting obvious implementation issues, evaluate a focused alternative such as **FlashList** against the actual workload.

Do not migrate to FlashList or another list library because the list has an arbitrary number of rows. Item count alone is not a performance diagnosis.

---

# 19. Custom Native Modules

Stay inside Expo and mature React Native libraries when they solve the requirement well.

Build app-specific native code only when a required platform/hardware/vendor capability is unavailable or measured performance/latency cannot be met otherwise.

Document build implications, platform ownership, testing, and long-term cost.

Prefer Expo Modules API for app-specific native modules when appropriate.

---

# 20. CNG / Prebuild and Native Project Ownership

Treat Continuous Native Generation as the normal model unless the project explicitly maintains native projects itself.

## CNG-owned project

- `ios/` and `android/` are generated output;
- persistent native configuration belongs in app config/config plugins/modules;
- `expo prebuild --clean` may delete manual edits;
- temporary native debugging changes should be migrated back into reproducible configuration.

## Explicitly owned native projects

Direct Xcode/Gradle/native-source edits are valid. Do not run Prebuild in a way that unintentionally overwrites them.

Determine ownership before editing native files.

References:

- https://docs.expo.dev/workflow/continuous-native-generation/
- https://docs.expo.dev/config-plugins/introduction/

---

# 21. UI Component Libraries

Do not use a UI library automatically.

Consider one when much of the application is conventional forms/settings/content UI, its accessibility/platform behavior is good enough, the product intentionally aligns with it, and the team accepts migration/dependency cost.

Avoid one when domain-specific interactions dominate or heavy overrides would immediately be required.

---

# 22. Figma MCP

Not required to begin implementation.

Use when a maintained Figma source exists and structured components/variables/design-system context materially improve implementation accuracy.

Figma is design input, not executable truth. Inspect the running application.

---

# 23. Analytics and Observability

Do not install a full observability stack before there is production behavior to observe.

Add crash reporting when external users rely on the app and crashes need remote diagnosis.

Add product analytics when there are explicit product questions to answer.

Add performance tracing when a user-visible performance problem requires measurement.

---

# 24. Offline Persistence and Sync

Online-only behavior is acceptable unless the product requires offline use.

Add offline infrastructure only after defining data scope, staleness, queued mutations, conflicts, retries, and visible sync state.

A network error screen is not an offline architecture.

---

# 25. Expo Go vs Development Build

Expo Go is acceptable for early experiments that fit its bundled native capabilities.

Move to a development build when native libraries outside Expo Go, config plugins, app-specific permissions/entitlements, custom native modules, vendor SDKs, or production-like native behavior matter.

Do not use Expo Go as proof once a feature depends on development-build native behavior.

---

# 26. Safe Areas, Edge-to-Edge, and Keyboard

Treat system insets and edge-to-edge layout as normal layout inputs.

Use `react-native-safe-area-context` when application content owns an inset. Do not blindly wrap every route or double-apply navigator-managed insets.

For keyboard behavior, start with React Native mechanisms such as `KeyboardAvoidingView` when sufficient.

A text-entry flow is not verified until it has been exercised with the keyboard open on targeted platforms.

Consider `react-native-keyboard-controller` only when complex scrollable forms, composer/chat interactions, synchronized keyboard animation, or repeated keyboard bugs demonstrate the need.

References:

- https://docs.expo.dev/versions/latest/sdk/safe-area-context/
- https://docs.expo.dev/guides/keyboard-handling/

---

# 27. EAS Build / Update / Submit

Do not make EAS an application-architecture requirement.

Use it when cloud builds, OTA policy, store submission automation, or Expo-integrated CI/CD materially reduce operational work.

Local native builds and other CI systems remain valid.

---

# 28. Decision Template

For a significant new dependency/framework, write a short note:

```markdown
## Decision: <tool or approach>

### Problem
What concrete problem exists now?

### Existing options
Why are React Native, Expo, or the current local abstraction insufficient?

### Decision
What are we adding or changing?

### Why this option
Why is it the smallest appropriate solution?

### Costs
What build, runtime, bundle, maintenance, migration, or learning cost does it introduce?

### Exit path
How difficult would it be to remove or replace later?
```

The note may live in an ADR, issue, or pull request. The purpose is clarity, not bureaucracy.

---

# 29. Final Rule

```text
platform primitive
    ↓
Expo capability
    ↓
small local abstraction
    ↓
focused dependency
    ↓
larger framework
    ↓
custom native solution
```

At every step ask whether the current level already solves the product problem adequately.

If it does, stop.

Complexity must be earned.