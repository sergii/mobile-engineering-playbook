# Product mobile engineering instructions

Use the Mobile Engineering Playbook as the shared React Native / Expo engineering baseline.

Playbook repository: https://github.com/sergii/mobile-engineering-playbook
Playbook version: v0.4.2
Playbook revision: 20b7e097bf9b7d8333f2c3ac780be4198fedcdb1

The exact revision is the reproducible baseline. The version is the human-readable release label.

Do not silently follow a newer `main` revision. Upgrade the product's adopted playbook baseline deliberately after reviewing shared-rule changes.

If a local checkout of the playbook is available, prefer the checkout at the recorded revision to avoid unnecessary network retrieval.

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
- the shared playbook's core safety rules.

Product-specific documented rules override generic examples in the shared playbook.

## Load shared guidance only when relevant

- architecture/startup/unfamiliar mobile decision → `MOBILE_ENGINEERING_PLAYBOOK.md`
- new or cross-cutting dependency → `guides/decision-ladder.md`
- suspicious generated/overbuilt code → `guides/agent-failure-modes.md`
- native config, auth routing/security boundaries, storage, safe areas, keyboard, OTA/native compatibility, lifecycle-sensitive behavior, `ios/` or `android/` → `guides/platform-native-rules.md`
- finishing a meaningful feature slice → `guides/vertical-slice-checklist.md`
- explicitly selected product archetype → that archetype's README/YAML/slices when relevant

Do not load every shared document for every small task.

## Product-specific rules

Add only product-owned guidance below, for example:

- domain terminology;
- core user workflows;
- important entities and invariants;
- API contracts and server-side authorization expectations;
- session/logout/user-data semantics;
- offline/consistency requirements;
- lifecycle/interruption behavior;
- OTA/release policy if used;
- interaction principles;
- supported platforms/devices;
- security or regulatory constraints;
- deliberate deviations from the shared playbook.

<!-- Add product-specific rules here. -->
