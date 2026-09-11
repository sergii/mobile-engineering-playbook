# Mobile engineering instructions

Use the Mobile Engineering Playbook as the shared React Native / Expo engineering baseline:

https://github.com/sergii/mobile-engineering-playbook

When the playbook is available in the workspace or session context, follow:

- `MOBILE_ENGINEERING_PLAYBOOK.md`
- `guides/decision-ladder.md`
- `guides/agent-failure-modes.md`
- `guides/platform-native-rules.md`
- any archetype explicitly selected for this product

Product-specific documented rules override generic examples. Do not silently select an archetype.

Repository defaults:

- Expo-first, React Native primitives first, `StyleSheet` first;
- strict TypeScript;
- vertical slices before broad infrastructure;
- dependencies only for demonstrated current problems;
- do not infer API adoption from transitive dependencies;
- determine whether native projects are CNG-generated or explicitly owned before editing `ios/` / `android/`;
- prefer declarative Expo Router route protection;
- verify significant UI/native changes in the running simulator/device;
- add deterministic regression coverage to critical flows when justified.

Keep this file short. Store product-specific terminology, workflows, and architecture decisions in the product repository, and keep shared engineering guidance in the canonical playbook.
