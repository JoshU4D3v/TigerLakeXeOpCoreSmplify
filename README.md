# OpenCore Simplify (Tiger Lake / Iris Xe Mod) 🚀

[![OpenCore](https://img.shields.io/badge/OpenCore-1.0.8-blue.svg)](https://github.com/acidanthera/OpenCorePkg)
[![macOS](https://img.shields.io/badge/macOS-Sonoma%20%7C%20Sequoia%20%7C%20Tahoe-success.svg)](https://apple.com/macos)
[![Hardware](https://img.shields.io/badge/Intel-Tiger%20Lake%20(11th%20Gen)-orange.svg)](https://intel.com)
[![Graphics](https://img.shields.io/badge/Metal-Metal%202%20Accelerated-brightgreen.svg)](https://github.com/b00t0x/WhateverXe)
[![Google Drive](https://img.shields.io/badge/Download-Google%20Drive-4285F4?logo=googledrive&logoColor=white)](https://drive.google.com/file/d/1PfamW2eJGjiveBMQ0GevJvJEWkDQROU4/view?usp=sharing)
[![License](https://img.shields.io/badge/License-MIT-lightgrey.svg)](LICENSE)

An automated toolchain and post-build patcher that brings full **hardware-accelerated graphics (Metal 2, Core Animation, Liquid Glass)** to **Intel 11th Gen (Tiger Lake) Core laptops** using **OpCore Simplify** and the latest **WhateverXe v0.8.0**.

---

## 📥 Download Kit

> [!TIP]
> **Get the complete, pre-configured package** (Includes modded OpCore Simplify, WhateverXe v0.8.0, AMFIPass, OpenCanopy, and automated patchers):
> 
> 📦 **[Download OpCore-Simplify-TigerLake-v0.8.0.zip (Google Drive)](https://drive.google.com/file/d/1PfamW2eJGjiveBMQ0GevJvJEWkDQROU4/view?usp=sharing)**

---

## 🌟 Overview

Historically, 11th Gen Intel laptops (Tiger Lake) were considered a dead-end for Hackintoshes because Apple never natively shipped a Tiger Lake Mac. 

This repository modifies **OpCore Simplify** to bypass hardware blocking checks and provides an automated post-build patcher that configures Apple's internal Tiger Lake driver stack (`WhateverXe` + companion packages), giving you a fully functioning, accelerated Hackintosh out of the box!

---

## 💻 Hardware Compatibility

### Supported Processors:
* **Intel 11th Gen Core (Tiger Lake-U / UP3 / H)**:
  * Core i3-1115G4, i3-1125G4
  * Core i5-1135G7, i5-1145G7, **i5-1155G7**
  * Core i7-1165G7, i7-1185G7, i7-1195G7
* **iGPU Architecture**: Intel Iris Xe-LP (`8086:9A49`, `8086:9A40`, or `8086:9A60`).

### Tested Hardware:
* **Lenovo IdeaPad 3 15ITL6** (i5-1155G7, Iris Xe `8086:9A49`, Realtek ALC257, Intel AC 9560)
* **Dell Latitude 5420** (i5-1145G7, Iris Xe `8086:9A49`, Intel AX201)

### What Works:
* [x] **Full Metal 2 Hardware Graphics Acceleration** (Dock transparency, window blur, liquid glass)
* [x] **Hardware Display & Brightness Controls** (`BrightnessKeys.kext`)
* [x] **Audio Input & Output** (Internal speakers, microphone, 3.5mm combo jack)
* [x] **Wi-Fi & Bluetooth** (Intel AC 9560 / AX201 via `AirportItlwm` or `itlwm`)
* [x] **I2C Precision Touchpad & Multi-touch Gestures** (`VoodooI2C` + `VoodooI2CHID`)
* [x] **Battery Status & Power Management** (`VirtualSMC` + `SMCBatteryManager`)
* [x] **NVMe Storage & Sleep/Wake Management**

---

## ⚙️ BIOS Settings (Required)

Before booting the OpenCore installer, ensure these settings are configured in your BIOS:

| Setting | Required Value | Notes |
| :--- | :--- | :--- |
| **Intel VMD / RST** | **Disabled** | Must use native AHCI NVMe mode |
| **Secure Boot** | **Disabled** | Required for third-party bootloaders |
| **Fast Boot** | **Disabled** | Prevents Windows hybrid sleep conflicts |
| **VT-d** | **Enabled** | (Or `DisableIoMapper = True` in OpenCore) |
| **SATA / Storage Mode** | **AHCI** | |

---

## 🚀 Quick Start Guide

### 1. Prerequisites (Windows)
* Install [Python 3.9+](https://www.python.org/downloads/) (Make sure to check **"Add Python to PATH"** during installation).
* Download and extract the **[OpCore-Simplify-TigerLake-v0.8.0.zip](https://drive.google.com/file/d/1PfamW2eJGjiveBMQ0GevJvJEWkDQROU4/view?usp=sharing)** package from Google Drive.

### 2. Generate the Base EFI
1. Navigate to the `OpCore-Simplify` folder.
2. Run `OpCore-Simplify.bat`.
3. Follow the wizard to detect your laptop's hardware.
   * *Note: The Tiger Lake GPU check will automatically pass without blocking.*
   * Select **AppleALC + SimpleHDA** for Audio (especially if targeting macOS Tahoe).
   * Select **AirportItlwm** (laobamac fork) or **itlwm** for Wi-Fi.
4. When finished, OpCore Simplify will generate your base EFI in `Results\EFI`.

### 3. Run the Automated Tiger Lake Patcher
1. Double-click **`Patch_Lenovo_TigerLake_EFI.bat`** (or pass your EFI folder path).
2. The patcher will automatically:
   * Replace `WhateverGreen` with **`WhateverXe v0.8.0`** + **`AMFIPass v1.4.1 Mod`**.
   * Force `device-id = 499A0000` (`0x9A49`) for direct GPU binding.
   * Apply Tiger Lake booter quirks (`SetupVirtualMap = False`, `RebuildAppleMemoryMap = True`).
   * Inject stable boot-args: `-allow3d -disablegfxfirmware -igfxdvmt -lilubetaall -wex-skip-gputicks ipc_control_port_options=0 amfi=0x80 amfi_get_out_of_my_way=1 -wex-nogva` (no verbose text).
   * Reorder Bluetooth kexts (`Firmware -> Patcher -> BlueToolFixup`).
   * Set SIP `csr-active-config` to `03080000`.
   * Automatically validate with `ocvalidate` (0 errors).
3. The patched EFI will be placed in `EFI Folders\EFI_PATCHED` (or copied directly to your boot drive `D:\EFI`).

---

## 📦 Post-Install (Enabling Full Metal Acceleration)

Once macOS is installed and you reach the desktop:

1. **Install the WhateverXe Graphics Payload**:
   * Double-click and install **`WhateverXe-Graphics-0.4.1.pkg`**.
   * Open **System Settings → Privacy & Security**, scroll to **Security**, and click **Allow** if prompted.
2. **Copy the Metal Error Mode LaunchAgents**:
   * Copy `local.wex.metal-env.plist` and `local.wex.metal-env-xpc.plist` from `PostInstall/LaunchAgents/` to `~/Library/LaunchAgents/`.
3. **Disable GPU S3 Sleep**:
   * Tiger Lake display engines do not support legacy S3 display sleep without crashing. Run this in Terminal:
     ```bash
     sudo pmset -a disablesleep 1 hibernatemode 0
     ```
4. **Restart your Mac**.

### Verify Metal Acceleration:
Run this in Terminal:
```bash
ioreg -lw0 -d1 | grep TGLAcceleratorAdmissionReady
system_profiler SPDisplaysDataType | grep Metal
```
You should see:
```text
"TGLAcceleratorAdmissionReady" = Yes
Metal Support: Metal 2
```

---

## 🛠️ Credits & References

* [b00t0x](https://github.com/b00t0x) for the incredible [WhateverXe](https://github.com/b00t0x/WhateverXe) and [WhateverXe-Graphics](https://github.com/b00t0x/WhateverXe-Graphics) projects.
* [lzhoang2801](https://github.com/lzhoang2801) for the original [OpCore-Simplify](https://github.com/lzhoang2801/OpCore-Simplify) tool.
* [Acidanthera](https://github.com/acidanthera) for OpenCorePkg, Lilu, VirtualSMC, and AppleALC.
* [laobamac](https://github.com/laobamac/itlwm) for modern AirportItlwm builds.
* [tiwarisatyadeep](https://github.com/tiwarisatyadeep) for Tiger Lake kernel hook research and testing.
