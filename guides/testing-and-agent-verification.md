# Testing and Agent Verification Strategy

Status: **baseline for the production starter**

The starter separates deterministic regression coverage from agentic exploration and debugging.

> Agents discover and verify. Deterministic tests protect. Evidence proves.

## Default testing stack

### Unit

Use **Jest + `jest-expo`** for pure logic and modules that do not require a real device.

### Component / integration

Use **React Native Testing Library** for component behavior and integration tests.

Prefer behavior-oriented assertions over implementation details.

### Router integration

When routing behavior can be tested below the full device level, use the Expo Router testing utilities appropriate to the installed SDK.

Do not replace real deep-link cold-start E2E coverage with router-only tests.

### Deterministic E2E

Use **Maestro** as the default cross-platform deterministic E2E layer.

Reasons:

- simple declarative flows;
- black-box behavior that is relatively independent of React Native internals;
- works with iOS and Android;
- fits Expo/EAS workflows;
- can capture screenshots and recordings;
- easy to keep a small set of critical flows readable;
- open-source CLI.

The starter should keep Maestro coverage intentionally small and high-value.

Examples:

- cold launch;
- sign in / session restoration;
- deep link;
- primary product action;
- logout / account switch;
- upgrade migration;
- minimum-version/update behavior when applicable.

### Agentic verification

Use **agent-device** as the primary agent-facing device automation and evidence layer.

Its job is not to replace the deterministic suite. It gives coding agents a live feedback loop:

```text
open app
-> inspect accessibility snapshot
-> act through refs/selectors
-> re-inspect
-> capture screenshot/video/logs/traces when useful
-> verify the actual change
```

Prefer accessibility snapshots, semantic selectors, and test IDs over pixel-coordinate guessing.

A successful exploratory flow may be saved as a replayable `.ad` script or exported/promoted into a strict Maestro flow when it deserves durable regression coverage.

### iOS native deep debugging

Use **XcodeBuildMCP** only when a task requires deeper iOS-native build/test/debug control.

Examples:

- signing;
- native build failures;
- Xcode-specific diagnostics;
- simulator/device launch issues;
- LLDB/native debugging.

It is an optional engineering tool, not a required application dependency.

## Alternatives

### Detox

Detox remains a strong React Native gray-box E2E framework, especially when synchronization with React Native internals materially reduces flakiness.

Do not make it the default starter framework while the selected Expo/React Native snapshot is outside Detox's clearly documented tested support envelope. Re-evaluate per snapshot.

### Appium

Use Appium when the product needs the WebDriver ecosystem, broad device-farm support, hybrid/WebView testing, cross-language QA tooling, or enterprise-scale device matrices.

It is intentionally not part of the default starter because the operational surface is larger than we need for a small critical E2E suite.

### Native platform tests

XCUITest and Espresso/UiAutomator remain valid escape hatches for platform-specific behavior that cannot be tested reliably at the cross-platform layer.

Do not create parallel native suites by default.

### mobile-mcp / DeviceKit and other agent drivers

Track emerging agent-oriented mobile-control stacks, including mobile-mcp/DeviceKit and research agent systems, but do not make them the production baseline without compatibility, stability, licensing, and maintenance proof for the selected snapshot.

## Evidence model

A test result and a visual/runtime proof are different artifacts.

For meaningful mobile changes, prefer collecting only evidence that adds review value:

- deterministic assertion result;
- screenshot of the final state;
- video for interaction/timing bugs;
- logs for runtime failures;
- trace/performance sample for performance regressions;
- crash context when relevant.

Do not collect large recordings or traces by default when a small assertion/screenshot proves the change.

## Promotion path

A useful operating loop is:

```text
agent explores with agent-device
        ↓
agent finds a reliable workflow
        ↓
workflow is replayed
        ↓
if it protects an important invariant
        ↓
promote to deterministic regression coverage
        ↓
Maestro / unit / component test owns the contract
```

The durable test suite should not require an LLM to decide each action at runtime.

## CI boundary

Use GitHub Actions for cheap repository checks and policy gates.

Use EAS-native capabilities when they materially simplify native build, internal distribution, OTA, simulator/emulator execution, or mobile E2E.

The starter should not force all CI into one provider.

## Accessibility implication

Because both deterministic UI automation and agent-device benefit from semantic UI, accessibility metadata is part of testability.

Provide clear labels, roles, and stable test IDs where semantics alone are insufficient.

Do not add test IDs to every element by habit. Add them where they provide a stable automation contract.

## Reference implementation tracking

Track **Ignite** as a reference implementation, not as the starter base.

For each major snapshot, compare at least:

- Expo / React Native / React versions;
- navigation choice;
- state and persistence defaults;
- API/server-state approach;
- testing stack;
- i18n/theme/keyboard handling;
- generators and developer tooling;
- EAS/release integration;
- agent/device verification tooling.

Adopt ideas when they solve our goals better. Do not copy dependencies merely because Ignite includes them.

## Upstream references

- https://docs.expo.dev/develop/unit-testing/
- https://docs.expo.dev/eas/workflows/examples/e2e-tests/
- https://github.com/mobile-dev-inc/Maestro
- https://github.com/callstack/agent-device
- https://github.com/infinitered/ignite
- https://docs.infinite.red/ignite-cli/
