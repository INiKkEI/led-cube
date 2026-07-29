# LED Cube — Integration Test Plan

Checks that the blocks in `architecture.md` work together across their interfaces. Ordered as a bring-up sequence: each test assumes only what came before it, so a failure points at the block just added.

Stage 1 runs on a breadboard before the PCB is ordered. Everything after assumes the board has arrived.

Equipment: multimeter, breadboard, a few spare LEDs and resistors, the firmware debug mode.

## Stage 1 — Breadboard, before ordering the PCB

| Test | How to test | Pass if | Result |
|---|---|---|:--:|
| IT-01 | Flash a minimal program that prints over serial and toggles one pin. | Serial output appears and the pin toggles. | |
| IT-02 | Wire one 74HCT595 on 5 V, driven from the ESP32 at 3.3 V. Shift in 10101010 and latch. | Outputs alternate high and low — the 3.3 V drive is being read correctly at a 5 V supply. | |
| IT-03 | Add eight 220 Ω resistors and eight LEDs to that register's outputs, cathodes to ground. | Each LED lights when its bit is set. About 6.3 mA per LED, about 50 mA total into the register. | |
| IT-04 | Put an N-channel MOSFET between the LEDs' common cathode and ground, gate driven from a spare pin. | LEDs light only when the gate is high. Less than 50 mV across the MOSFET while conducting. | |
| IT-05 | Chain a second 595 from the first. Shift 16 bits containing a single 1 and note where it appears. | The bit lands on the predicted output. Record which register receives the first byte sent — firmware depends on it. | |
| IT-06 | Drive OE high, then low, with data latched. | All outputs go inactive while OE is high, whatever is latched. | |
| IT-07 | Run the timer interrupt shifting 72 bits and switching a dummy layer line. | Debug output reports 200 Hz or more, with jitter inside 250 µs. | |

## Stage 2 — Bare PCB, nothing fitted

| Test | How to test | Pass if | Result |
|---|---|---|:--:|
| IT-08 | Measure resistance between the 5 V and ground nets, then check continuity from a sample of column pads to their register pins. | No short between 5 V and ground. Every sampled net continuous. | |
| IT-09 | Fit only the jack and the 470 µF bulk capacitor, then apply power. | 5.0 V ±5 % at every IC supply pad. Nothing warm to the touch after a minute. | |

## Stage 3 — Board populated, matrix not yet attached

| Test | How to test | Pass if | Result |
|---|---|---|:--:|
| IT-10 | Fit all nine registers and the controller. Shift a single 1 through all 72 bits, one step at a time. | Exactly one output active at each step, advancing in the expected order across all nine registers. | |
| IT-11 | Fit the eight layer MOSFETs and select each layer in turn. | Each LAY net pulls below 50 mV when selected, and floats when not. | |
| IT-12 | Set each column bit in turn and measure the 64 column pads. | All 64 go high. None stuck, and none pulling a neighbour with it. | |

## Stage 4 — Matrix attached

| Test | How to test | Pass if | Result |
|---|---|---|:--:|
| IT-13 | Attach the bottom layer only, and light all 64 of its LEDs. | All 64 light. Layer current about 400 mA. Brightest and dimmest within ±20 % of each other. | |
| IT-14 | Attach the remaining layers. Light each of the 512 LEDs one at a time. | Exactly one lights each time, at the coordinate you asked for. | |
| IT-15 | Light a single LED and darken the room. | No other LED visibly glows. | |
| IT-16 | Press the button. | Firmware registers exactly one press and the animation changes. | |
| IT-17 | Command every LED on and run for 10 minutes. | Supply current 700 mA or less. No component above 80 % of its rated temperature. | |
