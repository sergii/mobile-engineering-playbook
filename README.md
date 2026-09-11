# Mobile Engineering Playbook

A reusable engineering playbook for building modern mobile applications with React Native, Expo, TypeScript, and AI coding agents such as Codex.

The playbook is intentionally **architecture-first and UI-library-neutral**. It favors React Native and Expo primitives, vertical slices, real-device or simulator verification, and adding complexity only when a concrete problem justifies it.

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

- [`MOBILE_ENGINEERING_PLAYBOOK.md`](./MOBILE_ENGINEERING_PLAYBOOK.md) - the canonical engineering guide.
- [`AGENTS.md`](./AGENTS.md) - compact operating instructions for Codex and other coding agents.
- [`guides/decision-ladder.md`](./guides/decision-ladder.md) - when to introduce libraries and architectural abstractions.

## How to use it

Use this repository as shared baseline context when starting or working on a React Native / Expo project.

For an AI coding session, provide this repository as context and instruct the agent to follow `AGENTS.md` and `MOBILE_ENGINEERING_PLAYBOOK.md` unless the target project contains a more specific documented rule.

Project-specific requirements always take precedence over generic examples in this playbook.

## Philosophy

- Expo-first.
- React Native primitives first.
- `StyleSheet` first.
- Domain components over generic UI abstractions.
- Vertical slices over horizontal infrastructure projects.
- Working behavior before visual sophistication.
- Deterministic tests for known behavior.
- Agent/device inspection for exploratory and visual verification.
- Dependencies must solve an existing, identifiable problem.
- Complexity must be earned.
