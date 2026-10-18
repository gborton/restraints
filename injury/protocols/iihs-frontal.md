# IIHS Frontal Crashworthiness — Small Overlap, Moderate Overlap, Moderate Overlap 2.0

Consumer ratings: Good / Acceptable / Marginal / Poor (G/A/M/P). Sources:

- Small Overlap Rating Protocol **Version VII (April 2024)** [S27]
- Small Overlap Crash Test Protocol Version VII (May 2021) [S28]
- Moderate Overlap 2.0 Rating Guidelines Version II (May 2024) [S8]
- Original moderate overlap "Guidelines for Rating Injury Measures" (Sept 2014) [S7]

## Tests and dummies

| Test | Configuration | Dummy / seat | Injury table |
|---|---|---|---|
| Driver-side small overlap | **64.4 ± 1 km/h (40 ± 0.6 mph), 25 ± 1 % overlap** [S27] | H3 50M driver | Table A (with KTH) |
| Passenger-side small overlap | Same, passenger side [S27 Appendix] | **H3 50M driver and H3 50M front passenger**, both "evaluated based on the criteria listed in Table 2" [S27 Appendix] | Table A (with KTH) |
| Moderate overlap (original) | (see IIHS test protocol) | H3 50M driver | Table A, femur-force version [S7] |
| Moderate Overlap 2.0 | (see IIHS test protocol) | H3 50M driver + **H3 5F 2nd-row left** (seat `4`) [S8] | Driver: Table A (with KTH). Rear: Table B |

## Data processing (IIHS small overlap test protocol v VII) [S28]

- 10 kHz sampling, 16-bit, 3 kHz analog anti-alias prefilter.
- Offset removal: mean of 100 points from −50 to −40 ms, subtracted from each channel.
- Digital filtering to SAE J211/1 (2014) classes. Foot accelerations CFC 180. Sternum deflection
  rate computed from deflection filtered at CFC 60.
- Calculations: head resultant, 3 ms clip of head and spine resultants, HIC-36 and HIC-15, Nij
  "using formulas presented in 65 Fed. Reg. 30680 [2000]", VC, sternum deflection rate, tibia
  resultant moments and TI with adjusted A-P moments, foot resultant. "All calculations comply with
  the recommendations of SAE … J1727 (2015a)", which implies a **continuous** 3 ms clip [S26].
- Polarity: SAE J1733 (2018).

## Rating rules [S7, S27]

- Each body region takes the **lowest** rating among its parameters.
- "The thigh/hip and leg/foot ratings are based on the lowest rating scored from either the left or
  right limb" [S27].
- "Injury results that round to the values shown … will receive the better of the two ratings they
  separate."

## Table A — H3 50M (small overlap v VII Table 2 [S27]; MO 2.0 Table 1 [S8])

| Region | Parameter | IARV | G/A | A/M | M/P |
|---|---|---|---|---|---|
| Head & neck | HIC15 | 700 | 560 | 700 | 840 |
| | Nij | 1.00 | 0.80 | 1.00 | 1.20 |
| | Neck tension (kN) | 3.3 | 2.6 | 3.3 | 4.0 |
| | Neck compression (kN) | 4.0 | 3.2 | 4.0 | 4.8 |
| Chest | Thoracic spine accel, 3 ms clip (g) | 60 | 60 | 75 | 90 |
| | Sternum deflection (mm) | −50 | −50 | −60 | −75 |
| | Sternum deflection rate (m/s) | −8.2 | −6.6 | −8.2 | −9.8 |
| | Viscous criterion (m/s) | 1.0 | 0.8 | 1.0 | 1.2 |
| Thigh & hip | **Knee-thigh-hip (KTH) injury risk** | – | 5 % | 15 % | 25 % |
| Leg & foot (L, R) | Tibia-femur displacement (mm) | −15 | −12 | −15 | −18 |
| | Tibia index (upper, lower) | 1.00 | 0.80 | 1.00 | 1.20 |
| | Tibia axial force (kN) | −8.0 | −4.0 | −6.0 | −8.0 |
| | Foot acceleration (g) | 150 | 150 | 200 | 260 |

**Original moderate overlap (2014 guidelines)** uses the same table, except the leg rows include
**femur axial force 7.3 / 9.1 / 10.9 kN** (IARV 9.1) with force–duration corridors, and there is
no KTH row [S7 Table 1].

Nij intercepts: 6806 / 6160 N, 310 / 135 Nm [S7 Table 2; S8 Table 2].

Additional rules:

- **Neck force–duration corridors** (tension, compression, shear): falling outside lowers an
  otherwise Good head/neck to Acceptable [S7 Fig. 2–4; S27 Fig. 10–11]. See `criteria/neck.md`.
- **Head contact**: peak resultant > 70 g from a hard-surface contact lowers head/neck one level.
  In small overlap, two distinct head contacts > 70 g are also scored as a kinematics demerit
  [S27]. Rebound handling follows the flow chart [S7 Fig. 1; S27 Fig. 9].
- **KTH** (impulse from start of compression to when force last falls below 4,050 N; corridor
  figures) [S27 Fig. 13–14; S8 App. C]. See `criteria/lower-extremity.md`.
- **Tibia moments** are adjusted before TI: upper `My − Fz·0.02832`, lower `My + Fz·0.006398`
  [S7, S8, S27].

## Table B — H3 5F rear occupant (Moderate Overlap 2.0) [S8 Table 4]

| Region | Parameter | IARV | G/A | A/M | M/P |
|---|---|---|---|---|---|
| Head & neck | HIC15 (only with contacts) | 779 | 560 | 700 | 840 |
| | Nij (only with contacts) | 1.00 | 0.80 | 1.00 | 1.20 |
| | Neck tension (kN) | 2.6 | 2.0 | 2.4 | 2.8 |
| | Neck compression (kN) | 2.5 | 2.0 | 2.5 | 3.0 |
| | Head resultant from contact | – | > 70 g → downgrade one level | | |
| Chest | Chest Index | – | 35 | 40 | 45 |
| | Shoulder belt tension (kN) | – | Good ≤ 5.9 < Marginal | | |
| Thigh | Femur axial compression (kN) | 6.2 | 4.9 | 6.2 | 7.4 |

- Nij intercepts for 5F: 4287 / 3880 N, 155 / 67 Nm [S8 Table 5].
- Neck rating always uses tension/compression. Nij and HIC apply only when the head contacts the
  interior [S8].
- Chest Index: see `criteria/chest.md`.
- MO 2.0 also rates rear-occupant restraints and kinematics (head excursion, submarining, belt
  position). These are not injury calculations [S8].
