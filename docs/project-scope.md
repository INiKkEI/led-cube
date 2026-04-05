# Project Scope

## Version 1 scope

### Included

- 8×8×8 monochrome LED cube
- One controller PCB in the base
- ESP32 as the main controller
- LED driving electronics for all 512 LEDs
- Multiplexed scanning with one active layer at a time
- 5 V USB-C power input
- Firmware for:
  - cube refresh
  - hardware test patterns
  - preloaded animations
  - serial debugging
  - one physical button input for switching between preloaded animations
- GitHub documentation of design decisions and progress

### Excluded from version 1

- RGB LEDs
- wireless app control
- audio-reactive features
- battery operation
- distributed multi-board architecture
- advanced PC visualization software
- enclosure optimization beyond basic mechanical support

## High-level architecture

- 512 monochrome LEDs arranged in 8 layers of 8×8
- One active layer is scanned at a time
- Column lines are controlled through driver circuitry
- Layers are switched using transistors or MOSFETs
- An ESP32 manages refresh timing, animation data, and the serial debug interface
- The whole system is powered from a regulated 5 V USB-C input

## Main technical decisions

### Cube size
8×8×8

### LED type
Monochrome

### Controller PCB
One controller PCB for the first version, placed in the base.

### Main control method
ESP32 microcontroller on the main PCB.

### LED driving method
Multiplexed layer scanning. One layer is enabled at a time while column states define which LEDs in that layer are lit.

### Power input method
5 V input through USB-C.

### Firmware
Included in version 1.

Firmware will provide:
- refresh timing and multiplexing control
- LED addressing logic
- startup and hardware test patterns
- several preloaded animations
- serial debug output for bring-up and testing

## Success criteria for version 1

Version 1 is successful if it can:
- light all 512 LEDs reliably
- display stable animations without obvious flicker
- operate safely from a 5 V USB-C supply
- run preloaded animation sequences from firmware
- provide useful serial debug output during testing
- be understandable and reproducible from the repository documentation
