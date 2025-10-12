# 1. Environment Setup — Ubuntu VM for Yocto

## Week 1 — Setting up the Virtual Machine
### VirtualBox Configuration
| Setting | Value |
|----------|--------|
| VirtualBox version | 7.1 + Extension Pack |
| Ubuntu ISO | ubuntu-22.04.1-desktop-amd64.iso (headless install) |
| CPUs | 6 allocated |
| Disk | 1 TB dynamic VDI |
| RAM | 8 GB |
| USB Controller | Enabled (xHCI / USB 3.0) |
| Shared Clipboard | Bidirectional |
| Networking | NAT |

I installed **VirtualBox 7.1** and the **Extension Pack**, created a headless Ubuntu 22/04 VM, and connected to it using **PuTTY and SuperPuTTY** for automatic SSH logins.  

I later connected it to **VS Code Remote SSH** so I could edit files directly inside the VM.

---

### Installing Dependencies

After updating the system, I installed all Yocto and BitBake prerequisites:

```bash
sudo apt update
sudo apt install -y gawk wget git diffstat unzip texinfo gcc-multilib \
  build-essential chrpath socat cpio python3 python3-pip python3-pexpect \
  xz-utils debianutils iputils-ping libsdl1.2-dev xterm
```

### Setting up Yocto and meta-maaxboard
I used the source code for the Yocto build from https://github.com/Avnet/meta-maaxboard/tree/mickledore.

```bash
cd ~
git clone -b mickledore https://github.com/Avnet/meta-maaxboard.git
# This script automatically fetches Poky and dependencies
```

### Setting up swap memory
Since the build process required more RAM than available, I created a 64 GB swap file to prevent crashes during compilation:
```bash
sudo fallocate -l 64G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
free -h
```
This prevented memory exhaustion when buildling the Yocto image.

### Troubleshooting notes
| Problem | Cause | Fix |
|----------|--------|-----|
| **VM froze during installation** | Too few CPU cores or RAM allocated | Increase to at least 4 cores and 8 GB memory in VirtualBox |
| **Network not working in VM** | NAT adapter misconfigured | Check VirtualBox → Settings → Network → Attached to “NAT” |
| **Out of memory during build** | Swap not configured | Create 64 GB swap file using `fallocate` |
