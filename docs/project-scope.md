# Project goals

- Build an 8×8×8 monochrome LED cube
- Design one controller PCB for the first version
- Implement reliable multiplexed LED driving
- Develop firmware for refresh, testing, and demo animations
- Document the full design process clearly on GitHub

# Version 1 summary

- 8×8×8 monochrome LED cube
- Single controller PCB in the base
- ESP32 as the main controller
- Multiplexed layer scanning
- 5 V USB-C power input
- Firmware with preloaded animations and serial debug

# Repository structure

- `docs/` — project documentation and design notes
- `hardware/` — PCB design files, calculations, and exports
- `firmware/` — embedded code
- `simulations/` — circuit or timing simulations
- `manufacturing/` — BOM, Gerbers, and assembly outputs
- `notes/` — engineering log and progress notes
