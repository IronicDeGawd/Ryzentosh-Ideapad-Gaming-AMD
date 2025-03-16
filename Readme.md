# Hackintosh Sequoia 15 - Lenovo Ideapad Gaming 3

This guide details the installation and post-installation process for running macOS Sequoia 15 on the Lenovo Ideapad Gaming 3 with an AMD Ryzen 5600H CPU. It includes the kexts, ACPI patches, boot arguments, and necessary configurations.

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
| CtlnaAHCIPort.kext | Enables AHCI support |
| GenericUSBXHCI.kext | USB 3.0 controller support |
| HoRNDIS.kext | Enables Android USB tethering [GitHub](https://github.com/jwise/HoRNDIS) |
| Lilu.kext | Required for macOS kernel patching |
| NootedRed.kext | Radeon GPU support |
| RealtekRTL8111.kext | Ethernet driver |
| RestrictEvents.kext | Prevents unwanted events from affecting macOS |
| USBMap.kext | USB port mapping [GitHub](https://github.com/corpnewt/USBMap) |
| VirtualSMC.kext | Simulates Apple SMC |
| VoodooPS2Controller.kext | PS/2 keyboard and trackpad support |
| XLNCUSBFix.kext | Fixes USB issues |
| VoodooI2C.kext | I2C input support |
| AMDRyzenCPUPowerManagement.kext | Power management for Ryzen CPUs |
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

## Boot Arguments

| Argument | Purpose |
|----------|---------|
| revpatch=sbvmm | Enables AMD kernel patching |
| csr-active-config=0xA85 | Enables SIP-related settings |
| keepsyms=1 | Keeps kernel symbols for debugging |
| alcid=11 | Audio codec layout fix |
| amfi_get_out_of_my_way=1 | Bypasses AMFI restrictions |
| agdpmod=pikera | Enables Radeon dGPU support |
| -lilubetaall | Enables Lilu on beta macOS versions |


This EFI setup is tailored for **Lenovo Ideapad Gaming 3 with AMD Ryzen 5600H**. If using on a different system, generate fresh ACPI patches, USB maps, and SMBIOS.
