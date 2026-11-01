# GUILintKit · 0.1.0

A reusable, read-only native GUI linter, integrated into the existing **buoy-dev
gui-lint** command. It uses LayoutKit's actual source model, shared relational
measurements, explicit project policies and source-matched PreviewKit evidence.
It never runs the opened source, rewrites frames, or certifies accessibility from
a screenshot.

## Use

```sh
./dist/buoy-dev gui-lint --list-rules
./dist/buoy-dev gui-lint examples/preview/Login.bui --platform macos
./dist/buoy-dev gui-lint examples/preview/Login.bui \
  --platform macos --profile examples/gui-lint/profile.json --json dist/gui-lint.json
```

The JSON report includes stable rule IDs, severity, evidence/basis, control paths,
source byte offsets and one-based line/UTF-16 columns. `complete` means the lint
run completed; it does not mean every behaviour was verified. `compilerChecked`
is false: this tool is complementary to compiler diagnostics, not a substitute.

Exit codes are 0 for no findings above the selected threshold, 1 for errors or warnings,
and 2 for invalid inputs. `--strict` also fails on advisory findings.
`--require-evidence` also fails when a requested check remains unverified.
Reports keep `unverifiedChecks` and individual reasons separate from findings.

## Current rules

| IDs | Checks |
| --- | --- |
| GUI001–002 | Empty/negative visible frames and possible parent clipping |
| GUI101 | Single-line default-font TextField height exceeds project guidance |
| GUI102–105 | Alignment, margin, spacing/distribution and centering near misses |
| GUI201 | Resize/minimum-size policy review when no visible binding is found |
| GUI301–306 | Names, icon actions, hidden/focus conflicts, keyboard opt-outs, hints and roles |
| GUI501–503 | Dialog default/cancel expectations and competing default declarations |
| GUI601–603 | Expected menu bar, command tags and localized native menu titles |
| GUI701–702 | Failed keyboard/command behaviour from matching interaction evidence |
| GUI801–804 | Splitter pane structure, resize bindings, native limits and resize probes |
| GUI901 | Possible overlap between visible literal-framed leaf siblings |
| BAMA001–003 | Opt-in macOS BAMA grid, explicit font size and wording conventions |
| WIN001–003 | Windows fixed-color review, static-label tab stops and Command modifiers |
| GTK001–003 | Linux menu modifiers, splitter keyboard probes and accessibility-mode probes |

Recommendations are advisory where intent matters. Large multiline fields and
custom fonts are exempt from the default-font height suggestion. Hidden controls,
unsupported/computed geometry, constraints and custom container coordinate spaces
are handled conservatively. Splitter direct children belong to separate panes;
their frames are not compared as if they shared one pane. Collapsed panes are
not incorrectly treated as empty controls.

`GUI901` is preventive: it reports a pair once when both overlap dimensions exceed
`alignmentTolerance` (default 3). It compares known leaf controls in the same
Window or plain View coordinate space. Containers, Canvas and ImageView surfaces
are excluded because they commonly layer content. Hidden controls are excluded;
computed/constrained/read-only frames, custom controls, expression visibility and
unsupported ancestor containers remain unverified when another potential leaf
sibling exists. A lone leaf cannot overlap another leaf. `AccessibleHidden` alone does
not exempt an interactive control. Findings include `relatedControl` and overlap
dimensions. Intentional overlap can be suppressed with a reason on the finding's
primary (first source-order) control. This source heuristic does not certify native
occlusion, font fitting or z-order, and does not diagnose unpainted backgrounds.

## Profiles and platform policy

The example profile (not included in this repository) demonstrates window
purpose, platform overrides, and style values. Its margins/spacing are **example
project conventions**, not universal Windows/Linux/macOS recommended pixels.
Microsoft's classic spacing tables use font/DPI-qualified dialog units; GTK,
GNOME and KDE have different conventions. Native intrinsic sizes remain a
different concept from an editor's insertion size or a documentation example.
The default TextField ceiling of 28 comes from Buoy's documentation example and
is explicitly advisory; set `textFieldMaxHeight: 0` to disable that suggestion.

`platforms.macos`, `.windows` and `.linux` override common profile values.
Window policy fields merge by declared root name. `purpose: "dialog"` requests
default and cancel reviews by default; set `defaultButton` or `cancelButton`
false when deliberately absent. `resizable: false` records fixed-size intent.
Expected menu commands are **stable application tags**, not guessed English
captions. `nativeMenuTitles` are explicitly localized top-level titles, checked
against an available complete native accessibility observation.

Disable a rule through `disabledRules`, or suppress a specific finding using an
`ignore` entry with `rule`, full `control` path, and nonempty `reason`. Suppressed
findings remain in the report. A reasoned per-control ignore can also waive an
unverified check: the report retains `status: "waived"`, the reason and the
original uncertainty. Such a waiver satisfies `--require-evidence` without
claiming the behaviour passed. Unknown profile fields/rule IDs are rejected.

## Native evidence and limitations

Supply `--preview path/to/result.json` with one inspected source file. It must
be a successful PreviewKit report whose hash matches that source file. An
explicit requested platform must match its backend. This validates the inspected
source, not the current state of every dependency, fixture or application service.
The report schema and observation shapes are validated; this is not producer
authentication. Only unambiguous root/control matches supply runtime evidence.
Matched runtime visibility overrides the declaration for that node; a visible
child observation cannot unhide an unobserved hidden ancestor. A successful
capture establishes that its host Window was shown. Unsupported parent bounds
remain an unverified clipping check rather than silently passing.

PreviewKit supplies runtime bounds, Buoy accessibility properties, first-level
menu tags/shortcuts/enabled states and splitter pane limits. On macOS the existing
permission holder also supplies an accessibility-tree observation when allowed.
Buoy property getters are not a substitute for inspecting UIA/MSAA/AT-SPI/AX
provider associations or listening with a screen reader. Windows may derive an
edit control's accessible name from a preceding native static label in tab order;
source proximity alone proves neither a missing nor a valid association.

Menu presence/enabled state does not prove callback dispatch. A `Default`/`Cancel`
declaration does not prove Return/Escape behaviour. Core Buoy Button has no
documented Default/Cancel property; custom controls and code-behind can supply
those behaviours. Escape, macOS Cmd+Period, Copy/Paste effects, splitter movement,
large text and contrast-mode checks need explicit interaction evidence. **The
current runner does not automatically generate these interaction probes.** Their
absence stays unverified, never silently passing.

Evidence can be attached by a trusted test host as `preview.interactions`:

- Keyboard: `key` = `escape`, `cmd+period` or `return`.
- Command: `command` = a policy tag such as `copy` or `paste`.
- Splitter resize: `kind: "splitter-resize"`, `member` = tracked splitter name.
- GTK splitter keyboard: `kind: "splitter-keyboard"`, `member` = tracked name.
- Accessibility modes: `kind: "accessibility-mode"`, `mode` = `large-text` or
  `high-contrast`.

Each record has `status: "passed" | "failed" | "unverified"`, and may explicitly
specify `root` and `platform`. Evidence is scoped to the selected root/backend.
Control members use full or root-relative paths; leaf shorthand is accepted
only when unique within that root. Duplicate matching observations or probes
remain unverified. A test host owns the probe and its observable assertion; the linter consumes the
result, it does not infer a successful effect from a caption or shortcut string.

macOS application-menu fallback that was not observed remains unknown. For a
typed application instance, a preview host can set `PreviewSession.ApplicationMenu`
to that instance's MenuBar. Native system menus are inspected separately. Linux
does not universally require a Windows-style menu bar or Quit entry.

## Shared designer measurements

`layoutkit.LayoutRelations` exposes `NearMisses`, `Snap`, `AlignEdits`, and
`DistributeEdits`. The linter's profile provides `margins`, `spacing`,
`snapTolerance` and `centerTolerance`; use the same values in the designer host.
Margins/padding are inferred from plain parent View geometry, not invented
properties on arbitrary controls. Optical offsets remain valid suppressions.

LayoutEditor exposes optional `RelationTolerance`, `RelationMargins`,
`RelationSpacing` and `RelationGuides`. Relational snapping defaults off until
the host sets a tolerance. `AlignNodes` and `DistributeNodes` commit one
revision-bound source edit batch. No multi-selection toolbar or guide-line
painting is added here; hosts can use the APIs and guide records. Existing grid
snapping remains available.

## Extend

Install `guilintkit`, `layoutkit`, `editorkit`, and the toolchain's `crypto`
package. Import `guilintkit` for `GuiLinter`, `guilintkit.RuleContext` for the
extension contract/context, and `layoutkit.LayoutDocument` for source snapshots.
Call `RegisterBuiltinRules()` once, then explicitly `Register(yourRule)`.
`GuiLintRule.RuleIds()` declares unique stable IDs; `Check(context)` inspects
source/observations and calls `Emit` or `Unverified` using its declared IDs.
`Emit` accepts `info`, `warning` or `error`; errors fail the CLI run. Each compiled collection is
independent; opening a project never loads executable plugins automatically.
Extensions can read their data-only settings from `profile.ruleOptions[ruleId]`;
these entries merge by rule ID in platform overrides and are validated by the
owning compiled rule. Unknown rule IDs are rejected. The
extension test demonstrates registration,
options and duplicate-ID rejection.
The focused platform tests demonstrate
the API. IDE hosts can pass unsaved immutable LayoutDocument snapshots directly.

Research catalogs retain primary sources, applicability, exceptions and future
checks requiring stronger evidence:

- [BAMA](../research/bama-guidelines.json), pinned to a repository commit and opt-in
  through `bama: true` on macOS; these are third-party conventions.
- [Windows](../research/windows-guidelines.json), including font/DPI-qualified
  layout guidance and native accessible-name providers.
- [GTK/GNOME/KDE](../research/linux-guidelines.json), kept distinct rather than
  presented as one Linux standard.

Test suites and fixtures are maintainer-local and are not part of this public repository.
