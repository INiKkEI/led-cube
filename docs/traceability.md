# LED Cube — Traceability

Links each requirement in `requirements.md` to the quantities in `system-specifications.md` that make it measurable, and to the test in `acceptance-plan.md` that verifies it. Each specification is in turn verified by the matching system test in `system-test-plan.md` — SPEC-nn by ST-nn.

## Requirements

| Requirement | What it covers | Pri | Specs | Test |
|---|---|:--:|---|---|
| REQ-01 | At least 5 different animations | M | SPEC-01 | AT-01 |
| REQ-02 | Button changes the animation | M | SPEC-02 | AT-02 |
| REQ-03 | One press gives exactly one change | M | SPEC-03, SPEC-04 | AT-03 |
| REQ-04 | Animations cycle round, none unreachable | M | SPEC-05 | AT-04 |
| REQ-05 | No visible flicker or stutter | M | SPEC-06 … SPEC-09 | AT-05 |
| REQ-06 | LEDs that are off stay dark | M | SPEC-10, SPEC-11 | AT-06 |
| REQ-07 | Even brightness across the cube | S | SPEC-12 | AT-07 |
| REQ-08 | Readable in daylight | M | SPEC-13, SPEC-14 | AT-08 |
| REQ-09 | LEDs straight and evenly spaced | M | SPEC-15 … SPEC-17 | AT-09 |
| REQ-10 | Runs from one ordinary adapter | M | SPEC-18 … SPEC-20 | AT-10 |
| REQ-11 | 4 h continuous run | M | SPEC-21 … SPEC-24 | AT-11 |
| REQ-12 | No dimming over 4 h | S | SPEC-25 | AT-12 |
| REQ-13 | Survives losing power | M | SPEC-26 … SPEC-28 | AT-13 |
| REQ-14 | Stands unaided | M | SPEC-29 | AT-14 |
| REQ-15 | Survives being carried | M | SPEC-30 | AT-15 |
| REQ-16 | Safe to touch, and safe to stand on metal | M | SPEC-31, SPEC-32 | AT-16 |
| REQ-17 | Buildable in 20 h | S | SPEC-33, SPEC-34 | AT-17 |
| REQ-18 | Reflash without dismantling | M | SPEC-35, SPEC-36 | AT-18 |
| REQ-19 | New animation without touching anything else | S | SPEC-37, SPEC-38 | AT-19 |
| REQ-20 | Touchable surfaces stay under 45 °C | M | SPEC-39, SPEC-40 | AT-20 |
| REQ-21 | No sharp edges or wire ends | M | SPEC-41, SPEC-42 | AT-21 |
| REQ-22 | Parts cost 100 EUR or less | M | SPEC-43 | AT-22 |

## Interfaces

Links each interface in `architecture.md` to the tests in `integration-test-plan.md` that exercise it.

| Interface | What it carries | Integration tests |
|---|---|---|
| IF-1 | Power input from the barrel jack | IT-09, IT-17 |
| IF-2 | Controller to register chain — SCK, MOSI, LATCH, OE | IT-02, IT-05, IT-06, IT-07, IT-10 |
| IF-3 | Column registers to LED anodes, 64 lines | IT-03, IT-12, IT-13 |
| IF-4 | Layer register to MOSFET gates, 8 lines | IT-11 |
| IF-5 | Layer switches to ground, 8 LAY lines | IT-04, IT-11, IT-15 |
| IF-6 | Button to controller | IT-16 |
| IF-7 | USB, for flashing and debug output | IT-01 |
| IF-8 | Matrix to board, 72 through-holes | IT-08, IT-14 |

## Coverage

22 requirements, 43 specifications, 8 interfaces. Verified by 22 acceptance tests, 43 system tests and 17 integration tests.

Every requirement is quantified by at least one specification and verified by one acceptance test. Every specification is verified by one system test. Every interface is exercised by at least one integration test, and every integration test exercises at least one interface. Nothing exists without a requirement or an interface behind it.

18 requirements are **M** (must) and 4 are **S** (should): REQ-07, REQ-12, REQ-17, REQ-19. Those four are the ones that may fail with a written reason; the other 19 have to pass.
