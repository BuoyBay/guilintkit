---
title: "GUILintKit API"
summary: "Linter registration, report structure, profiles, and the compiled extension contract."
category: "Reference"
order: 20
keywords: ["GUI lint", "native UI", "accessibility", "layout", "evidence", "project policy"]
symbols: ["GuiLinter", "DefaultGuiLintProfile", "GuiLinter.Register", "GuiLinter.RegisterBuiltinRules", "GuiLinter.RegisteredRuleIds", "GuiLinter.Analyse", "GuiLintRule", "GuiLintContext", "GuiLintContext.Emit", "GuiLintContext.Unverified", "ResolveGuiLintProfile", "GuiGeometryRules", "GuiMeasurementRules", "GuiAccessibilityRulesV2", "GuiPlatformRules", "GuiSplitterRules", "GuiBamaRules", "GuiWindowsGuidelineRules", "GuiGtkGuidelineRules"]
related: ["index.md", "getting-started.md", "typed-api.md"]
---

# GUILintKit API

## Entry module: `guilintkit`

| Symbol | Behavior |
| --- | --- |
| `DefaultGuiLintProfile() As JSONItem` | Returns schema-1 default project guidance and empty rule disable/suppression settings. |
| `GuiLinter()` | Independent rule registry; no builtins are installed implicitly. |
| `RegisterBuiltinRules()` | Registers the supplied collections; call once per registry. |
| `Register(rule As GuiLintRule)` | Explicitly registers compiled rules; Nil, empty IDs, and duplicate IDs throw. |
| `RegisteredRuleIds() As Array Of String` | Returns the registered stable identifiers. |
| `Analyse(document, sourcePath)` | Runs with platform `any`, the default profile, and no preview evidence. |
| `Analyse(document, sourcePath, platform, profile, preview)` | Uses explicit policy and optional source-matched PreviewKit evidence. A Nil document rejects. |

## Library report

The JSON result includes `schema`, `complete`, `path`, snapshot `version`, selected `platform`, registered `rules`, `diagnostics`, `suppressed`, `unverified`, and `compilerChecked`. The command-line wrapper can add its own threshold/evidence summary fields; the library’s unresolved coverage array is named `unverified`.

Findings carry stable rule IDs, severity, source/control identity, message, and basis/evidence. Locations use source byte offsets with one-based line/UTF-16 columns where available. `complete: true` does not certify behavior; `compilerChecked: false` explicitly records that the compiler was not run by the linter.

## Failure recovery and ownership

Invalid shared policy, unknown rule IDs in options/disables/suppressions, and invalid or stale preview evidence throw `InvalidArgumentException`; callers should catch the exception and present its message instead of treating the failed call as a clean report. A rejected policy can be corrected and analysed again using the same registry. A rule extension's own exception propagates; no partial report is returned.

Profile resolution clones the caller's JSON before merging platform overrides. Each analysis creates fresh finding, suppression and coverage arrays. Built-in rules read the supplied layout document; analysis does not compile or rewrite its source. Registered extension objects are retained by the registry, and their context exposes the supplied document and preview directly: extensions must treat these as borrowed read-only inputs and manage their own mutable state. The linter does not isolate extension side effects or make extension failures transactional.

The repository consumer recovery test, `tests/guilint_consumer_recovery_test.bui`, exercises an unknown-rule rejection followed by a successful analysis, platform override isolation, independent finding objects across reports, and preservation of document source using the geometry collection. This is a source-analysis contract test; it does not establish native GUI behavior.

## Extensions: `guilintkit.RuleContext`

`GuiLintRule` declares `RuleIds() As Array Of String` and `Check(context As GuiLintContext)`. Compiled collections execute only after explicit host registration. Unique stable IDs allow settings and suppressions to remain meaningful.

`GuiLintContext` exposes `Document`, source `Path`, `Platform`, resolved `Profile`, optional `Preview`, `Findings`, `Suppressed`, and `Coverage`. Helpers include `ControlPath(node)`, `Hidden(node)`, `Literal(node)`, `Disabled(rule)`, `Emit(rule, node, severity, message, basis)`, and `Unverified(rule, node, reason)`.

Extensions can read their data-only settings from `profile.ruleOptions[ruleId]`; platform overrides merge these entries by rule ID. The owning compiled rule validates its option values. Unknown option rule IDs reject. Emit only conclusions supported by the available source/evidence, and record unmet evidence requirements with `Unverified`.

`guilintkit.Profiles.ResolveGuiLintProfile(profile, platform)` resolves and validates shared/platform policies. Built-in collections are independently importable from their named modules: `GuiGeometryRules`, `GuiMeasurementRules`, `GuiAccessibilityRulesV2`, `GuiPlatformRules`, `GuiSplitterRules`, `GuiBamaRules`, `GuiWindowsGuidelineRules`, and `GuiGtkGuidelineRules`.

## Continue reading

- [Overview](index.md)
- [Getting started](getting-started.md)
- [API reference](api.md)
- [Rules and evidence](rules.md)


[Full typed API: properties, parameters, defaults, and returns](typed-api.md)
