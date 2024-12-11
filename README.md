# Fast Artillery Sidewinder X2 Marlin Configuration Files

## General Description

This repository contains two Marlin configuration files for the Artillery Sidewinder X2 3D printer. The configuration is tweaked for fast feedrates during probing and homing and enables the latest features of [`Marlin v2.1.3-beta1`](https://github.com/MarlinFirmware/Marlin/tree/release-2.1.3-beta1) that are relevant for the Sidewinder X2 3D printer.

## Configuration

### Base Configuration

The base file used is the **Artillery Sidewinder X2 example** from the official Marlin Configurations repository:  
[`Configurations/config/examples/Artillery/Sidewinder X2`](https://github.com/MarlinFirmware/Configurations/tree/release-2.1.3-b1/config/examples/Artillery/Sidewinder%20X2).

This configuration is based on the branch: [`release-2.1.3-b1`](https://github.com/MarlinFirmware/Configurations/tree/release-2.1.3-b1).

### Based On 

This configuration is based on [markrossington's work](https://github.com/markrossington/sidewinder-x2-marlin).  
His repository includes a [table of changes](https://github.com/markrossington/sidewinder-x2-marlin?tab=readme-ov-file#new-features-vs-stock) that describes all the modifications made from the [official Artillery version](https://github.com/artillery3d/sidewinder-x2-firmware)


---

## My Changes

| File                      | Setting                       | Old Value                     | New Value                 | Details                                                                                                                                           |
|---------------------------|-------------------------------|-------------------------------|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------                |
| Both configuration files  | Marlin Version                | V2.1.2                        | V2.1.3-1b                 | Updated MarkRossington's configuration files to the newer Marlin version, ensuring compatibility with the latest features and changes.            |
| Configuration_adv.h       | `Z_STEPPER_AUTO_ALIGN`        | Enabled                       | Disabled                  | Disabled MarkRossington setting, X2 Z-axis uses a belt to sync two stepper motors, making this setting unnecessary and potentially problematic.   |
| Configuration_adv.h       | `ALLOW_LOW_EJERK`             | Enabled                       | Removed                   | Removed MarkRossington setting, it is now deprecated in `Marlin v2.1.3-beta1`                                                                     |
| Configuration_adv.h       | `BLTOUCH_HS_MODE`             | Disabled                      | Enabled                   | Enables High-Speed mode for the BLTouch, speeding up automatic bed leveling (ABL).                                                                |
| Configuration.h           | `MULTIPLE_PROBING`            | 1                             | 2                         | Increases probing accuracy, compensating for losses due to High-Speed mode and high feedrates.                                                    |
| Configuration.h           | `HOMING_FEEDRATE_MM_M`        | 100mm/s (XY), 25mm/s (Z)      | 150mm/s (XY), 30mm/s (Z)  | Increases homing speed for all axes.                                                                                                              |
| Configuration.h           | `XY_PROBE_FEEDRATE`           | 133mm/s                       | 150mm/s                   | Adjusts the XY feedrate during probing to match faster speeds.                                                                                    |
| Configuration.h           | `Z_PROBE_FEEDRATE_FAST`       | 10mm/s                        | 30mm/s                    | Speeds up the Z-axis feedrate for the first approach when `MULTIPLE_PROBING` is set to 2.                                                         |
| Configuration.h           | `Z_CLEARANCE_DEPLOY_PROBE`    | 10mm                          | 5mm                       | Reduces travel time by lowering the clearance required to deploy the probe.                                                                       |
| Configuration.h           | `Z_CLEARANCE_BETWEEN_PROBES`  | 5mm                           | 3mm                       | Reduces clearance between probe points for faster probing.                                                                                        |
| Configuration.h           | `Z_CLEARANCE_MULTI_PROBE`     | 5mm                           | 3mm                       | Reduces clearance during multiple probe operations to save time.                                                                                  |
| Configuration_adv.h       | `BLTOUCH_HS_EXTRA_CLEARANCE`  | 7mm                           | 5mm                       | Lowers extra clearance for BLTouch High-Speed mode to save time.                                                                                  |
| Configuration.h           | `PROBE_OFFSET_ZMIN`           | -2.5                          | -3                        | Keep the sensitive probe away from the bed.                                                                                                       |
| Configuration.h           | `PROBE_OFFSET_ZMAX`           | 2.5                           | 3                         | To keep the interval symmetrical around 0.                                                                                                        |

> **Note 1**: I've retained `EXPERIMENTAL_SCURV` but the new version [`release-2.1.3-b1`](https://github.com/MarlinFirmware/Configurations/tree/release-2.1.3-b1) has removed it. No deprecated warning generated when building marlin, TBD if they still have any effect.  
> **Note 2**: The NOZZLE_TO_PROBE_OFFSET and setting in `Configuration.h` has been tweaked to my setup. you will need to adjust it before building Marlin. Alternatively, you can update the offset after flashing the firmware using the `M851` G-code command.

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