# Setting up my linux image build

This repo is my practical documentation for building and debugging a custom **Yocto Linux image** for the **Avnet MaaXBoard** from scratch inside an **Ubuntu 22.04 VirtualBox VM**. It covers environment setup, BitBake builds, SD flashing, and the most common bring-up issues (serial console, HDMI, Weston/terminal debugging).

## What is the MaaXBoard?
The **Avnet MaaXBoard** is an **embedded development board**. It is commonly used for:
- embedded Linux development
- Yocto/Buildroot workflows
- hardware bring-up and debugging
- serial/UART and display pipeline debugging
- learning real boot chains (U-Boot → kernel → userspace)

This repo treats the MaaXBoard as a real embedded target, not a desktop Linux system.
## What’s in here
- **00_index.md** — start here (table of contents + summary)
- **01_environment_setup.md** — VM setup + dependencies + swap
- **02_building_bitbake.md** — Yocto build environment + BitBake workflow
- **03_flashing_sdcard.md** — writing the `.wic` image to an SD card
- **04_serial_connection.md** — UART serial connection + troubleshooting
- **05_hdmi_and_display_fix.md** — display/overlay fixes
- **06_weston_and_terminal_debug.md** — disabling Weston / console boot
- **07_testing_common_issues.md** — common failures + fixes
- **08_gallery.md** — build + hardware photos (see note below)

## Photos / Gallery (coming soon)
I completed the full build/bring-up process before taking good photos/screenshots, so the **gallery is currently a placeholder**. I’ll be uploading real images of:
- the VM/Yocto setup,
- BitBake build output,
- SD flashing,
- the MaaXBoard serial boot log,
- HDMI/Weston output,
- and the physical wiring/cables.

For now, the docs focus on the exact commands, configs, and debugging steps that worked for me.

## How to use this repo
Start with **00_index.md**, then follow the sections in order. Each file is written as a “do this → expect this → if it breaks, try this” checklist.

---

## NOTE:
I apologize if there aren't any photos so far. I spent the summer going through the build procedure, but unfortunately did not think to document my process with pictures or screenshots. I will be going through the entire build, flash, and booting procedure again sometime in the future to provide this documentation with more visuals.

**Author:** Eric Huang  
**Last updated:** October 2025
