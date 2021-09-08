# Hackintosh-OpenCore

## Hardware
- CPU: 3.6 GHz 10-Core Intel Core i9
- Mem: 16 GB 2133 MHz DDR4
- GPU: Radeon RX 580 8 GB
- Motherboard: ROG STRIX Z490-G GAMING (WiFi)
- SSD: Samsung SSD 970 Pro 500GB

## Pre-requisite Knowledge

### Boot Process with OpenCore
- UEFI firmware loads OpenCore EFI
- OpenCore loads as configured by `config.plist`
- OpenCore loads all ACPI and Kexts
- OpenCore loads macOS

### Kexts
[Kexts](https://developer.apple.com/library/archive/documentation/Security/Conceptual/System_Integrity_Protection_Guide/KernelExtensions/KernelExtensions.html), short for Kernel Extensions, are tools used to complement the macOS kernel for certain (extra) functionalities. 

NOTE: **Kexts could have inter-dependencies so they have to be loaded in a particular topological order.** For example, `WhateverGreen.kext` has a dependency on `Lilu.kext`. 
The order is defined by the `config.plist`.

Do note that starting from `macOS 11 Big Sur`, Apple is moving away from kexts and replace them with another concept called [System Extensions](https://developer.apple.com/documentation/systemextensions).

## Kexts Description

### Lilu.kext
- Generic kexts patcher, foundation of a number of other kexts
- Compulsory

### WhateverGreen.kext
- A plug-in of Lilu to select GPUs
- Depends on Lilu
- Compulsory

### VirtualSMC.kext
- Apple SMC simulator in the kernel
- Depends on Lilu
- Compulsory

### SMCProcessor.kext
- Used for monitoring CPU temperature, doesn't work on AMD CPU based systems
- A plug-in of VirtualSMC.kext
- Optional

### SMCSuperIO.kext
- Used for monitoring fan speed
- A plug-in of VirtualSMC.kext
- Optional

### USBInjectAll.kext
- Injects all USB ports to macOS kernals
- Used for initial injection. After boot, it's better to do your own USB mapping and replace this with the `USBMap.kext` mentioned below.

### USBMap.kext
- Custom USB mapping
- Replacing USBInjectAll.kext

### IntelMausi.kext
- For Ethernet
- Required for most of Intel NICs

## References
- [EFI](https://en.wikipedia.org/wiki/EFI_system_partition)
- [UEFI](https://en.wikipedia.org/wiki/Unified_Extensible_Firmware_Interface)
- [Apple deprecating macOS kernel extensions (KEXTs) is a great win for security](https://www.zdnet.com/article/apple-deprecating-macos-kernel-extensions-kexts-is-a-great-win-for-security/)