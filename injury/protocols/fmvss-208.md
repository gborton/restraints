# FMVSS No. 208 — Occupant Crash Protection (49 CFR 571.208)

Pass/fail compliance standard. Source: [S1] (text current as read 2026-10-18). Only the tests
using H3 50M and H3 5F frontal dummies are listed.

## Test matrix (vehicles certified to the advanced air bag requirements, S14)

| Test | Dummy | Conditions | Criteria | Section |
|---|---|---|---|---|
| Belted rigid barrier | H3 50M | Up to **56 km/h (35 mph)**, perpendicular (S14.3/S14.4 vehicles; 48 km/h for S14.1/S14.2) | S6.1, S6.2(b), S6.3, S6.4(b), S6.5, S6.6 | S5.1.1(b), S14.5.1 |
| Unbelted rigid barrier | H3 50M | **32–40 km/h (20–25 mph)**, 0° to ±30° | same | S5.1.2(b), S14.5.2 |
| Belted rigid barrier | H3 5F | Up to 48 km/h (S14.1/14.2) or **56 km/h** (S14.6/14.7), perpendicular ±5° | S15.3 | S15.1, S16.1(a) |
| Unbelted rigid barrier | H3 5F | 32–40 km/h, perpendicular ±5° | S15.3 | S15.2, S16.1(b) |
| Offset deformable barrier (ODB) | H3 5F, belted | Up to **40 km/h (25 mph)**, **40 % overlap, left side**, barrier per Part 587 Subpart C | S15.3 | S17, S18 |
| Driver low-risk deployment (OOP) | H3 5F | Static deployment, chin-on-module (pos. 1) and chin-on-rim (pos. 2) | S15.3 modified by S25.4 | S25.3, S26 |

Both front outboard seats are occupied in the barrier tests. Data window: **300 ms after the
vehicle strikes the barrier** (125 ms after final-stage deployment for driver low-risk deployment)
[S1 S4.11]. Filters: phaseless digital per SAE J211/1 MAR95 Appendix C [S1 S4.13].

## Injury criteria and limits

| Criterion | H3 50M (S6) | H3 5F (S15.3) | H3 5F OOP driver (S25.4) | Filter |
|---|---|---|---|---|
| Containment | All of dummy within passenger compartment (S6.1) | S15.3.1 | – | – |
| HIC15 | ≤ 700 (S6.2(b)) | ≤ 700 (S15.3.2) | ≤ 700 (via S15.3) | CFC 1000 (Part 572) |
| Chest resultant accel, 3 ms (text: "cumulative"; NHTSA software: continuous [S26]) | ≤ 60 g (S6.3) | ≤ 60 g (S15.3.3) | ≤ 60 g | CFC 180 |
| Sternum deflection | ≤ 63 mm (S6.4(b)) | ≤ 52 mm (S15.3.4) | ≤ 52 mm | Part 572: 50M CFC 180, 5F CFC 600. ⚠ NHTSA's NCAP lab uses 600 for both [S24]. TP-208 not checked |
| Femur axial force (each) | ≤ 2250 lb ≈ 10.0 kN (S6.5) | ≤ 6805 N (S15.3.5) | ≤ 6805 N | CFC 600 |
| Nij (each of 4 modes) | ≤ 1.0; Fzc 6806/6160 N, Myc 310/135 Nm (S6.6(a)) | ≤ 1.0; 4287/3880 N, 155/67 Nm (S15.3.6(a)) | ≤ 1.0; 3880/3880 N, 155/61 Nm | **CFC 600** (Fx, Fz, My) |
| Peak neck tension | ≤ 4170 N (S6.6(b)) | ≤ 2620 N (S15.3.6(b)) | ≤ 2070 N | CFC 1000 (Part 572 via S6.7/S15.3.7; same in NHTSA NCAP lab practice [S24, S25]) |
| Peak neck compression | ≤ 4000 N (S6.6(c)) | ≤ 2520 N (S15.3.6(c)) | ≤ 2520 N | CFC 1000 (as above) |

Legacy (vehicles not certified to S14, e.g. S5.1.1(a) and the S13 sled test): HIC36 ≤ 1000
(S6.2(a)) and sternum deflection ≤ 76 mm (S6.4(a)). The S13 sled test also has its own neck limits
(flexion 190 Nm, extension 57 Nm, tension 3300 N, compression 4000 N, shear 3100 N) [S1 S13.2].
These do not apply to current production vehicles certified to S14.

## Calculation checklist (agent)

1. Locate T0. Crop to T0 … T0 + 300 ms **after** filtering the full record.
2. Head: filter components at CFC 1000 → resultant → HIC15 (`criteria/head.md`).
3. Chest: spine accel components at CFC 180 → resultant → 3 ms value. Compute the continuous
   value to match NHTSA software, and also the cumulative value if the margin is close [S26].
   Sternum deflection at CFC 180 (50M) / CFC 600 (5F) per Part 572 → max compression.
4. Femur: CFC 600 → |min Fz| per leg.
5. Neck: Fx, Fz, My at CFC 600 → M_OCy = My − 0.01778·Fx → Nte/Ntf/Nce/Ncf peaks with the
   dummy's intercepts. Peak tension/compression from Fz.
6. Pass if every value is ≤ its limit (Nij ≤ 1.0 in all four modes).
