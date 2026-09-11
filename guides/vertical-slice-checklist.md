# Vertical Slice Checklist

Use this checklist for a meaningful feature slice. Not every item applies to every feature. Mark non-applicable items deliberately rather than adding infrastructure to satisfy the checklist.

## User goal

- [ ] The slice represents one clear user goal.
- [ ] The primary interaction is real, not only a static mock screen.
- [ ] The expected result is visible to the user.

## Runtime

- [ ] The application boots.
- [ ] The feature is tested in the correct runtime.
- [ ] Expo Go is used only if the feature fits its native capabilities.
- [ ] Native/config-plugin features are verified in a development or production-like build.

## Navigation

- [ ] The user can reach the flow through intended navigation.
- [ ] Back/cancel behavior is correct where relevant.
- [ ] Deep-link or notification entry is verified when the feature depends on it.

## Behavior

- [ ] The primary action works.
- [ ] Disabled state is handled where relevant.
- [ ] Loading state is handled where relevant.
- [ ] Empty state is handled where relevant.
- [ ] Permission-denied state is handled where relevant.
- [ ] Failure and retry behavior are handled where relevant.
- [ ] Repeated interaction does not leave the screen in an invalid state.

## Platform behavior

- [ ] Safe areas are correct.
- [ ] Keyboard behavior is correct where text input exists.
- [ ] Status bar/system UI behavior is acceptable.
- [ ] Native permissions are requested at an understandable point in the flow.
- [ ] Haptics, if present, communicate meaningful events.

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
Did we add only the complexity this slice actually needs?
```

If any answer is no, the slice is not done.
