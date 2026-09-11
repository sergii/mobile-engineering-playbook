# Mobile project instructions

Use the Mobile Engineering Playbook as the shared React Native / Expo engineering baseline:

https://github.com/sergii/mobile-engineering-playbook

When the playbook is available in the session or local workspace, read at minimum:

1. `MOBILE_ENGINEERING_PLAYBOOK.md`
2. `guides/decision-ladder.md`
3. `guides/agent-failure-modes.md`
4. `guides/platform-native-rules.md`
5. any archetype explicitly selected for this product

Then read this product repository's own README, architecture notes, domain documentation, and local agent instructions.

Product-specific documented rules override generic examples in the shared playbook.

Do not silently select an archetype.

Default behavior:

- build vertical slices;
- prefer React Native and Expo primitives;
- add dependencies only for demonstrated problems;
- use strict TypeScript;
- verify meaningful UI in the running mobile runtime;
- determine CNG/native-project ownership before editing `ios/` or `android/`;
- do not treat installed/transitive packages as automatically adopted APIs;
- keep architecture proportional to current product complexity.

Before calling a feature complete, exercise the changed flow and use the playbook's vertical-slice definition of done.
