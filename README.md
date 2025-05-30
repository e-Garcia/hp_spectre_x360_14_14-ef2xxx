# HP Spectre x360 14 (14-ef2xxx) — Linux Configuration (Ubuntu 24.04 Tested)

This repository documents a working Linux setup for the **HP Spectre x360 14-ef2xxx**, tested with **Ubuntu 24.04.2 LTS** and **kernel 6.11 (mainline)**.  
It includes device-specific workarounds—especially for **audio**—tailored to this model featuring a **13th Gen Intel i7-1355U** and **Realtek ALC245** codec.

In this repository, I’ve adapted prior community findings and added new fixes specific to this hardware variant.

---

## ✅ What Works

Tested with **Ubuntu 24.04 LTS** + **Kernel 6.11 (mainline)**

| Feature           | Status             | Notes                                                    |
|-------------------|--------------------|----------------------------------------------------------|
| CPU/GPU           | ✅ Working          | Full performance and graphics acceleration               |
| Touchscreen       | ✅ Working          | ELAN I2C HID touchscreen with multitouch and stylus      |
| Touchpad          | ✅ Working          | Synaptics I2C; supports gestures and palm rejection      |
| Wi-Fi / Bluetooth | ✅ Working          | Requires firmware for Intel AX211                        |
| Audio             | ⚠️ Partially Working| Output works; input/mic may require patches/quirks       |
| Webcam            | ❌ Not Working      | Likely MIPI/IPU6-based, no Linux support yet             |
| Suspend/Resume    | ✅ Mostly Working   | Occasionally needs power tuning                          |
| Thunderbolt 4     | ✅ Working          | Hotplug and docks supported                              |
| Sensors / HID     | ✅ Working          | Lid switch, brightness keys, HID 5-button array          |


## 🔬 Research In Progress

- Webcam
- Fingerprint reader (tested with `fprintd`, currently not functional)

---

## 🖥️ Hardware Overview

| Component        | Description                                   |
|------------------|-----------------------------------------------|
| Model            | HP Spectre x360 14-ef2xxx (2023)              |
| CPU              | Intel Core i7-1355U (2P+8E hybrid, Raptor Lake) |
| GPU              | Intel Iris Xe Graphics                        |
| RAM              | 16 GiB LPDDR4x (soldered, dual-channel)       |
| SSD              | WD PC SN810 NVMe 512 GB                       |
| Wi-Fi/Bluetooth  | Intel AX211 (Wi-Fi 6E + Bluetooth 5.3)        |
| Touchpad         | Synaptics SYNA32E9 (I2C)                      |
| Touchscreen      | ELAN2513:00 (I2C HID, with stylus)            |
| Audio            | Intel HDA (Raptor Lake-P/U/H cAVS)            |
| Camera           | MIPI sensor (likely IPU6, not working yet)    |
| Ports            | 2x Thunderbolt 4, 1x USB-A, microSD, Audio Jack |

---

## 🔧 Configuration Notes

### Audio (Intel HDA / Cirrus)
- Detected as: `Raptor Lake-P/U/H cAVS`
- Mic may not function correctly without:
  - Patching the kernel or using `alsa-ucm-conf`
  - Firmware updates (may require proprietary DSP firmware)
#### Audio Fix
Read - [My LinkedIn post documenting the fix](https://www.linkedin.com/posts/egarciaga_how-i-fixed-audio-on-my-hp-spectre-x360-running-activity-7320823718207782940-o8hd)
A dedicated instructions file will be available here soon.

### Touchpad (Synaptics SYNA32E9)
- Full gesture support via `libinput`
- Palm rejection improved with custom udev rules or a systemd service (included in this repo)

### Touchscreen & Stylus
- ELAN2513:00 over I2C
- Supports pen input, multi-touch, pressure sensitivity

### Wi-Fi / Bluetooth
- Intel AX211 (CNVi interface)
- Requires `iwlwifi-ty-a0-gf-a0-*.ucode` from `linux-firmware` (included in Ubuntu 24.04)

### Camera (MIPI/IPU6)
- Currently **non-functional**
- May need [ipu6-camera-bin](https://github.com/intel/ipu6-camera-bin) and kernel patches (TBD)

---

## 🛠 Tips

- PipeWire + WirePlumber works fine after the SOF override
- Use `power-profiles-daemon` or `TLP` for better battery life
- Consider disabling Secure Boot for kernel module overrides

---


## 🛠 Recommended Diagnostic Commands

To list and verify your hardware:

```bash
sudo lshw -short
lspci
lsusb
lsblk
dmesg | grep -iE 'error|fail|firmware'
```

## 📎 Sources & References

- [My LinkedIn post documenting the fix](https://www.linkedin.com/posts/egarciaga_how-i-fixed-audio-on-my-hp-spectre-x360-running-activity-7320823718207782940-o8hd)
- [SOF documentation](https://www.sofproject.org/)
- [Ubuntu PipeWire wiki](https://wiki.ubuntu.com/Audio/PipeWire)

---

## 🙏 Acknowledgments

Thanks to the Linux and open-source community. If you're using this config or hit issues, feel free to open an issue or connect via LinkedIn. Contributions welcome!
