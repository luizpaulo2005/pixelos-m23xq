# PixelOS for Samsung Galaxy M23 5G (m23xq)

Unofficial PixelOS builds for the Samsung Galaxy M23 5G (SM-M236B, m23xq).

PixelOS is an AOSP-based custom ROM that brings the Pixel look and features, with Google apps included.

## Current build: PixelOS 17

| | |
|---|---|
| **Device** | Samsung Galaxy M23 5G (SM-M236B, m23xq) |
| **Tested on** | SM-M236B/DS, Brazilian variant only. It should also work on the Galaxy F23 5G (SM-E236B), but that is untested. |
| **Android** | 17 (security patch 2026-11-01) |
| **Kernel** | Linux 4.19 |
| **Based on** | PixelOS 17 (seventeen) and akifakif32's m23xq sources |
| **Build type** | user, signed with private release keys |
| **Status** | Beta |
| **Maintainer** | [luizpaulo2005](https://github.com/luizpaulo2005) |

## Requirements

- Unlocked bootloader. **Unlocking trips Knox permanently.**
- **Latest stock firmware (One UI 6.1, Android 14).** Tested with baseband `M236BXXSEEZB1`. These builds don't include firmware (modem, bootloader). Flash the latest stock firmware for your region first.

## Downloads

- **ROM zip:** [SourceForge](https://sourceforge.net/projects/pixelos-m23xq/files/17/)
- **Images and source manifest:** [GitHub Releases](https://github.com/luizpaulo2005/pixelos-m23xq/releases)
- **Support and discussion:** [XDA thread](https://xdaforums.com/t/rom-17-unofficial-pixelos-17-for-samsung-galaxy-m23-5g-m23xq.4808278/)

| File | Use |
|---|---|
| `recovery-vbmeta.tar` | Recovery and vbmeta for Odin (AP slot) |
| `recovery.img` | Recovery (Heimdall) |
| `vbmeta.img` | Verified boot metadata (Heimdall) |
| `boot.img` | Kernel (already inside the zip) |
| `dtbo.img` | Device tree overlay (already inside the zip) |
| `build-manifest.xml` | Exact commit of every repository in the build |
| `sha256sums.txt` | Checksums |

## What works

Tested with a SIM card:

- Boot, SELinux Enforcing, file-based encryption, verified boot (green)
- Calls, SMS, mobile data (LTE with carrier aggregation)
- Wi-Fi, Bluetooth, NFC
- Cameras, audio, fingerprint, sensors, charging
- USB file transfer (MTP)
- Google apps and Play Store

## Not tested yet

- VoLTE and Wi-Fi calling (no VoLTE coverage in the maintainer's area). The 4G calling option is present. Reports are welcome.

## Known issues

- Vibration didn't work on the test unit, but that unit doesn't vibrate in the Samsung bootloader either (hardware fault). Reports from other units are welcome.
- Reconnecting to some WPA3 (SAE) networks may fail with "wrong password". WPA2 works.

Please report other issues with logs.

## Installation

Clean install. Your bootloader must be unlocked. **This wipes all your data.**

1. Make sure the phone is on the latest stock firmware.
2. Download the zip and either `recovery-vbmeta.tar` (Odin) or `recovery.img` and `vbmeta.img` (Heimdall).
3. Enter Download Mode: power off, hold **Volume Up + Volume Down** and plug in the USB cable, then press **Volume Up** to continue.
4. Flash recovery and vbmeta **without rebooting**:
   - **Odin (Windows):** put `recovery-vbmeta.tar` in **AP**, untick **Auto Reboot** in Options, then press Start.
   - **Heimdall (Linux/macOS):**
     ```
     heimdall flash --RECOVERY recovery.img --VBMETA vbmeta.img --no-reboot
     ```
5. Hold **Volume Down + Power** until the screen turns off, then **immediately** hold **Volume Up + Power** to boot into recovery. If stock boots first, it restores its own recovery and you need to repeat step 4.
6. In recovery: **Factory reset > Format data / factory reset**.
7. **Apply update > Apply from ADB**, then on your computer:
   ```
   adb sideload PixelOS_m23xq-17.0-<date>-UNOFFICIAL-signed.zip
   ```
8. **Reboot system now.** The first boot takes a few minutes.

## Updating

From a previous build of this ROM: Recovery > **Apply update > Apply from ADB** > `adb sideload` the new zip > Reboot. No wipe needed.

## Changes in these builds

- Ported akifakif32's device tree to PixelOS.
- Moved the kernel to the samsung-sm7225 `lineage-23.2-rebase` base, which the Android 17 BPF loader needs.
- Fixed a GPU (kgsl) memory leak that froze the phone after a few minutes of use.
- Fixed USB data (MTP and adb): restored Samsung's USB notify after probe and disabled the lock screen USB restriction.
- Fixed freezes during app installs (f2fs writeback limits).
- Fixed a first-boot bootloop (casefolding needs `CONFIG_UNICODE`).
- Fixed Wi-Fi disconnecting in standby (WoWLAN triggers).
- Removed the LineageOS SDK dependencies from Samsung Doze.
- Signed with private release keys.

## Sources

- Device tree: [luizpaulo2005/android_device_samsung_m23xq](https://github.com/luizpaulo2005/android_device_samsung_m23xq/tree/17.0-rebase)
- Vendor: [luizpaulo2005/android_vendor_samsung_m23xq](https://github.com/luizpaulo2005/android_vendor_samsung_m23xq/tree/17.0)
- Kernel: [luizpaulo2005/android_kernel_samsung_sm7225](https://github.com/luizpaulo2005/android_kernel_samsung_sm7225/tree/seventeen)
- Samsung hardware: [luizpaulo2005/android_hardware_samsung](https://github.com/luizpaulo2005/android_hardware_samsung/tree/lineage-24.0)
- IMS: [krazey/ImsStack](https://github.com/krazey/ImsStack) and [krazey/ImsMedia](https://github.com/krazey/ImsMedia)
- Local manifest: [m23xq.xml](m23xq.xml)
- PixelOS: [PixelOS-AOSP](https://github.com/PixelOS-AOSP)

## Credits

- PixelOS team
- akifakif32 for the m23xq device and vendor sources
- samsung-sm7225 kernel maintainers
- krazey for the IMS modules
- LineageOS
