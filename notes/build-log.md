# Build Log

## Phase 1 — Define scope

### Completed
- Project selected: 8×8×8 LED cube
- LED type fixed: monochrome blue
- Main controller fixed: ESP32
- Power input fixed: 5 V USB-C
- Firmware included in V1
- Firmware scope fixed:
  - refresh control
  - hardware test patterns
  - preloaded animations
  - serial debugging
  - one physical button for animation switching
- Initial repository structure created

### Result
The project scope for Version 1 is fixed and documented.

### Notes
Version 1 excludes RGB LEDs, wireless app control, audio-reactive features, battery operation, distributed multi-board architecture, advanced PC visualization software, and enclosure optimization beyond basic mechanical support.

---

## Phase 2 — Define requirements

### Completed
- Functional requirements documented
- Version 1 success criteria written
- Included and excluded features clarified
- High-level operating behavior defined

### Result
The project now has written requirements for what the cube must do before schematic work starts.

### Notes
Version 1 is considered successful if it can:
- light all 512 LEDs reliably
- display stable animations without obvious flicker
- operate safely from a 5 V USB-C supply
- run preloaded animation sequences from firmware
- provide useful serial debug output during testing
- remain understandable and reproducible from repository documentation

---

## Phase 3 — System architecture and early feasibility

### Completed
- High-level architecture documented
- LED driving approach fixed as multiplexed layer scanning
- Layer/column control concept defined
- Preliminary component planning documented
- Pin budget documented
- Power budget documented

### Result
The basic electrical architecture is now defined well enough to move into detailed schematic design.

### Notes
Current architecture:
- 512 monochrome LEDs arranged as 8 layers of 8×8
- one active layer scanned at a time
- column lines driven by dedicated driver circuitry
- layers switched by transistors or MOSFETs
- ESP32 handles refresh timing, animation data, and serial debug
- powered from regulated 5 V USB-C input

---

## Current repository state

### Present documentation
- `docs/project-scope.md`
- `docs/requirements.md`
- `docs/components.md`
- `docs/pin-budget.md`
- `docs/power-budget.md`
- `docs/project-archtecture.md`
- `notes/build-log.md`

---

## Next phase

### Phase 4 — Detailed schematic design
Planned work:
- choose exact LED driver ICs / shift registers
- choose exact layer switching devices
- define current-limiting strategy
- define ESP32 support circuitry
- define USB-C power input circuitry
- draw first full schematic
- review schematic against pin and power budgets

---

## Open questions
- Final LED current per LED
- Peak current per active layer
- Exact driver IC selection
- Exact transistor or MOSFET selection for layers
- Whether brightness control will be global only or per-voxel through PWM timing

---

## Revision note
This log should be updated whenever a phase is completed, a major design decision changes, or a document is added that affects architecture, power, firmware scope, or manufacturability.
