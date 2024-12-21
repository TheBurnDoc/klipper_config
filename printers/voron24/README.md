# Voron 2.4 Firmware Flashing Instructions
This documentation contains custom flashing instructions for the boards used in the
current setup for my Voron 2.4.

## Flashing Katapult
Katapult, formally CANBoot, is a boot loader that resides on both boards 
allowing for flashing of the main firmware over the CANbus network and without
any need for physical interaction, switching jumpers or SD Cards for example.

### Mainboard:
https://canbus.esoterical.online/USB_CAN_Bridge_Mainboard.html

## Flashing Klipper
Klipper is installed on both the mainboard and toolhead board, and since the
printer is setup with a functional CANbus network, this is done using Katapult.


### Mainboard: BigTreeTech Octopus
The toolhead board is connected to the CANbus port on the Octopus, which means
we need to put the board into CANbus bridge mode and connect the mainboard to 
the CANbus network just like the toolhead.

#### Preparation
The mainboard is connected to the Pi using a USB cable so we can flash using the
build system included with Klipper.

[Here](https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-V1.0/blob/master/Firmware/Klipper/README.md)
is the official documentation for the Octopus board; below are the steps taken
from this documentation and customised to this specific Voron 2.4.

#### Build Firmware
Start with the following commands:

```
cd ~/klipper
make clean KCONFIG_CONFIG=config.octopus
make menuconfig KCONFIG_CONFIG=config.octopus
```

The set the following settings in the configuration screen:

 - [*] Enable extra low-level configuration options
 - Micro-controller Architecture (STMicroelectronics STM32)
 - Processor model (STM32F446)
 - Bootloader offset (32KiB bootloader)
 - Clock Reference (12 MHz crystal)
 - Communication interface (USB (on PA11/PA12))
 - USB ids
 - [ ] Specify a custom step pulse duration
 - () GPIO pins to set at micro-controller startup

Quit and save and then build the firmware with:

```
make KCONFIG_CONFIG=config.octopus -j4
```

#### Flash Firmware




### Toolhead: Mellow Fly SB2040 V2
[Here](https://mellow-3d.github.io/fly_sb2040_v2_klipper_can_updating.html) is
the official documentation for this is proceedure; below are the steps taken
from this documentation and customised to this specific Voron 2.4.

#### Build Firmware
Start with the following commands:

```
cd ~/klipper
make clean KCONFIG_CONFIG=config.sb2040v2
make menuconfig KCONFIG_CONFIG=config.sb2040v2
```

Then set the following settings in the configuration screen:

 - [*] Enable extra low-level configuration options
 - Micro-controller Architecture (Raspberry Pi RP2040)
 - Bootloader offset (16KiB bootloader)
 - Communication interface (CAN bus)
 - (4) CAN RX gpio number
 - (5) CAN TX gpio number
 - (1000000) CAN bus speed
 - (gpio24) GPIO pins to set at micro-controller startup

Quit and save and then build the firmware with:

```
make KCONFIG_CONFIG=config.sb2040v2 -j4
```

#### Flash Firmware
The CANbus ID for the board is `66ac8f7e958a` so you can now flash the board
with Katapult:

```
python3 ~/katapult/scripts/flash_can.py -i can0 -f ~/klipper/out/klipper.bin -u 66ac8f7e958a
```