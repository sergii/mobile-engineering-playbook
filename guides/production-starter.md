# React Native Production Starter - Day-0 / Day-1 Specification

Status: **documentation-first / implementation planned**

This document defines the production baseline for a future reusable React Native / Expo starter. It is intentionally a specification first. The executable starter should be built later from this contract rather than letting a template accidentally become the source of truth.

The goal is not to implement every possible feature before the first screen. The goal is:

> No predictable production concern should require an architectural reset six months later.

## Provenance legend

The checklist preserves where each idea came from:

- **POST** - explicitly present in the original React Native community post.
- **COMMENTS** - explicitly proposed or materially expanded in the discussion comments.
- **COMMON** - broadly established mobile/software engineering practice independent of that thread.
- **OURS** - an addition or substantial refinement in this playbook.

Multiple labels mean the requirement is supported by more than one source.

## Adoption levels

Every item belongs to one of three adoption levels.

### Implement now

The starter should contain working implementation from its first executable release.

Examples: strict TypeScript, environments, release identity, CI checks, source-map upload, crash reporting, deep-link routing, secure credential boundary, baseline tests.

### Scaffold now

The starter should expose a stable seam, contract, or disabled-by-default module without forcing a product to adopt the feature.

Examples: OTA policy, feature flags, notifications, entitlements, review prompts, update policy, analytics, i18n.

### Document now

The starter should document the contract and decision points, but should not create fake infrastructure before a product needs it.

Examples: account deletion behavior, incident rollback, store compliance, backward-compatible API expectations, support impersonation.

---

# Complete checklist

## A. Build identity and environments

1. **Separate development, staging, and production environments.**  
   Source: POST, COMMON.  
   Use distinct bundle/application IDs and environment-specific endpoints so builds can coexist on one device.

2. **Make build identity explicit.**  
   Source: OURS, COMMON.  
   A running binary should be able to report version, build number, environment, git SHA, OTA revision when applicable, and native runtime version.

3. **Prevent environment drift.**  
   Source: POST, OURS.  
   A production binary must not silently point at staging because of local environment state.

4. **Keep configuration deterministic and reproducible.**  
   Source: COMMON, OURS.  
   CI and local builds should derive equivalent native/application configuration from committed configuration plus controlled secrets.

## B. Language and static correctness

5. **Enable TypeScript strict mode from the first commit.**  
   Source: POST, COMMON.

6. **Run linting, formatting, and type checking in CI.**  
   Source: COMMENTS, COMMON.

7. **Use pre-commit/pre-push hooks only as a convenience layer.**  
   Source: COMMENTS, OURS.  
   Husky/lint-staged may improve local feedback, but CI remains authoritative.

## C. CI/CD and release delivery

8. **Automate builds.**  
   Source: POST, COMMON.  
   A release must not depend on one developer's laptop.

9. **Make CI exercise the real release path.**  
   Source: COMMENTS, OURS.  
   The target pipeline is conceptually:

   ```text
   lint
   -> typecheck
   -> tests
   -> build
   -> sign
   -> upload to TestFlight / Play internal testing
   ```

10. **Manage signing and release credentials outside Git.**  
    Source: COMMENTS, COMMON.

11. **Keep a versioned release runbook.**  
    Source: OURS, COMMON.

12. **Preserve release evidence.**  
    Source: OURS, COMMON.  
    Record what commit, binary, environment, checks, and distribution target were used.

## D. Crash reporting and observability

13. **Install crash reporting from day one.**  
    Source: POST, COMMON.

14. **Upload source maps automatically as part of the release.**  
    Source: POST.

15. **Attach release identity to crashes and telemetry.**  
    Source: OURS, COMMON.

16. **Capture useful breadcrumbs and basic operational signals.**  
    Source: OURS, COMMON.  
    At minimum consider startup failures, navigation failures, failed network calls, and native/JS crashes.

17. **Provide a support/debug information surface.**  
    Source: OURS.  
    A support-safe screen or diagnostic export should expose non-secret build/environment/runtime identifiers needed to diagnose a report.

## E. Navigation and deep links

18. **Design deep linking from the beginning.**  
    Source: POST, COMMENTS.

19. **Define a canonical URL model.**  
    Source: OURS.  
    URLs should reflect durable product concepts rather than arbitrary screen implementation names.

20. **Test deep links from cold start and warm/resumed state.**  
    Source: OURS, COMMON.

21. **Make notifications and external links resolve through the same navigation contract.**  
    Source: OURS.

## F. Server state and client state

22. **Use a server-state abstraction such as TanStack Query when the product has remote server state.**  
    Source: POST, COMMON.

23. **Do not recreate caching, retry, invalidation, deduplication, loading, and refetch behavior repeatedly in effects.**  
    Source: POST, OURS.

24. **Keep server state conceptually separate from local UI/application state.**  
    Source: OURS, COMMON.

25. **Avoid turning Zustand/Redux/MMKV/AsyncStorage into an accidental second API cache.**  
    Source: OURS.

## G. Persistence and migrations

26. **Version persisted state.**  
    Source: COMMENTS, OURS.

27. **Use explicit persisted-state migrations.**  
    Source: COMMENTS, OURS.

28. **Treat on-device persisted data as production data.**  
    Source: OURS, COMMON.

29. **Define recovery behavior for corrupt or unmigratable local state.**  
    Source: OURS.

30. **Keep sensitive credentials out of ordinary application storage.**  
    Source: OURS, COMMON.

## H. Upgrade-path verification

31. **Maintain an upgrade-path smoke/E2E test.**  
    Source: COMMENTS, OURS.

32. **Keep the previous production release available as a test fixture.**  
    Source: COMMENTS, OURS.

33. **Exercise realistic user state before upgrading.**  
    Source: COMMENTS, OURS.

34. **Verify startup, auth/session continuity, storage migrations, deep links, and a critical product flow after upgrade.**  
    Source: COMMENTS, OURS.

35. **Keep clean-install and upgrade tests distinct.**  
    Source: OURS, COMMON.  
    A green clean install does not prove an existing user's upgrade path.

## I. OTA and version compatibility

36. **Configure OTA delivery before the first public release when the product adopts OTA.**  
    Source: POST.

37. **Treat native runtime compatibility as an explicit OTA contract.**  
    Source: OURS.

38. **Use separate OTA channels/environments.**  
    Source: OURS.

39. **Support staged rollout when the delivery system allows it.**  
    Source: OURS, COMMON.

40. **Define OTA rollback before relying on OTA for incident response.**  
    Source: OURS, COMMON.

41. **Model minimum-supported and latest app versions separately.**  
    Source: COMMENTS, OURS.

42. **Support optional-update and required-update flows.**  
    Source: COMMENTS, OURS.

43. **Do not model forced update as one permanent global boolean.**  
    Source: OURS.  
    Prefer version policy/capability compatibility.

44. **Check update/version state at safe lifecycle points such as foreground/resume without unnecessarily blocking first paint.**  
    Source: COMMENTS, OURS.

## J. Authentication and user-data boundaries

45. **Model the authentication lifecycle explicitly.**  
    Source: OURS, COMMON.  
    Include restoration, refresh, expiration, revocation, logout, and account switching.

46. **Store sensitive credentials in secure platform storage.**  
    Source: OURS, COMMON.

47. **Clear or invalidate user-scoped caches on logout/account switch when retention could leak data across identities.**  
    Source: COMMENTS, OURS.

48. **Keep authentication, credential storage, route protection, server authorization, profile data, and application state as separate concerns.**  
    Source: OURS, COMMON.

49. **Plan account deletion from the beginning when the product creates accounts.**  
    Source: COMMENTS.  
    The full feature need not be implemented before account creation exists, but the product contract must not make deletion structurally impossible.

50. **Design support impersonation/account switching with strict user-data isolation if such tooling is ever introduced.**  
    Source: COMMENTS, OURS.

## K. Permissions and notifications

51. **Centralize permission policy and state.**  
    Source: OURS, COMMON.

52. **Do not scatter camera/photo/microphone/location/notification permission prompts arbitrarily across screens.**  
    Source: OURS.

53. **Scaffold push-notification token lifecycle when notifications are a credible product requirement.**  
    Source: COMMENTS, OURS.

54. **Route notification taps through the canonical deep-link/navigation contract.**  
    Source: OURS.

## L. Feature control and incident safety

55. **Provide a feature-flag / remote-config seam.**  
    Source: OURS, COMMON.

56. **Provide kill-switch semantics for risky integrations or flows.**  
    Source: OURS.

57. **Document the mobile incident rollback hierarchy.**  
    Source: OURS, COMMON.  
    Consider feature disablement, OTA rollback, backend compatibility mode, store release, and minimum-version policy.

58. **Require backward-compatible backend API behavior for supported mobile versions.**  
    Source: OURS, COMMON.

59. **Use capability negotiation where version numbers alone are insufficient.**  
    Source: OURS.

## M. Monetization and engagement

60. **Expose an entitlement boundary before premium behavior is spread through the UI.**  
    Source: COMMENTS, OURS.

61. **Do not scatter direct `isPremium` checks throughout the application.**  
    Source: OURS.

62. **Plan purchase/subscription restoration and entitlement reconciliation when using IAP/subscriptions.**  
    Source: COMMENTS, OURS.

63. **Treat the paywall as product-dependent, but keep the architecture ready for entitlements.**  
    Source: COMMENTS, OURS.

64. **Centralize rate/review prompting and trigger it only from deliberate product moments.**  
    Source: COMMENTS, OURS.

## N. Analytics and privacy

65. **Define a stable analytics event taxonomy.**  
    Source: OURS, COMMON.

66. **Separate crash telemetry, product analytics, advertising tracking, and sensitive user content.**  
    Source: OURS, COMMON.

67. **Keep consent/privacy boundaries explicit.**  
    Source: OURS, COMMON.

## O. Network behavior

68. **Centralize network policy.**  
    Source: OURS, COMMON.  
    Define timeouts, retries, cancellation, auth headers, request/correlation IDs, and handling for 401/403/429/5xx.

69. **Define offline and degraded-backend UX.**  
    Source: OURS, COMMON.

70. **Do not let each screen invent its own transport semantics.**  
    Source: OURS.

## P. Application lifecycle

71. **Treat foreground/background/resume/process restart as architectural inputs.**  
    Source: COMMENTS, OURS.

72. **Use lifecycle transitions for safe refresh/update/session behavior rather than relying only on first mount.**  
    Source: COMMENTS, OURS.

73. **Test lifecycle-sensitive workflows explicitly.**  
    Source: OURS, COMMON.

## Q. Real devices and performance

74. **Keep a cheap Android device in the regular test path from the first week.**  
    Source: POST.

75. **Maintain a small real-device matrix.**  
    Source: OURS, COMMON.  
    A reasonable baseline is a slow/cheap Android, a current Android, the oldest supported iPhone class, and a current iPhone class.

76. **Do not treat emulator/simulator success as sufficient performance evidence.**  
    Source: POST, OURS.

## R. Automated verification

77. **Maintain critical-path E2E tests.**  
    Source: OURS, COMMON.

78. **Add regression tests for important fixed bugs.**  
    Source: COMMENTS, COMMON.

79. **Test cold start separately from warm navigation.**  
    Source: OURS.

80. **Test deep-link cold start.**  
    Source: OURS.

81. **Exercise realistic error/loading/permission states.**  
    Source: OURS, COMMON.

## S. Accessibility and internationalization

82. **Treat accessibility as correctness.**  
    Source: OURS, COMMON.

83. **Establish an i18n architecture early when localization is plausible.**  
    Source: COMMENTS, OURS.

84. **Prefer typed translation keys when the chosen i18n stack supports them cleanly.**  
    Source: COMMENTS, OURS.

85. **Avoid hard-coding product copy in a way that makes later localization structurally expensive.**  
    Source: OURS.

## T. Design-system primitives

86. **Define small design tokens rather than a speculative component framework.**  
    Source: OURS, COMMON.

87. **Centralize typography, spacing, semantic colors, radius, and interaction-state primitives.**  
    Source: OURS.

88. **Do not adopt a large UI framework merely because this is a starter.**  
    Source: OURS and existing playbook policy.

## U. Dependency and supply-chain policy

89. **Define who/what updates Expo, React Native, and native dependencies.**  
    Source: OURS, COMMON.

90. **Use automated dependency-update tooling with CI validation where useful.**  
    Source: OURS, COMMON.

91. **Treat native dependency changes as potential native-runtime changes for OTA compatibility.**  
    Source: OURS.

92. **Run secret scanning.**  
    Source: OURS, COMMON.

93. **Keep lockfiles committed and installs reproducible.**  
    Source: COMMON and portfolio CI baseline.

## V. Failure containment and diagnostics

94. **Define React error-boundary strategy for unexpected render/component-tree failures.**  
    Source: OURS, COMMON.

95. **Do not confuse error boundaries with ordinary async/event-handler error handling.**  
    Source: OURS and existing playbook policy.

96. **Define local-database/storage corruption recovery.**  
    Source: OURS.

97. **Provide a development/support reset path that does not require reinstalling the app for every diagnostic case.**  
    Source: OURS.

98. **Keep diagnostic tools support-safe and free of secrets by default.**  
    Source: OURS, COMMON.

## W. Store and product compliance

99. **Plan store-required account deletion when account creation exists.**  
    Source: COMMENTS, OURS.

100. **Keep privacy URLs, permission descriptions, required disclosures, and store metadata in the release checklist.**  
     Source: OURS, COMMON.

101. **Do not discover basic store-compliance requirements for the first time on submission day.**  
     Source: OURS.

---

# Starter implementation model

The future executable starter should not turn all 101 requirements into installed dependencies. It should use three implementation classes.

## Implemented in the template

Expected baseline:

- Expo / React Native with the playbook-supported versions;
- strict TypeScript;
- deterministic environment configuration;
- dev/staging/production application identity;
- lint/typecheck/test commands;
- CI baseline;
- release/build metadata;
- crash reporting adapter with source-map release integration;
- navigation/deep-link contract;
- secure credential storage boundary;
- network-client boundary;
- persisted-state version/migration contract;
- clean-install and upgrade-test harness;
- basic accessibility rules;
- support-safe build diagnostics.

## Scaffolded but optional

Create stable seams with minimal or disabled-by-default implementation:

- OTA configuration and runtime-version policy;
- minimum-version/update policy;
- feature flags and kill switches;
- push notifications;
- analytics;
- i18n;
- entitlement/paywall boundary;
- rate/review trigger;
- remote config;
- lifecycle hooks.

A product should be able to delete an unused scaffold cleanly.

## Documentation-only until justified

Do not fabricate unused systems. Document:

- account deletion contract;
- support impersonation isolation;
- subscription restoration expectations;
- incident rollback procedure;
- store-compliance checklist;
- backend compatibility policy;
- device-matrix expectations;
- capability negotiation.

---

# Planned implementation phases

## Phase 0 - specification

This document is the source of truth. No executable starter is claimed to exist yet.

## Phase 1 - minimal runnable starter

Create a template application that boots on iOS and Android and proves:

- strict TypeScript;
- environment identity;
- CI;
- deterministic build metadata;
- navigation/deep links;
- crash/source-map pipeline;
- baseline unit/integration test commands.

## Phase 2 - release and lifecycle safety

Add:

- TestFlight / Play internal distribution path;
- OTA/runtime compatibility policy;
- minimum-version policy;
- clean-install and upgrade-path E2E;
- previous-release fixture handling;
- lifecycle verification.

## Phase 3 - production seams

Add opt-in seams for:

- feature flags;
- notifications;
- analytics;
- i18n;
- entitlements/IAP;
- review prompts;
- support diagnostics.

## Phase 4 - proof

Create a tiny reference product from the starter and prove:

- clean install;
- upgrade from previous release;
- deep-link cold start;
- logout/account switch isolation;
- OTA-compatible update and rollback path;
- real-device execution on at least one low-end Android and one iPhone;
- automated internal distribution.

Only after these proofs should the starter be called production-ready.

---

# Non-goals

The starter must not:

- install a large UI framework by default;
- install Redux/Zustand/TanStack Query merely because they are popular;
- force paywalls, push notifications, analytics, or OTA on products that do not need them;
- hide native ownership decisions;
- replace product-specific architecture with template architecture;
- make CI green while leaving the actual store-distribution path manual;
- treat clean-install testing as a substitute for upgrade testing.

The starter is a production-safety baseline, not a product framework.
