# LED Cube — Power Budget

Current drawn from the 5 V rail, where the power ends up, and whether anything gets hot.

Worst case throughout is every LED commanded on. Because one layer is lit at a time, that means 64 LEDs conducting at any instant — never 512.

## Operating point

LED current depends on which 74HCT595 variant is fitted, since the register's own output resistance is in series with the column resistor:

```
I_led = (5.0 V − 3.2 V − V_mosfet) / (R_out + 220 Ω)
```

| Register | R_out | Current per LED | One layer |
|---|---|---|---|
| Weak | 85 Ω | 5.8 mA | 370 mA |
| **Nominal** | **67 Ω** | **6.0 mA** | **384 mA** |
| Strong | 50 Ω | 6.5 mA | 416 mA |

Budget figures below use **6 mA nominal**, with the strong-chip case carried through as worst case.

## Current from the 5 V rail

| Block | Nominal | Worst case | Basis |
|---|---|---|---|
| LED matrix | 384 mA | 416 mA | 64 LEDs, one layer at a time |
| ESP32 DevKit module | 150 mA | 150 mA | Core with WiFi and Bluetooth off, plus the board's regulator and USB-serial chip |
| Shift registers, quiescent | 10 mA | 10 mA | Nine packages |
| **Total** | **544 mA** | **576 mA** | |

Against the 700 mA ceiling in SPEC-19 that is 78 % nominal, 82 % worst case — 124 mA spare even with the most favourable registers fitted.

A 5 V 1.5 A adapter runs at about a third of its rating.

## Where the power goes

At 6 mA nominal:

| Consumer | Power | Share |
|---|---|---|
| LEDs, 64 lit | 1229 mW | 45.6 % |
| 64 column resistors | 507 mW | 18.8 % |
| 8 column shift registers | 196 mW | 7.3 % |
| Conducting layer MOSFET | 15 mW | 0.5 % |
| ESP32 module | 750 mW | 27.8 % |
| **Total** | **2696 mW** | |

Just under half the power reaches the LEDs. The rest is the cost of the drive scheme and the controller.

The resistors burning 507 mW is the price of setting current with a resistor rather than regulating it; the registers' 196 mW is the price of sourcing rather than sinking. Neither is a fault — both were chosen knowingly — but together they are a quarter of the total.

## Per-component dissipation

| Component | Each | Rating | Used |
|---|---|---|---|
| 220 Ω column resistor | 7.9 mW | 250 mW (1/4 W) | 3.2 % |
| 74HCT595 column register | 24.5 mW | DIP-16, ≈80 °C/W | +2.0 °C rise |
| Layer MOSFET (conducting) | 15 mW | — | negligible |

Nothing is close to a limit. SPEC-23 caps components at 80 % of their rated maximum; the worst case here is 3.2 %.

### The column resistors run continuously

Worth being explicit, because it is easy to get wrong. Each LED is lit 12.5 % of the time, so it is tempting to apply the same duty cycle to its resistor.

That is wrong. The resistor sits in the **column**, and with every LED commanded on, every column is driven in every layer period. The resistor conducts 100 % of the time — only the LED downstream of it changes.

```
per resistor:  P = I²R = (6 mA)² × 220 Ω = 7.9 mW      ← correct
not:           P = I²R × 0.125            = 1.0 mW     ← wrong by 8×
```

Even the correct figure is 3 % of a quarter-watt part, so the error would not have caused damage here. It would matter if the resistor value or the current were ever raised.

## Thermal

Total dissipation inside the base is roughly 1.5 W once the LEDs' own 1.2 W is excluded — and the LEDs are spread across a 160 mm lattice in open air, so they contribute almost nothing to base temperature.

The largest single source is the ESP32 module at 750 mW, most of it in the linear regulator dropping 5 V to 3.3 V. That is the part to check first against the 45 °C surface limit in SPEC-39.

A bare board on legs is the best case for cooling: every part is in free air with no enclosure trapping heat. ST-23 and ST-39 measure this after the four-hour run rather than relying on the estimate.
