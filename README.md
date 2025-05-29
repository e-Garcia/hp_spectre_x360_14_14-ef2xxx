# HP Spectre x360 14 (14-ef2xxx) — Linux Configuration (Ubuntu 24.04 Tested)

This repository documents a working Linux setup for the **HP Spectre x360 14-ef2xxx**, tested with **Ubuntu 24.04.2 LTS** and **kernel 6.11 (mainline)**.  
It includes device-specific workarounds—especially for **audio**—tailored to this model featuring a **13th Gen Intel i7-1355U** and **Realtek ALC245** codec.

In this repository, I’ve adapted prior community findings and added new fixes specific to this hardware variant.

---

## ✅ What Works

- Display with full resolution and brightness control
- Touchscreen and pen input
- Wi-Fi and Bluetooth
- Suspend/resume
- Power management and thermals
- **Audio (via manual SOF disable + legacy HDA driver fix)**

## 🔬 Research In Progress

- Webcam
- Fingerprint reader (tested with `fprintd`, currently not functional)

---

## 🔊 Audio Fix for Realtek ALC245

### 🐛 The Problem

By default, the system loads the **SOF (Sound Open Firmware)** driver stack with PipeWire. However:

- No analog stereo or internal speaker output appears
- `pavucontrol` shows no usable devices
- `aplay -l` lists `sof-hda-dsp`, but it's unusable

### ✅ The Solution: Disable SOF and Force Legacy HDA Driver

1. Edit/create the modprobe config file:

```bash
sudo nano /etc/modprobe.d/alsa-base.conf
```

2. Add this line to disable SOF and enable the legacy HDA driver:

```bash
options snd-intel-dspcfg dsp_driver=1
```

3. Regenerate initramfs and reboot:

```bash
sudo update-initramfs -u
sudo reboot
```

4. After reboot, verify with:

```bash
aplay -l
```

You should see something like:

```
card 0: PCH [HDA Intel PCH], device 0: ALC245 Analog
```

🎉 Internal speakers should now work correctly.

---

## 🧰 System Specs

- **Model**: HP Spectre x360 14-ef2xxx
- **CPU**: Intel i7-1355U (13th Gen)
- **Audio Codec**: Realtek ALC245
- **Graphics**: Intel Iris Xe
- **Display**: 13.5" 3:2 OLED Touch
- **OS**: Ubuntu 24.04.2 LTS
- **Kernel**: 6.11 (mainline)

---

## 🛠 Tips

- PipeWire + WirePlumber works fine after the SOF override
- Use `power-profiles-daemon` or `TLP` for better battery life
- Consider disabling Secure Boot for kernel module overrides

---

## 📎 Sources & References

- [My LinkedIn post documenting the fix](https://www.linkedin.com/posts/egarciaga_how-i-fixed-audio-on-my-hp-spectre-x360-running-activity-7320823718207782940-o8hd)
- [SOF documentation](https://www.sofproject.org/)
- [Ubuntu PipeWire wiki](https://wiki.ubuntu.com/Audio/PipeWire)

---

## 🙏 Acknowledgments

Thanks to the Linux and open-source community. If you're using this config or hit issues, feel free to open an issue or connect via LinkedIn. Contributions welcome!
