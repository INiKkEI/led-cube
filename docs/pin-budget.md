# LED Cube — Pin Assignment

Every connection the schematic needs: controller pins, shift register pins, chain order, and the mapping from bits to LEDs.

## Controller — ESP32 DevKit

| Signal | Pin | Direction | Notes |
|---|---|---|---|
| SCK | GPIO 18 | out | VSPI default clock |
| MOSI | GPIO 23 | out | VSPI default data out |
| LATCH | GPIO 21 | out | Common RCLK to all nine registers |
| OE | GPIO 22 | out | Common blanking for the eight column registers only |
| BTN | GPIO 27 | in | Momentary to ground, internal pull-up enabled |

Five pins used. GPIO 1 and 3 stay with the USB-serial bridge for flashing and debug output.

### Why these pins

VSPI rather than HSPI: HSPI's defaults include GPIO 12 and GPIO 15, both strapping pins. GPIO 12 sets the flash voltage and must be low at power-on — a register holding it high would leave the board unable to boot.

None of the five is a strapping pin (0, 2, 5, 12, 15), a flash pin (6–11), or input-only (34–39).

## Shift registers — 74HCT595 × 9

All nine share the same pin usage. U1–U8 drive the columns; U9 drives the layer MOSFET gates.

| Pin | Name | Connection |
|---|---|---|
| 16 | VCC | +5 V |
| 8 | GND | Ground |
| 14 | SER | Serial data in — from MOSI on U1, from the previous register's QH′ on U2–U9 |
| 11 | SRCLK | Shift clock — common to all nine, from GPIO 18 |
| 12 | RCLK | Latch — common to all nine, from GPIO 21 |
| 13 | OE | U1–U8: common, from GPIO 22. **U9: tied to ground** |
| 10 | SRCLR | Tied to +5 V on all nine (clear never used) |
| 9 | QH′ | Serial out to the next register's SER. Unused on U9 |
| 15 | QA | First output of that register's group |
| 1–7 | QB–QH | Remaining seven outputs |

There are no local decoupling capacitors at the registers — the design relies on a single 470 µF bulk capacitor at the power jack.

### Why U9's OE is grounded

Blanking removes the column drive, and with no column current no LED can light regardless of layer state. So blanking only needs to reach U1–U8.

If OE also blanked U9, the MOSFET gates would go high-impedance during every blanking interval and hold charge unpredictably. Leaving U9 permanently enabled keeps the gates firmly driven at all times.

## Column mapping

Columns are indexed `col = y × 8 + x`, with x and y from 0 to 7.

| Register | Columns | Row |
|---|---|---|
| U1 | 0–7 | y = 0 |
| U2 | 8–15 | y = 1 |
| U3 | 16–23 | y = 2 |
| U4 | 24–31 | y = 3 |
| U5 | 32–39 | y = 4 |
| U6 | 40–47 | y = 5 |
| U7 | 48–55 | y = 6 |
| U8 | 56–63 | y = 7 |

Within each register, QA is the lowest column of its group (x = 0) and QH the highest (x = 7). Each output goes through its own 220 Ω resistor to the column anode.

## Layer mapping

| U9 output | Layer | Drives |
|---|---|---|
| QA | 0 — bottom | Gate of layer 0 MOSFET |
| QB | 1 | Gate of layer 1 MOSFET |
| QC | 2 | Gate of layer 2 MOSFET |
| QD | 3 | Gate of layer 3 MOSFET |
| QE | 4 | Gate of layer 4 MOSFET |
| QF | 5 | Gate of layer 5 MOSFET |
| QG | 6 | Gate of layer 6 MOSFET |
| QH | 7 — top | Gate of layer 7 MOSFET |

Exactly one bit is set at a time. Each MOSFET's drain goes to that layer's common cathode, source to ground.
