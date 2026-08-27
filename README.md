<picture>
  <source media="(prefers-color-scheme: dark)" srcset="docs/branding/rouse-chime-mark-filled-dark-mode.svg">
  <source media="(prefers-color-scheme: light)" srcset="docs/branding/rouse-chime-mark-filled.svg">
  <img src="docs/branding/rouse-chime-mark-filled.svg" alt="Rouse & Chime logo" width="160">
</picture>

# Rouse & Chime

A small regulator/adapter board that feeds a Sony BRAVIA 8's undocumented
**S-CENTER SPEAKER IN** terminal from a line-level audio source and a 12V
trigger output — stepping the trigger down to the ~5V the terminal expects.

**Status:** Design phase — not yet built or bench-tested. Nothing in this
repo has been verified against real hardware yet.

## Background

The Sony BRAVIA 8 (K-55XR80) has an S-CENTER SPEAKER IN terminal, confirmed
in Sony's official Reference Guide and Setup Guide PDFs for this model, but
the terminal's electrical spec is not published by Sony. The pinout and
target voltage used here are reverse-engineered by the AVS Forum / AVForums
community, not from an official datasheet.

**Terminal spec (community-derived, not Sony-published):**
- 3.5mm TRS jack: Tip = line-level audio, Ring = DC trigger, Sleeve = ground
- Target trigger voltage: 5.0V (a genuine Sony soundbar measured ~6.7V
  open-circuit, but 5V is the safer, better-evidenced figure used here)
- Approximate input load at 5V: ~60mA, ~83Ω effective

Because this is an unofficial, reverse-engineered connection to an
expensive display, the project deliberately favours a slow, methodical
path: simulate first, prototype on a breadboard/Jiffy box, bench-test
under a dummy load, run it in the real system for a while, and only then
consider a manufactured PCB and case.

## How it works

The source device is a miniDSP Flex HT: line-level audio out on RCA, 12V
trigger out on a 3.5mm TS mono jack. The board combines both into a single
3.5mm TRS output matching the BRAVIA 8's S-CENTER SPEAKER IN jack.

- **Audio path:** line-level analog audio (RCA in from the miniDSP Flex HT)
  feeds straight through to the TRS tip.
- **Trigger path:** the 12V trigger (TS mono jack in) is stepped down
  through a 7805 linear regulator to a fixed 5.0V, feeding the TRS ring.
- **Ground:** audio ground and trigger ground share one common point on
  the board; cable shields are left floating (EMI protection only, not
  current-carrying) to avoid a second ground path.

## Circuit (initial values, subject to simulation/bench verification)

| Part | Role |
| --- | --- |
| 7805 (fixed 5V linear regulator) | Steps 12V trigger down to 5.0V |
| 330nF polyester capacitor | Regulator input capacitor |
| 100nF ceramic capacitor | Regulator output capacitor |

Estimated dissipation ~0.48W against the 7805's 2W free-air rating — no
heatsink expected to be necessary, but this should be confirmed once
built.

Bench test target: a clean 5.0V under an 82Ω, 0.5–1W dummy load before
connecting to the real TV.

## Repo layout

```
hardware/         KiCad project (schematic, PCB layout)
hardware/sim/      Standalone SPICE netlists and any sourced component models
manufacturing/     Gerbers, BOM, pick-and-place — populated once a PCB is ordered
docs/              Build guide, photos, exported schematic references
```

## Status / open items

- [ ] Circuit validated in simulation (KiCad/ngspice)
- [ ] Breadboard build, bench-tested into an 82Ω dummy load
- [ ] Prototype (Jiffy box) built and run in the real system
- [ ] KiCad PCB layout
- [ ] First manufactured board + case ordered (PCBWay or similar)

## License

Hardware design files (`hardware/`, `manufacturing/`) are licensed under
**CERN-OHL-P v2** (permissive) — see `LICENSE-HARDWARE`.

Documentation (this README, `docs/`) is licensed under **CC-BY-4.0** — see
`LICENSE-DOCS`.

## Disclaimer

This connects to an undocumented terminal on a Sony display using a
community-reverse-engineered specification, not an official one. Building
and using this adapter is at your own risk. Nothing here is endorsed by
or affiliated with Sony.
