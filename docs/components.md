# Major Components

## Purpose
This document lists the main hardware building blocks for Version 1 of the LED cube controller and records the intended role of each part group. It is not yet a final BOM. Its purpose is to make sure that every critical function in the architecture has a defined component class before schematic capture begins.

## System summary
Version 1 is an 8×8×8 blue monochrome LED cube controlled by an ESP32. The cube uses multiplexed scanning with one active layer at a time. Column data and layer selection are both generated through a serial driver chain. Layers are switched on the low side with MOSFETs. Power enters through USB-C at 5 V. The ESP32 is powered from 5 V through the regulator already present on the selected ESP32 board or carrier.

## Selection philosophy
Version 1 prioritizes:
- simplicity
- low bring-up risk
- easy documentation
- easy hand assembly where possible
- practical sourcing
- suitable current capability for a student-built project

This document freezes the required component categories and their design roles, even if the exact part numbers are still to be chosen.

## Major component groups

### 1. Main controller
A practical ESP32 board or module is used as the main controller.

#### Required role
- run refresh timing
- generate driver-chain control signals
- store and play preloaded animations
- read the animation-select button
- provide serial debug

#### Required characteristics
- enough usable GPIO for the driver chain, UART, and button input
- reliable 5 V input or well-defined power interface
- practical availability for assembly and debugging
- suitable documentation and community support

#### Reason for selection
The ESP32 gives more than enough processing capability for a monochrome multiplexed LED cube while keeping the firmware side interesting but manageable. Using a board or module with a known power path reduces risk compared with designing around a bare chip immediately.

## 2. Serial driver chain
A serially controlled output driver chain is used to generate:
- 64 column control outputs
- control outputs for the layer-switching MOSFET gates

#### Required role
- expand the ESP32 control interface into many outputs
- allow display data to be shifted in serially
- update outputs in a controlled and repeatable way
- reduce direct GPIO usage on the ESP32

#### Required characteristics
- serial input with clocked shifting
- latched or otherwise stable outputs
- practical voltage compatibility with the control scheme
- output structure suitable for the chosen column-driving approach
- enough total outputs when chained together

#### Reason for selection
Direct GPIO control of the full cube is not practical. A serial driver chain makes the system feasible, keeps routing cleaner, and matches the planned firmware architecture.

## 3. Layer-switching devices
Low-side MOSFETs are used for the 8 layer switches.

#### Required role
- connect one active layer cathode plane to ground
- sink the full current of one active layer
- switch cleanly during multiplex scanning

#### Required characteristics
- logic-compatible gate drive from the selected control scheme
- current capability above worst-case active-layer current
- low enough conduction loss to avoid unnecessary heating
- practical footprint and availability

#### Reason for selection
Low-side MOSFET switching matches the chosen common-cathode layer structure and is simpler than a comparable high-side approach for Version 1.

## 4. Current-limiting resistors
Current limiting is placed in the column paths.

#### Required role
- define the LED current during the active layer interval
- prevent overcurrent in the LEDs and drive path
- make brightness behavior more predictable

#### Required characteristics
- resistor value selected from the final LED and driver voltage conditions
- suitable power rating
- practical package for assembly and board area

#### Reason for selection
Column-path current limiting is a straightforward fit for the chosen multiplexed architecture and keeps current definition associated with the driven LED paths.

## 5. USB-C power input connector
A USB-C connector is used only as the 5 V power entry point.

#### Required role
- bring 5 V power onto the controller PCB
- provide a modern, common connector for bench supplies or adapters

#### Required characteristics
- suitable for the intended assembly method
- mechanically reasonable for repeated use
- compatible with a simple 5 V sink implementation

#### Reason for selection
USB-C makes the project easier to power with common cables and supplies while keeping the input connector modern and convenient.

## 6. USB-C support components
Supporting parts are required so the board behaves correctly as a 5 V-powered USB-C sink.

#### Required role
- define the board as a power sink where needed
- support correct 5 V availability at the connector

#### Required characteristics
- compatible with a 5 V sink-only design
- simple to implement
- appropriate for the intended current level

#### Reason for selection
The connector alone is not the whole input design. Supporting components are needed so the 5 V input stage behaves correctly and predictably.

## 7. Bulk capacitance
Bulk capacitance is required on the 5 V rail near the input and possibly near the display-driving section.

#### Required role
- support transient current demand
- reduce rail sag during switching events
- improve supply stability during multiplex operation

#### Reason for selection
The display load is dynamic. The board will not include extra protection parts such as fuses or TVS devices in Version 1, so bulk capacitance becomes the main intentional power-conditioning element on the 5 V rail.

## 8. Decoupling capacitors
Local decoupling is required near active devices.

#### Required role
- support stable local supply voltage
- reduce switching noise
- improve behavior of the ESP32 interface and driver chain

#### Required locations
- near driver ICs
- near the ESP32 power interface
- near other active components as required

#### Reason for selection
The design contains pulsed LED current and digital switching. Decoupling is essential for stable operation.

## 9. User input button
One physical button is included for animation switching.

#### Required role
- allow the user to step through preloaded animations
- provide a simple standalone interface without external control hardware

#### Required characteristics
- simple momentary action
- practical footprint and placement
- compatible with ESP32 GPIO input handling

#### Reason for selection
A single button adds useful local control without adding much complexity.

## 10. Pull resistors and support passives
A number of small passive parts will be required around the design.

#### Likely roles
- button pull-up or pull-down
- MOSFET gate biasing or gate resistors if needed
- logic defaults where required
- other small support functions in the control paths

#### Reason for selection
These parts are necessary to make the schematic electrically complete and predictable.

## 11. Programming and debug interface
The controller needs a practical way to flash firmware and access serial debug.

#### Required role
- upload firmware
- observe serial output during bring-up
- support debugging during development

#### Possible implementation forms
- direct access through the selected ESP32 board or carrier
- header or connector for serial access
- onboard USB-UART path, if the final controller arrangement requires it

#### Reason for selection
Bring-up and iteration are much easier if programming and debug access are planned from the start.

## Component groups by system function

### Control section
- ESP32 board or module
- programming/debug interface
- button input
- pull resistors and logic support passives

### Display-drive section
- serial driver chain
- current-limiting resistors
- layer-switch MOSFETs
- related gate or control passives

### Power-input section
- USB-C connector
- USB-C support components
- bulk capacitance
- local decoupling

## Selection constraints to remember

### Blue LED voltage headroom
Because the cube uses blue LEDs, the column-driving path must leave enough voltage for:
- LED forward voltage
- current-limiting resistor drop
- any driver/output-stage voltage loss

This means the driver chain cannot be chosen based only on output count.

### Active-layer current
The layer-switching devices and 5 V distribution must be chosen around the worst-case active-layer current, not just average logic-level current.

### Minimal input-power philosophy
Version 1 intentionally avoids extra protection parts such as fuses, resettable fuses, or surge suppressors. The power-input section is therefore kept simple:
- USB-C power entry
- required USB-C sink configuration parts
- bulk capacitance
- local decoupling

### Assembly practicality
Version 1 should avoid unnecessarily difficult parts where a simpler alternative gives similar educational value and performance.

## What is frozen in Phase 2
This document freezes the required component categories for:
- control
- serial output expansion
- low-side layer switching
- column current limiting
- 5 V USB-C input
- bulk power conditioning
- local user input
- programming/debug access

## What is not yet frozen
This document does not yet freeze:
- exact manufacturer part numbers
- exact footprints
- exact resistor values
- exact MOSFET type
- exact driver IC type
- exact capacitor values
- exact connector model
- exact ESP32 board or module choice

Those are part-selection and schematic-stage tasks.

## Phase 2 conclusion
Every major function in Version 1 now has a defined component class:
- ESP32 for control
- serial driver chain for output expansion
- low-side MOSFETs for layer switching
- resistors for column current limiting
- USB-C for 5 V input
- bulk and local capacitors for power stability
- support components for user interaction and debug access

This is enough to proceed into exact part selection and schematic capture without leaving any critical function undefined.
