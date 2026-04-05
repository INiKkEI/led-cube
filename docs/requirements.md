# Requirements

## Functional requirements

- The system shall control an 8×8×8 monochrome LED cube.
- The system shall address 512 LEDs using multiplexed scanning.
- The system shall use one controller PCB in the base.
- The system shall use an ESP32 as the main controller.
- The firmware shall display preloaded animations.
- The firmware shall provide serial debug output.

## Electrical requirements

- The system shall accept 5 V power input through USB-C.
- The LED driving circuit shall provide adequate current control for stable brightness.
- The PCB shall include programming and debugging access for the ESP32.

## Documentation requirements

- The repository shall contain project scope documentation.
- The repository shall contain hardware and firmware folders from the beginning.
- Major design decisions shall be recorded during development.
