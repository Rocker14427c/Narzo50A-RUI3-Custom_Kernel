<div align="center">

<img src="https://capsule-render.vercel.app/api?type=venom&color=gradient&customColorList=1,4,12&height=220&section=header&text=Narzo%2050A%20Kernel&fontSize=52&fontColor=fff&animation=twinkling&fontAlignY=36&desc=RUI3%20·%20Android%2012%20·%20Linux%204.14&descAlignY=62&descSize=18" width="100%"/>

<p>
  <img src="https://img.shields.io/badge/Kernel-4.14.186-007ec6?style=for-the-badge&logo=linux&logoColor=white"/>
  <img src="https://img.shields.io/badge/Firmware-RUI%203-e11d48?style=for-the-badge&logo=android&logoColor=white"/>
  <img src="https://img.shields.io/badge/Android-12-3ddc84?style=for-the-badge&logo=android&logoColor=white"/>
  <img src="https://img.shields.io/badge/SoC-Helio%20G85-10b981?style=for-the-badge&logo=arm&logoColor=white"/>
</p>

<p>
  <img src="https://img.shields.io/badge/Root-ReSukiSU-22c55e?style=flat-square&logo=shield-halved&logoColor=white"/>
  <img src="https://img.shields.io/badge/Stealth-SuSFS-3b82f6?style=flat-square&logo=ghost&logoColor=white"/>
  <img src="https://img.shields.io/badge/License-GPL--2.0-64748b?style=flat-square&logo=gnu"/>
  <img src="https://img.shields.io/github/last-commit/Rocker14427c/Narzo50A-RUI3-Kernel_Builder?style=flat-square&color=6366f1&logo=git&logoColor=white"/>
</p>

---

### *A clean, stable custom kernel with ReSukiSU & SuSFS for Realme Narzo 50A on Realme UI 3*

</div>

---

## 📱 Supported Devices

<div align="center">

| Device | Model | Codename | SoC | Status |
|:---|:---:|:---:|:---:|:---:|
| **Realme Narzo 50A** | `RMX3430` | `even` | Helio G85 | ✅ Stable |
| **Realme C25s** | `RMX3195` · `RMX3197` | `even` | Helio G85 | ✅ Stable |

> **Required Firmware:** Realme UI 3 (Android 12)

</div>

---

## ✨ Features

<table>
<tr>
<td width="50%" valign="top">

### 🛡️ Root & Stealth

- ✅ **ReSukiSU** — Kernel-level root access
- ✅ **SuSFS** — Advanced root hiding
  - Hide suspicious paths
  - Hide suspicious mounts
  - Spoof file statistics
  - Spoof kernel version
  - Hide kernel symbols
  - Redirect file opens
  - Hide memory maps
  - Spoof cmdline/bootconfig

</td>
<td width="50%" valign="top">

### 📦 Build Info

- **Kernel:** Linux 4.14.186
- **Defconfig:** `k68v1_64_defconfig`
- **Multi-Manager:** Supported
- **Flash:** AnyKernel3 zip
- **Logging:** Enabled

</td>
</tr>
</table>

---

## 📋 Changelog

| Commit | Description |
|:---|:---|
| `e71c3de1` | 📝 README for Realme Narzo 50A kernel |
| `cbe88098` | 🔒 Add ReSukiSU and SuSFS support |
| `0a48e08a` | 🔀 Merge pull request — community contribution |
| `c35da88e` | 🔧 Fix header path for `oplus_battery_*` drivers |
| `17dcd6b3` | 📤 Initial kernel source upload (C25/C25s/Narzo 50A) |

---

## 📥 Installation

```bash
# 1. Download the kernel zip from Releases
# 2. Reboot to TWRP / OrangeFox recovery
# 3. Flash the AnyKernel3 zip
# 4. Reboot
# 5. Install ReSukiSU Manager
# 6. (Optional) Flash susfs4ksu module for full hiding
```

> [!WARNING]
> **This kernel is for RUI3 (Android 12) only. Do NOT flash on RUI4.**

---

## 🔨 Build from Source

```bash
git clone --recurse-submodules https://github.com/Rocker14427c/Narzo50A-RUI3-Kernel_Builder.git
cd Narzo50A-RUI3-Kernel_Builder

export ARCH=arm64
export CROSS_COMPILE=aarch64-linux-gnu-

make O=out k68v1_64_defconfig
make -j$(nproc) O=out
```

---

## 🙏 Credits & Thanks

| | Contributor | Contribution |
|:---:|:---|:---|
| 📤 | **sarthakroy2002** | Original kernel source upload & community contributions |
| 🔒 | **ReSukiSU Team** | KernelSU fork with SuSFS inline hook support |
| 🛡️ | **simonpunk** | SuSFS — filesystem-level root hiding |
| 📦 | **osm0sis** | AnyKernel3 flashable zip template |
| ⚙️ | **MediaTek** | MT6768 (Helio G85) BSP |
| 📱 | **Realme / Oplus** | Device-specific kernel sources |

---

## ⚠️ Disclaimer

```c
/*
 * Your warranty is now void.
 * I am not responsible for bricked devices, dead SD cards,
 * or any damage caused by flashing this kernel.
 * YOU are choosing to make these modifications.
 */
```

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=venom&color=gradient&customColorList=1,4,12&height=80&section=footer&text=Made%20with%20❤️%20for%20the%20Narzo%2050A%20Community&fontSize=16&fontColor=fff&animation=twinkling" width="100%"/>

**⭐ Star this repo if it helped you!**

<p>
  <a href="https://github.com/Rocker14427c/Narzo50A-RUI3-Kernel_Builder/stargazers">
    <img src="https://img.shields.io/github/stars/Rocker14427c/Narzo50A-RUI3-Kernel_Builder?style=social"/>
  </a>
  <a href="https://github.com/Rocker14427c/Narzo50A-RUI3-Kernel_Builder/fork">
    <img src="https://img.shields.io/github/forks/Rocker14427c/Narzo50A-RUI3-Kernel_Builder?style=social"/>
  </a>
</p>

</div>
