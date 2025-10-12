# 6 · Weston and Terminal Debugging

## Week 4 — Debugging the Default Display Environment

After fixing HDMI, the MaaXBoard booted successfully, but Weston opened only a **tiny terminal window in the top-left corner** of the screen.  
It wasn’t practical for testing or debugging because it restricted visibility and didn’t give me full shell access.  
This section covers how I debugged and reconfigured Weston so the board booted directly into a full-screen root console.

---

### Problem: Weston Launching by Default

When `avnet-image-full` booted, it automatically started Weston.
I verified this by checking active services:

```bash
systemctl status weston.service
```
While Weston provided a graphical environment, it wasn’t stable or full-screen on my monitor.
The terminal window that appeared inside Weston was small, laggy, and limited to a pseudo-console.

I wanted it replaced to the default terminal.

### Step 1: Disabling Weston autostart during booting
To prevent Weston from launching automatically at boot, I disabled its systemd service:
```bash
systemctl disable weston.service
```
After rebooting, the board no longer launched the GUI. It booted into a blank screen with no shell.

### Step 2 — Boot Straight to the Root Console
To make the board boot directly into a usable terminal, I changed the system target:
```bash
systemctl set-default multi-user.target
```
After rebooting, the MaaXBoard displayed a full-screen root login directly over HDMI.
Now I could log in and use the terminal without Weston.

After disabling Weston
- The boot time improved.
- I could still manually start Weston later.

ADD PICUTRES

### Troubleshooting notes
| Problem | Cause | Fix |
|----------|--------|-----|
| **Tiny terminal window in Weston** | Weston default layout or incorrect display scaling | Switch to console mode with `systemctl set-default multi-user.target` |
| **No text after disabling Weston** | Boot target set incorrectly | Ensure target is `multi-user.target` (not `graphical.target`) |