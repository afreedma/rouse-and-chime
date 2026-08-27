# Rouse & Chime — logo variant spec

Four exported variants, all sharing the same underlying geometry:

| File | Sun | Text |
| --- | --- | --- |
| `rouse-chime-mark-filled.svg` | Filled | None |
| `rouse-chime-mark-outline.svg` | Outlined | None |
| `rouse-chime-lockup-filled.svg` | Filled | Comfortaa Bold, "Rouse" / "& Chime" |
| `rouse-chime-lockup-outline.svg` | Outlined | Comfortaa Bold, "Rouse" / "& Chime" |

These are geometric reference sketches, not production-clean vector
art. Treat them as the source of truth for shape and proportion — but
redraw with clean anchor points, proper stroke-to-fill conversion, and
embedded/outlined text before sending anything to a fabricator.

## Wave function (shared curve, used in every variant)

- Amplitude: ±4 units (mark-only files) / ±3 units (lockup files, at
  their smaller internal scale — see "Lockup scaling" below)
- Wavelength: 34 units at full scale (zero-crossings every 17 units)
- Drawn as quadratic Bezier segments between consecutive zero-crossings,
  control point offset by the amplitude, alternating sign each segment
- Runs from x = -85 to x = +85 at full scale

## Sun

- Radius: 17 units at full scale, centred at (0,0)
- Top: circular arc from (-17,0) to (17,0), sweeping over the top
- Base: the wave function's own segment between x = -17 and x = 17 —
  not a flat line, so the sun's underside is the same curve as the
  ocean outside it
- **Filled variants:** solid fill, same colour as the stroke
- **Outline variants:** no fill, same 2-unit stroke weight as
  everything else in the mark

## Sound-wave arcs

Three arcs, unfilled, sharing the sun's centre. Each spans +/-65 degrees
from vertical (130 degrees total, always passing through the top point
at (0,-r)). Gaps between successive elements grow rather than staying
constant — this progressive spacing is deliberate (final choice over an
earlier equal-gap version) and reads more like a chime's sound
spreading and fading outward.

| Arc | Radius (full scale) | Gap from previous element | Endpoint (+/-x, y) |
| --- | --- | --- | --- |
| 1 | 34 | 17 | (30.81, -14.37) |
| 2 | 58 | 24 | (52.57, -24.51) |
| 3 | 88 | 30 | (79.76, -37.19) |

## Lockup scaling

The mark-only files use the full-scale geometry above (sun radius 17,
arcs 34/58/88). The lockup files scale the same geometry down by a
factor of **0.7647** (sun radius 17 to 13) so the mark sits comfortably
next to a 34px wordmark — this was a deliberate proportional choice,
not a precise height-match: an earlier version calculated an exact
scale so the mark's height matched the wordmark's cap-height-to-baseline
span precisely, but it was set aside in favour of this looser,
more organic-feeling pairing. If redrawing by hand, apply the 0.7647
scale factor to every coordinate in the "Sun" and "Sound-wave arcs"
sections above, and use +/-3 unit wave amplitude / 26-unit wavelength
(i.e. also scaled by 0.7647) for the lockup's wave curve.

## Wordmark

- Typeface: **Comfortaa**, weight 700 (Bold) — free, SIL Open Font
  License, available via Google Fonts
- Two lines, left-justified, set to the right of the mark:
  - Line 1: "Rouse"
  - Line 2: "& Chime"
- Font size: 34px in the reference files
- Line spacing: 36px between baselines
- The second line's baseline is intended to align with the mark's own
  horizon line (the wave's zero-crossing level) — in the reference
  files this is baseline y=116, matching the mark's local y=0 mapped
  through its translate/scale.
- **Not embedded as outlines in the reference SVGs** — the `<text>`
  elements reference the font by name only. Before this goes anywhere
  near a fabricator or gets committed as final artwork, convert the
  text to outlines/paths in whatever vector tool does the final pass,
  so rendering doesn't depend on the font being installed.

## Considered and rejected

- **All-caps and small-caps wordmark treatments** — tried in both
  Orbitron and Comfortaa; mixed case was preferred throughout, and
  Comfortaa's small-caps rendering in particular was synthetic
  (browser-faked, since the font has no true small-cap glyphs) and
  looked poor.
- **Orbitron and Space Grotesk as the wordmark typeface** — both
  considered and rejected as too narrow/condensed/rigid against the
  mark's soft, organic curves. Comfortaa's wider, rounder letterforms
  were judged a better match.
- **Equal-span arcs with equal gaps** — an earlier version had all
  three arcs at the same 21-unit gap from each other; the progressive
  gap (17/24/30) in the current spec was preferred.
- **Precise height-matching between mark and wordmark** — tried and
  technically achieved (scale factor derived exactly from the
  wordmark's real cap-height, accounting for the fact neither "Rouse"
  nor "Chime" has descenders), but rejected in favour of the looser
  0.7647 scale described above, which was felt to look more organic.

## Known open item

Exact physical size for etching hasn't been decided. Recommend a test
etch on scrap material at the actual target size before committing to
a final case order — fine detail (particularly the wave's small
amplitude) is the most likely thing to blur or fill in depending on
the laser and material, and this matters for all four variants above.
