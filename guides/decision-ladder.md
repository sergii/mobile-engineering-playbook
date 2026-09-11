# Decision Ladder

This guide defines when to introduce common React Native / Expo tools.

The default rule is simple:

```text
Do not add a tool because it is available.
Add it when the application has a concrete problem that the tool solves better than the existing stack.
```

Use this document together with `MOBILE_ENGINEERING_PLAYBOOK.md`.

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

At that point compare focused options such as Unistyles, NativeWind/Uniwind, or another current solution against the actual requirement.

Do not migrate merely because a styling framework has cleaner syntax.

---

# 2. Expo Router

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

# 3. React Native Gesture Handler

## Default

Start with built-in interaction primitives:

```text
Pressable
ScrollView
FlatList
PanResponder where truly simple
```

## Add Gesture Handler when

- gestures must coordinate with scroll views;
- multiple gestures interact;
- swipe, pan, pinch, or long-press behavior becomes non-trivial;
- gesture cancellation and simultaneous recognition matter;
- custom interactions need production-grade gesture semantics.

Do not add it just because the application has buttons or simple taps.

---

# 4. Reanimated

## Default

Use simple built-in state changes and platform transitions where sufficient.

## Add Reanimated when

- animation is linked continuously to a gesture;
- high-frequency animation must stay smooth under JS load;
- shared values simplify complex interaction state;
- advanced transitions or coordinated motion are part of the product;
- animation work clearly benefits from UI-thread execution.

## Do not add Reanimated for

- a basic pressed opacity;
- trivial show/hide behavior;
- simple static state changes;
- animation that the platform already handles well.

Motion must communicate something meaningful.

---

# 5. `@expo/ui`

## Default

Use React Native controls and primitives first.

## Consider `@expo/ui` when

- native SwiftUI/Compose behavior creates a real product advantage;
- a platform control is difficult to reproduce correctly;
- platform-native menus, pickers, sheets, or settings interactions are important;
- using the native control improves accessibility or platform fidelity.

## Do not use it as

- a mandatory universal component library;
- a reason to make iOS and Android product behavior diverge unnecessarily;
- a replacement for domain-specific React Native components.

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

- the app has more than a few screens;
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

# 8. Storybook

## Default

Do not require Storybook for the first vertical slice.

## Add Storybook when

- shared components have multiple important states;
- components are used on several screens;
- visual iteration inside full flows is slow;
- agents repeatedly modify common UI primitives;
- a real design system is emerging;
- isolated component review becomes useful.

Good stories represent product states:

```text
idle
detecting
success
uncertain
error
disabled
loading
```

Do not build stories for every trivial wrapper.

---

# 9. Maestro

## Default

Manual verification is acceptable for the earliest prototype.

## Add Maestro when

- a user journey becomes critical;
- regression risk becomes meaningful;
- the same flow must work repeatedly across changes;
- CI should verify core application behavior.

Prioritize flows such as:

```text
launch
sign in
perform primary task
confirm result
```

Do not attempt to automate every screen immediately.

Maestro is for deterministic user journeys, not subjective visual quality.

---

# 10. agent-device

## Default

Use after a runnable UI exists.

## Use agent-device for

- exploratory interaction;
- visual verification;
- checking accessibility-tree output;
- agent-driven navigation through a flow;
- screenshots;
- checking whether generated UI actually behaves as intended;
- finding unexpected states that deterministic scripts do not cover.

## Do not use agent-device as

- a replacement for deterministic E2E tests;
- the first step before a screen can even run;
- proof that a critical workflow is regression-safe.

Recommended relationship:

```text
Maestro = known deterministic critical paths
agent-device = exploratory and visual verification
```

---

# 11. Argent or deeper device tooling

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

---

# 12. React Native Skia

## Default

Do not install Skia for ordinary UI.

## Consider Skia when

- rendering is genuinely graphics-heavy;
- Canvas-style drawing is core to the feature;
- complex custom visualization needs high-performance rendering;
- effects or animations are difficult or inefficient with normal views/SVG;
- measurement shows normal primitives are insufficient.

Examples may include:

- advanced charts;
- freehand drawing;
- image-processing UI;
- custom visualization;
- specialized camera overlays.

Do not use Skia to draw ordinary buttons, cards, or static icons.

---

# 13. SVG

## Default

Use normal views/text/images where appropriate.

## Add `react-native-svg` when

- product-specific vector graphics are needed;
- custom charts or dials are simple enough for SVG;
- scalable vector icons or diagrams are required.

SVG is often the right middle ground before Skia.

---

# 14. Custom Native Modules

## Default

Stay inside Expo and mature React Native libraries.

## Build custom native code only when

- the required platform capability is unavailable;
- performance or latency requirements cannot be met otherwise;
- a hardware/device integration requires native APIs;
- a vendor SDK must be integrated;
- a focused native implementation is clearly smaller than a workaround.

Before committing to native code, document:

- why Expo is insufficient;
- why existing packages are insufficient;
- supported platforms;
- build implications;
- testing strategy;
- long-term ownership cost.

Prefer Expo Modules API for custom native modules when appropriate.

---

# 15. UI component libraries

Examples include HeroUI Native, Gluestack, Tamagui, and similar systems.

## Default

Do not use one automatically.

## Consider a UI library when

- the application is primarily conventional forms/settings/CRUD UI;
- delivery speed matters more than a strongly custom interaction language;
- its accessibility and platform behavior are good enough;
- the design intentionally aligns with the library;
- the team accepts the dependency and migration cost.

## Avoid when

- the product is dominated by domain-specific operational interactions;
- the library would dictate the visual language;
- many components would immediately need heavy overrides;
- the application would become a wrapper around library conventions.

A UI library should accelerate the product, not reshape it.

---

# 16. Figma MCP

## Default

Not required to begin implementation.

## Use when

- a maintained Figma design exists;
- structured variables/components improve implementation accuracy;
- design-system context needs to flow into agent sessions;
- comparing design intent with implementation is valuable.

Do not treat Figma MCP as a code generator that removes the need for engineering judgment.

Always inspect the running application.

---

# 17. Analytics and Observability

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

# 18. Offline persistence

## Default

Online-only behavior is acceptable unless the product requires offline use.

## Add persistence/sync infrastructure only after defining

- which data must exist offline;
- staleness rules;
- queued mutations;
- conflict policy;
- retry behavior;
- user-visible sync state.

Do not install a database merely because mobile applications sometimes work offline.

---

# 19. Decision template

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

# 20. Final Rule

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
