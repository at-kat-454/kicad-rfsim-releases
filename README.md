# RF Sim for KiCad: downloads

RF Sim is a KiCad 10 plugin. It builds a 3D model from the open board and simulates printed antennas and
RF matching networks with the open-source field solver openEMS. It is meant as a low-cost complement to
full-wave tools such as Ansys HFSS or CST Studio: a simpler workflow, a lower barrier to entry for RF PCBs
and fewer prototype iterations.

This repository holds the downloads and the documentation. The source code is not published here.

> [!WARNING]
> ## Pre-release: results can contain errors
> RF Sim is a student project to see what an open-source solver can do inside KiCad. It is **not a finished
> product**. Much more validation is needed before its values can be fully trusted. Bugs that change results
> are still being found and fixed (for example in 0.27.2: a mesh short between a trace and a close ground pour).
> **Treat every result as a first estimate and check important designs with a measurement or a professional tool.**

> [!CAUTION]
> ## Simulation times are long
> openEMS runs on the **CPU only** (no GPU). On an older 4-core PC:
> - a first look (*Fast*): **minutes to about an hour** per setup,
> - final numbers (*Automatic*): **one to several hours** per setup; a sub-GHz antenna board took up to 3.6 h,
>   its matching network up to 4 h.
>
> **Reducing the simulation time is the main work in progress.** Release 0.27 already needs fewer runs per
> matching network and a shorter excitation; more will follow.
>
> **The time estimate is cautious:** while a run is going, the real time usually turns out **shorter than the
> estimate**, because a run stops as soon as the fields have died down.

## Download

Go to **[Releases](../../releases/latest)** and open **Assets** of the newest release:

| File | Content |
|---|---|
| `kicad-rfsim-pcm-<version>.zip` | the plugin, for KiCad's Plugin and Content Manager |
| `kicad-rfsim-plugin-<version>.zip` | the same plugin for a manual install |
| `kicad-rfsim-docs-<version>.zip` | all documents below, with LaTeX sources and pictures |
| `kicad-rfsim-boards-<version>.zip` | test boards for your own validation: generated reference boards (50 Ω line, filters, antennas) and the TI CC-Antenna-DK2 boards as KiCad projects with setups and reference values, **no simulation results** |
| `SHA256SUMS.txt` | checksums |

Plugin Manager repository (with updates):
`https://at-kat-454.github.io/kicad-rfsim-releases/repository.json`

## Documentation

| Document | |
|---|---|
| [Installation guide](docs/rfsim_install_guide.pdf) | download, install, first start, openEMS, first test |
| [Worked example](docs/rfsim_worked_example.pdf) | an 868 MHz sensor board, step by step from the layout to the matching values |
| [User manual](docs/rfsim_user_manual.pdf) | quick start, all pages of the window, result pages, operating band, good practice, limits |
| [Verification report (release 0.26.4)](docs/rfsim_verification_report_v0.26.4.pdf) | simulation compared with closed-form references, with TI's measurements of the CC-Antenna-DK2 kit and with own VNA measurements |
| [Install notes](docs/INSTALL.md) | the install steps in short, as on the release page |

The test boards contain no simulation results on purpose: results of different plugin versions are not
consistent with each other. Simulate the boards with the version you use and compare with the reference values.

## Feedback

Reports of wrong results, crashes and unclear steps are very welcome.

## Licence

The plugin is MIT licensed. openEMS (GPL) is downloaded on the first run and is not part of the plugin.
The TI CC-Antenna-DK2 boards are converted automatically from TI's published design files of this public kit.
