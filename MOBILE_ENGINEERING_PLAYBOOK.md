# Mobile Engineering Playbook

Version: 0.2

## Purpose

This playbook defines a reusable engineering baseline for modern mobile applications built with React Native and Expo.

It is intended for AI-assisted development and human contributors. It is deliberately principle-first and UI-library-neutral.

The goal is not to prescribe a visual style, component library, state library, or large application architecture before the product requires one.

The goal is to build applications that are:

- simple before they are sophisticated;
- functional before they are polished;
- native-feeling without unnecessary native code;
- domain-driven rather than framework-driven;
- easy for humans and agents to understand;
- testable and observable;
- able to evolve without early architectural lock-in.

The default strategy is:

```text
make it run
    ↓
make one real workflow work
    ↓
make it reliable
    ↓
make it understandable
    ↓
make it pleasant
    ↓
make it sophisticated only where needed
```

Complexity must be earned.

---

# 1. Core Principles

## 1.1 Start with the platform, not a framework collection

Default to:

```text
React Native
Expo
TypeScript
React Native primitives
StyleSheet
```

Do not add a library merely because it is popular or commonly included in starter projects.

Every abstraction must solve an identifiable problem.

"Modern React Native apps use it" is not a valid reason.

## 1.2 Expo-first

New applications should use the current stable Expo SDK unless there is a concrete documented reason not to.

Use the React Native and React versions supported by that Expo SDK.

Do not independently upgrade React Native underneath Expo without a documented compatibility reason.

Prefer Expo APIs when they adequately solve the product requirement.

The exact package list evolves. Verify current Expo documentation instead of relying on historical package lists.

## 1.3 React Native primitives first

Prefer existing React Native primitives before introducing a component framework.

Typical primitives include:

```text
View
Text
Pressable
ScrollView
FlatList
SectionList
Image
TextInput
Switch
Modal
ActivityIndicator
StyleSheet
```

A product does not need a generic UI kit in order to have excellent UI.

## 1.4 Product architecture before framework architecture

Libraries should adapt to the product, not the other way around.

Do not redesign the product's interaction model to fit a component library, state framework, or styling system without a compelling reason.

## 1.5 Prefer reversible decisions early

Early product development contains uncertainty.

Prefer choices that are easy to remove or replace:

- local state before global state;
- plain `StyleSheet` before a styling framework;
- local mock services before a large data layer;
- one real workflow before a universal architecture.

---

# 2. Modern Platform Baseline

## 2.1 New Architecture is the baseline

Current Expo SDKs run on React Native's New Architecture. Treat this as the normal platform, not an optional advanced mode.

For new projects:

- assume New Architecture;
- prefer libraries that explicitly support the current React Native and Expo versions;
- do not design new code around the legacy bridge;
- verify native dependencies against the current Expo compatibility guidance;
- prefer Expo Modules API for application-specific native modules.

Do not pin this playbook to one SDK number. Verify current behavior in Expo documentation.

Reference: https://docs.expo.dev/guides/new-architecture/

## 2.2 Expo Go vs development builds

Expo Go is useful for learning, experiments, and very early prototypes that fit inside its bundled native capabilities.

A development build is the normal runtime once the application depends on native configuration or native libraries that are specific to the product.

Move to a development build when any of the following becomes relevant:

- native libraries not bundled in Expo Go;
- config plugins;
- app-specific permissions or entitlements;
- custom native modules;
- native SDK integration;
- production-like runtime behavior.

Once a feature depends on a development build, verify that feature in the development build or a production-like build. Do not use Expo Go as proof that native behavior works.

Development builds can be built locally or through EAS. EAS is an integrated Expo delivery option, not a mandatory application architecture.

Reference: https://docs.expo.dev/develop/development-builds/introduction/

## 2.3 Native modules

Stay inside Expo and mature React Native libraries when they solve the requirement well.

When application-specific native code is justified, prefer Expo Modules API unless a different native integration model has a documented advantage.

Reference: https://docs.expo.dev/modules/overview/

---

# 3. Default Technical Foundation

The default starting stack is intentionally small:

```text
Expo
React Native
TypeScript
Expo Router
React Native primitives
StyleSheet
Expo platform APIs as needed
```

Do not automatically install:

```text
NativeWind
Unistyles
Tamagui
Gluestack
HeroUI
Redux
Zustand
Reanimated
Gesture Handler
TanStack Query
Storybook
Skia
```

These are useful tools, not foundations.

Introduce them when the application develops a problem they clearly solve.

---

# 4. TypeScript Baseline

Use TypeScript with strict checking in new projects unless a documented compatibility constraint prevents it.

Prefer explicit domain types over broad object shapes.

Avoid `any`.

If an unsafe external boundary forces an `any`-like value, isolate the unsafe boundary and convert the value into a validated application type as early as possible.

Do not add type assertions merely to silence a design, API, or state-model problem.

Do not weaken project-wide type safety to make generated code compile.

---

# 5. Dependency Policy

Dependencies are liabilities as well as capabilities.

Before adding a dependency, answer:

```text
What specific problem does this solve?

Can React Native already solve it?

Can Expo already solve it?

Can a small local abstraction solve it?

Is there already an adopted solution for this problem?

Is the problem present now or merely anticipated?

What maintenance, bundle, native-build, runtime, or upgrade cost does it introduce?
```

Do not add infrastructure for hypothetical future requirements.

Prefer a small dependency graph.

## 5.1 Installed does not mean adopted

A package may exist because another adopted tool depends on it.

The presence of a package in the dependency tree does not automatically mean application code should begin using its API.

Likewise, do not remove a package merely because application code does not import it directly. Verify whether Expo, Expo Router, a config plugin, or another dependency requires it.

Before adopting an already-present package in application code, apply the same decision rule as for a new dependency.

## 5.2 Record significant choices

When a dependency is cross-cutting or difficult to reverse, record the reason in a short ADR or pull request note.

See `guides/decision-ladder.md` for concrete escalation guidance.

---

# 6. Navigation

For real applications, prefer Expo Router.

Navigation is infrastructure and should not be reinvented once an application has multiple flows.

Use platform-native navigation concepts where possible:

```text
stack
tabs
modal
sheet
deep link
```

A tiny prototype may temporarily use local state to switch between one or two views.

Once navigation becomes product behavior, move to Expo Router.

Navigation should deliberately handle relevant behaviors such as:

- deep links and universal/app links;
- notification-driven destinations;
- modal flows;
- nested workflows;
- platform-correct back behavior;
- restoration where the product requires it.

Do not invent wrapper abstractions over Expo Router before real repetition demonstrates a need.

---

# 7. Styling

## 7.1 Start with StyleSheet

Start with React Native `StyleSheet` and local component styles.

Do not introduce a styling framework until plain `StyleSheet` creates a concrete limitation.

Possible future reasons include:

- complex runtime theming;
- multiple product themes;
- substantial phone/tablet responsive behavior;
- large-scale semantic token switching;
- repeated media-query-like behavior;
- difficult platform-specific style branching;
- design-system scale that makes plain imports hard to maintain.

When a problem appears, evaluate the smallest tool that solves it.

Do not pre-select Unistyles, NativeWind, Tamagui, or another styling framework globally.

## 7.2 Native code is not web code

Do not use DOM elements, browser CSS assumptions, or `className` by default in native application code.

A project may explicitly adopt a library that provides such syntax. That is an architectural choice and must be deliberate.

See `guides/agent-failure-modes.md`.

---

# 8. Design Tokens

Even with plain `StyleSheet`, repeated visual decisions should gradually become tokens.

Begin small:

```text
theme/
  colors.ts
  spacing.ts
  typography.ts
```

As the product matures, prefer semantic tokens over purely visual names.

Prefer:

```text
text.primary
text.secondary
surface.default
surface.elevated
border.subtle
action.primary
```

over raw names such as:

```text
green500
gray700
spacing16
```

For domain-heavy products, semantic tokens may become domain-specific:

```text
scanner.idle
scanner.detecting
scanner.success
scanner.uncertain
scanner.error
```

Do not attempt to design the complete token system before the product exists.

A token file is not useful if components continue to hard-code the same shared decisions inconsistently.

---

# 9. Domain-Driven UI

The user interface should reflect the domain.

Do not force every application into the same collection of generic cards, pills, sheets, dashboards, and forms.

Before designing a flow, ask:

```text
What is the main object the user is manipulating?
What event is taking place?
What information matters now?
What uncertainty exists?
What can go wrong?
What is the next likely action?
```

Create components around stable product concepts.

Examples across different domains:

```text
ScannerViewfinder
RecognitionConfidence
CheckoutPaymentSheet
InboxThread
RidePickupSelector
ThermostatDial
```

Prefer domain components over speculative wrappers such as `FancyCard`, `UniversalPanel`, or `MagicContainer` unless actual reuse demonstrates that a generic abstraction is valuable.

Domain-driven does not mean custom-build every control. Settings, forms, and conventional workflows should still use appropriate platform controls or focused libraries when those are the better fit.

---

# 10. Native Controls

Use existing native controls when they are good enough.

Examples include:

```text
Switch
TextInput
system dialogs
date/time controls
menus
navigation
```

Do not recreate platform controls merely for visual consistency.

`@expo/ui` may be introduced selectively when SwiftUI or Jetpack Compose controls provide a meaningful product advantage.

It should not automatically become the application's main component system.

A useful rule is:

```text
native control is already appropriate
→ use it

interaction is product-specific
→ build it from primitives
```

---

# 11. Build Vertical Slices

Development should proceed through vertical slices rather than horizontal infrastructure projects.

A vertical slice is a thin but complete path through the system that lets the user accomplish one meaningful goal.

Avoid starting with:

```text
complete API layer
complete design system
complete component library
complete state framework
complete analytics architecture
complete navigation abstraction
```

before a user can accomplish anything.

Instead build:

```text
one user goal
    ↓
one screen or flow
    ↓
real interaction
    ↓
state change
    ↓
visible result
```

Example:

```text
Open scanner
→ detect barcode
→ resolve item
→ display result
→ confirm item
```

Another example:

```text
Open checkout
→ choose payment method
→ confirm payment
→ show receipt
```

A good first slice may use local state, mocked data, one route, one service function, minimal styling, and minimal tests.

It should still exercise the real interaction shape of the product.

Once a slice works, replace mocks and local assumptions at the seams rather than rebuilding everything from scratch.

Use `guides/vertical-slice-checklist.md` as a compact completion checklist.

---

# 12. Development Phases

These phases are guidance, not bureaucracy. Small tasks may cross several quickly.

## Phase 0 - Walking Skeleton

Goal: the project runs in the correct runtime.

Requirements:

```text
Expo project exists
strict TypeScript works
application starts
routing works
one screen renders
iOS simulator runs when iOS is targeted
Android emulator runs when Android is targeted
```

Choose Expo Go only if the current native capability set permits it. Otherwise use a development build.

Do not optimize visual design at this stage.

## Phase 1 - First Vertical Slice

Implement one meaningful user workflow with real interaction rather than static mock screens.

Use local state and mocked data when necessary.

At the end of this phase, a user should be able to perform one real task.

## Phase 2 - Behavioral Correctness

Before increasing visual sophistication, make the slice reliable.

Check relevant states:

```text
main action
disabled
loading
empty
permission denied
failure
retry
back navigation
repeated interaction
```

Run TypeScript checks and relevant tests.

Run the application.

Do not rely solely on static code inspection.

## Phase 3 - Platform Behavior

Add platform behaviors that improve understanding and usability:

```text
haptics
keyboard behavior
safe areas
status bar behavior
system permissions
native sheets
native controls
```

Haptics should communicate meaningful events, not decorate every tap.

## Phase 4 - Product UI

Only after the workflow behaves correctly should significant visual refinement begin.

Improve hierarchy, spacing, typography, states, touch targets, contrast, motion, and domain-specific components.

This is where the application's visual language starts to emerge.

## Phase 5 - Motion and Gestures

Prefer built-in interactions first.

Introduce React Native Gesture Handler when gesture complexity justifies it.

Introduce Reanimated when motion requires gesture-linked animation, high-frequency UI-thread execution, complex transitions, or coordinated shared values.

Do not use Reanimated for every opacity change.

Motion must explain state, causality, continuity, or spatial relationships.

## Phase 6 - Scale and Hardening

Only after meaningful product behavior exists should the application add capabilities such as:

- offline behavior;
- cache policy;
- background processing;
- observability;
- performance profiling;
- feature flags;
- production error recovery;
- advanced accessibility;
- tablet/orientation layouts;
- localization;
- visual regression testing;
- release automation.

Add each capability in response to product needs.

---

# 13. State Management and Server State

Start with React state:

```text
useState
useReducer
Context
```

Introduce an external state library only when state complexity clearly exceeds these tools.

Server state and application state are different problems.

A server-state library such as TanStack Query becomes valuable when the application genuinely needs several of:

```text
request caching
background refresh
deduplication
retry policies
pagination
optimistic mutation
cache invalidation
multiple consumers of the same remote data
```

Do not install it merely because the application calls an API.

---

# 14. Data, Forms, and Services

Keep API and platform interaction behind clear seams when doing so improves readability or testability.

Avoid creating a universal service architecture before multiple services exist.

Prefer domain intent such as:

```ts
resolveScannedItem(code)
```

over leaking low-level request details throughout screens.

For forms, start with native inputs and local state when the form is small.

Introduce a form/validation library when repeated field orchestration, complex validation, nested structures, performance, or reusable schemas create real friction.

Do not choose a form library globally merely because the application has forms.

---

# 15. Error, Empty, Uncertain, and Recovery States

A modern mobile UI is not just the success screenshot.

For each meaningful flow, deliberately consider the states that can realistically occur:

```text
loading
empty
offline
permission denied
invalid input
server failure
partial data
uncertain recognition
retrying
success
```

For AI or recognition workflows, uncertainty should be visible rather than disguised as certainty.

At an appropriate application boundary, provide a way to contain unexpected render/runtime failures and recover or report them. Do not treat an Error Boundary as a substitute for explicit expected-state handling.

---

# 16. Accessibility

Accessibility is part of component correctness.

Interactive custom controls should define appropriate accessibility labels, roles, states, and hints where useful.

Touch targets must be large enough for reliable use.

Important information should not depend exclusively on color.

Custom precision interactions should provide an accessible alternative when appropriate.

Consider dynamic text sizing, screen readers, focus order, reduced motion, and contrast when relevant to the product audience.

---

# 17. Storybook

Storybook is optional at the beginning.

Introduce it when components have several meaningful states, are reused across screens, visual iteration in full flows becomes slow, agents frequently modify shared components, or a real design system is emerging.

Useful stories represent product states rather than decorative permutations.

Storybook is a component workbench. It is not a replacement for testing the real application.

---

# 18. Testing Strategy

Testing should grow with product risk.

Do not create hundreds of tests before product behavior exists.

## 18.1 Unit tests

Use unit tests for deterministic domain logic with meaningful branching or edge cases.

## 18.2 Component tests

Use React Native Testing Library or another project-adopted user-oriented component-testing approach when component interaction/state behavior is valuable to protect without full E2E execution.

Test user-observable behavior rather than implementation details.

Do not require component tests for every trivial wrapper.

## 18.3 End-to-end tests

Maestro is the default candidate when deterministic mobile E2E coverage is justified.

Prioritize critical user journeys rather than every screen.

Reference: https://docs.maestro.dev/

---

# 19. Device-Driven Verification

A coding agent should see the application it modifies whenever practical.

Use agent-device or equivalent tooling after runnable UI exists for exploratory interaction, accessibility-tree inspection, screenshots, and visual/behavioral verification.

Reference: https://github.com/callstack/agent-device

Use deterministic E2E tests for known regression-critical behavior.

A useful relationship is:

```text
Maestro = deterministic critical journeys
agent-device = exploratory and visual verification
```

Deeper tooling such as Argent may be introduced when debugging requires richer component-tree inspection, network diagnostics, visual regression, replay, or performance profiling.

Reference: https://github.com/software-mansion/argent

A useful development loop is:

```text
implement
↓
run
↓
interact
↓
inspect
↓
capture evidence when useful
↓
adjust
```

Do not declare a visual task complete merely because TypeScript compiles.

---

# 20. Figma and Design Inputs

Figma is a design source, not executable truth.

Figma MCP may be used when structured design context materially improves implementation.

Generated code must still be reviewed against platform behavior, accessibility, domain behavior, and the actual running UI.

Do not treat Figma-to-code as a compiler.

If no Figma design exists, do not block implementation. Product principles, domain states, and a running vertical slice can precede a formal design system.

---

# 21. Images, Lists, and Performance

Do not optimize imagined performance problems. Measure first.

Use normal React Native list primitives until evidence shows they are insufficient for the product's data volume or interaction profile. If list performance becomes a user-visible problem, profile and evaluate focused alternatives based on measured behavior.

Use the simplest image primitive that satisfies the feature. `expo-image` is a strong option when remote-image caching, transitions, placeholders, or richer image behavior provide value. A few simple local assets do not require image infrastructure.

Pay attention when the product contains large lists, continuous camera processing, heavy image rendering, complex gestures, maps, frequent animation, or expensive state propagation.

Do not memoize every component or callback by habit.

---

# 22. Security, Permissions, and Sensitive Data

Request permissions when the related feature is about to be used or when the flow clearly explains why permission is needed.

Handle denied and restricted states deliberately.

Avoid collecting or storing sensitive data unless required.

Use secure platform storage for credentials or sensitive secrets where appropriate.

Never log secrets, tokens, or personal data unnecessarily.

Validate incoming deep-link data before treating it as trusted navigation or command input.

Treat WebView content and bridge messages as trust boundaries. Minimize exposed capabilities and validate messages.

For products with meaningful security risk, use OWASP MASVS as a reference rather than attempting to invent a complete mobile security standard inside this playbook: https://mas.owasp.org/MASVS/

---

# 23. Offline and Network Behavior

Do not claim offline support unless the product has an explicit offline model.

When offline behavior matters, define what is cached, what may be stale, which mutations can queue, how conflicts are resolved, and how synchronization state is communicated.

A network error screen is not an offline architecture.

Introduce persistence and synchronization tools only after this behavior is defined.

---

# 24. Observability and Release

Do not install a large observability stack before there is production behavior to observe.

As the product matures, consider crash reporting, structured errors, important workflow events, performance traces, and release/build identification.

When repeatable team or production builds become necessary, establish a reproducible build/release path.

EAS Build, EAS Update, EAS Submit, and EAS Workflows are natural Expo-integrated options, but they are not mandatory if the project has another deliberate delivery system.

Choose release infrastructure in response to distribution, signing, CI, rollback, and team needs.

---

# 25. Expo Skills and Agent Instructions

When available, agents should use official Expo skills and current Expo documentation for version-sensitive guidance.

Project-specific rules override generic examples.

Each serious application should eventually define local domain instructions describing important entities, core workflows, interaction principles, critical states, error behavior, terminology, motion semantics, and accessibility expectations.

The agent should understand the product, not only React Native.

---

# 26. Agent Operating Rules

Before implementation:

- read repository instructions and project documentation;
- inspect existing code and dependencies;
- determine the smallest vertical slice that satisfies the task;
- use current official documentation for version-sensitive Expo/RN behavior.

During implementation:

- prefer existing platform capabilities;
- avoid adding dependencies unless required;
- keep abstractions proportional to actual complexity;
- run the application early;
- keep changes scoped to the task.

After implementation:

- run relevant checks;
- open the application in the correct runtime;
- exercise the changed flow;
- inspect the rendered UI;
- verify relevant failure/permission states;
- only then refine visual details or introduce additional abstraction.

For agent-specific traps, read `guides/agent-failure-modes.md`.

---

# 27. Architecture Decision Rule

Any significant new framework or cross-cutting dependency should have a short justification.

At minimum answer:

```text
What problem exists today?
Why are current tools insufficient?
What are we adding?
Why is it the smallest appropriate solution?
What costs does it introduce?
What is the exit path?
```

A short ADR, issue, or pull request note is enough.

The objective is clarity, not process overhead.

---

# 28. Suggested Project Shape

Do not treat this as mandatory structure.

A small-to-medium Expo app may begin with:

```text
app/
  ... Expo Router routes

src/
  components/
  features/
  services/
  theme/
  types/
```

As the application becomes domain-heavy, feature-oriented structure may become preferable.

Do not reorganize repeatedly for aesthetic reasons. Let actual domain boundaries drive structure.

Avoid barrel files by default. Add them when they express a useful public module boundary, not merely to shorten imports.

---

# 29. Definition of Done for a Vertical Slice

A feature slice is not complete merely because code exists.

At minimum:

- the application boots in the correct runtime;
- the flow can be reached;
- the primary action works;
- the expected result is visible;
- important failure/permission states are handled;
- strict TypeScript and relevant checks pass;
- custom interactive controls have basic accessibility;
- unnecessary dependencies were not introduced;
- the actual running UI has been inspected.

For critical production flows, add deterministic E2E coverage.

For important visual flows, perform device or simulator visual verification.

For reusable components with many meaningful states, consider Storybook.

See `guides/vertical-slice-checklist.md`.

---

# 30. Anti-Patterns

## Premature architecture

Building large layers before a real workflow exists.

## Dependency shopping

Installing libraries before identifying problems.

## Installed-means-adopted

Using a package simply because it appears in the dependency tree.

## Web-habit leakage

Generating DOM elements, browser CSS, browser storage, or web navigation patterns in native application code without a deliberate cross-platform reason.

## Generic component obsession

Creating reusable abstractions before there is actual reuse.

## Screenshot-driven completion

Polishing a static happy state while loading, failure, permissions, and interaction remain broken.

## AI-generated UI drift

Allowing each generated screen to invent new spacing, typography, states, or interaction rules.

## Figma-as-compiler

Treating generated design-to-code output as production implementation without running and inspecting it.

## Test-count optimization

Writing low-value tests to increase coverage while critical workflows remain unprotected.

## Premature performance engineering

Adding memoization, custom rendering, or native code without evidence of a bottleneck.

## Framework-shaped domain

Changing product concepts to match a library's architecture.

## Expo-Go-only verification

Declaring native functionality correct because JavaScript renders in Expo Go when the feature actually depends on native configuration or a development build.

---

# 31. Final Principle

Do not optimize for having the most sophisticated React Native architecture.

Optimize for having the simplest architecture that clearly expresses the product.

A healthy application should evolve roughly like this:

```text
React Native primitives
        ↓
working product
        ↓
repeated patterns
        ↓
small abstractions
        ↓
semantic product system
        ↓
specialized tools only where justified
```

The right architecture is not the one with the most layers.

It is the one that makes the current product easy to understand, change, verify, and extend.

Complexity must be earned.
