# Mobile Engineering Playbook

A reusable engineering playbook for building modern mobile applications with React Native, Expo, TypeScript, and AI coding agents.

The playbook is intentionally **principle-first and UI-library-neutral**. It favors React Native and Expo primitives, vertical slices, simulator/device verification, and adding complexity only when a concrete product problem justifies it.

> Product architecture before framework architecture.

## Core idea

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

## Documents

- [`MOBILE_ENGINEERING_PLAYBOOK.md`](./MOBILE_ENGINEERING_PLAYBOOK.md) - canonical engineering principles and defaults.
- [`AGENTS.md`](./AGENTS.md) - compact operating contract for coding agents.
- [`guides/decision-ladder.md`](./guides/decision-ladder.md) - when to introduce tools and abstractions.
- [`guides/agent-failure-modes.md`](./guides/agent-failure-modes.md) - common AI-agent mistakes in React Native projects.
- [`guides/vertical-slice-checklist.md`](./guides/vertical-slice-checklist.md) - practical completion checklist for a feature slice.

## How to use it

For a new or existing product repository:

1. Put a project-specific `AGENTS.md` in the product repository.
2. Reference this playbook as the shared baseline.
3. Add product-specific domain rules, terminology, workflows, and constraints locally.
4. Let project-specific documented rules override this generic baseline.
5. Do not vendor the entire playbook into every repository unless local/offline ownership is required.

For other agent ecosystems, adapt the compact operating rules from `AGENTS.md` into the native instruction mechanism used by that tool, such as `CLAUDE.md`, `.cursor/rules`, or repository-level Copilot instructions. Keep one canonical source and avoid maintaining divergent copies by hand.

The examples intentionally use domain-rich operational scenarios because they expose architectural trade-offs clearly. The same principles apply to CRUD apps, marketplaces, communication products, consumer apps, fintech, media, and other mobile products.

## Philosophy

- Expo-first.
- Current React Native New Architecture baseline.
- React Native primitives first.
- `StyleSheet` first.
- Domain components over speculative generic abstractions.
- Vertical slices over horizontal infrastructure projects.
- Working behavior before visual sophistication.
- Development builds once native runtime configuration matters.
- Deterministic tests for known behavior.
- Device inspection for exploratory and visual verification.
- Dependencies must solve an existing, identifiable problem.
- Complexity must be earned.

## Key references

- Expo documentation: https://docs.expo.dev/
- React Native New Architecture in Expo: https://docs.expo.dev/guides/new-architecture/
- Expo development builds: https://docs.expo.dev/develop/development-builds/introduction/
- Expo Modules API: https://docs.expo.dev/modules/overview/
- Maestro: https://docs.maestro.dev/
- agent-device: https://github.com/callstack/agent-device
- Argent: https://github.com/software-mansion/argent

## License

MIT. See [`LICENSE`](./LICENSE).
