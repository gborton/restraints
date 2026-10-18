# Quick Reference — Frontal Injury Limits, H3 50M & H3 5F

Single-glance matrix. Details, filters and caveats are in the linked files. `[S#]` → `sources.md`.
Forces in kN unless noted. "x–y" = Euro NCAP HPL–LPL (sliding scale). "G/A/M" = IIHS
Good/Acceptable, Acceptable/Marginal and Marginal/Poor boundaries.

## Which dummy, which test

| Program / test | Speed / config | Driver | Front passenger | Rear | File |
|---|---|---|---|---|---|
| FMVSS 208 rigid barrier | belted ≤ 56 km/h; unbelted 32–40 km/h | 50M and 5F (separate tests) | 50M and 5F | – | [fmvss-208](protocols/fmvss-208.md) |
| FMVSS 208 ODB | ≤ 40 km/h, 40 % left | 5F | 5F | – | |
| FMVSS 208 driver low-risk deployment | static | 5F (OOP) | – | – | |
| **US NCAP full frontal** | **56.3 km/h (35 mph)** rigid | **50M** | **5F** | – | [us-ncap-frontal](protocols/us-ncap-frontal.md) |
| IIHS small overlap, driver side | 64.4 km/h, 25 % | 50M | – | – | [iihs-frontal](protocols/iihs-frontal.md) |
| IIHS small overlap, passenger side | 64.4 km/h, 25 % | 50M | 50M | – | |
| IIHS moderate overlap (orig.) | – | 50M | – | – | |
| IIHS Moderate Overlap 2.0 | – | 50M | – | 5F (2nd row left) | |
| Euro NCAP 2026 MPDB | 50 + 50 km/h, 50 % | THOR | **5F** | Q6 / Q10 | [euro-ncap-frontal](protocols/euro-ncap-frontal.md) |
| Euro NCAP 2026 FWDB | **35 km/h** full width | **5F** | THOR | **5F** (behind passenger) | |
| Euro NCAP 2026 sled/virtual | 35 / 50 / 56 km/h pulses | 50M, 5F, 95M | 5F, 50M, 95M | – | |
| UN R94 ODB | 56 km/h, 40 % | 50M | 50M | – | [un-r94-r137](protocols/un-r94-r137.md) |
| UN R137 FW | 50 km/h rigid | 50M | 5F | – | |

## H3 50M limits

| Criterion | FMVSS 208 | US NCAP | IIHS G/A/M (small ovl. v VII / MO 2.0) | Euro NCAP 2026 | UN R94 | UN R137 |
|---|---|---|---|---|---|---|
| HIC15 | ≤700 | risk Eq.1 | 560/700/840 | 500–700 (cap 700)\* | HPC36 ≤1000\*\* | HPC ≤1000\*\* |
| Head 3 ms | – | – | (>70 g contact → −1 level) | 72–80 g\* (cumulative) | ≤80 g cumulative | ≤80 g cumulative |
| Nij | ≤1.0 | risk Eq.3 | 0.8/1.0/1.2 | – | – | – |
| Neck tension | ≤4.17 | risk Eq.4 | 2.6/3.3/4.0 (+corridor) | 2.7–3.3 | 3.3@0→2.9@35→1.1@≥60 ms | ≤3.3 |
| Neck compression | ≤4.00 | risk Eq.4 | 3.2/4.0/4.8 (+corridor) | – | – | – |
| Neck shear | – | – | corridor | 1.9–3.1 | 3.1@0→1.5@25–35→1.1@≥45 ms | ≤3.1 |
| Neck extension M_OC | – | – | – | 42–57 Nm | ≤57 Nm | ≤57 Nm |
| Chest 3 ms accel | ≤60 g (NHTSA calc: continuous) | – | 60/75/90 g | – | – | – |
| Sternum deflection | ≤63 mm | risk Eq.6 | 50/60/75 mm | 20–42 mm | ≤42 mm | ≤42 mm |
| VC | – | – | 0.8/1.0/1.2 | 0.5–1.0 | ≤1.0 | ≤1.0 |
| Defl. rate | – | – | 6.6/8.2/9.8 m/s | – | – | – |
| Femur | ≤10.0 (2250 lb) | risk Eq.8 | **KTH risk 5/15/25 %** (orig. MO: 7.3/9.1/10.9 kN) | 3.8–9.1 | 9.07@0→7.58@≥10 ms | ≤9.07 |
| Knee slider | – | – | 12/15/18 mm | 6–15 mm | ≤15 mm | – |
| Tibia Index | – | – | 0.8/1.0/1.2 (adjusted My) | 0.4–1.3 | ≤1.3 | – |
| Tibia compression | – | – | 4/6/8 | 2.0–8.0 | ≤8 | – |
| Foot accel | – | – | 150/200/260 g | – | – | – |

## H3 5F limits

| Criterion | FMVSS 208 (in-pos.) | FMVSS 208 OOP | US NCAP | IIHS MO 2.0 rear G/A/M | Euro NCAP 2026 | UN R137 |
|---|---|---|---|---|---|---|
| HIC15 | ≤700 | ≤700 | risk Eq.1 | 560/700/840 (contact only) | 500–700\* | HPC ≤1000\*\* |
| Head 3 ms | – | – | – | >70 g contact → −1 level | 72–80 g\* | ≤80 g |
| Nij | ≤1.0 (4287/3880 N, 155/67 Nm) | ≤1.0 (3880/3880, 155/61) | risk Eq.3 | 0.8/1.0/1.2 (contact only) | – | – |
| Neck tension | ≤2.62 | ≤2.07 | risk Eq.5 | 2.0/2.4/2.8 | 1.70–2.6 | ≤2.9 |
| Neck compression | ≤2.52 | ≤2.52 | risk Eq.5 | 2.0/2.5/3.0 | – | – |
| Neck shear | – | – | – | – | 1.2–2.0 | ≤2.7 |
| Neck extension M_OC | – | – | – | – | 36–49 Nm | ≤57 Nm |
| Chest 3 ms accel | ≤60 g | ≤60 g | – | – | – | – |
| Sternum deflection | ≤52 mm | ≤52 mm | risk Eq.7 (÷0.817) | Chest Index 35/40/45 | 18–34 mm (56 km/h: 22–42) | ≤34 mm (M1) |
| VC | – | – | – | – | 0.5–1.0 | ≤1.0 |
| Belt force | – | – | – | ≤5.9 kN | modifier ≥6.0 kN | – |
| Femur | ≤6.805 | ≤6.805 | risk Eq.9 | 4.9/6.2/7.4 | 2.6–6.2 | ≤7.0 |
| Knee slider | – | – | – | – | 6–15 mm | – |
| Tibia Index | – | – | – | – | LPL 1.3 | – |
| Tibia compression | – | – | – | – | LPL 8.0 | – |

\* Euro NCAP: HIC15 and Ares‑3ms only applied with hard contact or peak Ares > 80 g (otherwise
full head points).
\*\* UN HPC is computed only if the head contacts the vehicle.

## Constants

| Constant | H3 50M | H3 5F |
|---|---|---|
| OC offset D | 0.01778 m | 0.01778 m |
| Nij Fzc T / C | 6806 / 6160 N | 4287 / 3880 N (OOP 3880 / 3880) |
| Nij Myc F / E | 310 / 135 Nm | 155 / 67 Nm (OOP 155 / 61) |
| VC D_const, sf | 0.229 m, 1.3 | 0.187 m, 1.3 |
| TI (M_C)_R, (F_C)_Z | 225 Nm, 35.9 kN | 114 Nm, 22.9 kN |
| ISO MME dummy code | `H3` | `HF` |

## Filter quick look (sternum deflection and neck)

| Channel | US NCAP lab [S24] | Part 572 cert. [S2, S3] | Euro NCAP [S6] | UN [S9, S23] | IIHS [S28] |
|---|---|---|---|---|---|
| Sternum deflection | 600 (both dummies) | 50M 180 / 5F 600 | 180 | 180 | J211 (rate from CFC 60) |
| Neck force peaks | 1000 | 1000 | 1000 | 1000 | J211 |
| Nij / M_OC inputs | 600 | – | 600 (M_OC) | – | J211 / 65 FR 30680 |
