# Product mobile engineering instructions

Use the Mobile Engineering Playbook as the shared React Native / Expo engineering baseline.

Playbook repository: https://github.com/sergii/mobile-engineering-playbook
Playbook version: v0.4.3
Playbook revision: ec47e930b948fd67918bb0e812d704f1cf32362e

The exact revision is the reproducible baseline. The version is the human-readable release label.

Do not silently follow a newer `main` revision. Upgrade the product's adopted playbook baseline deliberately after reviewing shared-rule changes.

If a local checkout of the playbook is available, prefer the checkout at the recorded revision to avoid unnecessary network retrieval.

Because a commit cannot contain its own final SHA, this template may pin the release-content commit immediately before a template-only self-pin commit. The recorded revision must contain the shared rules represented by the declared version.

## Product context

Archetype: none
Target platforms: iOS, Android
Native ownership: CNG / Prebuild

<!--
If this product explicitly uses an archetype, replace `none` with its id, for example:
Archetype: camera-operational
Do not select an archetype only because the app looks similar.

Update Target platforms and Native ownership if this product deliberately differs.
-->

## Product commands

<!-- Replace these with the repository's real commands. Do not invent commands that do not exist. -->

```text
install: <command>
start: <command>
typecheck: <command>
lint: <command or none>
test: <command or none>
ios: <command or none>
android: <command or none>
```

## Always follow

- the product-specific rules in this repository;
- the current task requirements;
- the shared playbook's core safety rules at the recorded `Playbook revision`.

Product-specific documented rules override generic examples in the shared playbook.

Keep developer/global agent configuration, the repository engineering contract, and product/domain rules separate. Do not persist developer-local paths, personal hooks, local MCP/tool configuration, machine-specific aliases/binaries, or user-global preferences into repository instructions, configuration, or documentation unless the project explicitly adopts them as repository-owned behavior. Document portable project requirements instead of one developer's implementation path.

## Load shared guidance only when relevant

- architecture/startup/unfamiliar mobile decision → `MOBILE_ENGINEERING_PLAYBOOK.md`
- new or cross-cutting dependency → `guides/decision-ladder.md`
- suspicious generated/overbuilt code → `guides/agent-failure-modes.md`
- native config, auth routing/security boundaries, storage, safe areas, keyboard, OTA/native compatibility, application-lifecycle/interruption behavior, `ios/` or `android/` → `guides/platform-native-rules.md`
- finishing a meaningful feature slice → `guides/vertical-slice-checklist.md`
- explicitly selected product archetype → that archetype's README/YAML/slices when relevant

Read shared documents at the recorded `Playbook revision`; do not silently substitute the current `main` branch.

Do not load every shared document for every small task.

## Product-specific rules

Add only product-owned guidance below, for example:

- domain terminology;
- core user workflows;
- important entities and invariants;
- API contracts and server-side authorization expectations;
- session/logout/account-switch/user-data semantics;
- offline/consistency requirements;
- application-lifecycle/interruption behavior;
- OTA/release policy if used;
- interaction principles;
- supported platforms/devices;
- security or regulatory constraints;
- deliberate deviations from the shared playbook.

<!-- Add product-specific rules here. -->
