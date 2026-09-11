# Mobile project instructions

Use the Mobile Engineering Playbook as the shared React Native / Expo baseline:

https://github.com/sergii/mobile-engineering-playbook

Resolve the adopted `Playbook revision` from this product repository's local engineering contract. Read shared playbook documents at that revision; do not silently substitute a newer `main` baseline.

If this root `AGENTS.md` is the product's local contract, preserve or add explicit `Playbook version` and `Playbook revision` fields here.

## Always read

- this product repository's local instructions;
- the product/domain documentation relevant to the current task.

Product-specific documented rules override generic examples in the shared playbook.

## Load shared guidance only when relevant

- architecture/startup/unfamiliar mobile decision → `MOBILE_ENGINEERING_PLAYBOOK.md`
- adding/adopting a dependency → `guides/decision-ladder.md`
- suspicious generated or overbuilt code → `guides/agent-failure-modes.md`
- native config, auth/security boundaries, storage, safe areas, keyboard, OTA/native compatibility, application-lifecycle/interruption behavior, `ios/` or `android/` → `guides/platform-native-rules.md`
- finishing a meaningful feature slice → `guides/vertical-slice-checklist.md`
- explicitly selected archetype → that archetype when relevant

Do not load every shared document for every small task. Do not silently select an archetype.

Default behavior:

- build vertical slices;
- prefer React Native and Expo primitives;
- start with strict TypeScript and `StyleSheet`;
- add dependencies only for demonstrated problems;
- determine CNG/native-project ownership before persistent native edits;
- do not infer API adoption from transitive dependencies;
- keep auth identity, credential storage, route protection, server authorization, profile/server data, and app state as separate concerns;
- never treat client route protection as backend authorization;
- verify meaningful UI/native behavior in the running mobile runtime;
- verify interruption/restart behavior when the workflow depends on it;
- keep architecture proportional to current product complexity.

Before calling a meaningful feature complete, exercise the changed flow and use the playbook's vertical-slice definition of done.
