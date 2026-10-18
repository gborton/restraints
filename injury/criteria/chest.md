# Chest Criteria

## Inputs

| Signal | ISO MME | CFC by protocol |
|---|---|---|
| Spine-box accel X, Y, Z | `??CHST0000[H3|HF]AC[X,Y,Z]C` | 180 (Part 572 E & O, Euro NCAP) [S2, S3, S6] |
| Sternum deflection (chest pot) | Euro NCAP `??CHST0003[H3|HF]DSX?`. NHTSA `??CHST0000[H3|HF]DSX?` | **US NCAP: 600 for both dummies** (lab procedure "Chest Compression 600" [S24]; NHTSA plots `…DSXB` [S25]). Part 572 certification classes: 50M 180 [S2], 5F 600 [S3]. **Euro NCAP 180** [S6]. **UN R94/R137 180** [S23, S9] |

## Sternum (chest) deflection

```
D_chest = max |D(t)|     over the evaluation window, compression (sternum toward spine)
```

FMVSS 208: "Compressive deflection of the sternum relative to the spine" [S1 S6.4, S15.3.4].
UN: ThCC = absolute value of thorax deformation in mm [S9 Annex 4 §3.1]. Euro NCAP: `max(Dchest(t))`
from `??CHST0003??DSXC` [S6 §3.4.1]. Polarity: compression is negative per J211. Take the magnitude
of the compressive peak.

## Chest resultant acceleration, 3 ms

```
a_chest_R(t) = sqrt(ax^2 + ay^2 + az^2)    (components at CFC 180)
```

FMVSS 208: "shall not exceed 60 g's, except for intervals whose cumulative duration is not more
than 3 milliseconds" [S1 S6.3, S15.3.3]. That wording is cumulative. However, NHTSA's own software
(SAE J1727) computes it as a **continuous** 3 ms clip [S26]. IIHS also follows J1727 [S28]. Euro
NCAP (where used) is cumulative [S19]. Definitions and code are in `criteria/head.md`.

Euro NCAP: "Chest acceleration peaks caused by the firing of seatbelt pretensioners early in the
loading event will be ignored" [S5 §3.5.5].

## Viscous Criterion (VC, V*C)

Euro NCAP [S6 §3.4.6] and UN R94 / R137 [S9 Annex 4 §5, S10 Annex 4 §6]:

```
C(t)  = D(t) / D_const                        # compression ratio, D in metres
V(t)  = [8*(D(t+1) - D(t-1)) - (D(t+2) - D(t-2))] / (12*δt)    # m/s
VC(t) = sf * V(t) * C(t)
VC    = max VC(t)
```

| Constant | H3 50M | H3 5F | Source |
|---|---|---|---|
| D_const (m) | 0.229 | 0.187 | [S6 §3.4.6; S9 Annex 4 §5.2] |
| sf | 1.3 | 1.3 | [S6 §3.4.6; S10/S23 Annex 4 §6: "(V*C)(t) = 1,3 x (V(t) x C(t))"]. R137 uses the same procedure, with sf shown in its calculation diagram (image) [S9] |

- Filter D(t) once at **CFC 180** before differentiating [S10 Annex 4 §6.2].
- Max δt = 1.25 × 10⁻⁴ s [S10 Annex 4 §6.2].
- Use compression-positive D for VC (flip the J211 sign) so the peak VC is positive.

```python
def viscous_criterion(defl_m, dt, D_const, sf=1.3):
    d = -np.asarray(defl_m)        # J211 compression negative -> positive
    v = np.zeros_like(d)
    v[2:-2] = (8*(d[3:-1] - d[1:-3]) - (d[4:] - d[:-4])) / (12*dt)
    return np.max(sf * v * d / D_const)
```

## Sternum deflection rate (IIHS H3 50M)

IIHS rates the peak sternum deflection rate (m/s). Good ≤ 6.6, acceptable ≤ 8.2, marginal ≤ 9.8
[S7, S8 Table 1, S27 Table 2]. "The sternum deflection rate is calculated from the sternum
deflection filtered to SAE CFC 60" [S28]. Differentiate the CFC 60 deflection (no sf, no
normalisation) and take the max compressive rate. ⚠ IIHS does not state the differentiation
scheme. J1727 is cited for all calculations [S28].

## IIHS Chest Index (H3 5F rear, Moderate Overlap 2.0)

Corrects 5F sternum deflection for shoulder-belt vertical position [S8 Eq. 4–5, Tables 6–7]:

```
if belt_pos_mm <= 17:
    ChestIndex = |D_sternum_mm|
else:
    R = (0.5/100) * (belt_pos_mm - 17)      # 0.5 % per mm above rib 3 position
    ChestIndex = |D_sternum_mm| / (1 - R)
ChestIndex = floor(ChestIndex)              # rounded down before rating
```

- `belt_pos_mm`: vertical distance from the sternum pot ball (uncompressed thorax) to the belt
  centreline at the time of max sternum deflection, measured with a high-frequency pressure
  sensor (S8 Appendix A).
- Measured sternum deflection uses two decimals (e.g. 25.64) [S8 Table 7].
- Rating boundaries 35 (G/A), 40 (A/M), 45 (M/P) [S8 Table 4]. Chest Index is rounded **down**
  before rating [S8 Table 6]. IIHS's general rule is that results which round to a boundary
  value get the better rating [S7], so a Chest Index of exactly 35 rates Good. ⚠ This applies the
  general rounding rule. S8 does not restate it for Chest Index.
- Shoulder-belt tension: peak ≤ 5.9 kN required (Good ≤ 5.9 < Marginal) [S8 Table 4].

## Limits by protocol

| Protocol | Criterion | H3 50M | H3 5F | Source |
|---|---|---|---|---|
| FMVSS 208 (S14 vehicles) | Sternum deflection | ≤ 63 mm | ≤ 52 mm | [S1 S6.4(b), S15.3.4] |
| FMVSS 208 (legacy non‑S14) | Sternum deflection | ≤ 76 mm | – | [S1 S6.4(a)] |
| FMVSS 208 | Chest 3 ms (text "cumulative"; NHTSA computes continuous [S26]) | ≤ 60 g | ≤ 60 g | [S1 S6.3, S15.3.3] |
| US NCAP | Deflection → P(AIS3+) | Eq. 6 | Eq. 7 (scale 0.817) | [S4] (see `protocols/us-ncap-frontal.md`) |
| US NCAP | Chest 3 ms accel, sternum deflection | reported only (deflection rated via Eq. 6) | reported only (Eq. 7) | [S4, S25] |
| IIHS (H3 50M) | Spine accel 3 ms G/A/M | ≤ 60 / 75 / 90 g | – | [S7, S8, S27] |
| IIHS (H3 50M) | Sternum deflection G/A/M | ≤ 50 / 60 / 75 mm | – | [S7, S8] |
| IIHS (H3 50M) | VC G/A/M | ≤ 0.8 / 1.0 / 1.2 m/s | – | [S7, S8] |
| IIHS MO 2.0 (H3 5F rear) | Chest Index G/A/M boundaries | – | 35 / 40 / 45 | [S8 Table 4] |
| Euro NCAP 2026 | Dchest HPL–LPL (cap), 35 km/h (FWDB full-scale, 35 km/h sled/virtual) & 50 km/h (MPDB, 50 km/h sled/virtual) | 20–42 mm (42) | 18–34 mm (34) | [S5 §3.5.2] |
| Euro NCAP 2026 | Dchest HPL–LPL (cap), 56 km/h (sled/virtual) | – | 22–42 mm (42) | [S5 §3.5.2] |
| Euro NCAP 2026 | VC HPL–LPL (cap) | 0.5–1.0 (1.0) | 0.5–1.0 (1.0) | [S5 §3.5.2] |
| UN R94 (05 series, Suppl. 1) | ThCC / VC | **≤ 42 mm** / ≤ 1.0 m/s | – | [S23 §5.2.1.4–5]. The 2012 text [S10] had 50 mm |
| UN R137 (02 series) | ThCC | ≤ 42 mm | ≤ 34 mm (M1), ≤ 42 mm (N1) | [S9 §5.2.1.1.3, §5.2.1.2.3] |
| UN R137 | VC | ≤ 1.0 m/s | ≤ 1.0 m/s | [S9] |

Euro NCAP LPL footnotes: the 50M chest LPL corresponds to a 65-year-old biomechanical limit. For
the 5F, LPL 34 mm is 65-year-old and LPL 42 mm (56 km/h) is 45-year-old [S5 §3.5.2].
