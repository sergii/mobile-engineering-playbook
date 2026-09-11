# Agent Failure Modes

AI coding agents are useful React Native contributors, but they often import assumptions from web development, starter kits, or package names they discover in the dependency tree.

This guide records failure modes that should be treated as explicit guardrails.

## 1. Web-habit leakage

### Failure

The agent generates:

```text
div
span
button
CSS files
browser storage
browser navigation APIs
className
```

inside native application code without a deliberate cross-platform architecture.

### Rule

Use React Native primitives and native platform APIs by default.

`className` or CSS-like syntax is allowed only when the project has explicitly adopted a library that provides it.

Do not install a styling framework to preserve web syntax familiarity.

---

## 2. Starter-kit dependency inflation

### Failure

The agent installs a familiar collection such as:

```text
NativeWind
Zustand
TanStack Query
Reanimated
form library
UI kit
```

before the product needs those capabilities.

### Rule

Each dependency must solve a concrete current problem.

Use `guides/decision-ladder.md` before introducing cross-cutting tools.

---

## 3. Installed means adopted

### Failure

The agent sees Gesture Handler, Reanimated, react-native-screens, or another package in the dependency tree and begins using its API everywhere.

### Rule

Presence is not adoption.

A package may be transitively required by Expo Router or another tool. Application code adopts a package only after the same architectural decision process used for a new dependency.

---

## 4. Unused means removable

### Failure

The agent removes a package because application code does not import it directly.

### Rule

Before removing a package, determine whether Expo, Expo Router, a config plugin, build configuration, or another dependency requires it.

Never infer removability from source imports alone.

---

## 5. Expo Go as universal runtime

### Failure

The agent treats `npx expo start` + Expo Go as sufficient verification after native libraries, config plugins, entitlements, or custom native modules have entered the project.

### Rule

Expo Go is a constrained runtime.

Once native product configuration matters, use and verify a development build or production-like build.

Reference: https://docs.expo.dev/develop/development-builds/introduction/

---

## 6. Compile-success completion

### Failure

The agent reports a feature complete because:

```text
TypeScript passes
Metro starts
unit tests pass
```

without exercising the changed mobile flow.

### Rule

For meaningful UI or native behavior, run the application, reach the flow, interact with it, and inspect the actual result.

Use device-driving tools when useful.

---

## 7. Type erasure to silence errors

### Failure

The agent introduces:

```ts
any
as unknown as Something
// @ts-ignore
```

because generated code does not match the actual data model.

### Rule

Keep strict TypeScript.

Fix the model, validate the boundary, or isolate the unsafe external value. Do not weaken the codebase to make generation easier.

---

## 8. Barrel-file proliferation

### Failure

The agent creates `index.ts` re-export files in every directory by habit.

### Rule

Use a barrel only when it expresses a real public module boundary or materially improves ownership and imports.

Avoid barrels that obscure dependencies or create circular imports.

---

## 9. Generic abstraction before repetition

### Failure

The agent sees two visually similar blocks and immediately creates a universal component with many props.

### Rule

Prefer local duplication over premature abstraction when the product concepts are still changing.

Extract shared components when stable product meaning or real repeated behavior appears.

---

## 10. UI library shapes the product

### Failure

The agent changes the product interaction to fit the components available in a UI kit.

### Rule

Product architecture comes before framework architecture.

A UI kit may accelerate conventional UI. It must not redefine important product concepts merely to reduce implementation effort.

---

## 11. Happy-state screenshot optimization

### Failure

The agent makes the primary screenshot look polished while loading, permission denial, empty, retry, disabled, or failure behavior remains absent.

### Rule

Behavioral correctness comes before visual sophistication.

A visually polished happy state does not complete a vertical slice.

---

## 12. Decorative motion

### Failure

The agent adds animations and haptics to make the interface feel modern without a state or interaction reason.

### Rule

Motion should explain state, causality, continuity, focus, or spatial relationship.

Haptics should signal meaningful events.

---

## 13. Figma as executable truth

### Failure

The agent reproduces a design literally even when it conflicts with platform behavior, accessibility, safe areas, keyboards, or real product states.

### Rule

Figma is design input, not a compiler.

Validate the implementation in the running app.

---

## 14. Premature performance fixes

### Failure

The agent adds memoization, Skia, worklets, custom native code, or list migrations without evidence of a bottleneck.

### Rule

Measure first.

Define the user-visible performance problem, profile it, change the smallest relevant thing, and measure again.

---

## 15. Platform details deferred forever

### Failure

The agent treats keyboard handling, safe areas, permissions, status bars, back behavior, or accessibility as optional polish.

### Rule

These are part of mobile correctness once the relevant flow exists.

They should be added after the first working interaction, before the feature is considered production-ready.

---

# Recovery sequence

When an agent-generated implementation feels overbuilt or web-shaped, recover in this order:

```text
identify the actual user goal
    ↓
remove speculative architecture
    ↓
return to RN / Expo primitives
    ↓
make one vertical slice work
    ↓
run it on the correct runtime
    ↓
verify behavior
    ↓
reintroduce only the abstractions that now solve demonstrated problems
```

Complexity must be earned.
