# Power Budget

## Purpose
This document estimates the electrical load of Version 1 of the LED cube and defines the design-level power targets for schematic capture. It covers LED current, worst-case layer current, controller overhead, and the required 5 V input capability.

## System summary
Version 1 is an 8×8×8 blue monochrome LED cube controlled by an ESP32. The display is multiplexed, with one layer active at a time.

Main architectural assumptions:
- USB-C used as 5 V power input
- LED drive powered directly from 5 V
- ESP32 powered from 5 V through the regulator already present on the selected ESP32 board or carrier
- low-side layer switching with common-cathode layers
- column paths provide LED current
- one physical button is used to switch between animations

## Display size and multiplexing
- total LEDs: 512
- layers: 8
- LEDs per layer: 64
- active layers at one time: 1

This means the electrical design must support the current of one full layer at a time, not all 512 LEDs conducting simultaneously.

## Design assumptions

### LED assumptions
The exact LED part is not yet frozen, so these values are preliminary planning values.

- LED color: blue
- LED forward voltage: to be confirmed from the selected LED datasheet
- target instantaneous LED current when on: 10 mA

### Why 10 mA was chosen
The display is multiplexed at 1:8 duty cycle, so each LED is on for only part of the total frame time. A 10 mA instantaneous current is a conservative first design target for Version 1. It keeps the power stage manageable and is suitable for early design calculations.

This value is a planning assumption, not a final measured operating point.

## LED current calculations

### Current for one lit LED
When one LED is on during its active layer time:

`I_LED = 10 mA`

### Worst-case current for one active layer
If all 64 LEDs in the selected layer are on at the same time:

`I_layer_peak = 64 × 10 mA = 640 mA`

So the worst-case active-layer current is:

`I_layer_peak = 0.64 A`

This is the key current value for:
- layer MOSFET sizing
- ground return design
- 5 V power distribution
- connector and trace planning

### Average total LED current for a fully filled cube
If the displayed pattern turns on every voxel, each layer still conducts one at a time. Over time, the average total LED supply current is approximately equal to the current of one fully lit active layer:

`I_LED_avg_total ≈ 640 mA`

So for planning purposes:

- instantaneous LED current, worst case: 640 mA
- average LED supply current for a fully filled cube: about 640 mA

## Controller and logic current budget

### ESP32 budget
The ESP32 is powered from the 5 V rail through its onboard regulator or converter. Exact input current depends on the selected ESP32 board or carrier and firmware activity.

Reserve the following design budget:

`I_ESP32_budget = 150 mA`

This includes margin and is intentionally conservative for early planning.

### Driver chain and support logic
The driver chain, button input, and other support logic consume much less than the LED array, but should still be included.

Reserve:

`I_logic_budget = 50 mA`

### Total non-LED current budget
`I_non_LED = 150 mA + 50 mA = 200 mA`

## Total 5 V current budget

### Estimated operating total
Add LED current and controller overhead:

`I_total_estimated = 640 mA + 200 mA = 840 mA`

So the first-pass total operating estimate is:

`I_total_estimated ≈ 0.84 A`

### Recommended supply rating
The design should not be sized exactly at the estimate. Margin is needed for:
- startup conditions
- part tolerance
- voltage drop in wiring and connectors
- animation patterns with heavy LED usage
- possible future adjustments
- uncertainty in early assumptions

Recommended target:

`5 V, 2 A external supply`

This gives comfortable headroom for Version 1.

## Power path implications

### 5 V input
The 5 V input path must support close to 1 A operating current with margin. This means the power input should be treated as a real power path, not a light logic-only connection.

### USB-C input
USB-C is used only as the 5 V input source. The connector and its surrounding circuitry must be chosen and routed so that the board can safely and cleanly receive the required current.

### 5 V distribution
The 5 V rail feeds:
- column-driving circuitry
- any driver ICs that require 5 V
- the ESP32 5 V input path

The 5 V path therefore needs:
- sensible trace widths or pours
- bulk capacitance near the input
- local decoupling near active circuitry
- low-impedance return paths

## Layer-switching implications
The cube uses low-side MOSFET layer switching. Each layer switch must handle the full active-layer current.

Minimum planning requirement:

`layer switch current > 0.64 A`

In practice, the chosen MOSFET should have clear margin above this number.

The gate control path must also switch the layer cleanly enough to avoid:
- ghosting
- incomplete turn-on
- unnecessary heating

## Column-driving implications
The column path must support the selected per-LED current of 10 mA for every lit LED in the active layer.

That means the column-driving architecture must be chosen with enough voltage and current headroom for:
- blue LED forward voltage
- current-limiting resistor drop
- driver/output-stage voltage drop

## Power dissipation notes

### Current-limiting resistors
The exact resistor values are not yet fixed. Their final value depends on:
- selected blue LED forward voltage
- selected driver architecture
- actual voltage lost in the column path
- desired LED current

So resistor dissipation is not frozen yet, but it must be checked during schematic design.

### Layer MOSFETs
The low-side MOSFETs carry the full active-layer current when enabled. Their conduction loss depends on:

`P_MOSFET = I^2 × R_DS(on)`

Exact dissipation depends on the chosen MOSFET and is not yet frozen, but the current budget already shows that these devices must not be selected casually.

### ESP32 onboard regulation
The ESP32 board or carrier handles its own 5 V to 3.3 V conversion. That means some dissipation occurs on that board rather than in a separate regulator on the controller PCB.

## Summary table

| Item | Value |
|---|---:|
| Total LEDs | 512 |
| LEDs per layer | 64 |
| Multiplex ratio | 1:8 |
| Instantaneous current per lit LED | 10 mA |
| Worst-case active-layer current | 640 mA |
| Estimated ESP32 budget from 5 V | 150 mA |
| Estimated driver/support logic budget | 50 mA |
| Estimated total current | 840 mA |
| Recommended external supply | 5 V, 2 A |

## Frozen Phase 2 conclusions
The Phase 2 power plan for Version 1 is:

- use 5 V input power through USB-C
- design around 10 mA instantaneous current per lit LED
- assume 640 mA worst-case active-layer current
- reserve about 200 mA for ESP32 and support logic
- plan for about 840 mA total estimated load
- use a 5 V, 2 A supply target for comfortable margin

These numbers are sufficient to continue with:
- component selection
- layer MOSFET selection
- driver-chain selection
- connector planning
- schematic capture

## Items not yet frozen
The following are still to be confirmed later:
- exact blue LED part number
- exact LED forward voltage from datasheet
- exact resistor values
- exact column-driver voltage drop
- exact MOSFET losses
- actual ESP32 current from the chosen board or carrier
- measured current under real animation patterns
