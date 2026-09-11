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

## Navigation and access

- [ ] The user can reach the flow through intended navigation.
- [ ] Back/cancel behavior is correct where relevant.
- [ ] Deep-link or notification entry is verified when the feature depends on it. For custom URI schemes, a quick simulator/device check can use the project's real URL with `npx uri-scheme open <url> --ios` or `--android`.
- [ ] Route-level auth/authorization uses declarative route protection when appropriate.
- [ ] Protected Routes are not treated as a server-side authorization boundary; protected API operations authenticate and authorize independently on the backend.
- [ ] Session restoration has an explicit loading/unknown state when required, so protected UI does not flash before auth state is known.
- [ ] Credential refresh/rotation for the same authenticated identity does not accidentally behave like logout.
- [ ] Logout/account-switch/expired-session behavior removes credentials and invalidates user-scoped data where product security requires it.
- [ ] Expected product failures are modeled explicitly rather than being delegated to Error Boundaries.
- [ ] Unexpected React render/component-tree failures have an appropriate recovery boundary where justified.
- [ ] Event-handler and async-operation errors are handled explicitly rather than assumed to be caught by an Error Boundary.

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

## Application lifecycle and interruption

When the feature owns long-lived, persisted, auth-sensitive, payment-sensitive, upload, queue, or mutation state:

- [ ] Cold start behavior is correct.
- [ ] Background → foreground resume behavior is correct.
- [ ] Process restart does not silently corrupt or duplicate the workflow.
- [ ] Expired/stale session or data is handled deliberately after resume/restart.
- [ ] Interrupted mutations are safe to retry, deduplicated/idempotent where necessary, or clearly resolved for the user.
- [ ] Drafts/uploads/queues are restored, restarted, discarded, or re-confirmed according to explicit product behavior rather than accidental component state.

## Persistence

- [ ] Any persisted data has an explicit reason to persist.
- [ ] Sensitive values use secure platform-backed storage where appropriate.
- [ ] Non-sensitive key-value state does not introduce a database without need.
- [ ] A database or sync framework is introduced only when the data model/offline model justifies it.
- [ ] Browser storage APIs were not introduced into native code by habit.

## OTA / release compatibility

When the product uses OTA updates:

- [ ] The update is compatible with the target binary/runtimeVersion.
- [ ] Native-runtime changes trigger a new compatible binary rather than an OTA-only release.
- [ ] The update was exercised on a preview/staging build with the intended runtime before production promotion.
- [ ] A rollback path is known for a bad update.
- [ ] Gradual rollout is considered when the change carries meaningful production risk.

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
Did we verify interruption/restart behavior when the workflow depends on it?
Did we add only the complexity this slice actually needs?
```

If any answer is no, the slice is not done.
