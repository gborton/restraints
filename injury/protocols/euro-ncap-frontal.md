# Euro NCAP 2026 — Crash Protection, Frontal Impact

Sources:

- "Crash Protection — Frontal Impact Protocol" **v1.2 (July 2026)** [S5]. Its injury limit tables
  are unchanged from v1.1 (Sept 2025). The scoring presentation changed (see below).
- Technical Bulletin CP 005 "Data Acquisition and Injury Calculation" v1.4.3 (June 2026) [S6].
- CP 007 "Assessment Modifiers" v1.2 (July 2026) [S21].

H3 dummies appear only in some seats. THOR 50M and Q6/Q10 are out of scope.

## Full-scale tests [S5 Figures 1–2, §1.3]

| Test | Configuration (read from protocol figures) | Driver | Front passenger | Rear |
|---|---|---|---|---|
| **MPDB** | Vehicle **50 ± 1 km/h** into an oncoming Mobile Progressive Deformable Barrier at **50 ± 1 km/h**. **50 % ± 25 mm** VUT overlap, vertical alignment ± 25 mm. Trolley + face **1400 ± 20 kg** [S22] | THOR 50M | **H3 5F** (seat `3` in LHD) | Q6 (behind driver), Q10 (behind passenger) |
| **FWDB** | Full Width Deformable Barrier face (CP 102) on a rigid wall, **35 ± 1 km/h** | **H3 5F** | THOR 50M | **H3 5F behind the front passenger** (seat `6` in LHD) |

The pre-2026 Euro NCAP full-width rigid test was 50 ± 1 km/h (FW test protocol v1.2.1, Nov 2021
[S32]). **The 2026 FWDB test is 35 ± 1 km/h with a deformable face.**

## Sled & virtual load cases [S5 Scoring, §3.3]

| Occupant | Dummies / severities as printed in the scoring table |
|---|---|
| Driver | 50 km/h **H3 50M**; 56 km/h H3 95M; 35 km/h **H3 50M**; 35 km/h **H3 5F**; 56 km/h **H3 5F** |
| Front passenger | 50 km/h H3 95M; 56 km/h **H3 5F**; 35 km/h **H3 5F**; 35 km/h **H3 50M**; 56 km/h H3 95M |

Pulses: 35 km/h = official Euro NCAP FWDB pulse. 50 km/h = UN R137 pulse (VM-supplied). 56 km/h
= VM full-width rigid wall pulse, or a generic high-severity pulse [S5 §3.3.1]. Knee slider and
tibia criteria are not applied to virtual tests. Lower-leg points are awarded by default unless a
physical test exceeds the TI or tibia-force LPL [S5 §3.3].

## Points [S5 Scoring]

Frontal impact total 40: offset (MPDB) 20, full width 10, sled & virtual 10.

| Test / seat | Points |
|---|---|
| MPDB H3 5F front passenger | 5 (4 body regions × 1.25) |
| FWDB H3 5F driver | 5 |
| FWDB H3 5F rear passenger | 2.5 |
| Sled & virtual (per occupant) | 2.5 sled + 2.5 virtual |

Adult body regions: **Head & Neck**, **Chest & Abdomen**, **Knee, Femur & Pelvis**, **Lower Leg,
Foot & Ankle**.

## Scoring method

- **Two limits per criterion**: higher performance limit (HPL), below which full score is given,
  and lower performance limit (LPL), at or beyond which the score is zero [S5 §3.4–3.5].
- **Sliding scale between HPL and LPL.** CP 007 v1.2 states "a sliding scale is applied to HIC and
  Ares 3ms" [S21 §3]. The superseded AOP v9.3 defined the general method as "linear
  interpolation" between the two limits [S20 §2.1]. The v1.2 colour-band scheme table shows
  `< HPL = 100 %`, intermediate columns labelled `80 % / 40 % / 20 %`, and `≥ LPL = 0 %`, with band
  width `(LPL − HPL)/3` [S5 §3.4]. ⚠ How the 80/40/20 % labels combine with the sliding scale is
  specified in CP 006 "Colour band and test data", which Euro NCAP lists as **"coming soon"** (not
  published as of this pass).
- **Lowest-scoring criterion** sets the region score [S5 §3.5].
- **Region colour (v1.2)** from the region's % score after modifiers: Green = 100 %, Yellow 66.67 –
  < 100 %, Orange 33.33 – < 66.67 %, Brown 0 – < 33.33 %, Red = 0 % [S5 §3.5]. (v1.1 instead
  mapped colours to fixed points 1.25 / 1.0 / 0.75 / 0.5 / 0.)
- **Capping limits** are listed per criterion (head/neck/chest). The 2026 documents do not state
  the consequence. The last published definition (AOP v9.3) reads: "Exceeding a capping limit
  generally indicates unacceptable high risk at injury … In all cases, this leads to loss of all
  points related to the tests" [S20 §2.1.1]. ⚠ Assume this still applies until CP 006 is
  published. v1.1 stated capping is not applied to sled and virtual tests [S5 v1.1 §3.5]. That
  sentence was removed in v1.2.
- **Modifiers** (CP 007): −20 % to −100 % of a region or test score for things like head
  bottoming-out, unstable airbag contact, steering-wheel contact, shoulder-belt load
  (≥ 6.00 kN for 5th and 50th), submarining (iliac force drop within 1 ms), knee load
  variable/concentrated (femur > 3.8 kN or knee > 6 mm expected in the extended zone), pedal
  displacement/blocking, A-pillar and steering-column displacement, and door opening
  [S5 §4.3.1; S21].

## Evaluation rules [S5, S6]

- **Loading phase only.** It usually ends when filtered head Ax crosses 0 g after its minimum
  peak [S6 §3].
- **Head**: HIC15 and Ares‑3ms (cumulative [S19]) are applied only when peak Ares > 80 g or there
  is other evidence of hard contact. Otherwise the head gets full points [S5 §3.5.1; S21 §3].
- **Neck extension My is evaluated only with head-to-interior contact** [S5 §3.5.5].
- Chest-acceleration spikes from early seatbelt pretensioner firing are ignored [S5 §3.5.5].
- Data are delivered unfiltered/prefiltered in ISO-MME 1.6 SI units. Euro NCAP software applies
  the filters [S6 §1–2].

## H3 limits (HPL – LPL, capping in parentheses) [S5 §3.5.1–3.5.4]

| Region | Criterion | H3 5F | H3 50M |
|---|---|---|---|
| Head & neck | HIC15 | 500–700 (700) | 500–700 (700) |
| | Ares‑3ms (g) | 72–80 (80) | 72–80 (80) |
| | Fx shear (kN) | 1.2–2.0 (2.7, driver only) | 1.9–3.1 (3.10) |
| | Fz tension (kN) | 1.70–2.6 (2.9, driver only) | 2.7–3.3 (3.30) |
| | My extension @ OC (Nm) | 36–49 (57, driver only) | 42–57 (57) |
| Chest & abdomen | Dchest (mm), 35 km/h | 18–34 (34) | 20–42 (42) |
| | Dchest (mm), 50 km/h | 18–34 (34) | 20–42 (42) |
| | Dchest (mm), 56 km/h | 22–42 (42) | – |
| | VC (m/s) | 0.5–1.0 (1.0) | 0.5–1.0 (1.0) |
| Knee, femur, pelvis | Femur force (kN) | 2.6–6.2 | 3.8–9.1 |
| | Knee slider (mm) (not virtual) | 6–15 | 6–15 |
| Lower leg | Tibia Index (not virtual) | LPL 1.3 | 0.4–1.3 |
| | Tibia compression (kN) (not virtual) | LPL 8.0 | 2.0–8.0 |

Chest-severity rows map to tests: 35 km/h = FWDB full-scale and 35 km/h sled/virtual. 50 km/h =
MPDB and 50 km/h sled/virtual. 56 km/h = sled/virtual only. Chest LPLs are biomechanical:
65-year-old for 18–34 and 20–42, and 45-year-old for 22–42 [S5 §3.5.2].

## Calculation details (CP 005) [S6 §3]

| Criterion | Formula / channel | Filter |
|---|---|---|
| Ares | √(Ax²+Ay²+Az²) of `??HEAD0000??AC[X,Y,Z]A` | CFC 1000 |
| HIC15 | standard (`criteria/head.md`) | CFC 1000 |
| Ares‑3ms | cumulative 3 ms exceedance [S19] | CFC 1000 |
| Neck Fx shear, Fz tension | peaks of `??NECKUP00??FO[X,Z]` | CFC 1000 |
| My extension @ OC | `My − Fx·0.01778` using `MOYB`, `FOXB` | CFC 600 |
| Dchest | max compression of `??CHST0003??DSXC` | CFC 180 |
| VC | sf 1.3; D_const 0.229 (50M), 0.187 (5F); 5-point derivative | CFC 180 |
| Femur | abs(min(`??FEMR[LE,RI]00??FOZB`)) | CFC 600 |
| Knee slider | abs(min(`??KNSL[LE,RI]00??DSXC`)) | CFC 180 |
| Tibia Index | `|M_R/(M_C)_R| + |Fz/(F_C)_Z|` while Fz < 0. 50M 225 Nm / 35.9 kN; 5F 114 Nm / 22.9 kN | CFC 600 |
| Belt force (modifier) | max(`??SEBE0003B3FO0D`) | CFC 60 |
| Submarining (modifier) | sudden drop in either iliac force within 1 ms (`??ILAC[LE,RI]00??FOXB`) | CFC 600 |
