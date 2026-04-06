# Component Selection

## Purpose

This document selects the main components for the first schematic version of the 8×8×8 monochrome LED cube controller PCB.

The goals for this revision are:

- keep the design simple enough for a first custom PCB
- keep the design understandable and easy to debug
- accept limited brightness and current margin in exchange for simplicity

This revision intentionally uses a shift-register-based drive approach instead of dedicated LED driver ICs.

---

## Selected architecture

### Final choice

- Main controller board: **ESP32-DevKitC-32E**
- LED line control: **9 × 74HCT595**
- Layer switching: **8 × BD241C**
- USB-C power input on the controller PCB
- Programming/debug through the ESP32-DevKitC-32E board itself

### Functional split

- **8 × 74HCT595** are used for the **64 LED control lines**
- **1 × 74HCT595** is used for the **8 layer-control outputs**
- **8 × BD241C** are used as the **layer switching transistors**
- the display is scanned by **multiplexing one layer at a time**

### Why this architecture was selected

- 74HCT595 is cheap, common, and easy to understand
- HCT is preferred over HC because the controller uses 3.3 V logic while the register chain runs from 5 V
- BD241C is available and easy to hand-solder
- ESP32-DevKitC-32E already includes the ESP32 module, USB interface, and onboard power regulation
- this is a practical student-project choice even if it is not the most optimized electrical solution

---

## Main controller choice

### Selected part

- **ESP32-DevKitC-32E**

### Why this part

- includes an ESP32 development board based on the ESP32 family
- includes Bluetooth and BLE support
- includes onboard USB interface and support circuitry
- includes onboard regulation for the ESP32 section
- reduces schematic risk compared with placing a bare ESP32 or module plus support parts on the first PCB

### Important consequence

The custom controller PCB does **not** need a separate 3.3 V regulator just to power the ESP32 board.

The custom PCB still needs a **5 V rail**, and that 5 V rail is supplied to the DevKit board input. The DevKit then generates the ESP32-side 3.3 V rail internally. :contentReference[oaicite:0]{index=0}

---

## Power architecture

### Main supply

The system uses a **5 V main rail** from USB-C.

That 5 V rail is used for:

- the **74HCT595** chain
- the LED cube side
- the **5 V input** to the ESP32-DevKitC-32E board

---

## Logic-level compatibility

The ESP32 GPIOs are **3.3 V logic**.

The shift-register chain is powered from **5 V**.

A plain **74HC595** is not the preferred part in this situation, because with 5 V supply its input-high requirement is not as clean a match for a 3.3 V MCU output.

A **74HCT595** is the correct choice here because it is intended to accept TTL-level logic inputs while running from 5 V. TI lists the SN74HCT595 as a 4.5 V to 5.5 V device with **TTL-compatible CMOS inputs**, **VIH(min) = 2 V**, and **VIL(max) = 0.8 V**, which makes it directly compatible with 3.3 V ESP32 GPIO.

### Decision

Use **74HCT595**, not 74HC595, for all 9 shift registers.

---

## 74HCT595 current limits used in this design

This design uses the **recommended output-current limit**, not the absolute-maximum stress limit.

TI lists the SN74HCT595 with:

- **IOL(max) = 6 mA**
- **IOH(max) = -6 mA**
- **input current II ≤ 1 µA**
- **supply voltage 4.5 V to 5.5 V**

### Interpretation for this project

For this cube, each 74HCT595 output is treated as a **6 mA maximum practical output**.

That is the current basis used for all first-pass calculations below.

This document does **not** size the LED resistors at a higher current such as 10 mA or 20 mA, because that would not match the chosen driver architecture.

---

## Display-drive method

The cube is driven by **layer multiplexing**.

At any given moment:

- only **one layer** is active
- the 64 LED control lines define which LEDs in that layer are on
- the active layer is changed rapidly to create a full 3D image

### Consequence

Each LED is only on for about **1/8 of the time**, so average brightness is lower than with static drive.

This limitation is accepted in Version 1.

---

## LED current target

Since the selected 74HCT595 output current limit is **6 mA maximum per output**, the first revision should be designed around that value.

### Design current

- **target peak LED current: 6 mA maximum per LED line**

This is the highest current that will be used in the first-pass calculations.

In practice, actual LED current may be lower because the 74HCT595 output itself has voltage drop under load.

---

## LED resistor calculation for 6 mA

### Assumptions

Use a first-pass estimate with:

- supply voltage: **5.0 V**
- blue LED forward voltage: about **3.0 V**
- transistor/switch path drop: about **0.2 V**
- target LED current: **6 mA**

### Formula

`R = (VCC - VF_LED - Vswitch) / I_LED`

### Calculation

`R = (5.0 - 3.0 - 0.2) / 0.006`

`R = 1.8 / 0.006 = 300 Ω`

### Nearest standard values

- **300 Ω**
- **330 Ω**

Because the real 74HCT595 output also drops voltage under load, the actual current with a 300 Ω or 330 Ω resistor will be somewhat less than the ideal calculation suggests.

### Final tentative resistor choice

For the first schematic version, choose:

- **330 Ω per LED line**

---

## Worst-case layer current estimate

Each layer contains **64 LEDs**.

If all 64 LEDs in the active layer are turned on at the same time, the layer transistor carries the sum of those LED currents.

### Using 6 mA per LED

`I_layer_max = 64 × 6 mA = 384 mA`

### Design implication

The layer-switching device must comfortably handle about **0.384 A** of pulsed current in the worst case.

That is why the design uses discrete layer transistors instead of trying to switch layers directly from logic outputs.

---

## Total current demand on one 74HCT595

Each 74HCT595 has **8 outputs**.

If all 8 outputs of one chip are on at the same time and each output is at the 6 mA design limit:

`I_chip_outputs = 8 × 6 mA = 48 mA`

This is one reason the design is still plausible with 74HCT595:

- **48 mA per chip** is below the usual **70 mA total through VCC or GND** family limit often associated with this class of part
- but it is still close enough that the design should remain conservative and should not be pushed to higher LED current without redesign

So the first revision keeps the LED current target at **6 mA max per output**, not above it.

---

## Layer transistor choice

### Selected part

- **BD241C**

### Why it was selected

- easy to solder because it is a through-hole TO-220 device
- strong enough for the estimated layer current
- easy to document and easy to replace later if needed

### Drawbacks

- physically large
- not compact
- requires base current because it is a BJT
- not the most elegant PCB solution, but acceptable for a first prototype

---

## Layer-control method

One dedicated **74HCT595** is used for the 8 layer-control signals.

Each of its outputs drives one **BD241C base** through a resistor.

This is acceptable for a first prototype, but it is not a high-margin drive scheme.

### Why it is still accepted

- the design target is simplicity
- expected layer current is below 0.4 A worst case using the 6 mA LED-current limit
- the project accepts reduced margin in exchange for a simpler architecture

### Required support parts per BD241C

Each transistor should include:

- **one base resistor**
- **one base-emitter pull-down resistor**

Tentative values:

- base resistor: **1 kΩ**
- base-emitter pull-down: **10 kΩ**

---

## Base-drive estimate for BD241C

This is only a first-pass plausibility check.

### Assumptions

- 74HCT595 output high is near **5 V**
- transistor base-emitter voltage is about **0.7 V**
- base resistor is **1 kΩ**

### Calculation

`I_B ≈ (5.0 - 0.7) / 1000`

`I_B ≈ 4.3 mA`

This is a reasonable first estimate for a directly driven prototype layer transistor stage.

It is not a full optimized saturation analysis. It is only enough to justify that the first revision is electrically plausible.

### Final tentative values

- **RB = 1 kΩ**
- **RBE = 10 kΩ**

---

## USB-C power input choice

### Selected part

- **USB4085-GF-A**

### Why this part

- suitable for a simple USB-C 5 V sink-only input
- easy to justify and easy to source
- mechanically stronger than weaker connector choices
- practical for a student PCB

### Implementation notes

This board only needs **5 V USB-C sink operation**, not USB Power Delivery.

The power-input section should therefore include:

- VBUS to 5 V rail
- GND to board ground
- **5.1 kΩ pulldown resistor on CC1**
- **5.1 kΩ pulldown resistor on CC2**

---

## Shift-register choice

### Selected part

- **74HCT595**

### Quantity

- **9 total**

### Use

- **8 devices** for the 64 LED control outputs
- **1 device** for the 8 layer-control outputs

### Why this part

- easy to source
- easy to understand
- easy to route in a chain
- compatible with 3.3 V ESP32 logic when powered from 5 V
- TI’s published practical output-current rating of **6 mA per output** gives a clear current basis for the first design calculations
### Limitation

This is not a dedicated LED-driver solution, so brightness headroom is limited.

That limitation is accepted in Version 1.

---

## Programming and debug

The **ESP32-DevKitC-32E** already provides the practical controller-board interface for programming and serial debug.

### Decision

Do not add a separate USB-UART bridge to the custom PCB in Version 1.

Only expose the necessary power and signal connections between the custom PCB and the ESP32-DevKitC-32E.

---

## Tentative part list

### Main active components

- **U1** — ESP32-DevKitC-32E
- **U2 to U10** — 74HCT595
- **Q1 to Q8** — BD241C
- **J1** — USB4085-GF-A

### Required support components

- **64 × LED current-limiting resistors**  
  tentative value: **330 Ω**
- **8 × BD241C base resistors**  
  tentative value: **1 kΩ**
- **8 × BD241C base-emitter pull-down resistors**  
  tentative value: **10 kΩ**
- **2 × USB-C CC resistors**  
  value: **5.1 kΩ**
- local decoupling capacitors for all logic ICs
- bulk capacitance on the 5 V rail near the LED-driving section
- headers or connectors to mount/interface the ESP32-DevKitC-32E board

---

## Design limitations accepted in Version 1

This component choice is intentional, but it has known limits:

- brightness headroom is limited because 74HCT595 is not a dedicated LED-driver IC
- the 6 mA-per-output design limit directly constrains LED brightness
- BD241C is bulky and not ideal for compact layout
- direct register-to-BJT base drive is workable but not highly optimized
- using a dev board instead of a module makes the controller section larger and less integrated

These compromises are accepted because the main goal is a working, well-documented first controller PCB.

---

## Final decision

Version 1 will use:

- **ESP32-DevKitC-32E** as the controller board
- **9 × 74HCT595** as the shift-register chain
- **8 × BD241C** as the layer transistors
- **USB4085-GF-A** as the USB-C power connector
- **330 Ω** as the tentative LED resistor value
- **1 kΩ / 10 kΩ** as the tentative BD241C base-drive resistor values
- **6 mA maximum per 74HCT595 output** as the current basis for the design calculations

This is the selected component set for the first schematic revision.
