# Mobile Engineering Playbook

Version: 0.1

## Purpose

This playbook defines the default engineering approach for building modern mobile applications with React Native and Expo.

It is intended for AI-assisted development with Codex and similar coding agents, but the principles apply equally to human contributors.

The goal is not to prescribe a visual style, component library, state-management library, or application architecture prematurely.

The goal is to produce applications that are:

- simple before they are sophisticated;
- functional before they are polished;
- native-feeling without unnecessary native code;
- domain-driven rather than framework-driven;
- easy for humans and coding agents to understand;
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

Examples of valid reasons:

- complex gesture coordination;
- advanced animations;
- server-state caching;
- responsive theme switching;
- deterministic end-to-end testing;
- high-performance custom rendering;
- platform APIs not covered by Expo.

"Modern React Native apps use it" is not a valid reason.

## 1.2 Expo-first

New applications should use the current stable Expo SDK unless there is a concrete documented reason not to.

Use the React Native and React versions supported by that Expo SDK.

Do not independently upgrade React Native underneath Expo without a documented compatibility reason.

Prefer Expo APIs when they adequately solve the product requirement.

Examples may include:

```text
expo-camera
expo-haptics
expo-image
expo-notifications
expo-secure-store
expo-file-system
expo-location
expo-av / current Expo media APIs
```

The exact package list evolves. Always verify the current Expo documentation instead of relying on an old package list.

Introduce custom native code only when Expo or a mature React Native package cannot satisfy the requirement.

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

Domain-specific components are usually more valuable than generic wrappers.

Prefer:

```tsx
<ScannerViewfinder />
<ItemRecognitionResult />
<StorageLocationCard />
<ThermostatDial />
<MoveConfirmation />
```

over speculative abstractions such as:

```tsx
<FancyCard />
<UniversalPanel />
<MagicContainer />
```

unless real reuse demonstrates that the abstraction is useful.

## 1.4 Product architecture before library architecture

Libraries should adapt to the product, not the other way around.

Do not redesign the product's interaction model to fit a component library or state framework unless there is a compelling reason.

## 1.5 Prefer reversible decisions early

Early product development contains uncertainty.

Prefer choices that are easy to remove or replace:

- local state before global state;
- plain StyleSheet before a styling framework;
- local mock services before a large data layer;
- one real workflow before a universal architecture.

---

# 2. Default Technical Foundation

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

# 3. Version Policy

## 3.1 Prefer current stable versions

At project creation time:

1. determine the current stable Expo SDK;
2. use the React Native and React versions supported by that SDK;
3. use current stable TypeScript compatible with the toolchain;
4. verify Expo Router and required Expo packages against the same SDK.

Do not hard-code this playbook to a historical SDK number.

## 3.2 Upgrade intentionally

Before an Expo SDK upgrade:

- read the Expo upgrade guide;
- review React Native breaking changes;
- run type checks and tests;
- run the application on supported platforms;
- inspect critical UI flows;
- verify native permissions and config plugins;
- verify third-party native dependencies.

Use official Expo upgrade tooling and skills when available.

---

# 4. Dependency Policy

Dependencies are liabilities as well as capabilities.

Before adding a dependency, answer:

```text
What specific problem does this solve?

Can React Native already solve it?

Can Expo already solve it?

Can a small local abstraction solve it?

Is the problem present now or merely anticipated?

What maintenance, bundle, native-build, runtime, or upgrade cost does it introduce?
```

Do not add infrastructure for hypothetical future requirements.

Prefer a small dependency graph.

When a dependency is cross-cutting or difficult to reverse, record the reason in a short ADR or pull request note.

See `guides/decision-ladder.md` for concrete escalation guidance.

---

# 5. Navigation

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

Avoid long-lived application navigation implemented through large conditional trees such as:

```tsx
activeScreen === 'home'
  ? <Home />
  : activeScreen === 'profile'
    ? <Profile />
    : ...
```

A tiny prototype may temporarily use this approach.

Once navigation becomes part of the product architecture, move to Expo Router.

Navigation should support product behavior such as:

- deep links;
- notification-driven destinations;
- modal flows;
- nested workflows;
- back behavior;
- restoration where required.

Do not invent wrapper abstractions over Expo Router before real repetition demonstrates a need.

---

# 6. Styling

## 6.1 Start with StyleSheet

Start with React Native `StyleSheet`.

```tsx
const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
});
```

Do not introduce a styling framework until plain StyleSheet creates a concrete limitation.

Possible future reasons for another styling layer include:

- complex runtime theming;
- multiple product themes;
- responsive breakpoints across phone and tablet;
- large-scale theme propagation;
- high-contrast operational modes;
- extensive platform-dependent styling;
- design-system scale that makes plain imports difficult to maintain.

When such a problem appears, evaluate the smallest tool that solves it.

Do not globally pre-select Unistyles, NativeWind, Tamagui, or another styling framework.

## 6.2 Avoid style abstraction for its own sake

Do not turn every style object into a helper.

Local component styles are good when the style is local to that component.

Extract style concepts only when they represent shared product meaning or repeated decisions.

---

# 7. Design Tokens

Even with plain StyleSheet, repeated visual decisions should gradually become tokens.

Begin small.

Example:

```text
theme/
  colors.ts
  spacing.ts
  typography.ts
```

A small token layer is enough for an early product.

## 7.1 Primitive tokens

Primitive tokens describe raw values:

```text
spacing.sm
spacing.md
spacing.lg
fontSize.body
fontSize.title
```

These are useful, but they are not the final design language.

## 7.2 Semantic tokens

As the product matures, prefer semantic tokens over purely visual names.

Instead of:

```text
green500
gray700
spacing16
```

prefer:

```text
text.primary
text.secondary
surface.default
surface.elevated
border.subtle
action.primary
```

For domain-heavy products, semantic tokens may become domain-specific:

```text
scanner.idle
scanner.detecting
scanner.success
scanner.uncertain
scanner.error
storage.available
storage.occupied
```

Tokens should express product meaning where useful.

Do not attempt to design the complete token system before the product exists.

Let repeated product decisions reveal the system.

## 7.3 Avoid fake token adoption

A token file is not useful if components continue to hard-code the same values inconsistently.

When a token represents a deliberate shared product decision, use it consistently.

Do not force one-off values into the token system merely to avoid literals.

---

# 8. Domain-Driven UI

The user interface should reflect the domain.

Do not force every application into the same collection of generic cards, pills, sheets, dashboards, and forms.

Before designing a flow, ask:

```text
What is the main object the user is manipulating?

What physical or digital event is taking place?

What information matters at this moment?

What uncertainty exists?

What can go wrong?

What is the next likely action?
```

UI structure should follow those answers.

For operational applications, domain state is often more important than decorative hierarchy.

Examples:

```text
scanning
recognizing
recognized
uncertain
confirmed
moving
stored
failed
```

These are product states and should often have explicit UI representations.

## 8.1 Prefer domain components

Create components around stable product concepts.

Examples:

```text
ScannerViewfinder
RecognitionConfidence
StorageLocation
PickTask
MoveConfirmation
ThermostatDial
```

Avoid building a generic internal component framework before these product concepts are understood.

---

# 9. Native Controls

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

Use custom React Native UI where product identity or domain interaction requires it.

A useful rule is:

```text
native control is already appropriate
→ use it

interaction is product-specific
→ build it from primitives
```

---

# 10. Build Vertical Slices

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

That is a useful vertical slice.

"Build scanner architecture" is not.

## 10.1 Slice boundaries

A good first slice may use:

- local state;
- mocked or local data;
- one route;
- one service function;
- minimal styling;
- minimal tests.

It should still exercise the real interaction shape of the product.

## 10.2 Expand from working seams

Once a slice works, replace mocks or local assumptions at the seams:

```text
mock data → API
local state → shared state only if necessary
simple visual → refined visual
manual verification → deterministic test where valuable
```

Do not rebuild the slice from scratch merely because the architecture becomes more sophisticated.

---

# 11. Development Phases

These phases are guidance, not bureaucracy. Small tasks may cross several phases quickly.

## Phase 0 - Walking Skeleton

Goal: the project runs.

Requirements:

```text
Expo project exists
TypeScript works
application starts
routing works
one screen renders
iOS simulator runs
Android emulator runs when the product targets Android
```

Do not optimize visual design at this stage.

## Phase 1 - First Vertical Slice

Implement one meaningful user workflow.

The workflow should include real interaction rather than static mock screens.

Use local state and mocked data when necessary.

Do not introduce global state management simply because the future application may need it.

At the end of this phase, a user should be able to perform one real task.

## Phase 2 - Behavioral Correctness

Before increasing visual sophistication, make the slice reliable.

Check:

```text
main action works
disabled state works
failure state exists
loading state exists when relevant
empty state exists when relevant
back navigation works
screen survives repeated interaction
```

Run TypeScript checks and relevant tests.

Run the application.

Do not rely solely on static code inspection.

## Phase 3 - Platform Feedback

Add platform behaviors that improve understanding.

Examples:

```text
haptics
keyboard behavior
safe areas
status bar behavior
system permissions
native sheets
native controls
```

Haptics should communicate meaningful events.

Good examples:

```text
successful scan
selection changed
critical confirmation
operation failed
```

Do not add haptics as decoration.

## Phase 4 - Product UI

Only after the workflow behaves correctly should significant visual refinement begin.

Improve:

```text
hierarchy
spacing
typography
states
motion
touch targets
contrast
domain-specific components
```

This is where the application's visual language starts to emerge.

Extract reusable components when reuse is demonstrated.

Do not create a generic design system merely to satisfy architectural aesthetics.

## Phase 5 - Motion and Gestures

Prefer built-in interactions first.

Use `Pressable`, scrolling, native gestures, or simple state transitions where sufficient.

Introduce React Native Gesture Handler when interaction complexity justifies it.

Introduce Reanimated when motion requires:

- gesture-linked animation;
- continuous high-performance animation;
- complex transitions;
- animation on the UI thread;
- sophisticated shared-value behavior.

Do not use Reanimated for every opacity change.

Motion must communicate state, causality, continuity, or spatial relationships.

## Phase 6 - Scale and Hardening

Only after meaningful product behavior exists should the application be optimized for broader scale.

Potential concerns include:

- offline behavior;
- cache policy;
- background processing;
- observability;
- performance profiling;
- large data sets;
- feature flags;
- production error recovery;
- advanced accessibility;
- tablet or orientation layouts;
- localization;
- visual regression testing.

Add each capability in response to product needs.

---

# 12. State Management

Start with React state.

Use:

```text
useState
useReducer
Context
```

where appropriate.

Introduce an external state-management library only when state complexity clearly exceeds these tools.

Typical signs include:

- many distant components need to mutate the same state;
- state transitions are difficult to reason about locally;
- persistent global state has become a real product concept;
- Context usage causes excessive coupling or churn;
- workflow state deserves an explicit model.

Do not install Redux, Zustand, Jotai, or another state library simply because the application is non-trivial.

## 12.1 Server state is different

Server state and application state are different problems.

A server-state library such as TanStack Query becomes valuable when the application genuinely needs:

```text
request caching
background refresh
deduplication
retry policies
pagination
optimistic mutation
cache invalidation
```

Do not install it merely because the application calls an API.

---

# 13. Data and Services

Keep API and platform interaction behind clear seams when doing so improves testability or readability.

Avoid creating a universal service architecture before multiple services exist.

Good early structure may be as small as:

```text
services/
  items.ts
  scanner.ts
```

A service should express domain intent where possible.

Prefer:

```ts
resolveScannedItem(code)
```

over low-level request details leaking throughout screens.

Do not bury simple one-off calls under many layers solely to satisfy a pattern.

---

# 14. Error, Loading, Empty, and Uncertain States

A modern mobile UI is not just the success screenshot.

For each meaningful flow, consider:

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

Not every screen needs every state.

But any state that can realistically occur should be considered deliberately.

For AI or recognition workflows, uncertainty should be visible rather than disguised as certainty.

---

# 15. Accessibility

Accessibility is part of component correctness.

Interactive custom controls should define appropriate:

```text
accessibilityLabel
accessibilityRole
accessibilityState
accessibilityHint
```

where useful.

Touch targets must be large enough for reliable use.

Important information should not depend exclusively on color.

Custom controls should provide alternate ways to perform precision interactions when appropriate.

Example:

A rotary temperature control may support dragging but should also provide accessible increment/decrement actions.

Consider dynamic text sizing, screen readers, focus order, reduced motion, and contrast when they are relevant to the product audience.

---

# 16. Motion and Haptics

Motion is product communication, not decoration.

Good motion can explain:

- what changed;
- where an object moved;
- whether an action succeeded;
- which element has focus;
- how one screen relates to another.

Avoid adding animation because the interface feels "too static".

Haptics should correspond to meaningful interaction events.

Use them sparingly enough that each signal retains meaning.

Respect reduced-motion or platform accessibility settings where applicable.

---

# 17. Storybook

Storybook is optional at the beginning.

Introduce it when at least one of the following becomes true:

- components have several meaningful states;
- components are reused across several screens;
- UI regression becomes difficult to inspect inside full workflows;
- agents frequently modify shared components;
- a product design system has started to emerge.

Useful stories should show states rather than decorative variants.

Example:

```text
ScannerViewfinder

idle
detecting
recognized
uncertain
permission-denied
error
```

Storybook is a component workbench.

It is not a replacement for testing the real application.

---

# 18. Testing Strategy

Testing should grow with product risk.

Do not create hundreds of tests before product behavior exists.

At minimum, protect critical behavior.

## 18.1 Unit tests

Use unit tests for deterministic domain logic that has meaningful branching or edge cases.

Examples:

- parsing;
- calculations;
- reducers;
- state machines;
- validation;
- data transformations.

Do not test trivial implementation details merely to increase coverage.

## 18.2 Component tests

Add component tests when interactions or state behavior are valuable to protect without running full E2E flows.

Prefer testing user-observable behavior over internal implementation details.

## 18.3 End-to-end tests

For end-to-end mobile flows, Maestro is the default candidate when deterministic E2E coverage is justified.

Good Maestro flows include:

```text
launch app
navigate to core feature
perform primary action
verify result
```

Focus first on critical user journeys rather than every screen.

Do not attempt to encode all visual nuance into E2E assertions.

---

# 19. Agent-Device and Device-Driven Verification

Agent-device is a visual and behavioral inspection tool.

Use it after a runnable UI exists.

It is especially useful for:

```text
opening the application
navigating flows
tapping controls
reading the accessibility tree
taking screenshots
discovering unexpected UI problems
checking whether an implementation matches intent
```

Do not use agent-device as a replacement for deterministic tests.

Use:

```text
Maestro
```

for known critical behavior.

Use:

```text
agent-device
```

for exploratory, visual, and agent-driven verification.

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
capture screenshot when useful
↓
compare against intended behavior
↓
adjust
```

Agents should see the application they modify whenever practical.

Deeper device tooling such as Argent may be introduced when debugging requires richer React Native introspection, network inspection, performance profiling, or platform tooling.

---

# 20. Visual Verification

For meaningful UI changes, source-code inspection alone is insufficient.

At minimum:

```text
run the screen
interact with it
inspect the result
```

For substantial product UI work:

```text
run simulator/device
capture screenshots when useful
inspect multiple states
verify touch interaction
verify text does not clip
verify safe areas
verify keyboard behavior
verify light/dark or orientation behavior when supported
```

Do not declare a visual task complete merely because TypeScript compiles.

---

# 21. Figma and Design Inputs

Figma is a design source, not executable truth.

Figma MCP may be used when structured design context materially improves implementation.

Agents may read:

```text
layout
components
variables
design tokens
spacing
typography
```

from Figma.

Generated code must still be reviewed against:

```text
platform behavior
accessibility
domain behavior
actual running UI
```

Do not treat Figma-to-code as a compiler.

If no Figma design exists, do not block implementation. Product principles, domain states, and a running vertical slice can precede a formal design system.

---

# 22. Expo Skills and Agent Instructions

When available, agents should use official Expo skills and current Expo documentation for Expo-specific guidance.

Relevant skill areas may include:

```text
project structure
Expo Router
animation
native UI
design systems
data fetching
Expo modules
SDK upgrades
```

Project-specific rules override generic examples.

Each serious application should eventually define its own domain instructions describing:

```text
important entities
core workflows
interaction principles
critical states
error behavior
domain terminology
motion semantics
accessibility expectations
```

The agent should understand the product, not only React Native.

---

# 23. Codex Operating Rules

## Before implementation

Read:

```text
AGENTS.md
this playbook
project-specific documentation
existing code
```

Determine the smallest vertical slice that satisfies the task.

Inspect existing dependencies before proposing new ones.

Use current official documentation for version-sensitive Expo or React Native behavior.

## During implementation

Prefer existing platform capabilities.

Avoid adding dependencies unless required.

Keep abstractions proportional to actual complexity.

Run the application early.

Do not build large amounts of infrastructure before validating the workflow.

Keep changes scoped to the task unless adjacent changes are necessary for correctness.

## After implementation

Run relevant checks.

Open the application.

Exercise the changed flow.

Inspect the rendered UI.

For significant UI work, inspect actual simulator or device output.

Only then refine visual details or introduce additional abstraction.

---

# 24. Escalation Rule

Architecture should become more sophisticated only when the current architecture produces identifiable friction.

The default decision sequence is:

```text
Can React Native solve it?
        ↓ no
Can Expo solve it?
        ↓ no
Can a small local abstraction solve it?
        ↓ no
Can a focused library solve it?
        ↓ no
Do we need a larger framework or native implementation?
```

Move downward only when necessary.

See `guides/decision-ladder.md` for specific tools.

---

# 25. Architecture Decision Rule

Any significant new framework or cross-cutting dependency should have a short justification.

At minimum answer:

```text
Why are we adding this?

What problem exists today?

Why are existing tools insufficient?

What new constraints does this dependency introduce?

How difficult would it be to remove later?
```

This does not require bureaucracy.

A short ADR or pull-request note is enough.

The objective is to prevent accidental architectural drift.

---

# 26. Suggested Project Shape

Do not treat this as mandatory structure. It is a reasonable starting point for a small-to-medium Expo application.

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

As the application becomes domain-heavy, feature-oriented structure may become preferable:

```text
src/
  features/
    scanning/
    inventory/
    moves/
  shared/
  theme/
```

Do not reorganize repeatedly for aesthetic reasons.

Let actual domain boundaries drive structure.

---

# 27. Performance

Do not optimize imagined performance problems.

First measure.

Pay attention when the product contains:

- large lists;
- continuous camera processing;
- heavy image rendering;
- complex gestures;
- frequent animation;
- maps;
- expensive state propagation;
- large JSON transformations;
- native bridges or custom modules.

Use profiling tools when a real problem appears.

Do not memoize every component or callback by habit.

Do not introduce Skia, worklets, custom native code, or elaborate caching without measured need.

---

# 28. Offline and Network Behavior

Do not claim offline support unless the product has an explicit offline model.

When offline behavior matters, define:

- what data is cached;
- what data may be stale;
- which mutations can queue;
- how conflicts are resolved;
- what the user sees while offline;
- how synchronization status is communicated.

A network error screen is not an offline architecture.

Introduce persistence and synchronization tools only after this behavior is defined.

---

# 29. Permissions and Privacy

Request permissions only when the related feature is about to be used or when the product flow clearly explains why permission is needed.

Handle denied and restricted states deliberately.

Avoid collecting or storing sensitive data unless required.

Use secure platform storage for secrets or sensitive credentials where appropriate.

Never log secrets, tokens, or personal data unnecessarily.

---

# 30. Observability

Do not install a large observability stack before the application has production behavior to observe.

As the product matures, consider:

- crash reporting;
- structured application errors;
- important workflow events;
- performance traces;
- release/build identification.

Prefer observability that helps answer real operational questions.

Do not turn analytics into a substitute for product thinking.

---

# 31. Definition of Done for a Vertical Slice

A feature slice is not complete merely because the code exists.

At minimum:

- the application boots;
- the flow can be reached;
- the primary action works;
- the expected result is visible;
- important failure states are handled;
- TypeScript checks pass;
- interactive custom controls have basic accessibility;
- unnecessary dependencies were not introduced;
- the actual running UI has been inspected.

For critical production flows, also add deterministic E2E coverage.

For important visual flows, also perform agent-device or equivalent visual verification.

For reusable components with many states, consider Storybook.

---

# 32. Anti-Patterns

Avoid these defaults:

## Architecture-first development

Building large layers before a real workflow exists.

## Dependency shopping

Installing libraries before identifying problems.

## Generic component obsession

Creating reusable components before there is actual reuse.

## Screenshot-driven completion

Polishing a static happy-state screen while loading, failure, permissions, and interaction remain broken.

## AI-generated UI drift

Allowing each generated screen to invent new spacing, typography, states, or interaction rules.

## Figma-as-compiler

Treating generated design-to-code output as production implementation without running and inspecting it.

## Test-count optimization

Writing low-value tests to increase coverage metrics while critical workflows remain unprotected.

## Premature performance engineering

Adding memoization, custom rendering, or native code without evidence of a bottleneck.

## Framework-shaped domain

Changing product concepts to match the architecture of a library.

---

# 33. Final Principle

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
