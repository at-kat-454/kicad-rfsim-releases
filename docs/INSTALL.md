# RF Sim for KiCad: install

Pre-release for **KiCad 10 on Windows**. Plugin, KiCad API and solver are set up one after the other. You need an
internet connection for the first start. Afterwards RF Sim works offline.

> [!WARNING]
> **Pre-release, results can contain errors.** RF Sim is a student project and not a finished product. Treat every
> result as a first estimate and check important designs with a measurement or a professional tool.

> [!CAUTION]
> **Simulation times are long** (openEMS, CPU only): minutes for a first look, hours for final numbers on an older
> PC. Reducing them is the main work in progress. The time estimate is cautious: the real run usually ends sooner.

**Documents:**
[Installation guide](https://github.com/at-kat-454/kicad-rfsim-releases/blob/main/docs/rfsim_install_guide.pdf) ·
[Worked example](https://github.com/at-kat-454/kicad-rfsim-releases/blob/main/docs/rfsim_worked_example.pdf) ·
[User manual](https://github.com/at-kat-454/kicad-rfsim-releases/blob/main/docs/rfsim_user_manual.pdf) (quick start: section 3) ·
[Verification report (release 0.26.4)](https://github.com/at-kat-454/kicad-rfsim-releases/blob/main/docs/rfsim_verification_report_v0.26.4.pdf)

## Downloads (Assets below)

| File | Content |
|---|---|
| `kicad-rfsim-pcm-<version>.zip` | **the plugin**, for KiCad's Plugin and Content Manager |
| `kicad-rfsim-plugin-<version>.zip` | the same plugin for a manual install |
| `kicad-rfsim-boards-<version>.zip` | test boards for your own validation: KiCad projects, setups and reference values, no simulation results |
| `kicad-rfsim-docs-<version>.zip` | installation guide, worked example, user manual, verification report (PDF and LaTeX) |
| `SHA256SUMS.txt` | checksums. Check a file with `certutil -hashfile <file> SHA256` |

## 1. Install the plugin

**From the file:** KiCad → *Plugin and Content Manager* → *Install from File…* → select
`kicad-rfsim-pcm-<version>.zip` → *Apply Pending Changes*.

**From the repository, with updates:** *Plugin and Content Manager* → *Manage…* → add
`https://at-kat-454.github.io/kicad-rfsim-releases/repository.json` → *Save*. Open the tab *Plugins*, select
*RF Sim for KiCad* → *Install* → *Apply Pending Changes*.

Install only once. A copy in `Documents\KiCad\10.0\plugins` and one in `Documents\KiCad\10.0\3rdparty\plugins`
at the same time may load the old version.

## 2. Switch on the KiCad API

*Preferences → Preferences… → Plugins*: tick *Enable KiCad API*. The Python interpreter must be KiCad's own
`C:\Program Files\KiCad\10.0\bin\pythonw.exe` (*Auto Detect* sets it). Press *OK* and close all KiCad windows.

## 3. First start

Start KiCad and open a board in the PCB editor. KiCad installs the Python packages of the plugin (internet, a few
minutes). Then the **RF Sim** button appears in the toolbar of the PCB editor. If not, restart KiCad once more.

## 4. Solver (openEMS)

openEMS is GPL and not part of the plugin. Press **RF Sim** → *Simulate* and start a simulation: RF Sim offers to
download openEMS 0.37 (about 65 MB) into `%LOCALAPPDATA%\kicad-rfsim\openEMS`.
If the download is blocked, copy an openEMS folder to the PC and select it on *Solver Settings* → *Browse*, then
*Search Again*.

## 5. First test

Unzip `kicad-rfsim-boards-<version>.zip`. Open `validation\ms50\ms50.kicad_pro` in KiCad and its PCB, press
**RF Sim** → *Simulate*, start the setup. The line impedance must be about 50 Ω (± 2 Ω).
The `README.txt` of each board lists its expected values.

## Problems

| Symptom | What to do |
|---|---|
| No RF Sim button | Check *Enable KiCad API* and the interpreter (step 2). Restart KiCad. Check *Installed* in the Plugin and Content Manager. |
| Python packages never finish | The first start needs the internet. A proxy or firewall blocks it. |
| Old version starts | Two copies exist (step 1). Delete the old folder. |
| "openEMS not found" / "did not start" | *Solver Settings* → *Browse* / *Search Again*. A virus scanner may block `openEMS.exe`. |
| Repository cannot be added | Open the `repository.json` address in a browser. If it shows no text, the network blocks it. Use *Install from File*. |

## Update and uninstall

Install the new zip the same way, or use the update in the Plugin and Content Manager. To uninstall: *Plugin and
Content Manager → Installed → RF Sim → Uninstall → Apply*. Data: delete `%LOCALAPPDATA%\kicad-rfsim`.

## About

A project to see what is possible with an open-source solver. Not a finished product: much more validation is
needed before the values can be fully trusted. Feedback on the installation, the workflow and anything that does not
work is welcome.

openEMS (GPL) is downloaded at runtime. TI and Murata material is used as published by the vendors. Plugin: MIT.
