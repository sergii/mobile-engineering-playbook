# Decision Ladder

This guide defines when to introduce common React Native / Expo tools and abstractions.

The default rule is simple:

```text
Do not add a tool because it is available.
Add it when the application has a concrete problem that the tool solves better than the existing stack.
```

Use this document together with `MOBILE_ENGINEERING_PLAYBOOK.md`.

A package being present in the dependency tree does not mean the project has adopted that package's API. Apply this ladder before using a transitive dependency directly in application code.

---

# 1. Runtime: Expo Go vs development build

## Default

Expo Go is acceptable for early experiments that fit entirely inside its bundled native capabilities.

## Move to a development build when

- a native library is not bundled in Expo Go;
- config plugins matter;
- app-specific permissions or entitlements matter;
- custom native modules are introduced;
- native SDKs are integrated;
- production-like native behavior must be verified.

Once the application depends on a development build, verify native features there rather than treating Expo Go as the source of truth.

Development builds may be local or EAS-built. EAS is not mandatory.

Reference: https://docs.expo.dev/develop/development-builds/introduction/

---

# 2. Styling

## Default

Use:

```text
React Native StyleSheet
small token files
local component styles
```

## Stay with StyleSheet when

- the application has one primary theme;
- layouts are mostly phone-oriented;
- token access through imports is understandable;
- platform differences are small;
- styles remain local to components;
- there is no meaningful runtime theme complexity.

## Consider a styling layer when

A concrete problem appears, such as:

- many runtime themes;
- substantial tablet/phone responsive rules;
- complicated dynamic theme propagation;
- large-scale semantic token switching;
- repeated media-query-like behavior;
- difficult platform-specific style branching.

At that point compare focused current options against the actual requirement.

Do not migrate merely because another syntax is fashionable or shorter.

---

# 3. Expo Router

## Default

Use Expo Router for real application navigation.

## A tiny prototype may avoid it when

- there is one screen;
- navigation is temporary;
- the purpose is to validate one isolated control or experiment.

## Introduce Expo Router once

- multiple routes exist;
- tabs or stacks become product behavior;
- deep links matter;
- notification destinations matter;
- modal navigation matters;
- back behavior should be platform-correct.

Do not build a long-lived custom router from React state.

---

# 4. React Native Gesture Handler

## Default

Start with built-in interaction primitives:

```text
Pressable
ScrollView
FlatList
PanResponder when truly simple
```

## Add Gesture Handler when

- gestures must coordinate with scroll views;
- multiple gestures interact;
- swipe, pan, pinch, or long-press behavior becomes non-trivial;
- gesture cancellation and simultaneous recognition matter;
- custom interactions need production-grade gesture semantics.

Do not add or start using it merely because another dependency brought it into the tree.

---

# 5. Reanimated

## Default

Use simple built-in state changes and platform transitions where sufficient.

## Add Reanimated when

- animation is continuously linked to a gesture;
- high-frequency animation must remain smooth under JS load;
- shared values simplify complex interaction state;
- advanced transitions or coordinated motion are part of the product;
- UI-thread execution provides a real benefit.

## Do not add Reanimated for

- a basic pressed opacity;
- trivial show/hide behavior;
- static state changes;
- animation the platform already handles well.

Motion must communicate something meaningful.

---

# 6. `@expo/ui`

## Default

Use React Native controls and primitives first.

## Consider `@expo/ui` when

- native SwiftUI/Compose behavior creates a real product advantage;
- a platform control is difficult to reproduce correctly;
- platform-native menus, pickers, sheets, or settings interactions are important;
- using the native control improves accessibility or platform fidelity.

## Do not use it as

- a mandatory universal component library;
- a reason to make platforms diverge unnecessarily;
- a replacement for domain-specific React Native components.

Use selectively.

---

# 7. External State Management

## Default

Start with:

```text
useState
useReducer
Context
```

## Consider an external state library when

- many distant components mutate the same persistent application state;
- state ownership is no longer clear;
- Context creates excessive coupling or update churn;
- state transitions deserve explicit centralized modeling;
- debugging shared application state has become difficult.

Do not add external state merely because the app has many screens.

---

# 8. Server-State Library

## Default

Use straightforward service calls for simple request/response workflows.

## Add TanStack Query or another server-state library when the product needs several of

- caching;
- stale-time policies;
- background refresh;
- request deduplication;
- retries;
- pagination;
- optimistic mutations;
- invalidation after mutation;
- multiple consumers of the same remote data.

Do not add it merely because the app has an API.

---

# 9. Forms and Validation

## Default

Use native inputs, local state, and focused validation for small forms.

## Add a form/validation library when

- many fields require coordinated state;
- nested/repeating fields exist;
- validation schemas are reused;
- form performance or orchestration is becoming difficult;
- server/client validation mapping creates repeated complexity.

Do not choose a form stack globally just because some screens contain forms.

---

# 10. Storybook

## Default

Do not require Storybook for the first vertical slice.

## Add Storybook when

- shared components have multiple important states;
- components are used on several screens;
- visual iteration inside full flows is slow;
- agents repeatedly modify common components;
- a real design system is emerging;
- isolated component review becomes useful.

Good stories represent product states, not decorative permutations.

---

# 11. Component Tests

## Default

Do not test every wrapper.

## Add React Native Testing Library or an equivalent project-adopted tool when

- component interaction has meaningful branching;
- accessibility behavior should be protected;
- loading/error/selection states are valuable to verify without full E2E execution;
- regression risk exists below the full-flow level.

Prefer user-observable assertions over implementation details.

---

# 12. Maestro

## Default

Manual verification is acceptable for the earliest prototype.

## Add Maestro when

- a user journey becomes critical;
- regression risk becomes meaningful;
- the same flow must work repeatedly across changes;
- CI should verify core application behavior.

Prioritize critical paths rather than every screen.

Maestro is for deterministic user journeys, not subjective visual quality.

Reference: https://docs.maestro.dev/

---

# 13. agent-device

## Default

Use after a runnable UI exists.

## Use agent-device for

- exploratory interaction;
- visual verification;
- accessibility-tree inspection;
- agent-driven navigation;
- screenshots and evidence;
- checking whether generated UI behaves as intended;
- finding unexpected states outside deterministic scripts.

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
- native platform profiling becomes relevant;
- visual regression/replay materially helps debugging;
- black-box interaction is insufficient.

Use richer tooling in response to a debugging need, not as mandatory infrastructure.

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

Use normal views/text/images where appropriate.

## Add `react-native-svg` when

- product-specific vector graphics are needed;
- custom charts or dials are simple enough for SVG;
- scalable vector icons or diagrams are required.

SVG is often the right middle ground before Skia.

---

# 17. Images

## Default

Use the simplest image primitive that satisfies the feature.

## Consider `expo-image` when

- remote-image caching matters;
- placeholders or transitions matter;
- loading large/remote image sets is user-visible;
- richer image behavior clearly improves the product.

Do not add image infrastructure for a few static local assets.

---

# 18. Large Lists

## Default

Start with React Native list primitives.

## Evaluate alternatives when

- profiling shows list rendering is a user-visible bottleneck;
- data volume and cell complexity exceed the current approach;
- memory or frame performance is measurably poor.

Measure first. Do not migrate because a list is expected to become large someday.

---

# 19. Custom Native Modules

## Default

Stay inside Expo and mature React Native libraries.

## Build custom native code only when

- the required platform capability is unavailable;
- performance or latency requirements cannot be met otherwise;
- hardware/device integration requires native APIs;
- a vendor SDK must be integrated;
- a focused native implementation is clearly smaller than a workaround.

Prefer Expo Modules API for application-specific native modules unless another approach has a documented advantage.

Reference: https://docs.expo.dev/modules/overview/

---

# 20. UI Component Libraries

Examples include HeroUI Native, Gluestack, Tamagui, and similar systems.

## Default

Do not use one automatically.

## Consider a UI library when

- the application is primarily conventional forms/settings/CRUD UI;
- delivery speed matters more than a strongly custom interaction language;
- its accessibility and platform behavior are good enough;
- the design intentionally aligns with the library;
- the team accepts dependency and migration cost.

## Avoid when

- the product is dominated by domain-specific operational interactions;
- the library would dictate the visual language;
- many components immediately require heavy overrides.

A UI library should accelerate the product, not reshape it.

---

# 21. Figma MCP

## Default

Not required to begin implementation.

## Use when

- a maintained Figma design exists;
- structured variables/components improve implementation accuracy;
- design-system context should flow into agent sessions;
- comparing design intent with implementation is valuable.

Do not treat Figma MCP as a compiler.

Always inspect the running application.

---

# 22. Analytics and Observability

## Default

Do not install a full analytics or observability stack before there is production behavior to observe.

Add crash reporting, product analytics, or performance tracing when each answers a concrete operational or product question.

Do not collect telemetry without a purpose.

---

# 23. Offline Persistence

## Default

Online-only behavior is acceptable unless the product requires offline use.

## Add persistence/sync infrastructure only after defining

- which data must exist offline;
- staleness rules;
- queued mutations;
- conflict policy;
- retry behavior;
- user-visible sync state.

A network error screen is not an offline architecture.

---

# 24. EAS Build / Update / Submit / Workflows

## Default

Do not make cloud delivery infrastructure part of the first walking skeleton unless the project needs it immediately.

## Introduce EAS delivery tooling when

- repeatable signed builds are needed;
- testers need internal distributions;
- store submission should be automated;
- OTA update policy is defined and appropriate;
- CI needs Expo-integrated mobile jobs.

Local native builds remain valid. EAS is a strong Expo-integrated option, not an architectural requirement.

---

# 25. Decision Template

For any significant new dependency or framework, write a short note:

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

# 26. Final Rule

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
