# LED Cube — Risk Register

Likelihood and impact are High, Medium or Low. Impact is judged against the requirements in `requirements.md`.

| ID | Risk | Lik. | Imp. | Mitigation |
|---|---|:--:|:--:|---|
| RISK-01 | Visible brightness steps every eight columns. Each shift register's output resistance differs, and all eight of its columns inherit it. Fails SPEC-12. | M | M | Buy all nine registers from one reel or batch. Measure each chip's output voltage under load before fitting. The 220 Ω resistor is already sized to dominate the variation. |
| RISK-02 | Blue LED forward voltage higher than the assumed 3.2 V. At 3.5 V only about 0.65 V is left across the resistor, roughly halving the current and making it far more sensitive to spread. | M | H | Measure the forward voltage of a sample at 6 mA, then choose the resistor from that measurement rather than from the assumed value. |
| RISK-03 | Shift registers run at 46–52 mA against a 70 mA absolute maximum. Absolute maximum ratings are limits, not design points. | L | H | Never fit a resistor below 220 Ω. Confirm the per-package current with ST-12 and ST-19 before any long run. |
| RISK-04 | ESP32 interrupt jitter breaks the refresh timing. Interrupt handlers left in flash are blocked whenever flash is accessed, and the WiFi stack pre-empts freely. Fails SPEC-07 and SPEC-08. | M | H | Put the refresh handler in IRAM, leave WiFi and Bluetooth disabled, pin the timer to one core, and keep flash reads out of the interrupt path. |
| RISK-05 | Ghosting — faint light on LEDs that should be off, as charge on the column lines bleeds into the next layer. Fails SPEC-10. | M | M | Blanking is already specified in SPEC-11. Make sure the layer switch turns off *before* new column data is latched, not after. |
| RISK-06 | 5 mm LED leads too short for the 900 mil pitch. Typical leads are 24–26 mm; bending and joining consumes length, and the pitch is 22.86 mm. | M | H | Measure the leads on the LEDs you actually buy before building the jig. If they are short, run tinned copper wire for the horizontal rows instead of relying on the leads. |
| RISK-07 | Lattice too floppy to stand or be carried. 160 mm of unsupported lead. Fails SPEC-32 or SPEC-33. | M | M | Use solid wire for the horizontal rows rather than lead-to-lead joints. Build and test one full layer before committing to all eight. |
| RISK-08 | First PCB revision has errors and needs a respin. This is the first board designed in Altium. | H | M | Run ERC and DRC, have the schematic reviewed, and breadboard one shift register with one MOSFET and eight LEDs before ordering. Order only one or two boards on the first run. |
