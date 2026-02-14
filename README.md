# Raspberry Pi 4 — HDMI Not Working (No Signal / Black Screen)

This guide helps you quickly troubleshoot **Raspberry Pi 4 HDMI output issues** (no signal, black screen, wrong resolution, etc.). Most problems come from **cable/port selection, monitor detection, power**, or **display mode configuration**.

---

## ✅ Quick Checklist (Most Common Fixes)

### 1) Confirm you’re using the correct HDMI port
Raspberry Pi 4 has **two micro-HDMI ports**:

- **HDMI0 (primary)** = **closest to the USB-C power port**
- Try **HDMI0 first**

### 2) Check cable & adapter
- Use a proper **micro-HDMI → HDMI cable** (Type D → Type A)
- Avoid cheap adapters + old HDMI cables (can cause “no signal”)
- Try a **different cable** if possible

### 3) Check monitor input & hotplug
- Set the monitor to the correct **HDMI input** (HDMI1/HDMI2)
- Plug HDMI in **before powering** the Pi (some monitors don’t detect hotplug reliably)

### 4) Power supply matters (very common cause)
Pi 4 needs a solid **5V 3A USB-C** power supply.
- Weak power can cause partial boot + **no HDMI output**
- Official Raspberry Pi PSU recommended

---

## 1) Is the Pi Actually Booting?

Look at LEDs:

- **Red PWR LED**: should be **solid**
- **Green ACT LED**: should **blink** during boot/activity

If you can access the Pi over the network:
- Try **ping/SSH** (if SSH was enabled and you know the IP)
  - If it boots but no display → likely HDMI config/cable/monitor
  - If it doesn’t boot → SD card / OS / power issue (HDMI may be fine)

---

## 2) Force HDMI Output (config.txt Fix)

If the Pi boots but the screen stays black or shows “No Signal”, force compatible HDMI settings.

### Where to edit `config.txt`
Depends on your OS version:

- **Raspberry Pi OS Bookworm**: `/boot/firmware/config.txt`
- **Raspberry Pi OS Bullseye / older**: `/boot/config.txt`

💡 You can edit this by inserting the SD card into a PC and opening the **boot** partition.

---

### Option A: Force Safe HDMI Mode (fastest)
Add this near the bottom of `config.txt`:


`hdmi_safe=1`

### Option B: Force HDMI Detection + 1080p (for “No Signal” monitors)

Use this if the display doesn’t detect the Pi:


`hdmi_force_hotplug=1`

`hdmi_group=2`

`hdmi_mode=82`


hdmi_mode=82 = 1080p @ 60Hz

### Option C: If 4K was forced and your monitor can’t handle it

Remove any 4K forcing lines (if present) and use Option B to force 1080p.

3) Rainbow Square → Then Black Screen

If you see a rainbow splash briefly, the Pi likely starts output but switches to a mode your monitor rejects.

Try Option B (1080p) above.

You can also try swapping KMS overlays (advanced; only if needed):

Find this line (if present):

`dtoverlay=vc4-kms-v3d`


Try replacing with:

`dtoverlay=vc4-fkms-v3d`


Then reboot and test.
