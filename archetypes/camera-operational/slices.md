# Camera-Operational Vertical Slices

These are examples of thin, complete workflows for a camera-first operational product.

They are not a roadmap and not every product needs every slice.

## Slice 1: Identify and confirm

```text
open operational screen
→ obtain camera permission if needed
→ detect identifier
→ resolve domain object
→ show recognized object
→ confirm
→ show operation result
```

Minimal implementation may use:

- one route;
- local state;
- mocked resolver data;
- `expo-camera` when appropriate;
- a simple viewfinder overlay;
- one confirmation action;
- haptic confirmation;
- explicit permission and failure states.

The slice is valuable when the user can complete the real interaction shape, even if persistence and backend integration are still mocked.

## Slice 2: Uncertain result and correction

```text
detect evidence
→ recognition returns uncertain result
→ show uncertainty explicitly
→ user chooses/corrects identity
→ confirm
→ show final result
```

This slice validates whether uncertainty is understandable rather than hidden behind a polished happy state.

## Slice 3: Identify object and location

```text
identify object
→ identify destination/location
→ show object + location together
→ confirm relationship or movement
→ show result
```

This is useful for products where the physical operation connects two domain entities.

Do not generalize this into a universal "scanner architecture" before the slice works.

## Slice 4: Repeated high-frequency operation

```text
complete operation
→ return immediately to ready state
→ process next object
→ repeat
```

This slice exposes product forces that are easy to miss in a single-use demo:

- reset latency;
- accidental duplicate operations;
- idempotency/deduplication when repeated confirmation could mutate domain state twice;
- haptic fatigue;
- visual state persistence;
- throughput;
- error recovery;
- one-handed ergonomics.

Do not treat duplicate prevention as only a UI debouncing problem when the operation has server-side effects. Define the product/API idempotency or deduplication behavior when repeated execution matters.

## Slice 5: Connectivity loss

Only build this when offline behavior is a real requirement.

```text
perform operation
→ network unavailable
→ preserve local intent
→ communicate pending state
→ reconnect
→ synchronize
→ show final state or conflict
```

Before implementing this slice, define:

- what data may be stale;
- whether operations can queue;
- conflict policy;
- idempotency requirements;
- how pending/synced/failed state is shown.

A generic network error screen is not this slice.

## Application-lifecycle interruption

Camera-operational workflows often cross OS/application-lifecycle boundaries because camera access, permissions, recognition, uploads, queues, or mutations may be active when the app backgrounds or the process is terminated.

When the product depends on continuity across interruption, verify the real intended behavior for:

```text
scanning/capture
→ app backgrounds
→ app resumes
→ permission/camera readiness is revalidated when necessary
→ interrupted work is restored, restarted, discarded, or re-confirmed deliberately
```

Do not prescribe one universal camera resource-management implementation here. The product and current camera/runtime APIs determine the correct mechanism.

## Verification emphasis

Camera-operational slices should usually be exercised in a running simulator or device, and camera-critical behavior should eventually be checked on a physical device.

Also exercise background/resume and repeated-operation behavior when those interruptions can affect correctness.

Use deterministic E2E coverage for critical stable paths and device-driving tools for exploratory/visual verification.
