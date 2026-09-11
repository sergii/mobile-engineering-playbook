# Product mobile engineering instructions

Use the Mobile Engineering Playbook as the shared React Native / Expo engineering baseline:

https://github.com/sergii/mobile-engineering-playbook

## Product context

Archetype: none

<!--
If this product explicitly uses an archetype, replace `none` with its id, for example:
Archetype: camera-operational
Do not select an archetype only because the app looks similar.
-->

## Always follow

- the product-specific rules in this repository;
- the current task requirements;
- the shared playbook's core safety rules.

Product-specific documented rules override generic examples in the shared playbook.

## Load shared guidance only when relevant

- architecture/startup/unfamiliar mobile decision → `MOBILE_ENGINEERING_PLAYBOOK.md`
- new or cross-cutting dependency → `guides/decision-ladder.md`
- suspicious generated/overbuilt code → `guides/agent-failure-modes.md`
- native config, auth routing, storage, safe areas, keyboard, `ios/` or `android/` → `guides/platform-native-rules.md`
- finishing a meaningful feature slice → `guides/vertical-slice-checklist.md`
- explicitly selected product archetype → that archetype's README/YAML/slices when relevant

Do not load every shared document for every small task.

## Product-specific rules

Add only product-owned guidance below, for example:

- domain terminology;
- core user workflows;
- important entities and invariants;
- API contracts;
- offline/consistency requirements;
- interaction principles;
- supported platforms/devices;
- security or regulatory constraints;
- deliberate deviations from the shared playbook.

<!-- Add product-specific rules here. -->
