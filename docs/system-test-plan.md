# LED Cube — System Test Plan

One test per specification in `system-specifications.md`. ST-nn verifies SPEC-nn.

Equipment: multimeter, thermometer, calipers, a firmware debug mode that prints timing and counters over serial.

| Test | How to test | Pass if | Result |
|---|---|---|:--:|
| ST-01 | Cycle through the whole animation list and count the entries. | 5 or more, each visibly different. | |
| ST-02 | Debug mode timestamps the button release and the first frame of the new animation; repeat 20 times. | Every reading 200 ms or less. | |
| ST-03 | Debug mode prints the debounce window it is using. | Between 25 and 35 ms. | |
| ST-04 | Press the button 100 times while firmware counts presses and animation changes. | 100 presses, 100 changes. | |
| ST-05 | Press repeatedly while logging the animation index. | Returns to the first animation after exactly as many presses as there are animations. | |
| ST-06 | Debug mode prints frames per second averaged over 10 s. | 200 Hz or more. | |
| ST-07 | Read the frame period from the same debug output. | 5.00 ms ±0.25 ms. | |
| ST-08 | Debug mode records the largest gap between consecutive frame periods over 60 s. | 250 µs or less. | |
| ST-09 | Read the buffer-swap code and confirm the swap can only happen between frames. | No path exists that displays a half-written buffer. | |
| ST-10 | In a dark room, after 2 minutes of letting your eyes adjust, light one LED and look at its unlit neighbours. | No glow visible on any unlit LED. | |
| ST-11 | Debug mode prints the blanking interval in microseconds. | 5 µs or more. | |
| ST-12 | Measure the voltage across every column resistor with all LEDs on, and divide by 220 Ω to get each current. | Highest and lowest within ±20 % of the average. | |
| ST-13 | Measure one LED's current, read its intensity at that current from the datasheet, and multiply by 0.125 for the duty cycle. | 20 mcd or more. | |
| ST-14 | Read the viewing angle at half intensity from the LED datasheet. | 60° or wider. | |
| ST-15 | Measure the centre-to-centre spacing of 10 adjacent LED pairs in each axis with calipers. | Every reading 22.86 mm ±1 mm. | |
| ST-16 | Hold a printed 22.86 mm grid against each face and find the worst-offset LED. | 1 mm or less. | |
| ST-17 | Lay a straightedge along each outer row and column and measure the largest bow. | 1.5 mm or less. | |
| ST-18 | Multimeter across the 5 V rail at the board, cube running at full brightness. | Between 4.75 and 5.25 V. | |
| ST-19 | Multimeter in series with the supply, every LED commanded on. | 700 mA or less. | |
| ST-20 | Switch the cube on and off 10 times and watch the fuse. | Never trips. | |
| ST-21 | Run for 4 h with debug mode printing uptime. | Uptime reaches 4 h with no break. | |
| ST-22 | Read the boot counter before and after that run. | Unchanged. | |
| ST-23 | Thermometer on every IC and MOSFET at the end of the run; compare each against its datasheet maximum. | All at 80 % of rated maximum or below. | |
| ST-24 | Thermometer in the room during the run. | Between 20 and 30 °C. | |
| ST-25 | Measure the same column resistor's voltage at 5 min and again at 4 h. | Second reading at least 90 % of the first. | |
| ST-26 | Pull the plug mid-animation, restore it, and time how long until an animation is running. Repeat 10 times. | Every time 2 s or less. | |
| ST-27 | Across those same 10 trials, watch for anything abnormal on restart. | 10 normal starts out of 10. | |
| ST-28 | Search the firmware for flash, EEPROM, or NVS write calls outside first-boot setup. | None found. | |
| ST-29 | Check the protection diode is fitted cathode to +5 V, and compare its surge current rating against the fuse's trip current and trip time. | Correctly oriented, and rated to carry the fault current until the fuse trips. | |
| ST-30 | With the cube unplugged, put the multimeter in diode mode across the power input, probing so the diode conducts. | Reads a normal forward drop, 0.7 V or less. | |
| ST-31 | Read the hold and trip currents from the fuse datasheet. | Holds at 1.0 A, trips by 2.0 A. | |
| ST-32 | Straightedge across the top layer, measure the sag in the middle. | 2 mm or less. | |
| ST-33 | Measure a few LED spacings, carry the cube 10 m by its base, then measure the same ones again. | No spacing changed by more than 1 mm. | |
| ST-34 | Multimeter from every conductor you can touch to ground, cube running. | Nothing above 5 V. | |
| ST-35 | Calipers from the lowest conductor under the board to the surface it stands on. | 5 mm or more. | |
| ST-36 | Keep a running total of your build hours. | 20 h or less, not counting firmware. | |
| ST-37 | Count the solder joints from the design: two per LED plus the board joints. | 1300 or fewer. | |
| ST-38 | Flash new firmware and count how many things you had to take apart. | Zero. | |
| ST-39 | Time a full flash from starting the upload to the cube running again. | 60 s or less. | |
| ST-40 | Add one new animation, then run `git diff --stat`. | 2 files changed or fewer. | |
| ST-41 | In that same diff, look at the lines changed outside the animation module. | Zero. | |
| ST-42 | Thermometer on every surface you would normally touch, immediately after the 4 h run. | All 45 °C or below. | |
| ST-43 | Thermometer in the room at the moment of that measurement. | Between 22 and 28 °C. | |
| ST-44 | Calipers on the longest leads sticking out past their solder joints. | 1 mm or less. | |
| ST-45 | Run a finger along every exposed edge and corner, then check the worst against a 0.5 mm reference. | Nothing sharper than 0.5 mm radius. | |
| ST-46 | Add up the parts list at the prices you actually paid. | 100.00 EUR or less, excluding shipping, tools and the adapter. | |
