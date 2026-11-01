---
title: "guilintkit typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["GuiAccessibilityRulesV2", "GuiBamaRules", "GuiGeometryRules", "GuiGtkGuidelineRules", "GuiLintContext", "GuiLintRule", "GuiLinter", "GuiMeasurementRules", "GuiOverlapRules", "GuiPlatformRules", "GuiSplitterRules", "GuiWindowsGuidelineRules"]
dependencies: []
symbols: ["GuiAccessibilityRulesV2", "GuiBamaRules", "GuiGeometryRules", "GuiGtkGuidelineRules", "GuiLintContext", "GuiLintRule", "GuiLinter", "GuiMeasurementRules", "GuiOverlapRules", "GuiPlatformRules", "GuiSplitterRules", "GuiWindowsGuidelineRules"]
related: ["api.md", "getting-started.md", "index.md"]
---

# guilintkit typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

## GuiAccessibilityRulesV2

`Public Class GuiAccessibilityRulesV2 Implements GuiLintRule`

Source declarations and matched native observations; no expression execution.

### RuleIds

`Public Function RuleIds() As Array Of String`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of String` |

### Check

`Public Sub Check(c As GuiLintContext)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `c` | `GuiLintContext` | `ByVal` | Required |

Returns: no value.

## GuiBamaRules

`Public Class GuiBamaRules Implements GuiLintRule`

Optional BAMA conventions, not Apple requirements or native intrinsic sizes. Source policy is pinned in research/bama-guidelines.json.

### RuleIds

`Public Function RuleIds() As Array Of String`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of String` |

### Check

`Public Sub Check(c As GuiLintContext)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `c` | `GuiLintContext` | `ByVal` | Required |

Returns: no value.

## ControlKinds functions

### GuiInteractiveType

`Public Function GuiInteractiveType(typeName As String) As Boolean`

Known interaction capability, independent of platform focus defaults. TextArea and Stepper are documented input controls (ui/textarea.md, ui/stepper.md). Containers and output-only controls are not assumed interactive.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `typeName` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### GuiDefaultFocus

`Public Function GuiDefaultFocus(typeName As String) As Optional Of Boolean`

Only defaults documented by Buoy's accessibility surface are asserted here.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `typeName` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Optional Of Boolean` |

## GUILintKit functions

### DefaultGuiLintProfile

`Public Function DefaultGuiLintProfile() As JSONItem`

Explicit policy values are guidance, not universal native widget defaults.

No parameters.

#### Returns

| Type |
| --- |
| `JSONItem` |

## GuiGeometryRules

`Public Class GuiGeometryRules Implements GuiLintRule`

### RuleIds

`Public Function RuleIds() As Array Of String`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of String` |

### Check

`Public Sub Check(c As GuiLintContext)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `c` | `GuiLintContext` | `ByVal` | Required |

Returns: no value.

## GuiLinter

`Public Class GuiLinter`

Read-only linter reusable by the CLI, IDE, and MCP hosts.

### Register

`Public Sub Register(rule As GuiLintRule)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `rule` | `GuiLintRule` | `ByVal` | Required |

Returns: no value.

### RegisteredRuleIds

`Public Function RegisteredRuleIds() As Array Of String`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of String` |

### RegisterBuiltinRules

`Public Sub RegisterBuiltinRules()`

No parameters.

Returns: no value.

### Analyse

`Public Function Analyse(document As LayoutDocument, sourcePath As String) As JSONItem`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `document` | `LayoutDocument` | `ByVal` | Required |
| `sourcePath` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `JSONItem` |

### Analyse (overload 2)

`Public Function Analyse(document As LayoutDocument, sourcePath As String, platform As String, profile As JSONItem, preview As JSONItem) As JSONItem`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `document` | `LayoutDocument` | `ByVal` | Required |
| `sourcePath` | `String` | `ByVal` | Required |
| `platform` | `String` | `ByVal` | Required |
| `profile` | `JSONItem` | `ByVal` | Required |
| `preview` | `JSONItem` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `JSONItem` |

## GuiGtkGuidelineRules

`Public Class GuiGtkGuidelineRules Implements GuiLintRule`

GTK contracts and GNOME recommendations apply on Linux; no Libadwaita mandate. Missing probes remain unverified. These rules do not change system settings.

### RuleIds

`Public Function RuleIds() As Array Of String`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of String` |

### Check

`Public Sub Check(c As GuiLintContext)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `c` | `GuiLintContext` | `ByVal` | Required |

Returns: no value.

## GuiMeasurementRules

`Public Class GuiMeasurementRules Implements GuiLintRule`

Shared measurements drive designer snapping and diagnostic near misses.

### RuleIds

`Public Function RuleIds() As Array Of String`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of String` |

### Check

`Public Sub Check(c As GuiLintContext)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `c` | `GuiLintContext` | `ByVal` | Required |

Returns: no value.

## GuiOverlapRules

`Public Class GuiOverlapRules Implements GuiLintRule`

Preventive source heuristic, not proof of native occlusion or incorrect z-order.

### RuleIds

`Public Function RuleIds() As Array Of String`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of String` |

### Check

`Public Sub Check(c As GuiLintContext)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `c` | `GuiLintContext` | `ByVal` | Required |

Returns: no value.

## GuiPlatformRules

`Public Class GuiPlatformRules Implements GuiLintRule`

Platform expectations are policy-driven. A declaration never proves a key handler.

### RuleIds

`Public Function RuleIds() As Array Of String`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of String` |

### Check

`Public Sub Check(c As GuiLintContext)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `c` | `GuiLintContext` | `ByVal` | Required |

Returns: no value.

## PreviewEvidence functions

### ValidateGuiPreview

`Public Sub ValidateGuiPreview(preview As JSONItem, document As LayoutDocument)`

Validate format/identity before a rule consumes observations. This does not authenticate the producer, verify dependency freshness, or prove probe effects.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `preview` | `JSONItem` | `ByVal` | Required |
| `document` | `LayoutDocument` | `ByVal` | Required |

Returns: no value.

## Profiles functions

### ResolveGuiLintProfile

`Public Function ResolveGuiLintProfile(profile As JSONItem, platform As String) As JSONItem`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `profile` | `JSONItem` | `ByVal` | Required |
| `platform` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `JSONItem` |

## GuiLintRule

`Public Interface GuiLintRule`

Extensions register compiled rules; linting never executes opened project code.

### RuleIds

`Function RuleIds() As Array Of String`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of String` |

### Check

`Sub Check(context As GuiLintContext)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `context` | `GuiLintContext` | `ByVal` | Required |

Returns: no value.

## GuiLintContext

`Public Class GuiLintContext`

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `Document` | `LayoutDocument` | No | No | — | — |
| `Path` | `String` | No | No | — | — |
| `Platform` | `String` | No | No | — | — |
| `Profile` | `JSONItem` | No | No | — | — |
| `Preview` | `JSONItem` | No | No | — | — |
| `Findings` | `JSONItem` | No | No | `JSONItem.NewArray()` | — |
| `Suppressed` | `JSONItem` | No | No | `JSONItem.NewArray()` | — |
| `Coverage` | `JSONItem` | No | No | `JSONItem.NewArray()` | — |
| `DeclaredRuleIds` | `Array Of String` | No | No | `[]` | — |

### Constructor

`Public Sub Constructor(document As LayoutDocument, sourcePath As String, platform As String, profile As JSONItem, preview As JSONItem)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `document` | `LayoutDocument` | `ByVal` | Required |
| `sourcePath` | `String` | `ByVal` | Required |
| `platform` | `String` | `ByVal` | Required |
| `profile` | `JSONItem` | `ByVal` | Required |
| `preview` | `JSONItem` | `ByVal` | Required |

Returns: no value.

### ControlPath

`Public Function ControlPath(node As LayoutNode) As String`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `node` | `LayoutNode` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `String` |

### MemberMatches

`Public Function MemberMatches(node As LayoutNode, member As String) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `node` | `LayoutNode` | `ByVal` | Required |
| `member` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### RuntimeControl

`Public Function RuntimeControl(node As LayoutNode) As JSONItem`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `node` | `LayoutNode` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `JSONItem` |

### Hidden

`Public Function Hidden(node As LayoutNode) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `node` | `LayoutNode` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### Literal

`Public Function Literal(node As LayoutNode) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `node` | `LayoutNode` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### Unverified

`Public Sub Unverified(rule As String, node As LayoutNode, reason As String)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `rule` | `String` | `ByVal` | Required |
| `node` | `LayoutNode` | `ByVal` | Required |
| `reason` | `String` | `ByVal` | Required |

Returns: no value.

### Disabled

`Public Function Disabled(rule As String) As Boolean`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `rule` | `String` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Boolean` |

### Emit

`Public Sub Emit(rule As String, node As LayoutNode, severity As String, message As String, basis As String)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `rule` | `String` | `ByVal` | Required |
| `node` | `LayoutNode` | `ByVal` | Required |
| `severity` | `String` | `ByVal` | Required |
| `message` | `String` | `ByVal` | Required |
| `basis` | `String` | `ByVal` | Required |

Returns: no value.

## GuiSplitterRules

`Public Class GuiSplitterRules Implements GuiLintRule`

Splitter direct children each belong to a different synthesized pane View. These rules never treat them as siblings sharing the splitter's frame.

### RuleIds

`Public Function RuleIds() As Array Of String`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of String` |

### Check

`Public Sub Check(c As GuiLintContext)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `c` | `GuiLintContext` | `ByVal` | Required |

Returns: no value.

## GuiWindowsGuidelineRules

`Public Class GuiWindowsGuidelineRules Implements GuiLintRule`

Windows-only review rules. These do not claim conformance from one snapshot. Provenance and unimplemented checks live in research/windows-guidelines.json.

### RuleIds

`Public Function RuleIds() As Array Of String`

No parameters.

#### Returns

| Type |
| --- |
| `Array Of String` |

### Check

`Public Sub Check(c As GuiLintContext)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `c` | `GuiLintContext` | `ByVal` | Required |

Returns: no value.
