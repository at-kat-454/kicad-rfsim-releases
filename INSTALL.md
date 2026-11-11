# Install RF Sim

For KiCad 10. Python and openEMS need no manual setup: openEMS (GPL) is downloaded on the first run.

## Downloads

| File | Content |
|---|---|
| `kicad-rfsim-pcm-X.zip` | Plugin for the KiCad Plugin and Content Manager |
| `kicad-rfsim-plugin-X.zip` | Plugin for a manual install |
| `kicad-rfsim-docs-X.zip` | User manual (PDF and LaTeX), quick start, user guide, validation notes, TI CC-Antenna-DK2 verification report |
| `kicad-rfsim-verification-X.zip` | Verification test bench: generated boards, expected values, scripts, report, TI comparison scripts and results |
| `SHA256SUMS.txt` | Checksums of all files |

## Option 1: Plugin and Content Manager, from file

1. Download `kicad-rfsim-pcm-X.zip`.
2. In KiCad open *Plugin and Content Manager*.
3. Press *Install from File...* and select the zip.
4. Press *Apply Pending Changes*.

## Option 2: Plugin and Content Manager, with updates

1. In *Plugin and Content Manager* press *Manage...*.
2. Add this repository URL: `https://at-kat-454.github.io/kicad-rfsim-releases/repository.json`
3. Select *RF Sim for KiCad*, install the plugin, apply.

New versions then show up as updates.

## Option 3: manual

Unzip `kicad-rfsim-plugin-X.zip` into the KiCad plugin folder, for example
`Documents/KiCad/10.0/plugins/` on Windows. Restart KiCad.

## First run

Open a board in the PCB editor and press the **RF Sim** button. Follow the [quick start](QUICKSTART.md).

## Verification pack

Unzip `kicad-rfsim-verification-X.zip` and read `verification/README.md`. Each folder in
`verification/boards/` is a small KiCad project with a ready setup. The TI boards are not included:
TI's design files are not redistributable. `verification/ti_dk2/build_ti.py` rebuilds them from TI's kit.
