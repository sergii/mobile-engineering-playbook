# Mobile engineering instructions

Use the Mobile Engineering Playbook as the shared React Native / Expo baseline:

https://github.com/sergii/mobile-engineering-playbook

Resolve the adopted `Playbook revision` from the product repository's local engineering contract. Read shared playbook documents at that revision; do not silently substitute a newer `main` baseline.

Always read the product repository's local instructions and task-relevant domain documentation first.

Load shared guidance only when relevant:

- architecture/startup/unfamiliar mobile decision → `MOBILE_ENGINEERING_PLAYBOOK.md`
- new/cross-cutting dependency → `guides/decision-ladder.md`
- suspicious generated or overbuilt code → `guides/agent-failure-modes.md`
- native config, auth/security boundaries, storage, safe areas, keyboard, OTA/native compatibility, application-lifecycle/interruption behavior, `ios/` or `android/` → `guides/platform-native-rules.md`
- feature completion → `guides/vertical-slice-checklist.md`
- explicitly selected archetype → read it only when relevant to the current task

Product-specific rules override generic examples. Do not silently select an archetype and do not load every shared document for every task.

Default behavior:

- Expo-first and current stable SDK;
- React Native primitives and `StyleSheet` first;
- strict TypeScript;
- vertical slices before broad infrastructure;
- dependencies only for demonstrated problems;
- declarative Expo Router route protection;
- client-side route protection is not backend authorization;
- correct CNG/native-project ownership before native edits;
- separate authentication, credential storage, route protection, server authorization, profile data, and application state;
- real simulator/device verification for meaningful UI/native behavior;
- application-lifecycle/interruption verification when the workflow depends on it;
- deterministic tests for critical regression paths when justified.

Keep product-specific domain rules here or elsewhere in the product repository. Do not duplicate the full shared playbook.
