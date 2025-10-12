# 5 · HDMI and Display Fix

## Week 4 — HDMI Debugging and Bring-Up

When I first powered on the MaaXBoard, the serial console worked, but **the HDMI monitor stayed completely black**.  
The system was clearly booting, kernel logs appeared over UART, but no video output was detected.  
This section documents how I fixed the HDMI display and what I learned about overlays.

---

### Initial Problem

After flashing `avnet-image-full-maaxboard.wic` and booting the board:

- Serial output appeared normally.
- The login prompt showed through Minicom.
- HDMI monitor remained blank (no signal).

At first, I suspected a bad cable or monitor, but it turned out the image needed a display overlay setting to enable HDMI output.

---

### Try 1: Edit the Runtime Configuration (`/boot/uEnv`)

Logged into the board via serial (UART) and opened the configuration file:

```bash
nano /boot/uEnv
```

Added the following line:
```bash
dtoverlay_display=hdmi
```

### Try 2: Modify the U-Boot Configuration in the Source Tree
I edited the file
```swift
imx-yocto-bsp/sources/meta-maaxboard/recipes-bsp/u-boot/files/uEnv-maaxboard.txt
```
and added
```
dtoverlay_display=hdmi
```
Then I rebuilt the bitbake image and reflashed the new .wic file to my SD card. The HDMI display came alive and I finally saw the Weston desktop.

After applying the fix:
- HDMI monitor detected signal immediately after U-Boot. (forced)
- Display switched to the Weston desktop environment.
- Serial console continued to work simultaneously.

### Troubleshooting notes
| Problem | Cause | Fix |
|----------|--------|-----|
| **No HDMI output** | Display overlay disabled | Add `dtoverlay_display=hdmi` to `/boot/uEnv` |
| **Screen still black after reboot** | U-Boot not updated | Edit `uEnv-maaxboard.txt` in source and rebuild |
| **Monitor shows “No Signal”** | Wrong overlay (MIPI mode selected) | Force HDMI overlay explicitly |