# LED Cube — Power Budget

Current drawn from the 5 V rail, where the power ends up, and whether anything gets hot.

Worst case throughout is every LED commanded on. Because one layer is lit at a time, that means 64 LEDs conducting at any instant — never 512.

## Operating point

LED current depends on which 74HCT595 variant is fitted, since the register's own output resistance is in series with the column resistor. The layer MOSFET contributes nothing measurable — at 3 mΩ it drops about 1 mV — so it is left out:

```
I_led = (5.0 V − 3.2 V) / (R_out + 220 Ω)
```

| Register | R_out | Current per LED | One layer |
|---|---|---|---|
| Weak | 85 Ω | 5.9 mA | 378 mA |
| **Nominal** | **67 Ω** | **6.3 mA** | **401 mA** |
| Strong | 50 Ω | 6.7 mA | 427 mA |

Budget figures below use the nominal case, with the strong-register case carried through as worst case.

## Current from the 5 V rail

| Block | Nominal | Worst case | Basis |
|---|---|---|---|
| LED matrix | 401 mA | 427 mA | 64 LEDs, one layer at a time |
| ESP32 DevKit module | 150 mA | 150 mA | Core with WiFi and Bluetooth off, plus the board's regulator and USB-serial chip |
| Shift registers, quiescent | 10 mA | 10 mA | Nine packages |
| **Total** | **561 mA** | **587 mA** | |

Against the 700 mA ceiling in SPEC-19 that is 80 % nominal, 84 % worst case — 113 mA spare even with the most favourable registers fitted.

A 5 V 1.5 A adapter runs at about a third of its rating.

## Where the power goes

At the nominal 6.3 mA:

| Consumer | Power | Share |
|---|---|---|
| LEDs, 64 lit | 1285 mW | 46.6 % |
| 64 column resistors | 554 mW | 20.1 % |
| 8 column shift registers | 169 mW | 6.1 % |
| Conducting layer MOSFET | 0.5 mW | 0.0 % |
| ESP32 module | 750 mW | 27.2 % |
| **Total** | **2757 mW** | |

Just under half the power reaches the LEDs. The rest is the cost of the drive scheme and the controller.

The resistors burning 554 mW is the price of setting current with a resistor rather than regulating it; the registers' 169 mW is the price of sourcing rather than sinking. Neither is a fault — both were chosen knowingly — but together they are a quarter of the total.

## Per-component dissipation

| Component | Each | Rating | Used |
|---|---|---|---|
| 220 Ω column resistor | 8.7 mW | 250 mW (1/4 W) | 3.5 % |
| 74HCT595 column register | 21.1 mW | DIP-16, ≈80 °C/W | +1.7 °C rise |
| IRLB3813PBF layer MOSFET | 0.5 mW | 230 W | 0.0 % |

Nothing is close to a limit. SPEC-23 caps components at 80 % of their rated maximum; the worst case here is 3.5 %.

### The column resistors run continuously

Worth being explicit, because it is easy to get wrong. Each LED is lit 12.5 % of the time, so it is tempting to apply the same duty cycle to its resistor.

That is wrong. The resistor sits in the **column**, and with every LED commanded on, every column is driven in every layer period. The resistor conducts 100 % of the time — only the LED downstream of it changes.

```
per resistor:  P = I²R = (6.3 mA)² × 220 Ω = 8.7 mW      ← correct
not:           P = I²R × 0.125              = 1.1 mW     ← wrong by 8×
```

Even the correct figure is 3.5 % of a quarter-watt part, so the error would not have caused damage here. It would matter if the resistor value or the current were ever raised.

## Thermal

Total dissipation inside the base is roughly 1.5 W once the LEDs' own 1.3 W is excluded — and the LEDs are spread across a 160 mm lattice in open air, so they contribute almost nothing to base temperature.

The largest single source is the ESP32 module at 750 mW, most of it in the linear regulator dropping 5 V to 3.3 V. That is the part to check first against the 45 °C surface limit in SPEC-39.

The layer MOSFETs dissipate half a milliwatt between them. They will be at ambient, and their TO-220 tabs need no heatsinking whatever.

A bare board on legs is the best case for cooling: every part is in free air with no enclosure trapping heat. ST-23 and ST-39 measure this after the four-hour run rather than relying on the estimate.
