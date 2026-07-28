# LED Cube — Acceptance Plan

The cube is accepted when every test below passes, or has a written reason for not passing.

Equipment: multimeter, thermometer.

| Test | How to test | Pass if | Result |
|---|---|---|:--:|
| AT-01 | Cycle through every animation and count them. | 5 or more, each clearly different from the others. | |
| AT-02 | Press the button and watch the display. | A different animation starts. | |
| AT-03 | Press once, 20 times over, noting the animation before and after each press. | 20 presses give exactly 20 changes. | |
| AT-04 | Keep pressing and write down the order until an animation repeats. | Every animation appears, then it comes back round to the first. | |
| AT-05 | Sit 0.5 m away in normal room light and watch each animation for 60 s, including the switch between them. | No flicker, stutter, or half-drawn image seen. | |
| AT-06 | In a dark room, light one LED at a time and check its neighbours and the rest of its column. | Only the commanded LED glows. | |
| AT-07 | Turn every LED on and look at the cube from each face. | No LED or area stands out as brighter or dimmer. | |
| AT-08 | On a table ~1 m from a window in daylight, no direct sun, show 10 known patterns and name each from 1 m away before checking. | All 10 identified correctly. | |
| AT-09 | Look straight down each face, then along the diagonals. | Rows and columns look straight and evenly spaced. | |
| AT-10 | Plug into the mains adapter with nothing else connected. | Runs normally. | |
| AT-11 | Run the brightest animation for 4 h, noting the time at the start and end. | Still running, never reset, nothing touched. | |
| AT-12 | During AT-11, look at the cube from the same spot in the same light at 5 min and at 4 h. | No visible dimming. | |
| AT-13 | Pull the plug mid-animation, 10 times, plugging back in each time. | Starts up normally every time, no damage. | |
| AT-14 | With the cube unplugged, measure resistance across the power input both ways round, then check the protection part is fitted the way the schematic shows. | The reverse direction reads open or very high, and the part is the right way round. | |
| AT-15 | Stand it on a flat table and let go. | Stands on its own, no leaning or sagging. | |
| AT-16 | Lift by the base, carry across the room, set down, then check the joints. | Nothing bent, broken, or loosened. | |
| AT-17 | Measure the gap under the board, then measure the voltage on every conductor you can touch while it runs, then stand it on a metal sheet. | Gap 5 mm or more, nothing above 5 V, and the cube runs normally on the metal. | |
| AT-18 | Write down your hours as you build. | 20 h or less, not counting firmware. | |
| AT-19 | Flash new firmware with the cube fully assembled. | New firmware runs, nothing was taken apart. | |
| AT-20 | Add one new animation. | Only the animation code changed, and everything else behaves as before. | |
| AT-21 | Straight after AT-11, measure every surface you would normally touch with the thermometer. | All readings 45 °C or below. | |
| AT-22 | Run a hand and then a cloth over every surface you handle. | Nothing catches, no wire ends sticking out. | |
| AT-23 | Add up the parts list. | 100 EUR or less, not counting shipping, tools, or the mains adapter. | |
