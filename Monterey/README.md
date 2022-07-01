# MacOS Monterey
## IMPORTANT
Read everything before attempting else you'll look like a clown. I spent hours getting this to work and this was my first time making this so if there are any issues or modifications to be made, do tell!
## Details
Tested with macOS Monterey 12.4 (2nd July 2022)
![Screen Shot 2022-07-02 at 1 28 27 AM](https://user-images.githubusercontent.com/32519167/176961262-59256da1-a6e3-4f85-bebe-ace7405b3633.png)
### Hardware Details
* CPU: Intel Core i7-8750H
* iGPU: Intel UHD Graphics 630
* dGPU: NVIDIA GeForce RTX 2060 (Disabled with -wegnoegpu)
* WiFi Chip: Killer Wireless AC-1550 (9260NGW)
* Ethernet: Killer E2500
* Audio: Realtek ALC289
### BIOS
* Version 2.13.0
* Secure Boot: Off
## Working
* Wi-Fi
* Bluetooth
* Audio
* Headphones (Headset mic doesn't seem to be working)
* Front Camera
## Tutorial Used
https://dortania.github.io/OpenCore-Install-Guide/prerequisites.html
https://dortania-github-io.thrrip.space/OpenCore-Install-Guide/extras/monterey.html
## Tools Used
* [OpenCore v0.8.1-RELEASE](https://github.com/acidanthera/OpenCorePkg/releases) The heart and soul
* [gibMacOS](https://github.com/corpnewt/gibMacOS/) Used for downloading MacOS (You can also use the method OpenCore uses too)
* [proprTree](https://github.com/corpnewt/ProperTree) Plist editing
* [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) Generate SMBIOS Data
## Kexts Used
* WiFi - [Airportltlwm v2.1.0](https://github.com/OpenIntelWireless/itlwm/releases)
* Audio - [AppleALC v1.7.2](https://github.com/acidanthera/AppleALC/releases)
* Bluetooth -
1 - [IntelBluetoothFirmware v2.1.0](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)
2 - [BrcmPatchRAM v2.6.2](https://github.com/acidanthera/BrcmPatchRAM/releases)
With Monterey, Apple has completely rewritten the bluetooth stack. As of writing, many bluetooth devices do not work (legacy Broadcom and Intel). With the rewrite, injector kexts break bluetooth support in Monterey, though firmware uploader kexts are still needed. Make sure that you disable injector kexts which is IntelBluetoothInjector.kext for Intel cards (Already done in config.plist)
* Required for AppleALC, WhateverGreen, VirtualSMC and many other kexts - [Lilu v1.6.0](https://github.com/acidanthera/Lilu/releases)
* Ethernet - [RealtekRTL8111 v2.4.2](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases)
* USB - [RehabMan-USBInjectAll v2018-1108](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/)
* Emulate SMC Chip - [VirtualSMC v1.2.9](https://github.com/acidanthera/VirtualSMC/releases)
* I2C/USB HID Devices - [VoodooI2C v2.7](https://github.com/VoodooI2C/VoodooI2C/releases)
* PS2 Keyboards/Trackpads - [VoodooPS2Controller v2.2.8](https://github.com/acidanthera/VoodooPS2/releases)
* Graphics - [WhateverGreen v1.5.9](https://github.com/acidanthera/WhateverGreen/releases)
## SSDT Used
https://dortania.github.io/Getting-Started-With-ACPI/ssdt-methods/ssdt-prebuilt.html#laptop-coffee-lake-8th-gen
## Config.plist Editing
Followed this
https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/coffee-lake.html#starting-point
and it's pretty much good except for:
#### 1.
In `Booter -> Quirks` it should be
* EnableWriteUnprotector -> True
* RebuildAppleMemoryMap -> False
* SyncRuntimePermissions -> False

as I had [OCABC: MAT support is 0](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/extended/kernel-issues.html#kernel-panic-on-invalid-frame-pointer)
#### 2.
In `PlatformInfo -> Generic`,
Change SystemSerialNumber, MLB and SystemUUID from the ones generated in GenSMBIOS. Model should be MacBookPro15,1 for good compatibility.
## EFI and USB is ready, yay!
You are ready padawan. Run the installer from the USB to install MacOS and follow the [post install](https://dortania.github.io/OpenCore-Post-Install/) guide to boot directly from hard drive instead of USB, use GUI based boot screen, audio issues, etc. You can also replace the [Debug version of Opencore with Release](https://caizhiyuan.gitee.io/opencore-install-guide/troubleshooting/debug.html) to prevent creation of log files and remove the delay at start
