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
- [`guides/vertical-slice-checklist.md`](./guides/vertical-slice-checklist.md) - compact Definition of Done for a vertical slice.
- [`archetypes/`](./archetypes/) - optional product-class context and examples.

## Archetypes

Current archetypes:

- [`camera-operational`](./archetypes/camera-operational/) - camera-first operational workflows such as inventory, field service, logistics, asset handling, or similar physical-world tasks.

More archetypes can be added later, for example e-commerce, marketplace, messaging, content consumption, field service, fintech, or real-time tracking.

Do not add an archetype merely to create a taxonomy. Add one when repeated product forces justify reusable guidance.

## How to use it

For a new product repository:

1. Put a small `AGENTS.md` in the product repository.
2. Reference this playbook as the shared engineering baseline.
3. Explicitly reference an archetype only when it is useful for that product.
4. Add product-specific domain rules, terminology, workflows, and constraints locally.
5. Let project-specific rules override generic examples.
6. Do not vendor the whole playbook into every project unless there is a concrete offline or governance reason.

For other agent environments, map the same operating rules into the local mechanism, such as `CLAUDE.md`, Cursor rules, or repository-level agent instructions.

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
- Archetypes guide context; they do not dictate architecture.
- Complexity must be earned.

## License

MIT. See [`LICENSE`](./LICENSE).
