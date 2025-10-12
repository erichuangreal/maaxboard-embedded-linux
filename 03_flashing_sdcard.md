# 3 · Flashing the SD Card

## Week 3 — Flashing Process

After successfully building the full `avnet-image-full` Yocto image, I needed to flash it to an SD card to boot the MaaXBoard.

I flashed the SD card directly from Ubuntu’s terminal using `dd`:
```bash
sudo dd if=avnet-image-full-maaxboard.wic of=/dev/sdX bs=4M status=progress
```

---

### Step 1 — Identifying the SD Card Device
I plugged the SD card reader into my host and connected it to my VM under the USB tab. Now that it was attached, Ubunut would be able to detected it. For me, the SD reader was assigned under
```bash
sbd
```
I knew because my SD card was around 30 GB. To verify, after the boot, the card was partitioned into a boot and root filesystem.

### Inserting the SD card and setting up the MaaxBoard to a serial connection
After flashing, I inserted the SD card into the MaaXBoard, connected:
1. HDMI display (desktop monitor)
2. Keyboard
3. USB-to-UART (serial cable)
4. Power supply

Then I powered the board and opened Minicom to monitor the serial boot log. However, I ran through a lot of issues with setting up the minicom as well as the display monitor, which I will talk about in the next few sections.

### Troubleshooting notes
| Problem | Cause | Fix |
|----------|--------|-----|
| **SD card not detected in VM** | USB device not attached to VirtualBox | Go to Devices → USB → select card reader |
| **Flashing to wrong disk** | Wrong `/dev/sdX` selected | Run `lsblk` to confirm target device before flashing |
| **SD card capacity mismatch** | Old partitions still present | Use `sudo wipefs -a /dev/sdX` before flashing |
