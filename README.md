# Lint native GUI layouts with extensible rules

A reusable, read-only native GUI linter, integrated into the existing **buoy-dev
gui-lint** command. It uses LayoutKit's actual source model, shared relational
measurements, explicit project policies and source-matched PreviewKit evidence.
It never runs the opened source, rewrites frames, or certifies accessibility from
a screenshot.

## Install

From a Buoy consumer project with BPM installed:

```sh
bpm add guilintkit --git https://github.com/BuoyBay/guilintkit.git --rev refs/heads/main
```

BPM records the resolved commit in its lockfile. Keep that lockfile with the
consumer project for reproducible installs.
BPM also resolves `layoutkit`, `editorkit`.

## Example

```basic
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

## Build from source

With the native `build-packages` driver from [devtools](https://github.com/BuoyBay/devtools)
on your `PATH`, run from this checkout:

```sh
build-packages --root "$PWD" --compile-consumer guilintkit
```

The driver checks source targets and compiles an import consumer. See the
[API guide](docs/API.md) for behavior, limits and any host prerequisites.

## Local Buoy setup

On your own machine, install Buoy and run:

```sh
buoy ai init --both
```


## Contributing

[Contributing](CONTRIBUTING.md) · [License](LICENSE)
