# RaspberryPi4-no-HDMI-error-Fix
how to solve the HDMI error in Raspberry-Pi in config

1) First: is the Pi actually booting?

Red PWR LED should be solid.

Green ACT LED should blink during boot.

If you can, try ping/SSH from another device (if you previously enabled SSH / know the IP).

If it boots but no display → likely HDMI config/cable/monitor.

If it doesn’t boot → SD card / power / OS issue (HDMI may be fine).

2) Common hardware gotchas (most common fixes)
Use the correct HDMI port

Pi 4 has two micro-HDMI ports:

HDMI0 (primary) = the port closest to the USB-C power.

Try HDMI0 first.

Cable/adapter issues

Use a proper micro-HDMI → HDMI cable (Type D to Type A).

Avoid cheap adapters + old HDMI cables (they often cause “no signal”).

Try a different cable if possible.

Monitor/input issues

Manually set your monitor to the correct HDMI input (HDMI1/HDMI2).

Plug HDMI in before powering the Pi (some monitors don’t hot-detect well).

Power issues (very common)

Pi 4 needs a solid 5V 3A USB-C supply (official PSU is best).

Weak power can boot partially and give no HDMI.

3) Force HDMI output via config.txt (software fix)

If the Pi boots but the screen stays black / “no signal”, force a safe mode.

Where the file is (depends on OS)

Raspberry Pi OS Bookworm: /boot/firmware/config.txt

Raspberry Pi OS Bullseye/older: /boot/config.txt

You can edit it by putting the SD card into a PC (open the “boot” partition), then add ONE of these options near the bottom:

Option A (quickest): safe HDMI mode

Add:

hdmi_safe=1


Boot and test. (This forces very compatible settings.)

Option B: force hotplug + 1080p

If your monitor doesn’t detect:

hdmi_force_hotplug=1
hdmi_group=2
hdmi_mode=82


(82 = 1080p 60Hz)

Option C: if you accidentally forced 4K and your monitor can’t do it

Remove any 4K forcing lines, or try 1080p settings above.

4) If it shows rainbow square then goes black

That often means it starts video output then the OS switches modes and the monitor refuses it.

Try Option B above (force 1080p).

If you’re using very new Pi OS graphics stack, you can also try switching KMS overlays (only if you already have display via SSH or can edit config):

In config.txt, look for:

dtoverlay=vc4-kms-v3d (common default)

You can try:

dtoverlay=vc4-fkms-v3d

Reboot and test.

5) Still dead on both HDMI ports?

Try this quick isolation:

Test the same monitor + cable with another device (laptop/console).

Test the Pi with another monitor/TV.

If both ports never work across known-good cables/monitors and the Pi boots headless, it could be port damage or rare board fault.
