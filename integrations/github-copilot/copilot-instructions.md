# Mobile engineering instructions

Use the Mobile Engineering Playbook as the shared React Native / Expo engineering baseline:

https://github.com/sergii/mobile-engineering-playbook

Always read the product repository's local instructions and task-relevant domain documentation first.

Load shared guidance only when relevant:

- architecture/startup/unfamiliar mobile decision → `MOBILE_ENGINEERING_PLAYBOOK.md`
- adding/adopting a dependency → `guides/decision-ladder.md`
- suspicious generated or overbuilt code → `guides/agent-failure-modes.md`
- native config, auth routing, storage, safe areas, keyboard, `ios/` or `android/` → `guides/platform-native-rules.md`
- finishing a meaningful feature slice → `guides/vertical-slice-checklist.md`
- explicitly selected archetype → read it only when relevant

Product-specific rules override generic examples. Do not silently select an archetype and do not load every shared guide for every task.

Repository defaults:

- Expo-first, React Native primitives first, `StyleSheet` first;
- strict TypeScript;
- vertical slices before broad infrastructure;
- dependencies only for demonstrated current problems;
- do not infer API adoption from transitive dependencies;
- determine CNG/native-project ownership before persistent native edits;
- keep authentication, credential storage, route protection, profile/server data, and application state separate;
- prefer declarative Expo Router route protection;
- verify significant UI/native changes in the running simulator/device;
- add deterministic regression coverage to critical flows when justified.

Keep this file short. Keep product-specific terminology, workflows, and architecture decisions in the product repository and shared engineering guidance in the canonical playbook.
