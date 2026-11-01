# BAMA research and adaptation

The project is [BAMA — Build Any Mac App](https://github.com/daveebbelaar/bama),
reviewed at commit `f0c2cdbf9e41d838c14d8b7596729581dd4833c0` on 2026-10-31.
It supplies third-party SwiftUI/macOS conventions. It is not an Apple standard.

`GuiBamaRules` provides three advisory checks when the resolved profile contains
`"bama": true` and the lint platform is `macos`. They cover authored grid
positions, small explicit fonts, and conflicting visible/accessibility wording.
Ordinary GUI rules remain independent of this opt-in policy.

The [layout-grid source](https://github.com/daveebbelaar/bama/blob/f0c2cdbf9e41d838c14d8b7596729581dd4833c0/skills/design/layout-grid/SKILL.md)
motivates BAMA001. This adaptation checks positions in supported source frames;
it excludes native dimensions, root desktop placement and unresolved geometry.
The shared relational rules handle separately configured margins and spacing.

The [accessibility source](https://github.com/daveebbelaar/bama/blob/f0c2cdbf9e41d838c14d8b7596729581dd4833c0/skills/design/ux-design/accessibility.md)
motivates BAMA002/003. These remain guidance: a font default is not a zero-point
font, an expanded spoken name can retain the visible words, and localization
can justify a documented exception. Full screen-reader usability needs native
interaction observations, beyond a source naming check.

The [Mac design source](https://github.com/daveebbelaar/bama/blob/f0c2cdbf9e41d838c14d8b7596729581dd4833c0/skills/design/mac-design/SKILL.md)
also motivates future native-size and appearance scenarios. We do not translate
SwiftUI intrinsic sizing into a mandatory fixed Buoy height.

[The machine-readable catalog](bama-guidelines.json) records provenance,
exemptions, existing rule mappings and candidate checks with evidence needs.
Unimplemented candidates are not reported as passing checks.
