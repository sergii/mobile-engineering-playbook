# Platform Native Rules

This guide records platform rules that are easy for coding agents to get wrong in Expo / React Native projects.

Load this guide when the task touches native configuration, authentication routing, storage, safe areas, keyboard behavior, OTA/native compatibility, or application-lifecycle/interruption behavior. Do not load it for unrelated small tasks.

## 1. Continuous Native Generation and Prebuild

First determine who owns the native projects.

### CNG / Prebuild owns `ios/` and `android/`

When the project uses Expo Continuous Native Generation (CNG) as its source of truth:

- treat `ios/` and `android/` as generated output;
- do not make persistent configuration changes directly in generated native files;
- express supported native configuration through `app.json` / `app.config.ts`;
- use library config plugins when available;
- use a local config plugin when a project-specific native configuration change is required;
- use Expo Modules API for application-specific native functionality when appropriate;
- assume `npx expo prebuild --clean` may delete manual edits inside generated native projects.

A temporary native edit can be useful for debugging or discovering the required native change. If the project remains CNG-owned, migrate the successful change back into reproducible app configuration, a config plugin, or a local Expo module before treating the task as complete.

### Native projects are explicitly owned by the repository

Direct edits to Xcode/Gradle/native source are valid when the project has explicitly chosen to maintain `ios/` and `android/` as source-controlled native projects instead of regenerating them with CNG.

Do not run `expo prebuild --clean` against manually owned native projects unless overwriting those customizations is intentional.

### Agent check before touching native files

Before editing `ios/` or `android/`, answer:

```text
Is this project CNG-owned or native-project-owned?

If CNG-owned, what reproducible config/plugin/module should own this change?
```

References:

- https://docs.expo.dev/workflow/continuous-native-generation/
- https://docs.expo.dev/config-plugins/introduction/

---

## 2. Expo Router authentication and authorization

Prefer declarative route protection over imperative redirect effects.

For current Expo Router projects, use Protected Routes when route-level authentication or authorization is the requirement.

Typical shape:

```tsx
<Stack>
  <Stack.Protected guard={!!session}>
    <Stack.Screen name="(app)" />
  </Stack.Protected>

  <Stack.Protected guard={!session}>
    <Stack.Screen name="sign-in" />
  </Stack.Protected>
</Stack>
```

Route groups remain useful for organizing authenticated and unauthenticated areas.

Do not make this the default auth pattern:

```tsx
useEffect(() => {
  if (!session) {
    router.replace('/sign-in');
  }
}, [session]);
```

Imperative redirects still have valid uses, but authentication guards should not be implemented primarily as screen effects when Protected Routes express the rule directly.

### Protected Routes are not a security boundary

Protected Routes control client-side navigation and UX. They do not replace server-side authentication or authorization.

Every backend/API operation that reads or mutates protected data must independently authenticate the caller and enforce authorization on the server.

Do not infer that hiding or protecting a screen makes its API operations secure.

### Session state and restoration

Model session restoration explicitly instead of collapsing it into a single boolean too early.

A useful state shape is:

```text
restoring
├─ valid identity → authenticated
└─ no/invalid identity → unauthenticated

authenticated
├─ refresh/rotate credentials for same identity → authenticated
├─ expired/revoked credentials → re-authentication required
├─ logout → unauthenticated
└─ account switch → clear previous-user scoped state → authenticate new identity
```

While session restoration is still unknown/loading, do not briefly render protected product UI and redirect afterward.

Refreshing or rotating credentials for the same authenticated identity is not logout and should not clear otherwise valid user state by default.

When credentials are expired or revoked, transition deliberately toward re-authentication and handle any pending user action according to product requirements.

On logout or account switch:

- remove credentials that should no longer remain on the device;
- clear or invalidate user-scoped cached/persisted data where retaining it could expose the previous user's data;
- reset navigation/access state deliberately;
- do not erase unrelated device-local data unless the product requires it.

References:

- https://docs.expo.dev/router/advanced/authentication/
- https://docs.expo.dev/router/advanced/protected/

---

## 3. Route error boundaries

Expected product states and unexpected React render/component-tree failures are different problems.

Examples of expected states:

```text
payment declined
network unavailable
empty result
permission denied
validation error
```

Model these explicitly in product UI.

Use Expo Router route or layout Error Boundaries for unexpected React render/component-tree errors where the recovery scope makes sense.

A route can export:

```tsx
import type { ErrorBoundaryProps } from 'expo-router';

export function ErrorBoundary({ error, retry }: ErrorBoundaryProps) {
  // Render a product-appropriate recovery UI.
}
```

Use the smallest useful boundary:

- route-level when one screen can recover independently;
- layout/navigator-level when a group of routes shares a recovery boundary;
- application-level for truly global render failures.

Error Boundaries are not universal runtime exception handlers. In particular, ordinary event-handler failures and most asynchronous callback/operation failures need explicit handling at the operation boundary.

For example:

```text
React render/component-tree failure
→ Error Boundary

event handler / async operation / network mutation failure
→ explicit try/catch/result handling + product state/reporting as appropriate
```

Do not use Error Boundaries as a substitute for normal loading/error/empty state modeling.

References:

- https://docs.expo.dev/router/error-handling/
- https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary

---

## 4. Edge-to-edge and safe areas

Treat edge-to-edge layout as the modern Android baseline.

System bars, notches, camera cutouts, home indicators, and similar UI are layout inputs, not exceptional device quirks.

Use `react-native-safe-area-context` as the standard source of safe-area information when application content must account for system UI.

Do not blindly wrap every route in a `SafeAreaView`.

Before applying insets, determine which edges are already handled by the navigator or containing layout. Avoid double padding.

Use only the edges the screen owns.

Verify screens that intentionally draw behind system bars as well as screens that require protected content areas.

Reference: https://docs.expo.dev/versions/latest/sdk/safe-area-context/

---

## 5. Keyboard correctness

A form or text-entry flow is not visually verified until it has been exercised with the keyboard open.

For each relevant flow verify:

- focused input remains visible;
- the primary action remains reachable;
- scrollable content can reach obscured fields;
- dismiss behavior is intentional;
- keyboard behavior works on both targeted platforms;
- bottom sheets/modals do not hide important controls;
- safe-area and keyboard offsets do not stack incorrectly.

Start with React Native keyboard APIs and `KeyboardAvoidingView` where they are sufficient.

Consider `react-native-keyboard-controller` when complex scrollable forms, chat/composer UX, synchronized keyboard animation, or repeated keyboard bugs create demonstrated friction.

Do not install advanced keyboard infrastructure preemptively.

Reference: https://docs.expo.dev/guides/keyboard-handling/

---

## 6. OTA updates and native compatibility

These rules apply only when the product adopts EAS Update or another OTA JavaScript update mechanism.

An OTA update can replace compatible JavaScript/assets. It cannot add native code or native capabilities to an already installed binary.

Treat `runtimeVersion` as the compatibility contract between an update and the native binary.

When the native runtime changes, for example because a native library, config plugin output, entitlement, native module, or relevant app configuration changes:

```text
native runtime changed
→ create a new compatible binary
→ ensure OTA updates target the matching runtimeVersion
```

Follow current Expo guidance for runtimeVersion policy instead of freezing a policy choice into the playbook. As of this revision, Expo recommends the `appVersion` policy for its common deployment workflow.

The `fingerprint` policy can make incompatible updates less likely by changing the runtimeVersion when native-impacting inputs change, but current Expo guidance describes it as experimental/not yet widely recommended. Consider it deliberately when that automation materially benefits the project; do not treat it as the universal default.

Before promoting an OTA update to production:

- verify it on a preview/staging build that uses the same compatible runtime;
- verify important migrations and startup behavior;
- know the rollback path;
- use a gradual rollout when product risk justifies it.

Do not publish one JavaScript update indiscriminately to binaries with incompatible native runtimes.

Do not treat OTA as a substitute for App Store / Play Store builds when native code must change.

References:

- https://docs.expo.dev/eas-update/deployment/
- https://docs.expo.dev/eas-update/runtime-versions/
- https://docs.expo.dev/build/updates/

---

## 7. Application lifecycle

Mobile processes are interruptible. A feature that works only while one screen stays mounted is not necessarily reliable product behavior.

When a workflow owns long-lived, persisted, authentication-sensitive, payment-sensitive, upload, queue, or mutation state, deliberately consider:

```text
cold start
foreground → background
background → foreground
process termination
session expiration while inactive
network loss / retry
interrupted mutation
```

Do not persist every transient UI state merely to survive process death. Persist only product state that is genuinely valuable or required to recover.

For retryable mutations, define idempotency/deduplication behavior where duplicate execution would matter.

For drafts, uploads, queues, and other resumable workflows, define whether the product should restore, restart, discard, or ask the user after interruption.

Use `AppState` or another appropriate platform mechanism only when actual application-lifecycle-aware behavior is required; do not add lifecycle infrastructure preemptively.

Reference: https://reactnative.dev/docs/appstate

---

## 8. Platform verification rule

For native-facing changes, verification means more than TypeScript and Metro.

Use this sequence:

```text
identify native ownership model
    ↓
implement reproducibly
    ↓
rebuild the correct runtime when required
    ↓
exercise the real flow
    ↓
inspect platform behavior
```

When a config plugin, native dependency, entitlement, permission, native module, or runtimeVersion-relevant native change occurs, rebuild the appropriate development/preview/production-like client before declaring the behavior verified.

---

## 9. Troubleshooting local physical-device runs

Treat a local physical-device run as a stack of independent layers:

```text
device discovery
    ↓
native toolchain
    ↓
native build
    ↓
signing / trust
    ↓
install / launch
    ↓
JavaScript delivery
    ↓
feature verification
```

Success at one layer does not prove the next layer works.

### Diagnose the failing layer first

Use this order before changing application code:

```text
Can the host see the device?
↓
Can the native project build?
↓
Is the app correctly signed and trusted?
↓
Can it install?
↓
Can the OS launch it?
↓
Can the development binary reach Metro?
↓
Does the actual feature work?
```

Do not debug camera code because Metro is unreachable, and do not debug Metro because signing failed.

### Device discovery

For Apple devices, prefer structured CoreDevice / `devicectl` data when available.

For automation, use stable hardware identifiers such as the device UDID rather than relying on display names.

Do not use loose substring checks for device state. For example, `unavailable` contains `available`.

For Android, distinguish physical devices from emulators explicitly when checking `adb` output.

### Native transport is not JavaScript transport

A device can be connected well enough for native operations such as:

- Xcode build targeting;
- code signing;
- installation;
- application launch;

while still being unable to load the JavaScript development bundle.

Do not assume:

```text
USB / CoreDevice / adb connection
=
Metro connectivity
```

For a development build, verify how the device reaches Metro: same LAN, tunnel, explicit forwarding, or another supported transport.

### Metro is part of the development runtime

Metro is the React Native JavaScript bundler/development server. It is not a test runner.

Typical development flow:

```text
TypeScript / JavaScript / assets
        ↓
      Metro
        ↓
development native binary
        ↓
physical device
```

A debug/development binary can build, sign, install, and launch successfully while still showing a splash screen or an error such as `No script URL provided` if Metro is stopped or unreachable.

Production-like binaries should not depend on a developer's Metro server; their JavaScript bundle is packaged according to the chosen release workflow.

### Signing, trust, and Developer Mode

A successful compile does not prove the OS will launch the installed app.

When iOS reports an invalid code signature, inadequate entitlements, or that a development profile has not been explicitly trusted, check the signing identity, provisioning profile, Developer Mode, and developer trust on the device before changing application code.

### CNG hygiene during local native debugging

For CNG-owned projects:

- run Prebuild from a clean worktree when practical;
- inspect `git status` / `git diff` after Prebuild;
- do not accidentally commit generated native output;
- do not keep incidental changes to project scripts/configuration unless they are intentional;
- keep `app.json` / `app.config.ts` and config plugins as the durable source of truth.

Prebuild can modify files outside `ios/` or `android/`; review those changes instead of assuming they are desired.

### Host native-toolchain drift

Local native development also depends on host tooling such as:

```text
Xcode / platform SDK
signing identity
provisioning / device trust
Ruby / CocoaPods
JDK / Gradle
platform command-line tools
```

Do not assume the globally newest installed tool or gem is compatible with the project.

When host-tool drift causes a real reproducibility problem, prefer a scoped project-owned version requirement, compatibility check, or wrapper over undocumented machine state.

Avoid mutating the entire developer environment merely to make one project build when a local reproducible solution is available.

### Symptom-oriented checks

Common symptoms map to different layers:

```text
"No device UDID or name matching ..."
→ verify device discovery and target by stable identifier

"invalid code signature" / "profile has not been explicitly trusted"
→ verify signing, Developer Mode, and device trust

CocoaPods/Ruby gem activation or keyword errors
→ inspect host-tool dependency compatibility before changing app code

"No script URL provided"
→ native binary launched, but JavaScript delivery is unavailable

app remains on development splash
→ verify Metro is running and reachable before debugging the screen itself
```

### Physical-device verification for native configuration

When product-specific native configuration matters, verify it in a development or production-like build rather than only Expo Go.

Examples include:

- permission descriptions;
- config-plugin output;
- entitlements;
- application identifiers;
- native modules;
- hardware-dependent behavior.

For permissions, verify both the configured native permission text and the actual runtime behavior on a physical device.

References:

- https://docs.expo.dev/workflow/continuous-native-generation/
- https://docs.expo.dev/more/expo-cli/
- https://reactnative.dev/docs/environment-setup
