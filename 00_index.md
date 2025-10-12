# Yocto MaaXBoard Documentation

Welcome to my technical documentation for building and debugging the **Avnet MaaXBoard Yocto Linux image** from scratch inside an Ubuntu VirtualBox VM.  
This project documents every major step I took — from environment setup to full system bring-up and kernel debugging.

---

## Contents

| Section | Description |
|----------|--------------|
| [01 · Environment Setup](01_environment_setup.md) | Setting up Ubuntu inside VirtualBox and configuring swap memory |
| [02 · Building with BitBake](02_building_bitbake.md) | Creating and configuring the Yocto build environment |
| [03 · Flashing the SD Card](03_flashing_sdcard.md) | Writing the `.wic` image to a 32 GB SD card using `dd` |
| [04 · Serial Connection](04_serial_connection.md) | Connecting to the MaaXBoard via UART using a Raspberry Pi cable |
| [05 · HDMI and Display Fix](05_hdmi_and_display_fix.md) | Enabling HDMI output and correcting overlay settings |
| [06 · Weston and Terminal Debug](06_weston_and_terminal_debug.md) | Disabling Weston and booting directly into a root console |
| [07 · Testing and Common Issues](07_testing_common_issues.md) | Summary of all major problems encountered and solutions |

---

## Summary

This documentation is based on my experience building the Yocto image for the **Avnet MaaXBoard (i.MX8M)** using the **mickledore** branch from Avnet’s meta-maaxboard repository.

The process included:
- Setting up an Ubuntu 22.04 VM on VirtualBox 7.1  
- Installing all Yocto dependencies and creating a 64 GB swap file  
- Running BitBake to build `avnet-image-full` for MaaXBoard  
- Flashing the image to a 32 GB SD card  
- Connecting the board using a Raspberry Pi USB-to-serial cable on the 40-pin GPIO header 
- Debugging Weston and configuring the system for console mode  

Each section contains real commands, problems I encountered, and how I solved them.

---

## Gallery

See the [Gallery](08_gallery.md) section for build screenshots and hardware photos.

*(Images located in `/docs/assets/`.)*

---

**Author:** Eric Huang  
**Project:** MaaXBoard Embedded Linux Bring-Up (Yocto mickledore)  
**Last Updated:** October 2025