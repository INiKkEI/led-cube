# LED Cube — Power Integrity

DC analysis of the 5 V distribution network, run in Altium's Power Analyzer by Keysight. Evidence for SPEC-18 (supply voltage 4.75–5.25 V), ahead of ST-18 measuring the built board.

Run 2026-08-02. Simulation engine: Keysight. Analyzer version 1.0.43.3260.

## Why this was worth running

The `+5` net has **no tracks and no vias**. All 21 of its pads connect through the top-layer polygon alone. The same layer carries 500 signal tracks, so the question was whether the pour is still one continuous conductor or has been cut into islands.

It is continuous. One connected region of 34 709 mm² containing all 21 pads, verified independently of the simulator by rasterising the layer and running a connectivity check.

## Configuration

| Parameter | Value |
|---|---|
| Copper | PCB copper, 47 000 S/mm base, 21.277 nΩ·m |
| Base / working temperature | 25 °C / 30 °C |
| Max current density, surface | 20 A/mm² |
| Via plating thickness | 0.025 mm |
| DC drop limit | 5 % |
| Ground included | Yes |

Loads taken from `power-budget.md`: U1–U8 at 51 mA each, U9 at 2 mA, MD1 at 150 mA. Total 560 mA, against the documented 561 mA nominal.

## Results — Pass

| Quantity | Simulated | Limit | Margin |
|---|---|---|---|
| Source voltage at J1 | 4.9994 V | — | — |
| Worst-case load voltage (U5, U6) | **4.9906 V** | 4.75 V | 0.24 V |
| Total DC drop | **9.4 mV (0.19 %)** | 5 % | 96 % |
| Source current | 560.05 mA | — | — |
| Non-via current density | 10.916 A/mm² | 20 A/mm² | 54.6 % |
| Via current density | 11.769 A/mm² | 20 A/mm² | 58.8 % |
| Network power | 2.8 W | — | — |

Zero violations.

## Reading the result

**Voltage drop is not a constraint on this board.** 0.19 % against a 5 % limit. A full-board pour carrying 0.56 A over 230 mm is roughly 1.4 squares of copper — under a milliohm. The 4.9906 V figure satisfies SPEC-18 with 0.24 V of margin at the worst load.

**Current density is where the copper actually works.** The peaks are not in the plane but at the thermal relief spokes where current enters and leaves pads — the barrel jack and the MOSFET source pads, each of which sinks 401 mA when its layer is lit. At 10.9 A/mm² these sit at 55 % of the limit. ADR-21 records the decision to keep the reliefs rather than switch to direct connect.

**The simulated power matches the hand calculation.** 2.8 W against the 2757 mW derived in `power-budget.md`, independently.

## Limits of this analysis

- DC only. It says nothing about switching transients, which is what RISK-10 (no local decoupling) is about.
- Load currents are the documented estimates, not measurements. They inherit the 3.2 V forward-voltage assumption flagged in ADR-05 — if Vf proves higher, the real currents are lower and this result gets better, not worse.
- ST-18 still measures the built board. This is analysis, not test.
