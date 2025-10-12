# 4 · Serial Connection and Login

## Week 3 — Connecting the MaaXBoard via UART (Serial)

Setting up the serial console took a while — I used a **Raspberry Pi USB-to-TTL serial cable** to connect directly to the **40-pin GPIO header** on the MaaXBoard.  
The pin layout is almost identical to a Raspberry Pi, but it took me several tries to get the wiring correct and see serial output in Minicom.

---

### Hardware Setup — Using the Raspberry Pi UART Cable

My USB-to-TTL cable had **four color-coded leads**:

| Wire Color | Cable Signal | Connect To (MaaXBoard 40-pin Header) | Pin Number | Notes |
|-------------|---------------|--------------------------------------|-------------|--------|
| **Black** | GND | GND | **Pin 6** | Common ground |
| **White** | RXD | TXD | **Pin 8** | Cable receives board output |
| **Green** | TXD | RXD | **Pin 10** | Cable transmits to board |
| **Red** | 5 V | Do not connect | — | Board already powered separately |

Here is a photo of my setup:
INCLUDE HERE MY PHOTO

At first I accidentally swapped the white and green leads. Minicom opened but showed no logs.  
Once I reversed them, the boot messages finally appeared.

### Getting a serial connection

After plugging the cable into my host (Ubuntu VM via VirtualBox USB Passthrough), I verified it appeared as:

```bash
dmesg | grep ttyUSB
```
It showed up as ttyUSB0.

To get a serial connection to the Maaxboard and read from a terminal, I installed Minicom
```bash
sudo apt install minicom
sudo minicom -D /dev/ttyUSB0 -b 115200
```
When I first tried 9600 bps, the text was unlegible. So I set it to 115200 8N1, fixing it.

I came across an issue, the minicom would run properly, but no text showed up.
SHOW IMAGE OF MINICOM SETTINGS.

Debugging:
When Hardware Flow Control was set to Yes, Minicom expected RTS/CTS signals that your Raspberry Pi cable didn’t provide.
As a result, the terminal stayed blank even though the board was transmitting.
Once you switched Hardware Flow Control → No, the text output started working.

### Troubleshooting notes
| Problem | Cause | Fix |
|----------|--------|-----|
| **No output** | TX/RX reversed | Swap white ↔ green wires |
| **Blank screen** | Hardware Flow Control enabled | Set to “No” in Minicom settings |
| **Unreadable text** | Wrong baud rate | Use `115200 8N1` |
| **Cable not detected** | USB driver issue or missing device | Replug the cable and check `dmesg` for `ttyUSB` |


Once the MaaXBoard booted from the SD card, the serial console showed:
SHOW PICTURE OF SERIAL OUTPUT


