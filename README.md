![OpenCore logo](https://github.com/acidanthera/OpenCorePkg/raw/master/Docs/Logos/OpenCore_with_text_Small.png)

# Surface-Go2-macOS
macOS on the Core m3-8100Y Microsoft Surface Go 2 thanks to [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg).

## Disclaimer
This repository is neither a howto nor an installation manual. Using these files requires at least basic knowledge of [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg), ACPI, UEFI and the art of hackintoshing in general. I recommend reading the excellent [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide), as well as all its linked resources.

Although this repository is a fork of [kingo132's Surface Go 2 hackintosh repository](https://github.com/kingo132/surface-go2-hackintosh), my OpenCore EFI folder was built from scratch by following [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide) with the latest releases of [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg) and every required kexts. Nevertheless, [kingo132's repository](https://github.com/kingo132/surface-go2-hackintosh) provided an extremely valuable and helpful resource for building my OpenCore EFI folder and hacking the tablet's UEFI BIOS.

## Recommendations
I would recommend against installing macOS 13.x Ventura at this time, as there are still a few issues that need fixing, mainly a nasty [VoodooInput bug](https://github.com/VoodooI2C/VoodooI2C/issues/507#issuecomment-1367947816) causing a kernel panic and weird behaviour from the [OpenIntelWireless AirportItlwm v2.2.0-alpha](https://github.com/OpenIntelWireless/itlwm/releases/tag/v2.2.0-alpha) Wifi drivers.

I also recommend completely erasing the device's SSD by creating a new GPT partition table before attempting to install macOS, as it makes the installation process much easier. You may use any Linux live ISO with a partitioning tool such as `GParted` or `KPartition` to erase the SSD.

For macOS to be able to boot on the Surface Go 2, the `Secure Boot` option needs to be _**disabled**_ in the BIOS. The boot screen will then display a large red bar with a lock icon at the top of the display when Secure Boot is disabled. This is normal.

Please be aware that all `PlatformInfo` and `SMBIOS` information was removed from the OpenCore `config.plist` files. Users will therefore need to generate their own `PlatformInfo` with [CorpNewt's GenSMBIOS tool](https://github.com/corpnewt/GenSMBIOS) before attempting to boot a Surface Go 2 with this repository's EFI folder.

This repository features an EFI folder with two distinct `config.plist` files. One is meant to be used without the UEFI BIOS hacks below (`config_easy.plist`), the other one (`config_hard.plist`) will only boot after the UEFI hacks have been applied. Simply rename the config file you plan to use to `config.plist` and delete the other config file.

## Software Specifications
| Software         | Version                            |
| ---------------- | ---------------------------------- |
| Target OS        | Apple macOS 12.6.2 Monterey |
| OpenCore         | MOD-OC v0.8.8 |
| SMBIOS           | MacBookAir8,1 |
| SSD format       | APFS file system, GPT partition table |

## Computer Specifications
| Device           | Hardware                           |
| ---------------- | ---------------------------------- |
| CPU              | Intel Core m3-8100Y (1.1 - 3.4 GHz, Amber Lake-Y) |
| iGPU             | Intel UHD Graphics 615 |
| Audio            | Realtek ALC298 |
| RAM              | 2x4 GB LPDDR3 1867 MHz |
| Wifi + Bluetooth | Wifi6 AX200, Bluetooth 5.0 |
| Storage          | Kioxia/Toshiba 128GB SSD |
| USB Type-C 3.1 Gen 1 | Supports Power Delivery and DisplayPort |
| MicroSD Card reader | Realtek PCI-E Card Reader, `152D:1237` |
| Front & Rear camera | Intel(R) AVStream Camera 2500, ISP Interface, `8086:591c`, 8 MPix/5 MPix |
| IR camera | Intel(R) AVStream Camera 2500, ISP Interface, `8086:591c` |
| Keyboard / Trackpad | Microsoft Type Cover, `045E:096F` |
| Display | 10.50 inch 3:2, 1920 x 1280 pixel 220 PPI, 10-Point Capacitive |
| Touchscreen | `ELAN9038`, `\_SB.PCI0.I2C1.TPL1`, `8086:9d61` |
| Screen | BOE CQ NV105WAM-N31, BOE088B |
| NFC | NXP NFC Client Device, NXP3001 |
| Battery | 26.81Wh 7.66v 3500mAh |
| LTE (if available) | Surface Mobile Broadband, USB device, `045E:09A5`, Qualcomm Snapdragon X16 LTE |
| GPS (if available) | `MSHW0142` |
| Accelerometers, gyroscopes, ambient light sensors | |

## What works
- [x] CPU power management (`CPUFriend.kext` with `CPUFriendFriend.kext`)
- [x] CPU SpeedStep (`CPUFriend.kext` with `CPUFriendFriend.kext`)
- [x] iGPU with full acceleration (`WhateverGreen.kext`, `AAPL,ig-platform-id 00001B59`)
- [x] SSD drive (`NVMeFix.kext`)
- [x] USB-C port (`USBMap.kext`)
- [x] WLAN (`AirportItlwm.kext`)
- [x] Bluetooth (`IntelBluetoothFirmware.kext` with `IntelBTPatcher.kext` and `BlueToolFixup.kext`)
- [x] Internal speakers, microphone and Combojack (`AppleALC.kext`, `alcid=3`)
- [x] Power, volume up and volume down buttons (`VoodooPS2.kext`)
- [x] Surface Type Cover keyboard with working brightness, volume and mute keys
- [x] Surface Type Cover trackpad with native multi-touch gestures (`VoodooI2C` with `VoodooI2CHID`)
- [x] Touchscreen (`VoodooI2C.kext` with `VoodooI2CHID.kext`)
- [x] Surface Pen, works but no pressure sensitivity (`VoodooI2C.kext` with `VoodooI2CHID.kext`)
- [x] MicroSD Card reader (`RealtekCardReader.kext` with `RealtekCardReaderFriend.kext`)
- [x] Battery percentage and cycle count (`VirtualSMC.kext` with `SMCBatteryManager.kext`)
- [x] USB Type-C to HDMI
- [x] USB Type-C to USB3 & USB2
- [x] USB Type-C Power Delivery

## What needs some more work
- [ ] Sleep/wake - the device wakes up from sleep displaying the Surface logo on the internal display
- [ ] Type Cover - the caps lock led does not light up when enabled: [issue fired](https://github.com/VoodooI2C/VoodooI2CHID/pull/52#issuecomment-841795827)
- [ ] Speakers - the speakers seem a little too quiet, using another AppleALC `device-id` may help
- [ ] Accelerometers, gyroscope
- [ ] Ambient light sensor

## What will probably never work
- [ ] Front & rear cameras
- [ ] IR camera (Windows Hello)
- [ ] NFC
- [ ] LTE

## Turning off BD PROCHOT
On this device, BD PROCHOT will activate at temperatures as low as 60°C ~ 70°C. When it kicks in, the CPU will throttle down to 0.4Ghz, making the device more or less unusable. That's why BD PROCHOT needs to be disabled in order to increase the performance of the machine.

You may use [arter97's DisablePROCHOT UEFI extension](https://github.com/arter97/DisablePROCHOT) which is already included in the EFI/OC/Drivers folder to disable BD PROCHOT. However, if the CPU continues to be fully loaded with BD PROCHOT disabled, the temperature may increase up to 90°C. Beyond 90°C, the device will become unstable and either crash or power off. To avoid this, the temperature needs to be kept under control.

## Lowering the temperature
By default, the long term TDP of the Surface Go 2 is set to 8W. One way to keep the temperature below 80°C is to set the long term TDP to 7W. Offsetting the CPU voltage by 115mV also helps cooling the device.

To control undervolting and TDP in macOS, there's a handy tool called [VoltageShift](https://github.com/sicreative/VoltageShift).
```
sudo ./voltageshift buildlaunchd -115 0 0 0 0 0 7 28 18 0.002 60
```
To disable and reset the device to its original TDP and voltage:
```
./voltageshift removelaunchd
```
To manually disable and remove the tool:
 ```
sudo rm /Library/LaunchDaemons/com.sicreative.VoltageShift.plist
sudo rm -R /Library/Application\ Support/VoltageShift
```
If you are unable to boot the system after installing, you need to:
1. Shut down the computer (not reboot)
2. Boot to recovery mode with Command-R
3. In the terminal, enable the CSR protection to stop the undervolting daemon from launching at boot 
```
csrutil enable    
```
4. Reboot and delete all the above files

## UEFI BIOS hacks
The UEFI BIOS UI of the Surface Go 2 shows only a few trivial settings. In order to take advantage of better CPU power management and graphics acceleration, there are a few other settings that need to be changed in the UEFI BIOS. The only way to achieve this is to use the [RU tool](http://ruexe.blogspot.com). The [Dortania OpenCore Post-Install guide](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html#turning-off-cfg-lock-manually) has detailed instructions on how to use the RU tool.

_**Please be aware that these hacks may potentially brick your computer! Proceed carefully and only if you know what you are doing!**_

### UEFI BIOS variables which need to be modified
| Name   | From    | To |
| ---------------- | --------- | --------- |
| CFG Lock | Enable | Disable |
| VT-d | Enable | Disable |
| DVMT | 32MB | 64MB |
| Enable Hibernation | Enable | Disable |
| Fast Boot | Enable | Disable |

## Addresses of the UEFI BIOS variables
* CFG Lock: VarStoreInfo (VarOffset/VarName): 0x3C, VarStore: 0x3 (CpuSetup)
* VT-d: VarStoreInfo (VarOffset/VarName): 0xE3, VarStore: 0x2 (SaSetup)
* DVMT Pre-Allocated, VarStoreInfo (VarOffset/VarName): 0xDF, VarStore: 0x2 (SaSetup)
* Enable Hibernation, VarStoreInfo (VarOffset/VarName): 0x1E6, VarStore: 0x1234 (Setup)
* Fast Boot:, VarStoreInfo (VarOffset/VarName): 0x101, VarStore: 0x1234 (Setup)
* Power Limit 1, VarStoreInfo (VarOffset/VarName): 0x57, VarStore: 0x3 (CpuSetup): Default is 0x1f40=8000=8Watt
* Power Limit 2, VarStoreInfo (VarOffset/VarName): 0x5B, VarStore: 0x3 (CpuSetup): Default is 0x4650=18000=18Watt
* Power Limit 1 Time Window, VarStoreInfo (VarOffset/VarName): 0x5F, VarStore: 0x3 (CpuSetup): Default is 0 (infinite)

## Enabling native HiDPI display settings in macOS
On the installed macOS system, the default display resolution is too small for a small device such as the Surface Go 2. To enable the native HiDPI settings in the Display Preferences of macOS, download and run the [one-key-hidpi-sgo2](https://github.com/jlempen/one-key-hidpi-sgo2) script. This fork of [xzhih's one-key-hidpi tool](https://github.com/xzhih/one-key-hidpi) was modified for the Surface Go 2.

## Disabling sleep
https://gist.github.com/pwnsdx/2ae98341e7e5e64d32b734b871614915

## Related issues
* https://github.com/VoodooI2C/VoodooI2CHID/pull/48
* https://github.com/VoodooI2C/VoodooI2C/issues/455
* https://www.reddit.com/r/hackintosh/comments/mwrzxg/intel_igpu_hdmiinternal_screen_problem/
* https://www.reddit.com/r/hackintosh/comments/mrfmmk/surface_go2_hackintosh/
* https://www.reddit.com/r/hackintosh/comments/nm6nh7/surface_go_2_has_anyone_ever_attempted_this/
* https://github.com/linux-surface/linux-surface/wiki/Surface-Go-2

## Useful links
* https://www.anandtech.com/show/6355/intels-haswell-architecture/3

## Related repositories
* https://github.com/kingo132/surface-go2-hackintosh
* https://github.com/stryses/Mi-Laptop-Air12.5-8100Y-Hackintosh
* https://github.com/anonymous5l/macpanda
* https://github.com/jibsaramnim/gpd-pocket2-hackintosh
