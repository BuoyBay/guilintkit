---
title: "GUILintKit"
summary: "Read-only GUI lint rules over immutable LayoutKit snapshots and source-matched runtime evidence."
category: "Overview"
order: 0
keywords: ["GUI lint", "native UI", "accessibility", "layout", "evidence", "project policy"]
related: ["getting-started.md", "api.md", "typed-api.md"]
---

# GUILintKit

GUILintKit is a reusable native GUI linter built on LayoutKit’s source model and shared relational measurements. Built-in rules inspect source data, project policies, and optional matching PreviewKit observations. Opening a document does not run its source, instantiate its controls, or rewrite its frames.

The library provides `GuiLinter`, independent compiled rule collections, and an explicit extension contract. It also powers the repository’s `buoy-dev gui-lint` command. An IDE can pass an unsaved `LayoutDocument` snapshot directly.

## Findings are evidence-dependent

Reports separate findings, suppressed findings, and unverified checks. `complete` means the lint run finished, not that all behavior was verified. `compilerChecked` is False: linting complements actual compiler diagnostics. A screenshot, a shortcut string, an enabled menu item, or a Default/Cancel declaration alone cannot establish successful interaction or native accessibility associations.

Project spacing and style values are explicit conventions, not universal platform pixel requirements. Intent-dependent suggestions remain advisory. Computed/unsupported geometry, custom coordinate spaces, hidden controls, and source without declarative roots are handled conservatively.

## Integration model

Install GUILintKit with LayoutKit, EditorKit, and the toolchain’s Crypto package. Register built-in rules once and explicitly register trusted compiled extensions. Discovery does not automatically load executable lint plugins from an opened project. No live preview evidence is required for source-only linting; checks requiring it remain unverified.

## Continue reading

- [Overview](index.md)
- [Getting started](getting-started.md)
- [API reference](api.md)
- [Rules and evidence](rules.md)
