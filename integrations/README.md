# Agent Integrations

These files are thin adapters for popular coding-agent environments.

They are intentionally small. The canonical rules remain in:

- `MOBILE_ENGINEERING_PLAYBOOK.md`
- `guides/decision-ladder.md`
- `guides/agent-failure-modes.md`
- `guides/platform-native-rules.md`
- any explicitly selected archetype
- the product repository's own documentation

Do not copy the full playbook into every agent-specific file. That creates drift.

## Precedence

Use this mental model:

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

## Available adapters

- `codex/AGENTS.md`
- `claude-code/CLAUDE.md`
- `cursor/mobile.mdc`
- `github-copilot/copilot-instructions.md`

Copy the relevant adapter into the conventional path used by the target tool, then customize the product-specific section locally.

The adapters should point to the playbook rather than duplicate it.
