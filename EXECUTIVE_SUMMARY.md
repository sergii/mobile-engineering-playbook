# Production Starter Executive Summary

## Purpose

Build a reusable React Native / Expo production starter that reduces avoidable production risk without becoming a maximal application framework.

The starter should establish the expensive-to-retrofit foundations early:

- compatible toolchain selection;
- deterministic environments and release identity;
- strict TypeScript and CI;
- crash reporting and source maps;
- navigation/deep-link contracts;
- secure credential and persistence boundaries;
- upgrade-path safety;
- OTA/runtime compatibility;
- real-device verification;
- deterministic E2E coverage;
- agent-driven exploratory verification with reviewable evidence.

The starter does **not** force every product to adopt analytics, push, subscriptions, feature flags, i18n, a global state library, or a UI framework. Those are opt-in seams or documented contracts until justified.

## Architecture model

The starter is governed by five layers:

```text
Production Starter specification
        ↓
Dated compatibility snapshot
        ↓
Executable starter repository
        ↓
Deterministic + agentic verification
        ↓
Proof matrix / release evidence
```

The specification says **what** must be safe.

The snapshot says **which exact compatible versions** are allowed.

The executable starter proves the rules can coexist in a real project.

Deterministic tests protect stable behavior.

Agents explore, debug, and collect evidence.

The proof matrix determines whether a snapshot/starter is actually verified.

## Current baseline

Current candidate stack snapshot:

- `snapshots/expo57-2026-09-26.yaml`
- Expo SDK 57
- React Native 0.86.3
- React 19.2.3
- Node 24 LTS
- CNG / Prebuild ownership
- development builds for native verification

The snapshot is **candidate**, not verified, until the executable starter passes the proof matrix.

## Testing baseline

```text
unit
→ Jest + jest-expo

component/integration
→ React Native Testing Library

router-level
→ Expo Router testing utilities

deterministic mobile E2E
→ Maestro

agentic exploration / evidence
→ agent-device

deep iOS-native debugging
→ XcodeBuildMCP when needed
```

Rule:

> Agents discover and verify. Deterministic tests protect. Evidence proves.

## Implementation strategy

The starter is built in ten steps:

1. freeze the current documentation baseline;
2. create the separate executable starter repository;
3. implement Phase 1 minimal runnable starter;
4. promote the compatibility snapshot from candidate to verified only after proof;
5. add the upgrade-path harness and previous-release fixture;
6. implement Phase 2 release/lifecycle safety;
7. add opt-in production recipes/seams;
8. build a tiny reference application and prove the whole path;
9. add developer-experience packaging such as a GitHub template or generator;
10. automate snapshot freshness detection without auto-promoting versions.

## Success condition

The project is not considered production-ready because documentation exists or CI is green.

It is production-ready only when a real generated app can:

- build and launch on iOS and Android;
- pass compatibility checks;
- pass deterministic tests;
- survive a real version upgrade with persisted state;
- follow the intended OTA/runtime rules;
- produce reviewable diagnostics/evidence;
- distribute through the internal release path;
- run on representative real devices.

## Non-goal

This is not "our preferred React Native architecture for every app."

It is a production-safe golden path with explicit escape hatches and evidence.
