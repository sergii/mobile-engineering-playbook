# Camera-Operational Archetype

This archetype describes mobile products where the camera is a primary operational input and the user is acting on physical-world objects or locations.

Typical examples may include inventory, logistics, field service, asset handling, maintenance, receiving, picking, inspection, or similar workflows.

This is **not** a warehouse template and not every camera-based application fits this archetype.

## Dominant interaction shape

A common interaction loop is:

```text
observe physical object
→ detect or capture evidence
→ resolve or recognize domain identity
→ expose confidence / ambiguity
→ user confirms or corrects
→ domain state changes
→ feedback confirms the result
```

The exact steps depend on the product. Some workflows use barcodes, some use OCR, some use object recognition, some use photos as evidence, and some mix several inputs.

## Product forces

Compared with a conventional form-heavy application, this archetype often has stronger pressure around:

- camera permissions and native runtime configuration;
- one-handed use;
- immediate visual feedback;
- haptics;
- uncertain or ambiguous recognition;
- physical environment constraints such as motion, poor lighting, gloves, or noisy surroundings;
- repeated high-frequency actions;
- offline or unstable-network operation;
- hardware-specific behavior;
- fast recovery from a wrong detection or wrong target.

These are tendencies, not requirements.

## Domain states

Common state families may include:

```text
idle
requesting-permission
ready
scanning
capturing
recognizing
recognized
uncertain
not-found
confirming
confirmed
failed
recovering
```

A specific product should keep only states that actually exist in its workflow.

## Domain components

Potential domain components include:

```text
ScannerViewfinder
CaptureControl
RecognitionResult
ConfidenceIndicator
ObjectIdentity
LocationIdentity
ConfirmationAction
CorrectionAction
OperationResult
```

These names are examples. They are product vocabulary, not a reusable UI framework.

## UI guidance

The camera surface is often the primary workspace rather than decorative background.

Prefer:

- one clear primary action;
- visible operational state;
- clear distinction between recognized, uncertain, and failed results;
- fast correction paths;
- meaningful haptic feedback;
- controls that remain usable one-handed when that matches the environment;
- minimal confirmation when the system is already sufficiently certain and the business risk allows it.

Do not hide uncertainty to make the UI look cleaner.

## Architectural bias

This archetype often reaches certain problems earlier than other product categories:

- development builds because camera/native configuration matters;
- explicit permission handling;
- device-driven testing;
- Gesture Handler for richer camera interactions;
- Reanimated for continuous overlays or gesture-linked feedback;
- local persistence when operations must survive connectivity loss;
- background or queued synchronization;
- performance profiling when continuous processing becomes expensive.

None of these is automatically required.

Use `archetype.yaml` as the compact machine-readable profile and `slices.md` for example vertical slices.

## Relationship to the core playbook

The core rules still apply:

```text
primitive first
→ Expo capability
→ local abstraction
→ focused dependency
→ larger framework/native code only when justified
```

This archetype changes which problems are likely to appear early. It does not change the escalation rule.
