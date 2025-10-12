# 2 · Building the Yocto Image with BitBake

## Week 2 — Yocto Build Setup and Configuration

### Setting up the Build Environment

MaaXBoard provides a setup script called `maaxboard-setup.sh`, which simplifies the setup process for all MaaXBoard variants.  

The script creates the necessary directory structure and configuration files for the specified machine.

```bash
MACHINE=<machine name> source sources/meta-maaxboard/tools/maaxboard-setup.sh -b <build dir>
```

To build Yocto, I had to initialize the build environment first and allocate the right amount of cores and parallel threads. I ran:
```bash
cd ~/imx-yocto-bsp
MACHINE=maaxboard source sources/meta-maaxboard/tools/maaxboard-setup.sh -b maaxboard/build
```

Whenever I wanted to leave the build environment, I didn’t need to re-run the full setup script. Instead, each time I opened a new terminal or rebooted the VM, I reloaded the build environment using:
```bash
cd ~/imx-yocto-bsp
source sources/poky/oe-init-build-env maaxboard/build
```

### Editing the Build Configuration
The Yocto image is very big and memory intensive. To optimize CPU usage and build speed, I allocated 4 cores, while leaving 1 free.
```bash
PARALLEL_MAKE ?= "-j4"
BB_NUMBER_THREADS ?= "4"
```
Initially, I allocated all the cores into the build and it eventually caused the Ubuntu VM to crash.

When rebuilding, I made sure that at least one core is set free.

### Building the Full BitBake Image
I wanted to build the full image, so that my Maaxboard could run a complete image, including kernels, drivers, and root filesystems, which I will be debugging in the future. I built the full image specifically so I could edit and rebuild kernels later, as well as debug display and hardware issues.

A complete build took three days of constant monitoring to finish.

### Problems with the build
A major problem that I came across was that the build would frequently pause, not cause by user interruptions. I had to clean the image before starting the build again,

```bash
bitbake -c cleanall avnet-image-full
bitbake avnet-image-full
```
The nice part was that the build would continue from the last uncorrupted file instead of starting from the beginning.

Also, I would monitor the CPU and memory usages during tough build sections, like the LLVM and python3 packages.

```bash
top
free -h
```

I would figure out that these packages (LLVM, Python3) had enormous C++ codebase and high memory use. The task appeared to be frozen for hours, stuck at a constant percentage. Example:
```
NOTE: Running task 8700 of 10000: do_compile (python3)
```

### Build outputs
After completion, build outputs appeared under
```
build/tmp/deploy/images/maaxboard/
```
Key files generated included,
1. avnet-image-full-maaxboard.wic — bootable SD card image
3. Image — compiled Linux kernel binary
3. imx8mq-maaxboard.dtb — device tree blob

These files were later used for flashing, kernel testing, and debugging.

### Troubleshooting notes
| Problem | Cause | Fix |
|----------|--------|-----|
| **Build froze for hours** | Large package (LLVM, Qt, Python3) compiling | Wait. Do NOT stop BitBake — it’s still building |
| **System crashed mid-build** | All cores allocated to BitBake | Leave 1 core free (`-j4` if 5 cores total) |
| **`bitbake` command not found** | Environment not sourced | Run `source sources/poky/oe-init-build-env` |
| **Build corrupted after power loss** | Incomplete intermediate packages | Clean and rebuild: `bitbake -c cleanall avnet-image-full` |
