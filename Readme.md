# Hackintosh Sequoia 15 - Lenovo Ideapad Gaming 3 15ACH6D1

This guide details the installation and post-installation process for running macOS Sequoia 15 on the Lenovo Ideapad Gaming 3 with an AMD Ryzen 5600H CPU. It includes the kexts, ACPI patches, boot arguments, and necessary configurations.

## Screenshots

| System | Cinebench Score : Multi Core (525) | Cinebench Score : Single Core (80) |
|-------------|-------------|-------------|
| ![Screenshot 1](https://github.com/user-attachments/assets/15d0f7c4-742d-49be-8998-6fc006c0d9f8) | ![Screenshot 2](https://github.com/user-attachments/assets/b58178c7-f238-4d44-875a-0030c46e7f96) | ![Screenshot 3](https://github.com/user-attachments/assets/1e51f766-ba8f-4589-8cae-bbfcca2d146e) |





## System Specifications

| Component        | Details                                      | Status       |
|-----------------|----------------------------------------------|-------------|
| **Processor**   | AMD Ryzen 5 5600H                           | ✅ Working |
| **iGPU**        | AMD Radeon RX Graphics 2GB                  | ✅ Working |
| **Audio**       | Realtek ALC257                              | ✅ Working |
| **Ethernet**    | Realtek RTL8111                             | ✅ Working |
| **Storage**     | NVMe + SATA Drives                          | ✅ Detected |
| **Bootloader**  | Triple boot (Windows, Ubuntu, macOS) via GRUB | ✅ Working |
| **Battery**     | Detected and working properly               | ✅ Fixed |
| **dGPU**        | Nvidia GTX 1650 (not supported)             | ❌ Not Working |
| **WiFi/Bluetooth** | Mediatek Card (not supported)          | ❌ Not Working |
| **Sleep**       | Mostly working                              | ✅ Fixed |
| **Keyboard + Touchpad** | Working properly | ✅ Fixed |

## Tools Used

| Tool | Purpose |
|------|---------|
| [Dortania's OpenCore Guide](https://dortania.github.io/OpenCore-Install-Guide/) | Main Hackintosh guide |
| [ChefKiss Guide](https://chefkissinc.github.io/guides/hackintosh/) | AMD-specific kexts and patches |
| [CorpNewt's UnPlugged](https://github.com/corpnewt/UnPlugged) | macOS offline installer creation |
| [SSDTTime](https://github.com/corpnewt/SSDTTime) | Generate SSDTs |
| [USBMap](https://github.com/corpnewt/USBMap) | USB mapping |
| [OCAuxiliaryTools](https://github.com/ic005k/OCAuxiliaryTools) | SMBIOS generation |
| [HoRNDIS](https://github.com/jwise/HoRNDIS) | Android USB tethering |

## Installation Steps

1. Follow [Dortania's guide](https://dortania.github.io/OpenCore-Install-Guide/) and [ChefKiss Guide](https://chefkissinc.github.io/guides/hackintosh/) to gather EFI files and patches.
2. Use [CorpNewt's UnPlugged](https://github.com/corpnewt/UnPlugged) to create an offline macOS installer.
3. Ensure all required kexts are present in `EFI/OC/Kexts`.
4. Replace `EFI/OC/ACPI` files with SSDTs generated using [SSDTTime](https://github.com/corpnewt/SSDTTime) specific to your machine.
5. Generate USB Mapping using [USBMap](https://github.com/corpnewt/USBMap) and replace `USBMap.kext` in `EFI/OC/Kexts`.
6. Generate SMBIOS using [OCAuxiliaryTools](https://github.com/ic005k/OCAuxiliaryTools):
   - Go to **PI (Platform Information)** > Select `MacBookPro16,3` > Generate SMBIOS.
7. Prepare USB installer using the online guide or offline method (recommended due to slow Ethernet/tethering speeds).

## Post-Installation Fixes

- **Audio Not Working:** Modify `device path` and `alcid` according to [Dortania's Audio Guide](https://dortania.github.io/OpenCore-Post-Install/universal/audio.html#finding-your-layout-id).
- **App Crashes (VSCode, Brave, etc.):** Disable **Hardware Acceleration** in settings.
- **Battery Not Detected:** Ensure **Switchable Graphics** is set to `UMA Graphics` in BIOS.
- **Disabled Secure Boot** to prevent boot loops.

## Notes
- **Sleep issues:** Mostly fixed, but still testing.
- **WiFi/Bluetooth:** Mediatek card is unsupported; use Android USB Tethering with [HoRNDIS](https://github.com/jwise/HoRNDIS).
- **Secure Boot:** Disabled as it may cause boot loops.
- **Switchable Graphics:** Set to `UMA Graphics` for battery functionality.

## Kexts Used

| Kext Name | Purpose |
|-----------|---------|
| AppleMCEReporterDisabler.kext | Disables Apple MCE Reporter |
| GenericUSBXHCI.kext | USB 3.0 controller support |
| HoRNDIS.kext | Enables Android USB tethering [GitHub](https://github.com/jwise/HoRNDIS) |
| Lilu.kext | Required for macOS kernel patching |
| NootedRed.kext | Radeon GPU support |
| RealtekRTL8111.kext | Ethernet driver |
| RestrictEvents.kext | Prevents unwanted events from affecting macOS |
| USBMap.kext | USB port mapping [GitHub](https://github.com/corpnewt/USBMap) |
| VirtualSMC.kext | Simulates Apple SMC |
| VoodooPS2Controller.kext | PS/2 keyboard and trackpad support |
| VoodooI2C.kext | I2C input support |
| AppleALC.kext | Audio support |
| BrightnessKeys.kext | Fixes brightness keys |
| ECEnabler.kext | Fixes Embedded Controller issues |
| ForgedInvariant.kext | Required for AMD CPU patching |
| IntelMKLFixup.kext | Fixes Intel MKL library crashes |
| SMCBatteryManager.kext | Battery support |
| SMCProcessorAMD.kext | AMD CPU monitoring |
| SMCRadeonSensors.kext | Radeon GPU sensors |
| VoodooI2CHID.kext | I2C HID touchpad support |

## ACPI Patches Used

| ACPI Patch | Purpose |
|------------|---------|
| SSDT-ALS0.aml | Ambient light sensor fix |
| SSDT-EC.aml | Embedded Controller patch |
| SSDT-PLUG-ALT.aml | CPU power management patch |
| SSDT-PNLF.aml | Backlight control |
| SSDT-USB-Reset.aml | USB initialization fix |
| SSDT-USBX.aml | USB power settings |
| SSDT-XOSI.aml | Windows compatibility fix |
| SSDT-GPRW.aml | Sleep/Wake fixes |

## Boot Arguments

| Argument | Purpose |
|----------|---------|
| revpatch=sbvmm | Enables AMD kernel patching |
| csr-active-config=0xA85 | Enables SIP-related settings |
| keepsyms=1 | Keeps kernel symbols for debugging |
| alcid=11 | Audio codec layout fix |
| amfi_get_out_of_my_way=1 | Bypasses AMFI restrictions |
| -lilubetaall | Enables Lilu on beta macOS versions |
| -revcpu=1 | Enables AMD CPU fixes |
| darkwake=0 | Disables unnecessary wake-ups |


This EFI setup is tailored for **Lenovo Ideapad Gaming 3 with AMD Ryzen 5600H**. If using on a different system, generate fresh ACPI patches, USB maps, and SMBIOS.
