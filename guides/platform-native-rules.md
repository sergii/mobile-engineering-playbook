# Platform Native Rules

This guide records platform rules that are easy for coding agents to get wrong in Expo / React Native projects.

Use it together with `MOBILE_ENGINEERING_PLAYBOOK.md` and `guides/decision-ladder.md`.

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

## 2. Expo Router authentication

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

Reference: https://docs.expo.dev/router/advanced/authentication/

---

## 3. Route error boundaries

Expected product states and unexpected runtime failures are different problems.

Examples of expected states:

```text
payment declined
network unavailable
empty result
permission denied
validation error
```

Model these explicitly in product UI.

For unexpected render/runtime failures in Expo Router, use route or layout error boundaries where the recovery scope makes sense.

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
- application-level for truly global failures.

Do not use Error Boundaries as a substitute for normal loading/error/empty state modeling.

Reference: https://docs.expo.dev/router/error-handling/

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

## 6. Platform verification rule

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

When a config plugin, native dependency, entitlement, permission, or native module changes, rebuild the development/production-like client before declaring the behavior verified.
