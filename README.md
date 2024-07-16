![OpenCore logo](https://github.com/acidanthera/OpenCorePkg/raw/master/Docs/Logos/OpenCore_with_text_Small.png)

# Surface-Go-2-OpenCore
macOS on the Core m3-8100Y Microsoft Surface Go 2 thanks to [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg).

## Abstract
Apart from the front and rear cameras, the IR camera (Windows Hello) and the LTE modem, everything is working perfectly like on a real MacBook Air/Pro. The battery runtime is around four to five hours.

## Disclaimer
This repository is neither a howto nor an installation manual. Using these files requires at least basic knowledge of [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg), ACPI, UEFI and the art of hackintoshing in general. I recommend reading the excellent [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide), as well as all its linked resources. For those who wish to improve their hackintoshing knowledge, [5T33Z0's OC-Little-Translated](https://github.com/5T33Z0/OC-Little-Translated) repository is the most comprehensive resource I've found on the subject.

Although this repository is a fork of [kingo132's Surface Go 2 hackintosh repository](https://github.com/kingo132/surface-go2-hackintosh), my OpenCore EFI folder was built from scratch by following [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide) with the latest releases of [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg) and every required kexts. Nevertheless, [kingo132's repository](https://github.com/kingo132/surface-go2-hackintosh) provided an extremely valuable and helpful resource for building my OpenCore EFI folder and hacking the tablet's UEFI Firmware.

## Recommendations
I recommend completely erasing the device's SSD by creating a new GPT partition table before attempting to install macOS, as it makes the installation process much easier. You may use any Linux live ISO with a partitioning tool such as `GParted` or `KPartition` to erase the SSD.

For macOS to be able to boot on the Surface Go 2, the `Secure Boot` option _**must be disabled**_ in the UEFI. The boot screen will then display a large red bar with a lock icon at the top of the display when Secure Boot is disabled. This is normal.

Please be aware that all `PlatformInfo` and `SMBIOS` information was removed from the OpenCore `config.plist` files. Users will therefore need to generate their own `PlatformInfo` with [CorpNewt's GenSMBIOS tool](https://github.com/corpnewt/GenSMBIOS) before attempting to boot a Surface Go 2 with this repository's EFI folder.

The `kexts` required to enable the trackpad and the touchscreen are special versions of `VoodooI2C.kext` and `VoodooI2CHID.kext` [patched and improved by lazd](https://github.com/jlempen/Surface-Go-2-OpenCore/issues/1#issuecomment-1705597716). Updating those `kexts` with the official ones from [the VoodooI2C repo](https://github.com/VoodooI2C) will most certainly break trackpad and touchscreen functionality! They were renamed to `VoodooI2C-SurfaceTouch.kext` and `VoodooI2CHID-SurfaceTouch.kext` so as not to be auto-updated by tools such as [OCAT](https://github.com/ic005k/OCAuxiliaryTools).

`AirportItlwm-Ventura.kext`, `AirportItlwm-Sonoma140.kext` and `AirportItlwm-Sonoma144.kext` from the [OpenIntelWireless repo](https://github.com/OpenIntelWireless/itlwm) are required to enable the Wifi chip and were renamed for the same reason. This EFI will dynamically load the appropriate kext for macOS Ventura or Sonoma depending on the running kernel. No need to manually replace the kext file when updating your version of macOS.

## Software Specifications
| Software         | Version                            |
| ---------------- | ---------------------------------- |
| Target OS        | Apple macOS 13.6.7 Ventura and 14.5 Sonoma |
| OpenCore         | [MOD-OC v1.0.1](https://github.com/wjz304/OpenCore_NO_ACPI_Build/releases/download/1.0.1_08932be/OpenCore-Mod-1.0.1-RELEASE.zip) |
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
| MicroSD Card reader | Realtek PCI-E Card Reader |
| Front & Rear camera | Intel(R) AVStream Camera 2500, ISP Interface, 8 MPix/5 MPix |
| IR camera | Intel(R) AVStream Camera 2500, ISP Interface |
| Keyboard / Trackpad | Microsoft Type Cover |
| Display | 10.50 inch 3:2, 1920 x 1280 pixel 220 PPI, 10-Point Capacitive |
| Touchscreen | `ELAN9038`, `\_SB.PCI0.I2C1.TPL1` |
| Screen | BOE CQ NV105WAM-N31, BOE088B |
| NFC | NXP NFC Client Device, NXP3001 |
| Battery | 26.81Wh 7.66v 3500mAh |
| LTE (if available) | Surface Mobile Broadband, USB device, Qualcomm Snapdragon X16 LTE |
| GPS (if available) | |
| Accelerometers, gyroscopes, ambient light sensors | |

## What works
- [x] CPU power management
- [x] CPU SpeedStep
- [x] iGPU with full acceleration
- [x] SSD drive
- [x] USB-C port
- [x] WLAN
- [x] Bluetooth
- [x] Internal speakers, microphone and Combojack
- [x] Power, volume up and volume down buttons
- [x] Surface Type Cover keyboard with working brightness, volume and mute keys, working caps lock light
- [x] Surface Type Cover trackpad with native multi-touch gestures
- [x] Touchscreen
- [x] Surface Pen
- [x] MicroSD Card reader
- [x] Battery percentage and cycle count
- [x] Hibernation (hibernatemode 25) - the device successfully wakes up from hibernation mode
- [x] USB Type-C to HDMI
- [x] USB Type-C to USB3 & USB2
- [x] USB Type-C Power Delivery

## What needs some more work
- [ ] Sleep (hibernatemode 3) - the device wakes up from sleep displaying the Surface logo on the internal display
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

The DisablePROCHOT.efi driver is included but disabled in both config.plist files, as enabling it leads to the CPU overheating and the computer shutting down during install. You may enable the driver once macOS is installed on the device.

## Undervolting to reduce heat and improve performance
The `VoltageShift.kext` undervolting tool is already included and enabled in this repository's `Kexts` folder. To be able to launch the `voltageshift` command line tool from anywhere, copy [VoltageShift from the Tools folder](https://github.com/jlempen/Surface-Go-2-OpenCore/blob/main/Tools/VoltageShift-EFI.zip) to your `/usr/local/bin` folder by entering `sudo cp voltageshift /usr/local/bin/` in the macOS terminal after navigating to the folder where you downloaded and unzipped the `voltageshift` executable file.

Please refer to the instructions found in the [VoltageShift repository](https://github.com/sicreative/VoltageShift), as well as to the excellent howto found in [zearp's repository](https://github.com/zearp/Nucintosh#undervolting).

## Disabling CFG Lock to improve Power Management
1. Boot into OpenCore
2. Press Space to see the list of tools
3. Select `Disable CFG Lock`
4. Press enter
5. Restart

To verify this worked, press Space and select the `Check CFG Lock State` tool -- if it was successful, you'll see:

> This firmware has UNLOCKED MSR 0xE2 register!

Finally, edit `config.plist` and set `Kernel -> Quirks -> AppleCpuPmCfgLock` and `Kernel -> Quirks -> AppleXcpmCfgLock` to `false` and reboot.

## Increase DVMT Gfx Memory to improve iGPU performance
1. Boot into OpenCore
2. Press Space to see the list of tools
3. Select `Set DVMT Pre-Allocated to 64M`
4. Press enter
5. Then select `Set DVMT Total Gfx Mem to MAX`
6. Press enter
7. Restart

Finally, edit `config.plist` and delete or comment out `framebuffer-fbmem`, `framebuffer-stolenmem` and `framebuffer-patch-enable` in `Device Properties -> Add -> PciRoot(0x0)/Pci(0x2,0x0)` and restart.

## More information on the modified UEFI Firmware variables 
| VarName | VarOffset | VarStore | From | To |
| ---------------- | -- | -- | --------- | --------- |
| CFG Lock | 0x3C | 0x3 (CpuSetup) | 0x1 (Enabled) | 0x0 (Disabled) |
| VT-d | 0xE3 | 0x2 (SaSetup) | 0x1 (Enabled) | 0x0 (Disabled) |
| DVMT Pre-Allocated | 0xDF | 0x2 (SaSetup) | 0x1 (32M) | 0x2 (64M) |
| DVMT Total Gfx Mem | 0xE0 | 0x2 (SaSetup) | 0x2 (256M) | 0x3 (MAX) |

To revert all changes made to the UEFI Firmware variables to their default values, enable the corresponding entries in the `config.plist` file under `Misc` -> `Tools`, restart to the OpenCore menu, press space to see the list of tools and revert the changes by launching the option you wish to revert to its default value:
- `Enable CFG Lock`
- `Enable VT-d`
- `Set DVMT Pre-Allocated to 32M`
- `Set DVMT Total Gfx Mem to 256M`

Repeat for every UEFI variable you wish to revert to its default value.

***Please be aware that you need to revert any changes made to your `config.plist` file before reverting the UEFI variables to their default values, or macOS won't boot anymore!***

## Enabling native HiDPI display settings in macOS
On the installed macOS system, the default display resolution is too small for a small device such as the Surface Go 2. To enable the native HiDPI settings in the Display Preferences of macOS, download and run the [one-key-hidpi](https://github.com/jlempen/one-key-hidpi) script and select the option `(4) 1920x1280 Display`. This fork of [xzhih's one-key-hidpi tool](https://github.com/xzhih/one-key-hidpi) was modified to add the 1920x1280 resolution needed for the Surface Go 2.

## Disabling Sleep and enabling Hibernate
As we still haven't found a solution for the Sleep/Wake issues on the Surface Go 2, disable Sleep altogether and use Hibernate for now:
```
sudo pmset restoredefaults
sudo pmset -a hibernatemode 25
```

## Fixing broken Apple Messages and FaceTime
To fix issues with Apple Messages and FaceTime related to the [Intel Wireless driver](https://github.com/OpenIntelWireless/itlwm) on macOS Sonoma, disable all `AirportItlwm-***.kext` entries under `Kernel -> Add` in your `config.plist` file and use the [itlwm_v2.3.0_stable.kext.zip](https://github.com/OpenIntelWireless/itlwm/releases/download/v2.3.0/itlwm_v2.3.0_stable.kext.zip) and its companion app [HeliPort](https://github.com/OpenIntelWireless/HeliPort/releases/download/v1.4.1/HeliPort.dmg) instead.
The latest version 2.3.0 of itlwm.kext is already included in the Kext folder and `config.plist` file.

## Related repositories
* https://github.com/kingo132/surface-go2-hackintosh
