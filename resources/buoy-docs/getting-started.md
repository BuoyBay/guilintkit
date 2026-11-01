---
title: "GUILintKit getting started"
summary: "Run built-in rules against a source snapshot without executing its Window."
category: "Guides"
order: 10
keywords: ["GUI lint", "native UI", "accessibility", "layout", "evidence", "project policy"]
related: ["index.md", "api.md", "typed-api.md"]
---

# GUILintKit getting started

Place `guilintkit`, `layoutkit`, `editorkit`, and `crypto` archives in the consumer’s `packages/` directory. Import `guilintkit` for the linter and `layoutkit.LayoutDocument` for snapshots.

This complete example checks a declarative Window containing a tall TextField. The source is data; the Window is never constructed. The default-font height policy is advisory and can be disabled or changed by project policy. The report also retains checks that cannot be verified without stronger evidence.

```buoy
Import guilintkit
Import layoutkit.LayoutDocument

Sub Main()
    Dim source As String = "Window Dialog\n    Frame = (0, 0, 400, 240)\n    Control TextField nameField\n        Frame = (24, 24, 200, 40)\n    End Control\nEnd Window\n"
    Dim snapshot As New LayoutDocument(source, 1)
    Dim linter As New GuiLinter()
    linter.RegisterBuiltinRules()
    Dim report As JSONItem = linter.Analyse(snapshot, "Dialog.bui", "any", DefaultGuiLintProfile(), Nil)
    Print report.ToString()
End Sub
```

## Customize policy deliberately

Use `DefaultGuiLintProfile()` as a starting point. `textFieldMaxHeight: 0` disables the default TextField-height suggestion. Profile `platforms.macos`, `platforms.windows`, and `platforms.linux` override shared values; declared-root Window policies merge by name. Select a platform policy with the five-argument `Analyse` overload. `any` makes no claim of a native backend observation.

For a known exception, use a specific rule/control suppression with a nonempty reason. Unknown profile fields and unknown rule IDs reject rather than silently doing nothing. Keep suppressed findings in review output.

## Add runtime observations

Pass a successful PreviewKit report only when its recorded input hash matches the exact snapshot source. A requested platform must match its backend; `any` can take the evidence backend. This validates the inspected source, not every dependency or application service.

The current runner does not automatically generate interaction probes. Native Return/Escape, Copy/Paste effects, splitter movement, large-text/high-contrast behavior, and native accessibility-provider associations need explicit trusted observations. Missing evidence remains unverified.

## Continue reading

- [Overview](index.md)
- [Getting started](getting-started.md)
- [API reference](api.md)
- [Rules and evidence](rules.md)
