# Mobile Engineering Playbook

A reusable engineering baseline for building modern mobile applications with React Native, Expo, TypeScript, and AI coding agents.

The playbook is intentionally **principle-first and UI-library-neutral**. It favors platform primitives, vertical slices, real simulator/device verification, and adding complexity only when a concrete product problem justifies it.

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

## Three context layers

This repository separates universal engineering guidance from product-shaped examples:

```text
LEVEL 1  Mobile Engineering Playbook
         universal defaults and decision rules
                     ↓
LEVEL 2  Application Archetype
         optional context for a class of products
                     ↓
LEVEL 3  Product Rules
         the actual application's domain and constraints
```

An archetype is **not a template to copy**. It describes common forces, likely states, architectural biases, and useful vertical slices for a class of applications.

Project-specific documented requirements always override an archetype, and archetypes never override the core engineering safety rules.

## Documents

- [`MOBILE_ENGINEERING_PLAYBOOK.md`](./MOBILE_ENGINEERING_PLAYBOOK.md) - canonical engineering principles and defaults.
- [`AGENTS.md`](./AGENTS.md) - compact operating contract for coding agents.
- [`guides/decision-ladder.md`](./guides/decision-ladder.md) - when to introduce common tools and abstractions.
- [`guides/agent-failure-modes.md`](./guides/agent-failure-modes.md) - common AI coding mistakes in React Native / Expo projects.
- [`guides/platform-native-rules.md`](./guides/platform-native-rules.md) - CNG/Prebuild ownership, Expo Router auth/error boundaries, safe areas, edge-to-edge, and keyboard correctness.
- [`guides/vertical-slice-checklist.md`](./guides/vertical-slice-checklist.md) - compact Definition of Done for a vertical slice.
- [`archetypes/`](./archetypes/) - optional product-class context and examples.
- [`integrations/`](./integrations/) - thin adapters for Codex, Claude Code, Cursor, and GitHub Copilot.

## Archetypes

Current archetypes:

- [`camera-operational`](./archetypes/camera-operational/) - camera-first operational workflows such as inventory, field service, logistics, asset handling, or similar physical-world tasks.

More archetypes can be added later, for example e-commerce, marketplace, messaging, content consumption, field service, fintech, or real-time tracking.

Do not add an archetype merely to create a taxonomy. Add one when repeated product forces justify reusable guidance.

## Platform baseline highlights

The current playbook explicitly covers several mobile/agent failure points that should not remain implicit:

- New Architecture as the modern Expo baseline;
- Expo Go vs development builds;
- CNG/Prebuild vs manually owned native projects;
- config plugins and reproducible native configuration;
- Expo Router Protected Routes for route-level access control;
- route/layout Error Boundaries for unexpected runtime failures;
- storage selection by sensitivity and data shape;
- edge-to-edge and safe-area ownership;
- keyboard-open verification for text-entry flows.

## How to use it

For a new product repository:

1. Put a small agent-instruction file in the product repository.
2. Reference this playbook as the shared engineering baseline.
3. Explicitly reference an archetype only when it is useful for that product.
4. Add product-specific domain rules, terminology, workflows, and constraints locally.
5. Let project-specific rules override generic examples.
6. Do not vendor the whole playbook into every project unless there is a concrete offline or governance reason.

Use the adapter examples under `integrations/` as starting points:

```text
Codex          → AGENTS.md
Claude Code    → CLAUDE.md
Cursor         → .cursor/rules/*.mdc
GitHub Copilot → .github/copilot-instructions.md
```

The adapters are intentionally thin. They point to the shared playbook rather than duplicating it.

## Example policy

Core documents use cross-domain or abstract examples on purpose. Domain-heavy examples belong in `archetypes/`.

Examples are illustrative, not normative. A scanner, checkout, inbox, map, or media flow demonstrates a principle but does not define the default architecture for every mobile application.

## Philosophy

- Expo-first.
- React Native primitives first.
- `StyleSheet` first.
- Domain concepts over framework-shaped abstractions.
- Vertical slices over horizontal infrastructure projects.
- Working behavior before visual sophistication.
- Deterministic tests for known behavior.
- Device inspection for exploratory and visual verification.
- Dependencies must solve an existing, identifiable problem.
- Native changes must respect the project's source-of-truth model.
- Archetypes guide context; they do not dictate architecture.
- Complexity must be earned.

## License

MIT. See [`LICENSE`](./LICENSE).
