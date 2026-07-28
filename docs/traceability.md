# LED Cube — Traceability

Links each requirement in `requirements.md` to the test in `acceptance-plan.md` that verifies it.

| Requirement | What it covers | Pri | Test |
|---|---|:--:|---|
| REQ-01 | At least 5 different animations | M | AT-01 |
| REQ-02 | Button changes the animation | M | AT-02 |
| REQ-03 | One press gives exactly one change | M | AT-03 |
| REQ-04 | Animations cycle round, none unreachable | M | AT-04 |
| REQ-05 | No visible flicker or stutter | M | AT-05 |
| REQ-06 | LEDs that are off stay dark | M | AT-06 |
| REQ-07 | Even brightness across the cube | S | AT-07 |
| REQ-08 | Readable in daylight | M | AT-08 |
| REQ-09 | LEDs straight and evenly spaced | M | AT-09 |
| REQ-10 | Runs from one ordinary adapter | M | AT-10 |
| REQ-11 | 4 h continuous run | M | AT-11 |
| REQ-12 | No dimming over 4 h | S | AT-12 |
| REQ-13 | Survives losing power | M | AT-13 |
| REQ-14 | Survives a wrong or reversed supply | M | AT-14 |
| REQ-15 | Stands unaided | M | AT-15 |
| REQ-16 | Survives being carried | M | AT-16 |
| REQ-17 | No touchable live parts | M | AT-17 |
| REQ-18 | Buildable in 20 h | S | AT-18 |
| REQ-19 | Reflash without dismantling | M | AT-19 |
| REQ-20 | New animation without touching anything else | S | AT-20 |
| REQ-21 | Touchable surfaces stay under 45 °C | M | AT-21 |
| REQ-22 | No sharp edges or wire ends | M | AT-22 |
| REQ-23 | Parts cost 100 EUR or less | M | AT-23 |

## Coverage

23 requirements, 23 tests, one test each. No requirement is untested, and no test verifies something that isn't a requirement.

19 requirements are **M** (must) and 4 are **S** (should): REQ-07, REQ-12, REQ-18, REQ-20. Those four are the ones that may fail with a written reason; the other 19 have to pass.
