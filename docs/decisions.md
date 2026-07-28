# LED Cube — Decision Records

One record per decision. Each states the situation, what was chosen, why, and what follows from it. Records are not edited once accepted — a later decision supersedes an earlier one and says so.

All accepted 2026-07-28 unless noted.

---

## ADR-01 — Blue diffused 5 mm LEDs

**Status:** Accepted

### Context

The LED sets almost every other number in the design: forward voltage fixes the electrical headroom, luminous intensity fixes whether the cube is readable, and lens type fixes how it looks from an angle. 512 of them are needed, so the choice is expensive to reverse.

### Decision

5 mm blue LEDs with a diffused lens.

### Options considered

| Colour | Forward voltage | Brightness to the eye | Headroom at 5 V |
|---|---|---|---|
| Green / yellow-green | ~2.1 V | Highest — eye sensitivity peaks near 555 nm | Comfortable |
| **Blue** | **~3.2 V** | **Lower than green at equal output** | **Tight** |
| White | ~3.2 V | Good, but more unit-to-unit variation | Tight |
| Red | ~2.0 V | Low in bright light | Best |

A water-clear lens concentrates light into a 20–30° cone and rates much higher in mcd; a diffused lens spreads it over 60–100° and rates lower. On a cube the LEDs are viewed from many angles at once, and inner LEDs are seen through their neighbours, so clear lenses make the cube look patchy and shift as the viewer moves.

### Reasoning

Blue was chosen for appearance rather than performance, and it is the harder choice electrically. At 5 V:

```
headroom = 5.0 V − 3.2 V = 1.8 V   (blue)
headroom = 5.0 V − 2.1 V = 2.9 V   (green)
```

That missing 1.1 V is what would otherwise sit across the current-setting resistor, which is what holds LED current steady against part variation.

### Consequences

- Every downstream electrical decision is constrained by 1.8 V of headroom.
- Daylight legibility becomes the weakest requirement in the project.
- Diffused lenses cost roughly 4–10× in rated mcd against clear, which compounds it.
- Superseded by nothing, but ADR-04 and ADR-05 exist largely to work within this.

---

## ADR-02 — 900 mil voxel pitch

**Status:** Accepted

### Context

Pitch fixes the physical size of the cube, the size of the controller board, and whether an LED's own leads can span the gap to its neighbour.

### Decision

900 mil between LED centres in all three axes.

### Reasoning

```
900 mil = 900 × 0.0254 mm      = 22.86 mm
        = 9 × 0.1 in            = 9 × 2.54 mm
lattice  = 7 gaps × 22.86 mm    = 160.02 mm
```

900 mil is exactly nine times the 0.1 inch grid that through-hole parts, headers and prototyping board all use, so every LED lands on a grid intersection rather than between them.

### Consequences

- Cube is a 160 mm lattice.
- The controller board cannot be smaller than 160 × 160 mm, since the 64 column holes span that.
- Typical 5 mm LED leads are 24–26 mm against a 22.86 mm gap. Bending and joining consumes the difference, so lead reach is a real risk (RISK-06) and wants measuring before a jig is cut.

---

## ADR-03 — Layer multiplexing

**Status:** Accepted

### Context

512 LEDs at 20 mA each would draw over 10 A if driven continuously, and would need 512 separate drive lines.

### Decision

Light one horizontal layer at a time, cycling through all eight.

### Reasoning

```
LEDs lit simultaneously = 512 / 8 = 64
drive lines needed      = 64 columns + 8 layers = 72   (not 512)
duty cycle per LED      = 1 / 8 = 12.5 %
average current         = peak current / 8
```

### Consequences

- Wiring drops from 512 lines to 72.
- Every LED is dark 87.5 % of the time, so apparent brightness is roughly an eighth of the same LED driven continuously. This is the single largest reason daylight legibility is marginal.
- A refresh rate must now be chosen high enough that the scanning is invisible — see ADR-07.
- Only one layer's worth of current flows at any instant, which is what keeps the supply requirement modest.

---

## ADR-04 — Plain 74HCT595 driving the LEDs directly

**Status:** Accepted

### Context

A 74HC/HCT595's absolute maximum current through its VCC or GND pin is **70 mA for the whole package**, not per output. The per-pin limit of 35 mA suggests more is possible, but the package limit is the binding one.

### Decision

Drive the LEDs straight from eight 74HCT595 shift registers, with no additional drivers, accepting the reduced current that implies.

### Options considered

| Option | Extra parts | Current per LED | Extra solder joints |
|---|---|---|---|
| **Plain 74HCT595** | **none** | **~6 mA** | **0** |
| TPIC6B595 | none (drop-in, 150 mA sinks) | 20 mA | 0, but flips the topology |
| 74HC595 + TD62783 source arrays | 8 chips | 20 mA | ~130 |
| 74HC595 + 64 PNP transistors | 128 parts | 20 mA | ~300 |

### Reasoning

At the 20 mA an LED is normally driven at:

```
8 outputs × 20 mA = 160 mA  →  2.3× over the 70 mA package limit
```

So direct drive at full current is not possible. Published 8×8×8 builds that do drive 595s directly run at 7–8 mA: one documented blue cube uses 220 Ω with a 3.5 V LED, giving (5 − 3.5) / 220 ≈ 7 mA, and 8 × 7 = 56 mA, inside the limit. The constraint is real and those builds design around it rather than ignore it.

### Consequences

- Fewest parts and least soldering of any option — directly serves the 20 h build budget.
- Cube runs at roughly a third of normal LED current, so it reads well indoors and probably not beside a bright window. REQ-08 is knowingly at risk.
- The 70 mA package limit becomes a hard floor under the resistor value (ADR-05).
- Reversible later: TPIC6B595 is pin-compatible in role and would restore 20 mA, at the cost of flipping the topology — its outputs sink rather than source, so columns would become cathodes and the layers would move to the high side.

---

## ADR-05 — 220 Ω column resistors

**Status:** Accepted

### Context

The resistor sets LED current. The exact 74HCT595 part is not chosen yet — it will be whichever is cheapest and in stock — and different manufacturers have different output resistance.

### Decision

220 Ω, one per column, 64 total.

### Reasoning

A shift register output is not an ideal voltage source. It behaves as 5 V behind an internal resistance of roughly 50–85 Ω depending on manufacturer, in series with the external resistor:

```
I = (5 V − Vf − Vds) / (Rout + R)
  = (5.0 − 3.2 − 0.05) / (Rout + R)
```

| R | Current (weak chip, 85 Ω) | Current (strong chip, 50 Ω) | Package load | % of 70 mA max | Spread |
|---|---|---|---|---|---|
| 150 Ω | 7.4 mA | 8.7 mA | 60–70 mA | **85–100 %** | ±8.0 % |
| **220 Ω** | **5.7 mA** | **6.5 mA** | **46–52 mA** | **66–74 %** | **±6.1 %** |

150 Ω reaches exactly 100 % of an absolute maximum rating on a strong chip. Absolute maximums are limits that must never be reached, not design targets.

220 Ω is also *better* for uniformity, which is counter-intuitive. The external resistor is identical across all 64 columns; the chip's internal resistance is the part that varies. Making the external resistor larger means it dominates the total, so chip-to-chip variation shrinks from ±8 % to ±6.1 %.

### Consequences

- Safe with any 74HCT595 variant, which was the point — the part can be chosen on price.
- Roughly 20 % dimmer than 150 Ω would have been, on a design already marginal for brightness.
- Operating range 5.7–6.5 mA peak, taken as 6 mA nominal: 0.75 mA average at 12.5 % duty, 384 mA per layer.
- Never reduce this value. It is the only thing keeping the shift registers inside their rating.

---

## ADR-06 — 74HCT, not 74HC

**Status:** Accepted

### Context

The ESP32 drives 3.3 V logic. The shift registers run from 5 V, because the LEDs need it.

### Decision

Use the HCT variant of the 595 throughout.

### Reasoning

```
74HC  minimum input high at VCC = 5 V:  0.7 × 5 V = 3.5 V   →  3.3 V is below it
74HCT minimum input high (TTL levels):  2.0 V               →  3.3 V is comfortably above
```

HC parts scale their input threshold with supply voltage; HCT parts use fixed TTL thresholds designed for exactly this situation.

### Consequences

- 3.3 V drive works directly, with no level shifters and no series resistors.
- Same pinout, same package, same price — this costs nothing.
- HC must never be substituted for HCT when restocking. A cube built with HC parts may appear to work and then fail intermittently, which is a miserable fault to chase.

---

## ADR-07 — 200 Hz refresh rate

**Status:** Accepted. Supersedes an initial choice of 60 Hz.

### Context

With layer multiplexing, each LED is pulsed rather than lit continuously. A refresh rate must be chosen high enough that no flicker is visible (REQ-05).

### Decision

200 Hz full-frame refresh. 60 Hz was considered first and rejected.

### Reasoning

60 Hz is the intuitive choice because monitors run at it — but a monitor's pixels emit light for essentially the whole frame, while these LEDs emit for 12.5 % of it. Short bright pulses are far more visible as flicker than continuous light at the same rate. Three specific failures at 60 Hz:

- Peripheral vision fuses at a higher rate than central vision, so the cube would look steady when stared at and shimmer when glimpsed — which is exactly the situation REQ-05 describes.
- Eye movement across a low-duty multiplexed display smears points into dotted trails.
- Mains here is 50 Hz, so most room lighting pulses at 100 Hz. A 60 Hz cube beats against that at a visible low frequency.

The cost of going higher is nil:

```
frame rate 200 Hz  →  layer rate = 200 × 8 = 1600 Hz
                   →  layer period = 1 / 1600 = 625 µs
shift 72 bits @ 4 MHz = 18 µs  =  2.9 % of the layer period
```

On a 240 MHz processor, 1600 interrupts per second is well under 1 % load. Duty cycle stays 12.5 % regardless of rate, so there is no brightness or power penalty either.

### Consequences

- Comfortably clear of flicker fusion in peripheral vision and under eye movement.
- 625 µs per layer, with 97 % of it idle after the shift completes.
- The interrupt must be in IRAM and WiFi disabled, or flash access and the WiFi stack will delay it (RISK-04).

---

## ADR-08 — Ninth shift register for layer selection

**Status:** Accepted

### Context

The eight layer MOSFET gates need driving. They could come from eight ESP32 GPIO pins, or from another shift register on the existing chain.

### Decision

A ninth 74HCT595 in the same daisy chain, driving the MOSFET gates.

### Reasoning

```
chain = 9 registers × 8 bits = 72 bits per layer
      = 72 × 8 layers        = 576 bits per frame
```

Adding a register costs three shared SPI pins rather than eight dedicated GPIOs, and puts the gates on the same 5 V logic level as everything else — which matters because a MOSFET gate driven from 3.3 V would switch less completely.

### Consequences

- Layer and column data travel together, so a single 72-bit transfer updates everything.
- Blanking must be timed carefully: the OE line has to disable outputs *before* new data latches, or the previous layer's pattern briefly appears on the new layer as ghosting (RISK-05).
- Six ESP32 pins freed compared with direct GPIO drive.

---

## ADR-09 — ESP32 DevKit module on headers

**Status:** Accepted

### Context

The controller can be a ready-made module plugged into headers, or a bare ESP32-WROOM soldered to the board.

### Decision

DevKit module on 0.1 inch headers.

### Consequences

- USB, the regulator and the flashing circuit all come with it — nothing to design or debug.
- A destroyed controller is a plug-out replacement rather than a rework job.
- Sits taller and looks less integrated than a soldered module.
- Loses the practice of designing USB and power circuitry, which was one of the project's stated learning goals. Worth revisiting on a second revision.

---

## ADR-10 — Barrel jack input

**Status:** Accepted

### Context

The cube needs 5 V from a mains adapter.

### Decision

5.5 / 2.1 mm barrel jack, centre positive.

### Options considered

| Option | Assembly | Notes |
|---|---|---|
| **Barrel jack** | **Through-hole, trivial** | **Needs a dedicated adapter; polarity can be got wrong** |
| USB-C | Fine-pitch SMD | Needs 5.1 kΩ CC resistors to draw over 0.5 A; adapters everywhere |
| Both | Both of the above | Needs care that both cannot be plugged in at once |

### Consequences

- Fits the through-hole-only constraint with no SMD work.
- Reverse polarity becomes physically possible, which drove ADR-13.
- A specific adapter has to be kept with the cube.

---

## ADR-11 — Matrix soldered directly to the board

**Status:** Accepted

### Context

The lattice has to connect to the controller board by 72 lines — 64 column anodes and 8 layer cathodes.

### Decision

Soldered directly into plated through-holes. No connector.

### Consequences

- Cheapest and most rigid, and the through-holes sit on the same 900 mil grid as the cube.
- Matrix and board become a single assembly: replacing either means desoldering 72 joints.
- A dead LED deep inside the lattice is awkward to reach. No requirement forces replaceability, so this is accepted rather than mitigated.

---

## ADR-12 — Bare PCB on legs, no enclosure

**Status:** Accepted

### Context

The electronics need somewhere to live.

### Decision

The controller board is the base. Legs raise it off the surface. No case.

### Consequences

- Nothing to design, print or buy.
- The original REQ-17 ("no live part may be touchable") could not be met and was rewritten. At 5 V there is no shock hazard to a person — that voltage cannot drive meaningful current through skin — so the requirement was re-aimed at the real risk: the board shorting against a conductive surface.
- Leg height becomes a specification: at least 5 mm clearance from the lowest conductor, which with a 1 mm limit on lead protrusion means legs at least 6 mm tall.
- No dust protection and no way to hide the wiring.

---

## ADR-13 — No reverse-polarity or over-current protection

**Status:** Accepted. Supersedes two earlier decisions.

### Context

A barrel jack can be connected backwards. The design went through three positions: a P-channel MOSFET in series, then a reverse-parallel diode with a resettable fuse, then neither.

### Decision

None. The jack feeds the 5 V rail directly.

### Reasoning

The intermediate step is worth recording, because it explains why a plain series diode — the obvious answer — was rejected. Anything in series with the supply drops voltage, and that drop comes out of the resistor's share, not the LED's:

| Protection | Drop | LED current | % of nominal |
|---|---|---|---|
| None | 0 mV | 5.74 mA | 100 % |
| P-channel MOSFET | 35 mV | 5.62 mA | 98 % |
| Series Schottky | 350 mV | 4.59 mA | **80 %** |
| Series silicon diode | 800 mV | 3.11 mA | **54 %** |

A silicon diode would have cost nearly half the brightness on a design already short of it. The reverse-parallel arrangement — a diode across the input, conducting only on reverse and blowing a fuse — avoided that entirely and cost nothing, but was dropped along with the requirement to test it.

### Consequences

- **A reversed or wrong-voltage adapter will destroy the board.** There is nothing to stop it.
- **A short across the 5 V rail has nothing to interrupt it.**
- No voltage is lost to protection, so LED current is at its maximum for the chosen resistor.
- Roughly 1 EUR and two parts would restore both protections. This is stated explicitly in IF-1 rather than left as a silent absence, so the omission is visible to anyone reading the design.

---

## ADR-14 — Timing measured by firmware, not instruments

**Status:** Accepted

### Context

Refresh rate, frame period, jitter, blanking interval and button latency all need verifying. There is no oscilloscope or logic analyser available.

### Decision

A firmware debug mode counts interrupts and prints measured timing over the serial port.

### Consequences

- Six specifications become measurable that would otherwise sit unverified.
- Measurement is continuous rather than a snapshot, which catches intermittent jitter that a scope trace might miss.
- The firmware is measuring itself. A fault in the timer configuration could plausibly produce both wrong behaviour and a wrong report of it — an external instrument would be an independent check, and remains worth borrowing once.
- The debug mode is a permanent part of the firmware, not scaffolding.

---

## ADR-15 — VSPI pins, avoiding the strapping pins

**Status:** Accepted

### Context

The design needs five ESP32 pins: SCK, MOSI, LATCH, OE and a button. The module exposes far more than five, but they are not interchangeable — several groups cannot be used at all, and the SPI peripheral has two different default pin sets.

### Decision

VSPI defaults for the bus — SCK on GPIO 18, MOSI on GPIO 23 — with LATCH on GPIO 21, OE on GPIO 22 and the button on GPIO 27.

### Reasoning

Three groups of pins are unavailable before any choice is made:

| Pins | Why |
|---|---|
| GPIO 6–11 | Wired to the internal SPI flash. Using them stops the chip working. |
| GPIO 34–39 | Input only, with no internal pull-ups. Cannot drive anything, and cannot host the button. |
| GPIO 1, 3 | UART0, used by the USB-serial bridge for flashing and the debug output that ADR-14 depends on. |

That leaves the choice between the two hardware SPI peripherals. **HSPI's defaults are GPIO 13, 12, 14 and 15 — and both GPIO 12 and GPIO 15 are strapping pins**, sampled at power-on to configure the chip. GPIO 12 sets the flash voltage and must be low at that moment. A shift register input holding it high would leave the board unable to boot, and the symptom looks like a dead module rather than a wiring error.

VSPI's defaults (GPIO 18 and 23) are ordinary pins. The remaining three signals were placed on GPIO 21, 22 and 27, none of which is a strapping pin either.

### Consequences

- Hardware SPI is used at its default pins, so no remapping is needed in firmware.
- No signal touches a strapping pin, so nothing the cube does can prevent the board from starting.
- OE needs an external pull-up rather than a firmware default, because the pin is high-impedance until firmware runs — recorded in IF-2.
- Five pins used of roughly a dozen unrestricted ones. Ample room for a second button or a status LED later.
