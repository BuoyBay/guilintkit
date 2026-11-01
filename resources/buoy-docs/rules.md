---
title: "GUILintKit rules and evidence"
summary: "Built-in rule families, platform policy, suppression, and the boundary between observations and verified behavior."
category: "Reference"
order: 30
keywords: ["GUI lint", "native UI", "accessibility", "layout", "evidence", "project policy"]
related: ["index.md", "getting-started.md", "api.md", "typed-api.md"]
---

# GUILintKit rules and evidence

## Built-in families

| IDs | Review scope |
| --- | --- |
| GUI001–002 | Empty/negative visible frames and possible parent clipping. |
| GUI101 | Advisory default-font single-line TextField-height guidance. |
| GUI102–105 | Alignment, margin, spacing/distribution, and centering near misses. |
| GUI201 | Resize/minimum-size policy where no visible binding is found. |
| GUI301–306 | Names, icon actions, focus/hidden conflicts, keyboard opt-outs, hints, and roles. |
| GUI501–503 | Dialog default/cancel expectations and competing declarations. |
| GUI601–603 | Expected menu bars, stable command tags, and explicitly localized native titles. |
| GUI701–702 | Failed keyboard/command behaviors from matching interaction evidence. |
| GUI801–804 | Splitter structure, resize bindings, limits, and resize probes. |
| BAMA001–003 | Opt-in macOS BAMA grid/font/wording conventions. |
| WIN001–003 | Windows fixed-color review, static-label tab stops, and Command modifiers. |
| GTK001–003 | Linux menu modifiers, splitter keyboard probes, and accessibility modes. |

The builtins are guidance with explicit applicability and exceptions. Multiline fields and custom fonts are exempt from the default-font height suggestion. Splitter children belong to distinct panes, and collapsed panes are not treated as empty controls sharing one coordinate space.

## Policies and exceptions

`windows` policies are keyed by declared root name. `purpose: "dialog"` requests default/cancel review; explicitly set the corresponding expectation False when deliberately absent. `resizable: false` records fixed-size intent. Expected menu commands use application tags, not guessed English captions. Native top-level titles are explicitly localized and require a complete available native observation.

`disabledRules` turns off named rules. Specific `ignore` entries require `rule`, the full `control` path, and a nonempty `reason`; suppressed findings remain in the result. BAMA checks are opt-in conventions rather than built-in macOS requirements. Project margins and spacing are not universal Windows, Linux, or macOS pixel tables.

## Runtime evidence boundaries

PreviewKit provides runtime bounds, Buoy accessibility properties, menu-object state, and splitter limits. Property getters do not prove native UIA/MSAA, AT-SPI, or AX provider relationships. Source proximity between a label and an edit control proves neither a missing association nor a correct native accessible name.

Trusted hosts can attach `preview.interactions` records for keyboard keys, command tags, splitter-resize/splitter-keyboard probes, and accessibility modes. Each carries `status` of `passed`, `failed`, or `unverified`, with optional explicit root/platform scoping. The host owns the probe and observable assertion; the linter consumes that result.

A declaration that resembles a dialog default action does not establish Return/Escape behavior. Core Buoy Button has no documented Default/Cancel property; custom code can supply behavior elsewhere. An unobserved application-menu fallback remains unknown. Linux does not universally require a Windows-style menu bar or Quit entry.

## Designer measurements

LayoutKit’s shared `LayoutRelations.NearMisses`, `Snap`, `AlignEdits`, and `DistributeEdits` use the same supported source model. Hosts can share profile margins, spacing, snap tolerance, and center tolerance with LayoutCanvas. Relational snapping defaults off until a host enables a tolerance. These measurements do not introduce a live constraint solver or certify optical alignment.

## Continue reading

- [Overview](index.md)
- [Getting started](getting-started.md)
- [API reference](api.md)
- [Rules and evidence](rules.md)
