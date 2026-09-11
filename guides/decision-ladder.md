# Decision Ladder

This guide defines when to introduce common React Native / Expo tools.

The default rule is simple:

```text
Do not add a tool because it is available.
Add it when the application has a concrete problem that the tool solves better than the existing stack.
```

Use this document together with `MOBILE_ENGINEERING_PLAYBOOK.md`.

Archetypes may indicate that a class of product is more likely to need a tool earlier, but they do not override this ladder.

---

# 1. Styling

## Default

Use:

```text
React Native StyleSheet
small token files
local component styles
```

## Stay with StyleSheet when

- the application has manageable theme needs;
- token access through imports is understandable;
- platform differences are limited;
- styles remain local to components;
- there is no meaningful runtime theme complexity.

## Consider a styling layer when

A concrete problem appears, such as:

- many runtime themes;
- substantial phone/tablet responsive rules;
- complicated dynamic theme propagation;
- large-scale semantic token switching;
- repeated media-query-like behavior;
- difficult platform-specific style branching.

At that point compare focused current options against the actual requirement.

Do not migrate merely because a styling framework has cleaner syntax.

---

# 2. Expo Router

## Default

Use Expo Router for real application navigation.

## A tiny prototype may avoid it when

- there is one screen;
- navigation is temporary;
- the purpose is to validate one isolated interaction or experiment.

## Use Expo Router once

- multiple routes exist;
- tabs or stacks become product behavior;
- deep links matter;
- notification destinations matter;
- modal navigation matters;
- back behavior should be platform-correct.

Do not build a long-lived custom router from React state.

## Authentication and authorization

For current Expo Router applications, prefer Protected Routes for route-level authentication and authorization.

Use route groups to organize areas of the application, but let route guards express access rules declaratively.

Do not default to `useEffect` + `router.replace()` as the primary auth-guard mechanism when Protected Routes express the same rule directly.

Imperative redirects remain valid for explicit transitions that are not access guards.

Reference: https://docs.expo.dev/router/advanced/authentication/

## Error boundaries

Expected product states such as validation failures, payment declines, offline state, and empty results belong in normal product UI.

Use Expo Router route/layout Error Boundaries for unexpected render/runtime failures and recovery scopes.

Reference: https://docs.expo.dev/router/error-handling/

---

# 3. React Native Gesture Handler

## Default

Start with built-in interaction primitives:

```text
Pressable
ScrollView
FlatList
PanResponder when genuinely simple
```

## Add Gesture Handler when

- gestures must coordinate with scroll views;
- multiple gestures interact;
- swipe, pan, pinch, or long-press behavior becomes non-trivial;
- gesture cancellation and simultaneous recognition matter;
- custom interactions need production-grade gesture semantics.

Do not add it just because the application has buttons or simple taps.

A transitive installation does not mean application code has adopted the API.

---

# 4. Reanimated

## Default

Use simple built-in state changes and platform transitions where sufficient.

## Add Reanimated when

- animation is linked continuously to a gesture;
- high-frequency animation must remain smooth under JS load;
- shared values simplify complex interaction state;
- advanced transitions or coordinated motion are part of the product;
- animation work clearly benefits from UI-thread execution.

## Do not add Reanimated for

- a basic pressed opacity;
- trivial show/hide behavior;
- simple static state changes;
- animation the platform already handles well.

Motion must communicate something meaningful.

---

# 5. `@expo/ui`

## Default

Use React Native controls and primitives first.

## Consider `@expo/ui` when

- native SwiftUI/Compose behavior creates a real product advantage;
- a platform control is difficult to reproduce correctly;
- native menus, pickers, sheets, or settings interactions are important;
- using the native control improves accessibility or platform fidelity.

## Do not use it as

- a mandatory universal component library;
- a reason to make platform behavior diverge unnecessarily;
- a replacement for product-specific React Native components.

Use selectively.

---

# 6. External State Management

## Default

Start with:

```text
useState
useReducer
Context
```

## Consider Zustand, Redux, Jotai, or another library when

- many distant components mutate the same persistent application state;
- state ownership is no longer clear;
- Context creates excessive coupling or update churn;
- state transitions deserve explicit centralized modeling;
- debugging shared application state has become difficult.

## Do not add external state because

- the app has many screens;
- a tutorial includes it;
- the API returns data;
- it might be needed later.

---

# 7. TanStack Query or another server-state library

## Default

Use straightforward service calls for simple request/response workflows.

## Add a server-state library when the product needs several of

- caching;
- stale-time policies;
- background refresh;
- request deduplication;
- retries;
- pagination;
- optimistic mutations;
- invalidation after mutation;
- multiple consumers of the same remote data.

## Do not add it merely because

- the app has a REST or GraphQL API;
- one screen fetches data;
- it is considered standard infrastructure.

---

# 8. Local Storage and Persistence

Persistence is not one problem. Choose a storage mechanism based on sensitivity, data shape, access pattern, and offline requirements.

## Sensitive key-value data

Examples:

```text
authentication token
refresh token
small credential material
sensitive session secret
```

Prefer `expo-secure-store` when the data is small, sensitive, and belongs in encrypted platform-backed key-value storage.

Do not use SecureStore as the only source of truth for large or irreplaceable application data.

Reference: https://docs.expo.dev/versions/latest/sdk/securestore/

## Small non-sensitive key-value data

Examples:

```text
hasSeenOnboarding
selectedTheme
lastUsedFilter
small user preference
```

Default to `@react-native-async-storage/async-storage` when simple asynchronous persistent key-value storage is sufficient.

It is unencrypted. Do not store secrets there.

Reference: https://docs.expo.dev/versions/latest/sdk/async-storage/

## When SQLite is already adopted

If the application already uses `expo-sqlite`, consider `expo-sqlite/kv-store` for simple key-value persistence instead of adding another storage dependency.

Its API is compatible with AsyncStorage-style usage and also exposes synchronous methods.

Reference: https://docs.expo.dev/versions/latest/sdk/sqlite/

## Structured/queryable local data

Consider `expo-sqlite` when the product needs persistent structured data, queries, transactions, or a real local data model.

Do not introduce a database for a handful of preferences.

## Synchronous/high-performance key-value access

Consider a focused solution such as MMKV only when measured startup/access latency, synchronous reads, or a demonstrated performance constraint makes the asynchronous baseline insufficient.

Do not choose it merely because it benchmarks faster.

## Offline synchronization

Do not choose WatermelonDB, PowerSync, or another sync/data framework before defining:

- which data exists offline;
- ownership/source of truth;
- staleness policy;
- queued mutations;
- conflict resolution;
- retry semantics;
- user-visible sync state.

Design the offline model first, then choose the smallest persistence/sync tool that implements it.

---

# 9. Forms and Validation

## Default

Use native inputs and local state for small forms.

## Add a form/validation library when

- many fields share repeated validation behavior;
- nested or dynamic fields become difficult to manage;
- reusable schemas are valuable;
- error mapping and submission orchestration create repeated complexity;
- measured form performance becomes a problem.

Do not add a form framework simply because a product contains forms.

---

# 10. Storybook

## Default

Do not require Storybook for the first vertical slice.

## Add Storybook when

- shared components have multiple important states;
- components are used on several screens;
- visual iteration inside full flows is slow;
- agents repeatedly modify common UI primitives;
- a real design system is emerging;
- isolated component review becomes useful.

Good stories represent meaningful states such as:

```text
default
loading
success
warning
error
disabled
empty
```

Do not build stories for every trivial wrapper.

---

# 11. React Native Testing Library

## Default

Do not require component tests for every component.

## Add component tests when

- interaction behavior is meaningful to protect;
- state transitions can be verified without a device flow;
- accessibility behavior matters;
- a shared component is becoming regression-prone.

Prefer user-observable behavior over internal implementation details.

---

# 12. Maestro

## Default

Manual verification is acceptable for the earliest prototype.

## Add Maestro when

- a user journey becomes critical;
- regression risk becomes meaningful;
- the same flow must work repeatedly across changes;
- CI should verify core application behavior.

Prioritize a small number of high-value journeys.

Maestro is for deterministic user journeys, not subjective visual quality.

Reference: https://maestro.mobile.dev/

---

# 13. agent-device

## Default

Use after a runnable UI exists.

## Use agent-device for

- exploratory interaction;
- visual verification;
- checking accessibility-tree output;
- agent-driven navigation through a flow;
- screenshots;
- checking whether generated UI actually behaves as intended;
- finding unexpected states deterministic scripts do not cover.

## Do not use agent-device as

- a replacement for deterministic E2E tests;
- the first step before a screen can run;
- proof that a critical workflow is regression-safe.

Recommended relationship:

```text
Maestro = known deterministic critical paths
agent-device = exploratory and visual verification
```

Reference: https://github.com/callstack/agent-device

---

# 14. Argent or deeper device tooling

## Default

Start with normal simulator/device debugging and agent-device.

## Introduce deeper tooling when

- React component-tree inspection is needed;
- network introspection is important;
- Hermes profiling is needed;
- CPU/render performance requires investigation;
- Xcode Instruments or Android Perfetto workflows become relevant;
- black-box interaction is insufficient to diagnose a problem.

Use richer tools in response to a debugging need, not as mandatory project infrastructure.

Reference: https://github.com/software-mansion/argent

---

# 15. React Native Skia

## Default

Do not install Skia for ordinary UI.

## Consider Skia when

- rendering is genuinely graphics-heavy;
- Canvas-style drawing is core to the feature;
- complex custom visualization needs high-performance rendering;
- effects or animations are difficult or inefficient with normal views/SVG;
- measurement shows normal primitives are insufficient.

Do not use Skia to draw ordinary buttons, cards, or static icons.

---

# 16. SVG

## Default

Use normal views, text, and images where appropriate.

## Add `react-native-svg` when

- product-specific vector graphics are needed;
- custom charts or dials are simple enough for SVG;
- scalable vector icons or diagrams are required.

SVG is often the right middle ground before Skia.

---

# 17. Images

## Default

Use React Native image primitives for simple local or straightforward image needs.

## Consider `expo-image` when

- remote image caching matters;
- placeholders or transitions improve UX;
- image loading/performance becomes meaningful;
- the product is image-heavy.

Do not adopt a richer image pipeline without a product reason.

---

# 18. Large Lists

## Default

Start with `FlatList` / `SectionList`.

## Consider a specialized list implementation when

- measurement shows render/frame problems;
- cells are unusually expensive;
- the list is central to the product and current primitives cannot meet performance requirements.

Do not replace lists based only on a guessed item-count threshold.

---

# 19. Custom Native Modules

## Default

Stay inside Expo and mature React Native libraries.

## Build custom native code only when

- the required platform capability is unavailable;
- performance or latency requirements cannot be met otherwise;
- a hardware/device integration requires native APIs;
- a vendor SDK must be integrated;
- a focused native implementation is clearly smaller than a workaround.

Before committing to native code, document why Expo and existing packages are insufficient, supported platforms, build implications, testing strategy, and long-term ownership cost.

Prefer Expo Modules API for application-specific native modules when appropriate.

---

# 20. CNG / Prebuild and Native Project Ownership

## Default for new Expo projects

Treat Continuous Native Generation as the normal ownership model unless the project explicitly chooses to maintain native projects manually.

## If CNG owns the native projects

- treat `ios/` and `android/` as generated output;
- express persistent native configuration through app config and config plugins;
- assume `expo prebuild --clean` can delete manual native edits;
- migrate successful temporary native debugging changes back into reproducible configuration.

## If the repository explicitly owns native projects

Direct Xcode/Gradle/native-source edits are valid.

Do not run Prebuild in a way that unintentionally overwrites manual native customizations.

Before editing native files, determine the ownership model first.

References:

- https://docs.expo.dev/workflow/continuous-native-generation/
- https://docs.expo.dev/config-plugins/introduction/

---

# 21. UI Component Libraries

Examples include HeroUI Native, Gluestack, Tamagui, and similar systems.

## Default

Do not use one automatically.

## Consider a UI library when

- much of the application is conventional forms/settings/content UI;
- delivery speed matters more than a strongly custom interaction language;
- its accessibility and platform behavior are good enough;
- the design intentionally aligns with the library;
- the team accepts the dependency and migration cost.

## Avoid when

- the product is dominated by custom domain interactions;
- the library would dictate the visual language;
- many components would immediately need heavy overrides;
- the application would become a wrapper around library conventions.

A UI library should accelerate the product, not reshape it.

---

# 22. Figma MCP

## Default

Not required to begin implementation.

## Use when

- a maintained Figma design exists;
- structured variables/components improve implementation accuracy;
- design-system context needs to flow into agent sessions;
- comparing design intent with implementation is valuable.

Do not treat Figma MCP as a compiler. Always inspect the running application.

---

# 23. Analytics and Observability

## Default

Do not install a full analytics or observability stack before there is production behavior to observe.

## Add crash reporting when

- external users rely on the app;
- crashes must be diagnosable outside development.

## Add product analytics when

- there are explicit product questions to answer;
- events can influence decisions.

## Add performance tracing when

- performance is a user-visible concern;
- measurement is needed to locate bottlenecks.

Do not collect telemetry without a purpose.

---

# 24. Offline Persistence and Sync

## Default

Online-only behavior is acceptable unless the product requires offline use.

## Add offline persistence/sync infrastructure only after defining

- which data must exist offline;
- staleness rules;
- queued mutations;
- conflict policy;
- retry behavior;
- user-visible sync state.

Do not install a database merely because mobile applications sometimes work offline.

Use the Local Storage and Persistence section above to distinguish simple persistence from an actual offline architecture.

---

# 25. Expo Go vs Development Build

## Default

Expo Go is acceptable for very early experiments that fit its bundled native capabilities.

## Move to a development build when

- a native library outside Expo Go is required;
- config plugins matter;
- app-specific native permissions or entitlements matter;
- custom native modules or vendor SDKs are introduced;
- production-like native behavior must be verified.

Once native behavior depends on the development build, do not use Expo Go as proof that the feature works.

---

# 26. Safe Areas, Edge-to-Edge, and Keyboard Handling

## Default

Treat edge-to-edge layout and system insets as normal mobile layout inputs.

Use `react-native-safe-area-context` when application content must account for safe areas.

Do not blindly add `SafeAreaView` to every route. Determine which insets the navigator/layout already owns and avoid double padding.

## Keyboard

Start with React Native keyboard APIs and `KeyboardAvoidingView` when they are sufficient.

A text-entry flow is not verified until it has been exercised with the keyboard open on each targeted platform.

Consider `react-native-keyboard-controller` when complex scrollable forms, chat/composer interactions, synchronized keyboard animation, or repeated keyboard bugs create demonstrated friction.

Do not install advanced keyboard infrastructure preemptively.

References:

- https://docs.expo.dev/versions/latest/sdk/safe-area-context/
- https://docs.expo.dev/guides/keyboard-handling/

---

# 27. EAS Build / Update / Submit

## Default

Do not make EAS an application-architecture requirement.

## Use EAS when

- its cloud build flow reduces operational work;
- OTA update policy is appropriate for the product;
- store submission automation is valuable;
- Expo-integrated release tooling simplifies CI/CD.

Local native builds and other CI systems remain valid choices.

---

# 28. Decision Template

For any significant new dependency or framework, write a short note using this structure:

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
What build, runtime, maintenance, migration, or learning cost does it introduce?

### Exit path
How difficult would it be to remove or replace later?
```

The note may live in an ADR, issue, or pull request.

The purpose is clarity, not process overhead.

---

# 29. Final Rule

Use this escalation sequence:

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
