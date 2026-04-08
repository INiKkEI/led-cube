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

## Phase 3 — System architecture and schematic capture

### Completed
- High-level architecture documented
- LED driving approach fixed as multiplexed layer scanning
- Layer/column control concept defined
- Preliminary component planning documented
- Pin budget documented
- Power budget documented
- Altium project structure added to the repository
- Initial schematic document created
- LED driver section drawn
- ESP32 core/support circuitry completed
- ERC run on the schematic
- Real ERC errors fixed
- Remaining warnings reviewed and understood
- Schematic considered ready for layout handoff

### Result
The project has moved beyond architecture planning and into completed schematic capture for Version 1. The schematic is now in a state suitable for PCB layout preparation.

### Notes
Current hardware implementation state:
- Altium project files are present under `hardware/altium/led-cube/`
- the main schematic currently exists as `Sheet1.SchDoc`
- schematic capture is functionally complete enough to hand off into PCB layout work
- future schematic edits may still happen, but layout can now begin

---

## Phase 4 — PCB layout preparation

### Completed
- PCB stackup defined
- Basic board constraints defined
- Layout preparation started

### In progress
- PCB document and board outline refinement
- Placement planning
- Mechanical fit of cube connection grid and controller board zones

### Result
The project has entered PCB layout preparation. Electrical design is far enough along that placement and board planning can proceed.

### Notes
The main remaining work is no longer schematic capture. It is PCB definition, placement, routing, and later bring-up preparation.

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

### Present hardware files
- `hardware/altium/led-cube/Sheet1.SchDoc`
- `hardware/altium/led-cube/led-cube.PrjPcb`
- `hardware/altium/led-cube/led-cube.PrjPcbStructure`

---

## Next work focus

### Immediate priorities
- create or refine the PCB board outline
- place major components by functional zones
- reserve and align the 8×8 cube connection grid area
- define power routing strategy on the PCB
- begin PCB routing and layout review

### Open questions
- Final LED current per LED
- Peak current per active layer
- Exact layer transistor or MOSFET choice
- Brightness-control strategy in final firmware
- Final board dimensions and cube connector/grid arrangement

---

## Revision note
This log should be updated whenever schematic status changes, PCB layout meaningfully progresses, or a major electrical or mechanical decision changes.
