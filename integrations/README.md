# Agent Integrations

These files are thin adapters for popular coding-agent environments.

They are intentionally small. The canonical guidance remains in the shared playbook and its focused guides.

Do not copy the full playbook into every agent-specific file. That creates drift and wastes context.

## Precedence

```text
shared playbook at the product-pinned revision
    ↓
explicitly selected archetype
    ↓
product repository instructions
    ↓
current task
```

Product-specific rules override generic examples.

## Adopted revision

The product repository owns the adopted playbook baseline.

Its local contract should record at least:

```text
Playbook repository: https://github.com/sergii/mobile-engineering-playbook
Playbook version: <human-readable version>
Playbook revision: <stable tag or exact commit SHA>
```

Adapters do not independently choose or upgrade that revision.

When loading shared guidance, read it at the product's recorded `Playbook revision`. Do not silently substitute a newer `main` revision.

If an adapter file is the product's root/local contract, add or preserve those baseline fields there. Otherwise, resolve them from the product's root `AGENTS.md` or equivalent local engineering contract.

## Context loading

Adapters should always load the product's local instructions and task-relevant domain context.

Shared documents are loaded conditionally:

- architecture/startup/unfamiliar mobile decision → core playbook;
- dependency choice → decision ladder;
- suspicious generated code → failure modes;
- native/auth-security/storage/layout/keyboard/OTA/application-lifecycle concern → platform-native rules;
- feature completion → vertical-slice checklist;
- archetype → only when explicitly selected and relevant.

Context budget is a resource. Do not load every guide for every task.

## Available adapters

- `codex/AGENTS.md`
- `claude-code/CLAUDE.md`
- `cursor/mobile.mdc`
- `github-copilot/copilot-instructions.md`

Copy or merge the relevant adapter into the conventional path used by the target tool, while preserving the product's pinned baseline metadata and product-specific guidance.

Antigravity CLI (`agy`) automatically reads workspace-root `AGENTS.md`, so the generic product template can be used directly; no separate Antigravity adapter is required for the shared baseline.

## Generic product template

For a new repository that needs a minimal local contract, start from:

- [`../templates/product/AGENTS.md`](../templates/product/AGENTS.md)

The template records the adopted playbook revision, target platforms, native ownership model, project commands, optional archetype, and local product rules. Keep the product pinned to a deliberate playbook revision rather than silently following a changing `main` baseline.
