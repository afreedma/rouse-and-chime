# Project context for Claude Code

## What this is

A small open-hardware regulator board that feeds a Sony BRAVIA 8's
undocumented S-CENTER SPEAKER IN terminal from a line-level audio source
and a 12V trigger signal, stepping the trigger down to ~5V. See README.md
for the full background — this file is working context for you (Claude)
across sessions, not user-facing documentation.

## Where things stand

Design phase. Nothing has been built or bench-tested yet. The priority
right now is: simulate the regulator circuit, confirm it behaves as
expected, and only then move to a physical breadboard/Jiffy box build.
PCB layout and manufacturing (KiCad → PCBWay) come later, after the
Jiffy-box prototype has run successfully in the real system for a while.

## Key constraints (don't relax these without being asked)

- Target trigger voltage: **5.0V**, not the ~6.7V a genuine Sony soundbar
  was measured at open-circuit. 5.0V was deliberately chosen as the safer,
  better-evidenced figure — this is a considered decision, not a
  placeholder.
- Approximate load at the TV end: **~60mA, ~83Ω effective**. Bench testing
  should use an 82Ω, 0.5–1W resistor as a dummy load stand-in for the TV.
- The terminal's pinout and voltage spec are **community-reverse-engineered**
  (AVS Forum / AVForums), not from an official Sony datasheet. Treat any
  numbers here as best-available-evidence, not certainty — flag anywhere
  the design leans on an assumption instead of a measured fact.
- This adapter connects to expensive AV equipment (a Sony BRAVIA 8). Be
  conservative: prefer verifying in simulation before physical changes,
  flag anything that could risk damaging the TV, and don't skip the
  bench-test-into-a-dummy-load step even if a change looks obviously safe.

## Component choices already made

- Regulator: 7805 fixed 5V linear regulator (matches the 5.0V target
  exactly — don't substitute an adjustable regulator without discussing
  it first).
- Input cap: 330nF polyester (substituted for ceramic at this value,
  which the user's usual supplier — Jaycar — doesn't stock).
- Output cap: 100nF ceramic.
- No heatsink planned (estimated dissipation ~0.48W against the 7805's 2W
  free-air rating) — worth re-confirming once real measurements exist.

## Tools available in this environment

- KiCad is installed (schematic capture, ngspice-based simulation, and
  PCB layout all in one tool).
- Prefer running the regulator simulation directly in KiCad's Eeschem
  simulator so the schematic used for simulation is the same one that
  later becomes the PCB layout — avoid maintaining a separate LTspice
  file that could drift from the KiCad version.
- Standalone netlists for quick circuit-only checks live in
  `hardware/sim/` — useful for fast iteration without opening the full
  schematic GUI.

## Licensing

Hardware design files: CERN-OHL-P v2 (permissive).
Documentation: CC-BY-4.0.
Keep new files under the right license split as the project grows
(design files vs. docs/photos/write-ups).

## Working style

- Be direct and unhedged — flag risks plainly rather than softening them.
- Don't skip or shortcut the validate-before-build sequence (simulate →
  breadboard → bench-test → prototype in system → PCB) even under time
  pressure, given this feeds real equipment.
