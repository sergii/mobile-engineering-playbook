# Vertical Slice Checklist

Use this checklist for a meaningful feature slice. Not every item applies to every feature. Mark non-applicable items deliberately rather than adding infrastructure to satisfy the checklist.

## User goal

- [ ] The slice represents one clear user goal.
- [ ] The primary interaction is real, not only a static mock screen.
- [ ] The expected result is visible to the user.

## Runtime and native ownership

- [ ] The application boots.
- [ ] The feature is tested in the correct runtime.
- [ ] Expo Go is used only if the feature fits its native capabilities.
- [ ] Native/config-plugin features are verified in a development or production-like build.
- [ ] Before native edits, the project was identified as CNG-owned or explicitly native-project-owned.
- [ ] In a CNG-owned project, persistent native changes are reproducible through app config, config plugins, or modules rather than existing only in generated `ios/` / `android/` files.

## Navigation

- [ ] The user can reach the flow through intended navigation.
- [ ] Back/cancel behavior is correct where relevant.
- [ ] Deep-link or notification entry is verified when the feature depends on it.
- [ ] Route-level auth/authorization uses declarative route protection when appropriate.
- [ ] Expected product failures are modeled explicitly rather than being delegated to Error Boundaries.
- [ ] Unexpected route/runtime failures have an appropriate recovery boundary where justified.

## Behavior

- [ ] The primary action works.
- [ ] Disabled state is handled where relevant.
- [ ] Loading state is handled where relevant.
- [ ] Empty state is handled where relevant.
- [ ] Permission-denied state is handled where relevant.
- [ ] Failure and retry behavior are handled where relevant.
- [ ] Repeated interaction does not leave the screen in an invalid state.

## Platform behavior

- [ ] Edge-to-edge/system-bar behavior is intentional.
- [ ] Safe-area insets are correct and not double-applied.
- [ ] Keyboard behavior is correct where text input exists.
- [ ] Text-entry flows have been exercised with the keyboard open on each targeted platform.
- [ ] Focused fields remain reachable and visible.
- [ ] Primary actions remain reachable while typing.
- [ ] Status bar/system UI behavior is acceptable.
- [ ] Native permissions are requested at an understandable point in the flow.
- [ ] Haptics, if present, communicate meaningful events.

## Persistence

- [ ] Any persisted data has an explicit reason to persist.
- [ ] Sensitive values use secure platform-backed storage where appropriate.
- [ ] Non-sensitive key-value state does not introduce a database without need.
- [ ] A database or sync framework is introduced only when the data model/offline model justifies it.
- [ ] Browser storage APIs were not introduced into native code by habit.

## UI and accessibility

- [ ] The actual rendered screen has been inspected.
- [ ] Important text is not clipped.
- [ ] Touch targets are usable.
- [ ] Important meaning is not communicated by color alone.
- [ ] Custom interactive controls expose appropriate accessibility semantics.
- [ ] Motion respects product meaning and reduced-motion needs where relevant.

## Code quality

- [ ] Strict TypeScript passes.
- [ ] No unnecessary `any`, type-ignore, or unsafe assertion was introduced.
- [ ] No unnecessary dependency was introduced.
- [ ] Transitive dependencies were not accidentally adopted or removed.
- [ ] Abstractions are proportional to actual reuse and complexity.
- [ ] Project/domain terminology is used consistently.

## Verification

- [ ] The changed flow was exercised manually or with device-driving tooling.
- [ ] Deterministic tests protect critical regression-prone behavior where justified.
- [ ] Maestro covers the critical E2E path when that level of protection is warranted.
- [ ] Storybook stories exist only when isolated component-state work is valuable.

## Before calling it done

Ask:

```text
Can the user accomplish the intended goal?
Did we verify it in the real mobile runtime?
Did we verify native/platform behavior that this slice depends on?
Did we add only the complexity this slice actually needs?
```

If any answer is no, the slice is not done.
