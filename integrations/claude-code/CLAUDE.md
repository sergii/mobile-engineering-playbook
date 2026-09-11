# Mobile engineering instructions

Use the Mobile Engineering Playbook as the shared React Native / Expo baseline:

https://github.com/sergii/mobile-engineering-playbook

When that playbook is available in context, read:

- `MOBILE_ENGINEERING_PLAYBOOK.md`
- `guides/decision-ladder.md`
- `guides/agent-failure-modes.md`
- `guides/platform-native-rules.md`
- any archetype explicitly selected for this product

Then read this product repository's own documentation.

Product-specific rules override generic examples. Do not silently select an archetype.

Follow these defaults unless the product documents a reason to differ:

- Expo-first and current stable SDK;
- React Native primitives and `StyleSheet` first;
- strict TypeScript;
- vertical slices before broad infrastructure;
- dependencies only for demonstrated problems;
- declarative Expo Router patterns for route protection;
- correct CNG/native-project ownership before native edits;
- real simulator/device verification for meaningful UI and native behavior;
- deterministic tests for critical regression paths.

Do not duplicate the entire shared playbook into this file. Keep product-specific domain rules here and leave shared engineering guidance in the canonical playbook.
