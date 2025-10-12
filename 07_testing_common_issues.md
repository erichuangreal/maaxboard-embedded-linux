# 7 · Testing and Common Issues

## Week 5 — Testing, Debugging, and Lessons Learned

Throughout the entire MaaXBoard setup and build process, I encountered multiple issues — from long BitBake builds to HDMI signal problems and serial console configuration errors.  
This section summarizes those experiences and how I fixed them.

---

### Testing Environment Summary

After flashing and booting the image, I tested the following:

- Serial console through the Raspberry Pi USB-to-TTL cable (UART)
- HDMI display output and overlay changes
- Weston vs. console-only boot targets
- Kernel logs and dmesg output
- USB keyboard and mouse input

I also experimented with rebuilding kernels, testing the `dtoverlay_display` changes, and confirming that the board booted cleanly from power cycles.

---

### Common Observations

- The **first BitBake build** took extremely long — several hours, especially during large packages like LLVM and Python3.
- I had to rebuild multiple times due to small mistakes in the configuration files.
- Adding a **64 GB swap file** on the VM was crucial to prevent “out of memory” errors.
- The **USB keyboard** initially didn’t respond because of the wrong VirtualBox USB controller setting.
- HDMI remained blank until I explicitly enabled `dtoverlay_display=hdmi`.
- Weston launched automatically but opened only a tiny terminal — switching to `multi-user.target` solved that.

---

### Troubleshooting Table

| Problem | Cause | Fix |
|----------|--------|-----|
| **Build froze for hours** | Compiling large packages (LLVM, Python3) | Wait. Avoid interrupting — BitBake still compiling internally |
| **VM froze during build** | All cores allocated to BitBake | Reduce to `-j4` and leave 1 CPU free |
| **Out of memory / “Killed” message** | No swap file or insufficient memory | Create a 64 GB swap file (`fallocate -l 64G /swapfile`) |
| **Keyboard not working on board** | Wrong VirtualBox USB controller | Enable USB 3.0 (xHCI) in VM settings |
| **No HDMI output** | Display overlay disabled | Add `dtoverlay_display=hdmi` to `/boot/uEnv` and rebuild |
| **HDMI still blank after edit** | U-Boot config not updated | Modify `uEnv-maaxboard.txt` and rebuild image |
| **Serial console blank** | Hardware Flow Control = Yes | Disable hardware flow control in Minicom |
| **Unreadable serial text** | Wrong baud rate | Use `115200 8N1` |
| **Tiny terminal window in Weston** | Weston default layout | Switch to console mode (`systemctl set-default multi-user.target`) |
| **No GUI after disabling Weston** | Wrong system target | Ensure target is `multi-user.target` and not graphical |
| **Build corrupted after crash** | Interrupted BitBake process | Clean and rebuild: `bitbake -c cleanall avnet-image-full` |
| **Slow SD card boot** | Low-quality SD card | Use Class 10 or UHS-1 32 GB card |
| **Image failed to flash** | Incorrect `/dev/sdX` target | Verify with `lsblk` before running `dd` |

---

### Lessons Learned

- Always leave **one CPU core free** for the Ubuntu host. Otherwise, the VM can hang.  
- Create **swap space early** to avoid restarts.  
- Check connections carefully, even small wiring mistakes caused unwanted output.  
- Weston is not required for testing; console mode is much more efficient.   
- Always back up the `build/conf/local.conf` and `uEnv` files before modifying them.

---

### System Summary After Debugging

| Component | Working State | Notes |
|------------|----------------|--------|
| **Yocto Build Environment** | Stable | Using `mickledore` branch |
| **Serial Console (UART)** | Stable | 115200 8N1, no flow control |
| **HDMI Display** | Working | `dtoverlay_display=hdmi` applied |
| **Weston GUI** | Optional | Disabled for console mode |
| **USB Keyboard / Mouse** | Working | Enabled via USB 3.0 passthrough |
| **Swap Memory** | Configured | 64 GB created for long builds |

---

After several weeks of debugging, the MaaXBoard finally booted reliably with both serial and HDMI access.  
All issues — from long BitBake builds to Weston GUI scaling — were resolved through persistent testing and small configuration tweaks.
