# LED Cube — User Requirements Specification

---

## 1. What This Is

This document says what the LED cube must do and how it must behave. It covers the finished cube — the LEDs, the base, and everything the user sees or touches. Version 1 leaves out colour, remote and computer control, battery power, and any way for users to write their own animations; it is also not meant to be sold.

I want to build it myself with the tools I already have, in reasonable time, cheaply, and be able to fix or change it later without starting over. It should look good — steady, bright, neat — with more than one thing to watch. It should survive a whole evening running and still work the next day, and be safe to touch, leave plugged in, and look at.

---

## 2. User Requirements

Each has a unique ID, a priority (**M** must / **S** should / **C** could), and a verification method (**T** test / **A** analysis / **I** inspection / **D** demonstration).

| ID | Requirement | Pri | Ver |
|---|---|:--:|:--:|
| REQ-01 | The cube must have at least **5** clearly different animations. | M | D |
| REQ-02 | Pressing a button must switch to another animation. | M | D |
| REQ-03 | One press must change the animation exactly once. | M | T |
| REQ-04 | Pressing enough times must come back round to the first animation, and every animation must be reachable. | M | D |
| REQ-05 | No flicker or stutter may be visible from 0.5 m in normal room light, watching for **60 s**, in any animation or while switching between them. | M | I |
| REQ-06 | LEDs that are switched off must stay dark, whatever pattern is showing. | M | I |
| REQ-07 | With every LED on, the cube should look evenly lit — no LED or area noticeably brighter or dimmer than the rest. | S | I |
| REQ-08 | On a table about 1 m from a window in daylight, with no direct sun on the cube, a viewer 1 m away must be able to correctly identify which LEDs are lit. | M | T |
| REQ-09 | The LEDs must look straight and evenly spaced from any side. | M | I |
| REQ-10 | The cube must run from one ordinary mains adapter and nothing else. | M | I |
| REQ-11 | The cube must run at full brightness for **4 h** or more without failing, resetting, or needing attention. | M | T |
| REQ-12 | After 4 h of running, the brightness should not have visibly dropped. | S | T |
| REQ-13 | Pulling the power at any moment must not damage the cube or stop it working normally next time. | M | T |
| REQ-14 | Plugging in the wrong or a reversed supply must not cause permanent damage. | M | T |
| REQ-15 | The cube must stand on a flat surface by itself, with nothing propping it up. | M | I |
| REQ-16 | The cube must survive being picked up by its base and carried across a room. | M | D |
| REQ-17 | No live part may be touchable in normal use. | M | I |
| REQ-18 | One person with average soldering skill should be able to build it in **20 h** or less, not counting firmware. | S | T |
| REQ-19 | The firmware must be updatable without taking the cube apart. | M | D |
| REQ-20 | Adding a new animation should not mean changing anything else. | S | D |
| REQ-21 | No surface you would normally touch may get hotter than **45 °C**, during or after the 4 h run in REQ-11. | M | T |
| REQ-22 | No surface you handle may have sharp edges, burrs, or wire ends sticking out. | M | I |
| REQ-23 | The parts for one complete cube must cost no more than **100 EUR** — LEDs, boards, connectors, case, controller. Shipping, tools, and the mains adapter don't count. | M | A |
