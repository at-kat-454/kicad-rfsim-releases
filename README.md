# RF Sim for KiCad: downloads

RF Sim is a KiCad 10 plugin. It builds a 3D model from the open board and simulates printed antennas and
RF matching networks with openEMS. It is a low-cost complement to full-wave tools such as Ansys HFSS or CST Studio:
a simpler workflow, a lower barrier to entry for RF PCBs and fewer prototype iterations.

This repository holds the downloads and the documentation. The source code is not published here.

## Download

Go to **[Releases](../../releases/latest)** and open **Assets** of the newest release:

| File | Content |
|---|---|
| `kicad-rfsim-pcm-<version>.zip` | the plugin, for KiCad's Plugin and Content Manager |
| `kicad-rfsim-plugin-<version>.zip` | the same plugin for a manual install |
| `kicad-rfsim-docs-<version>.zip` | all documents below, with LaTeX sources and pictures |
| `kicad-rfsim-boards-<version>.zip` | the validation boards (50 Ω line, filters, antennas) and the TI CC-Antenna-DK2 boards as KiCad projects |
| `kicad-rfsim-verification-<version>.zip` | verification scripts, report, expected values, TI reference data |
| `SHA256SUMS.txt` | checksums |

Plugin Manager repository (with updates):
`https://at-kat-454.github.io/kicad-rfsim-releases/repository.json`

## Documentation

| Document | |
|---|---|
| [Installation guide](docs/rfsim_install_guide.pdf) | download, install, first start, openEMS, first test |
| [User manual](docs/rfsim_user_manual.pdf) | all pages of the window, result pages, operating band, good practice, limits |
| [Verification report: TI CC-Antenna-DK2](docs/rfsim_ti_dk2_verification.pdf) | simulation compared with measurements of a public antenna kit |
| [Validation notes](docs/VALIDATION.md) | verification cases, deviations and open points |
| [Install notes](docs/INSTALL.md) | the install options in short |

## Licence

The plugin is MIT licensed. openEMS (GPL) is downloaded on the first run and is not part of the plugin.
Texas Instruments and Murata material is used as published by the vendors.
