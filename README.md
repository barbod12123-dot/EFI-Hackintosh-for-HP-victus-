# macOS Sequoia Hackintosh on HP Victus (i5-13420H / RTX 4050)

## Overview
Working EFI for installing **macOS Sequoia** on an **HP Victus laptop** with Intel Core i5-13420H. Installation was performed via the **UnPlugged** method, and several boot/mount issues were resolved along the way.

## Hardware
- **Laptop:** HP Victus
- **CPU:** Intel Core i5-13420H (Raptor Lake, 8C/12T)
- **iGPU:** Intel UHD Graphics (13th Gen)
- **dGPU:** NVIDIA GeForce RTX 4050 (unsupported in macOS)
- **RAM:** 16 GB
- **Ethernet:** [fill in]
- **Wi-Fi/Bluetooth:** [fill in]
- **Audio:** [fill in — codec]
- **Storage:** [fill in — NVMe model]
- 
## What Works
- Boot and installation
- Graphics acceleration via **Intel UHD** (WhateverGreen)
- Ethernet (if compatible kext is included)
- USB
- Keyboard

## Known Issues
- **Audio does not work** (layout-id / AppleALC still being tuned).
- **NVIDIA RTX 4050 is not supported** by macOS. Graphics run on the Intel UHD iGPU; the dGPU is disabled in config.plist.
- [Wi-Fi / Bluetooth — if not working]
- [Sleep/Wake — may require USB mapping]
- [Brightness keys, card reader, etc.]

## Key Fixes
- exFAT partition failed to mount in Recovery (`diskutil mount` errors) — solved with `mount_exfat` and correct Allocation Unit Size (≤1024 KB).
- Kernel process lock (`process 0`) preventing disk erase — resolved after reboot + `diskutil unmountDisk force`.
- UnPlugged temp folder creation failure — solved by setting `TMPDIR` and ensuring the exFAT volume was writable.
- Final install completed via `startosinstall` after expanding `InstallAssistant.pkg` with `pkgutil --expand-full`.
- dGPU disabled via `DeviceProperties` / `boot-args` to prevent conflicts with iGPU acceleration.

## Important Note
**SMBIOS serial numbers have been removed** from `config.plist` before publishing (`SystemSerialNumber`, `MLB`, `SystemUUID`, `ROM`).
You **must** generate your own serials using **GenSMBIOS** or **OpenCore Configurator** before use.

## How to Use
1. Download the EFI (or the release ZIP).
2. Replace SMBIOS serials in `config.plist` with your own.
3. Copy the EFI folder to the EFI partition of your USB drive.
4. BIOS settings:
   - Secure Boot: **Off**
   - CSM/Legacy: **Off**
   - SATA/NVMe mode: **AHCI**
   - Fast Boot: **Off**
   - VT-d: **Off** (or `DisableIoMapper` in config.plist)
5. Boot from USB and install macOS.

## Warning
This EFI is provided for the exact hardware listed. Use on other hardware at your own risk. NVIDIA RTX 4050 will never work on macOS — do not expect dGPU support.

## Credits
Thanks to the OpenCore community, Dortania guides, and the UnPlugged project.
