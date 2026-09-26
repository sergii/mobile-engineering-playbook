# Agent Bootstrap Contract - React Native Production Starter

Use this when asking an implementation agent to create or refresh the executable starter.

## Required inputs

- Production Starter specification: `guides/production-starter.md`
- Stack Snapshot guide: `guides/stack-snapshots.md`
- Exact snapshot: `snapshots/<snapshot-id>.yaml`
- Product/local `AGENTS.md`
- Expo's generated/versioned agent context for the chosen SDK

## Prompt template

```text
Build the React Native / Expo starter from the repository's Production Starter specification.

Use the exact stack snapshot referenced by this task as the compatibility baseline.

Rules:
1. Do not replace snapshot versions with "latest".
2. Treat Expo SDK as the compatibility anchor. Do not independently upgrade React or React Native.
3. Scaffold from the exact create-expo and expo-template-default versions in the snapshot.
4. After generation, preserve the generated package.json and lockfile as evidence.
5. Install Expo/native dependencies with `npx expo install` unless the snapshot explicitly says otherwise.
6. Run `npx expo install --check` and `npx expo-doctor@latest`.
7. Use CNG/Prebuild as the native source-of-truth model unless the snapshot says otherwise.
8. Use a development build for native verification. Expo Go is not sufficient proof for native configuration.
9. Do not install optional libraries simply because they appear in the production checklist. Follow Implement now / Scaffold now / Document now.
10. For any dependency not named in the snapshot, explain the concrete problem it solves and verify Expo/RN New Architecture compatibility before adding it.
11. Keep agent/framework guidance in project context files; do not paste the entire shared playbook into generated source code.
12. Record proof results against the snapshot verification matrix.
13. Do not mark the snapshot verified until every required proof for that phase has actually run.
14. If current upstream documentation conflicts with the snapshot, stop and report the conflict instead of silently changing versions.

Expected output:
- runnable project;
- committed lockfile;
- exact tool/package versions;
- compatibility-check output;
- test/build evidence;
- list of snapshot deviations, ideally empty;
- updated verification state only for proofs actually executed.
```

## Why this is deliberately small

The agent does not need a huge one-shot prompt. The snapshot supplies exact compatibility state. The playbook supplies engineering rules. Expo's SDK-specific agent context/skills supply current framework knowledge.

That separation keeps prompts short while making the result more reproducible.
