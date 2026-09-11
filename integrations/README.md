# Agent Integrations

These files are thin adapters for popular coding-agent environments.

They are intentionally small. The canonical guidance remains in the shared playbook and its focused guides.

Do not copy the full playbook into every agent-specific file. That creates drift and wastes context.

## Precedence

```text
shared playbook
    ↓
explicitly selected archetype
    ↓
product repository instructions
    ↓
current task
```

Product-specific rules override generic examples.

## Context loading

Adapters should always load the product's local instructions and task-relevant domain context.

Shared documents are loaded conditionally:

- architecture/startup/unfamiliar mobile decision → core playbook;
- dependency choice → decision ladder;
- suspicious generated code → failure modes;
- native/auth/storage/layout/keyboard concern → platform-native rules;
- feature completion → vertical-slice checklist;
- archetype → only when explicitly selected and relevant.

Context budget is a resource. Do not load every guide for every task.

## Available adapters

- `codex/AGENTS.md`
- `claude-code/CLAUDE.md`
- `cursor/mobile.mdc`
- `github-copilot/copilot-instructions.md`

Copy the relevant adapter into the conventional path used by the target tool, then customize product-specific guidance locally.

Antigravity CLI (`agy`) automatically reads workspace-root `AGENTS.md`, so the generic product template can be used directly; no separate Antigravity adapter is required for the shared baseline.

## Generic product template

For a new repository that needs a minimal local contract, start from:

- [`../templates/product/AGENTS.md`](../templates/product/AGENTS.md)

It demonstrates how to reference the shared playbook, optionally select an archetype, and keep product-owned rules local without copying the full standard.
