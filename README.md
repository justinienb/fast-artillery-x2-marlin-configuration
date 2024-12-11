# Fast Artillery Sidewinder X2 Marlin Configuration Files

## General Description

This repository contains two Marlin configuration files for the Artillery Sidewinder X2 3D printer. The configuration is tweaked for fast feedrates during probing and homing and enables the latest features of [`Marlin v2.1.3-beta1`](https://github.com/MarlinFirmware/Marlin/tree/release-2.1.3-beta1) that are relevant for the Sidewinder X2 3D printer.

## Configuration

### Base Configuration

The base file used is the **Artillery Sidewinder X2 example** from the official Marlin Configurations repository:  
[`Configurations/config/examples/Artillery/Sidewinder X2`](https://github.com/MarlinFirmware/Configurations/tree/release-2.1.3-b1/config/examples/Artillery/Sidewinder%20X2).

This configuration is based on the branch: [`release-2.1.3-b1`](https://github.com/MarlinFirmware/Configurations/tree/release-2.1.3-b1).

### Based On 

This configuration is heavily inspired by [markrossington's work](https://github.com/markrossington/sidewinder-x2-marlin).  
His repository includes a [table of changes](https://github.com/markrossington/sidewinder-x2-marlin?tab=readme-ov-file#new-features-vs-stock) that describes all the modifications made from the [official Artillery version](https://github.com/artillery3d/sidewinder-x2-firmware)

> **Note**: I've retained `EXPERIMENTAL_SCURV` and `ALLOW_LOW_EJERK` settings from markrossington's configuration files, but the new version [`release-2.1.3-b1`](https://github.com/MarlinFirmware/Configurations/tree/release-2.1.3-b1) has removed them. TBD if they still have any effect.

---

## Changes

- **Porting** markrossington's configuration, made for Marlin V2.1.2, to Marlin V2.1.3-1b.  
- **Disabling** markrossington's `Z_STEPPER_AUTO_ALIGN` as the X2 Z-axis is driven by two stepper motors connected by a belt, effectively syncing their movement. In this scenario, enabling `Z_STEPPER_AUTO_ALIGN` can cause problems with the belt and is not needed.  
- **Enabling** `BLTOUCH_HS_MODE` (High-Speed mode) for the official BLTouch (a common upgrade for this printer) in `Configuration_adv.h`.  
- **Setting** `MULTIPLE_PROBING` to 2 to increase accuracy lost due to HS Mode and high feedrates.  
- **Increasing** `XY_PROBE_FEEDRATE` from 133mm/s to 150mm/s in `Configuration.h`.  
- **Increasing** `Z_PROBE_FEEDRATE_FAST` from 10mm/s to 30mm/s (this setting adjusts the Z feedrate for the first approach when `MULTIPLE_PROBING == 2`) in `Configuration.h`.  
- **Decreasing** `Z_CLEARANCE_DEPLOY_PROBE` from 10mm to 5mm to reduce travel time.  
- **Decreasing** `Z_CLEARANCE_BETWEEN_PROBES` and `Z_CLEARANCE_MULTI_PROBE` from 5mm to 3mm.
- **Decreasing** `BLTOUCH_HS_EXTRA_CLEARANCE` from 7mm to 5mm in `Configuration_adv.h`.  
- **Increasing** `HOMING_FEEDRATE_MM_M` from 100mm/s to 150mm/s for the XY axis and from 25mm/s to 30mm/s for the Z-axis (same feedrate for every function) in `Configuration.h`.

> **Note**: The NOZZLE_TO_PROBE_OFFSET setting in `Configuration.h` has been tweaked to my setup. you will need to adjust it building Marlin. Alternatively, you can update the offset after flashing the firmware using the `M851` G-code command.

---

## How to Use These Files
1. **Clone this repository** or download the configuration files directly.  
2. **Clone the [Marlin repository](https://github.com/MarlinFirmware/Marlin)** and switch to the correct branch (or download it directly from the [Marlin releases page](https://github.com/MarlinFirmware/Marlin/releases))
   ```bash
   git clone https://github.com/MarlinFirmware/Marlin.git
   cd Marlin
   git checkout release-2.1.3-beta1
   ```  
3. **Replace the configuration files** in your Marlin source directory:  
   Copy the `Configuration.h` and `Configuration_adv.h` files from this repository into the `Marlin/` directory, overwriting the existing files.
4. *(Optional)* **Adjust the probe offset**:  
   Update the `NOZZLE_TO_PROBE_OFFSET` setting in `Configuration.h` for your setup (or change it later using the `M851` G-code command).  
5. **Compile the firmware** using the [Auto Build Marlin](https://marlinfw.org/docs/basics/auto_build_marlin.html) extension and the [PlatformIO](https://platformio.org/install/ide?install=vscode) plugin for VS Code.
6. **Flash the firmware** onto your printer. I recommend following [this guide](https://www.lesimprimantes3d.fr/forum/topic/44697-tuto-comment-flasher-le-firmware-des-x2-genius-pro-hornet/) (*note: the guide is in French*). Alternatively, you can try the PlatformIO upload feature, but it may not work reliably for STM32-based printers.