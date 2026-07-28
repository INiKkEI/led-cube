# LED Cube — System Specifications

Each requirement in `requirements.md` restated as measurable quantities with tolerances. Nothing here says how the cube is built — only what has to be true of it, in numbers that can be checked.

| ID | Req | Quantity | Value | Tolerance |
|---|---|---|---|---|
| SPEC-01 | REQ-01 | Animations available | 5 | minimum |
| SPEC-02 | REQ-02 | Latency, button release to first frame of the new animation | 200 ms | maximum |
| SPEC-03 | REQ-03 | Debounce hold-off after first contact | 30 ms | ±5 ms |
| SPEC-04 | REQ-03 | Animation changes per button press, over ≥ 100 presses | 1.00 | exact |
| SPEC-05 | REQ-04 | Presses needed to return to the first animation | equal to the animation count | exact |
| SPEC-06 | REQ-05 | Full-frame refresh rate | 200 Hz | minimum |
| SPEC-07 | REQ-05 | Frame period | 5.00 ms | ±5 % |
| SPEC-08 | REQ-05 | Cycle-to-cycle timing jitter | 250 µs | maximum |
| SPEC-09 | REQ-05 | Frames displayed while partially updated | 0 | exact |
| SPEC-10 | REQ-06 | Intensity of an off LED, relative to the same LED lit | 1 % | maximum |
| SPEC-11 | REQ-06 | Blanking interval while the layer changes | 5 µs | minimum |
| SPEC-12 | REQ-07 | Intensity spread across all 512 LEDs, all commanded on | ±20 % | maximum |
| SPEC-13 | REQ-08 | Time-averaged luminous intensity per lit LED | 20 mcd | minimum |
| SPEC-14 | REQ-08 | Viewing angle at half intensity | 60° | minimum |
| SPEC-15 | REQ-09 | Spacing between LED centres | 22.86 mm | ±1 mm |
| SPEC-16 | REQ-09 | Deviation of any LED from its nominal grid position | 1 mm | maximum, any axis |
| SPEC-17 | REQ-09 | Straightness of a row or column over the full 160 mm | 1.5 mm | maximum |
| SPEC-18 | REQ-10 | Supply voltage | 5.0 V | ±5 % (4.75–5.25 V) |
| SPEC-19 | REQ-10 | Steady-state supply current | 700 mA | maximum |
| SPEC-20 | REQ-10 | Inrush current at switch-on | 2 A for 10 ms | maximum |
| SPEC-21 | REQ-11 | Continuous run without intervention | 4 h | minimum |
| SPEC-22 | REQ-11 | Unplanned resets during that run | 0 | exact |
| SPEC-23 | REQ-11 | Component temperature, as a fraction of its rated maximum | 80 % | maximum |
| SPEC-24 | REQ-11 | Ambient temperature during the run | 25 °C | ±5 °C |
| SPEC-25 | REQ-12 | Intensity at 4 h, relative to intensity at 5 min | 90 % | minimum |
| SPEC-26 | REQ-13 | Time to resume normal operation after power is restored | 2 s | maximum |
| SPEC-27 | REQ-13 | Successful recoveries from abrupt power loss | 10 of 10 | exact |
| SPEC-28 | REQ-13 | Writes to non-volatile memory during normal operation | 0 | exact |
| SPEC-29 | REQ-14 | Lattice deflection under its own weight | 2 mm | maximum |
| SPEC-30 | REQ-15 | Permanent deformation after being carried 10 m | 1 mm | maximum |
| SPEC-31 | REQ-16 | Voltage present on any conductor a user can touch | 5 V | maximum |
| SPEC-32 | REQ-16 | Clearance from the lowest conductor to the surface the cube stands on | 5 mm | minimum |
| SPEC-33 | REQ-17 | Assembly time for one person, excluding firmware | 20 h | maximum |
| SPEC-34 | REQ-17 | Total solder joints | 1300 | maximum |
| SPEC-35 | REQ-18 | Disassembly steps needed to reflash firmware | 0 | exact |
| SPEC-36 | REQ-18 | Time to flash new firmware | 60 s | maximum |
| SPEC-37 | REQ-19 | Files changed to add one animation | 2 | maximum |
| SPEC-38 | REQ-19 | Lines changed outside the animation module | 0 | exact |
| SPEC-39 | REQ-20 | Temperature of any surface a user would normally touch | 45 °C | maximum |
| SPEC-40 | REQ-20 | Ambient temperature when that measurement is taken | 25 °C | ±3 °C |
| SPEC-41 | REQ-21 | Wire or lead protruding beyond a solder joint | 1 mm | maximum |
| SPEC-42 | REQ-21 | Radius of any exposed edge or corner | 0.5 mm | minimum |
| SPEC-43 | REQ-22 | Total parts cost, excluding shipping, tools and the adapter | 100.00 EUR | maximum |
