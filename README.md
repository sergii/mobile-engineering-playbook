# Mobile Engineering Playbook

A reusable engineering baseline for building modern mobile applications with React Native, Expo, TypeScript, and AI coding agents.

Current iteration: **v0.4.2**

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
- [`AGENTS.md`](./AGENTS.md) - compact operating contract and conditional context-loading rules for coding agents.
- [`guides/decision-ladder.md`](./guides/decision-ladder.md) - when to introduce common tools and abstractions, including auth/session, storage, lists, and native boundaries.
- [`guides/agent-failure-modes.md`](./guides/agent-failure-modes.md) - common AI coding mistakes in React Native / Expo projects.
- [`guides/platform-native-rules.md`](./guides/platform-native-rules.md) - CNG/Prebuild ownership, auth/security boundaries, Error Boundaries, safe areas, keyboard behavior, OTA/native compatibility, and lifecycle rules.
- [`guides/vertical-slice-checklist.md`](./guides/vertical-slice-checklist.md) - compact Definition of Done for a vertical slice, including lifecycle and OTA checks when relevant.
- [`archetypes/`](./archetypes/) - optional product-class context and examples.
- [`templates/product/AGENTS.md`](./templates/product/AGENTS.md) - minimal product-repository agent contract.
- [`integrations/`](./integrations/) - thin adapters for Codex, Claude Code, Cursor, GitHub Copilot, and tools that consume root `AGENTS.md`.

## Archetypes

Current archetypes:

- [`camera-operational`](./archetypes/camera-operational/) - camera-first operational workflows such as inventory, field service, logistics, asset handling, or similar physical-world tasks.

Do not add an archetype merely to create a taxonomy. Add one when repeated product forces justify reusable guidance.

`architecture_bias` values in archetype YAML describe likelihood and relative product pressure. They are not priorities, dependency requirements, or install instructions.

## Platform baseline highlights

The playbook explicitly covers mobile/agent failure points that should not remain implicit:

- New Architecture as the modern Expo baseline;
- Expo Go vs development builds;
- CNG/Prebuild vs manually owned native projects;
- config plugins and reproducible native configuration;
- Expo Router Protected Routes for client-side route access, without confusing them with server authorization;
- explicit session restoration, expiration, and logout behavior;
- route/layout Error Boundaries for React render/lifecycle failures, not ordinary async/event-handler errors;
- storage selection by sensitivity and data shape;
- separation of authentication, credential storage, route protection, profile/server data, and application state;
- edge-to-edge and safe-area ownership;
- keyboard-open verification for text-entry flows;
- conditional EAS Update/runtimeVersion compatibility and rollback rules;
- cold-start/background/resume/process-restart verification for lifecycle-sensitive workflows.

## How to use it

For a new product repository:

1. Copy [`templates/product/AGENTS.md`](./templates/product/AGENTS.md) as the starting local contract, or use the adapter for your agent environment.
2. Reference this repository as the shared engineering baseline rather than copying all shared rules into the product.
3. Record the playbook revision adopted by the product. Prefer a stable tag or exact commit, not an implicit moving `main` baseline.
4. Explicitly reference an archetype only when it is genuinely useful for that product.
5. Add product-specific terminology, workflows, invariants, constraints, runtime commands, platform targets, native-ownership model, and deliberate deviations locally.
6. Let product-specific documented rules override generic examples.

Updating a product from one playbook revision to another should be deliberate: review the shared-rule changes, then update the pinned baseline.

Use the adapter examples under `integrations/` when appropriate:

```text
Codex          → AGENTS.md
Claude Code    → CLAUDE.md
Cursor         → .cursor/rules/*.mdc
GitHub Copilot → .github/copilot-instructions.md
Antigravity    → root AGENTS.md
```

## Context-loading policy

Do **not** ask an agent to read the entire playbook before every small change.

Always load the product's local instructions and task-relevant domain context. Load shared guidance when the decision requires it:

```text
architecture/startup/unfamiliar mobile question
→ core playbook

new or cross-cutting dependency
→ decision ladder

suspicious generated/overbuilt code
→ agent failure modes

native config / auth routing / storage / safe area / keyboard / OTA / lifecycle-sensitive behavior
→ platform-native rules

feature completion
→ vertical-slice checklist

archetype
→ only if explicitly selected and relevant
```

Context budget is also a resource. Use the smallest relevant guidance set.

## Reproducible product integration

A product should make the shared baseline discoverable and stable.

Recommended local metadata:

```text
Playbook repository: https://github.com/sergii/mobile-engineering-playbook
Playbook revision: v0.4.2   # or an exact commit SHA
Archetype: none             # or an explicitly selected archetype
```

If the playbook is available as a local sibling checkout, agents may use it to avoid repeated network retrieval. The product should still record the adopted revision and should not silently follow a newer `main` without an intentional upgrade.

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
- Client-side route protection is not server authorization.
- OTA updates must respect native-runtime compatibility.
- Lifecycle-sensitive behavior must survive the interruptions the product promises to support.
- Archetypes guide context; they do not dictate architecture.
- Agent context should also be proportional to the task.
- Complexity must be earned.

## License

MIT. See [`LICENSE`](./LICENSE).
