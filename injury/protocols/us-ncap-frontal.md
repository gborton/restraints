# US NCAP — Full Frontal Rigid Barrier, 35 mph (56 km/h)

Consumer rating by NHTSA.

- **Rating math**: Federal Register notice "Consumer Information; New Car Assessment Program,"
  73 FR 40016 (July 11, 2008), Appendix C (risk curves) and §d (star bands) [S4].
- **Test and data processing**: NHTSA "Laboratory Procedure for NCAP Full Frontal Rigid Barrier
  Impact Testing," **May 2018** [S24]. A Dec 2024 NHTSA test report states it still follows this
  procedure [S25].
- **Current status**: NHTSA's later NCAP notices (May 2023 RFC, Nov 2024 final decision) cover
  crashworthiness **pedestrian** protection and do not change the frontal dummies or risk curves
  [S31]. A THOR-based frontal upgrade has not been adopted in those notices.

## Test [S24, S25]

| Item | Value |
|---|---|
| Configuration | Full frontal, fixed rigid barrier, **56.3 km/h (35 mph)** |
| Driver | **H3 50M** (Part 572 E), ISO code prefix `11` |
| Right front passenger | **H3 5F** (Part 572 O), prefix `13` |
| Data | Not pre-filtered, ≥ 10,000 samples/s, recorded from ≤ −50 ms to 300 ms, time-zero bias removed |

### Filters (NCAP lab procedure §I, "Filtering requirements") [S24]

| Channel | CFC |
|---|---|
| Head acceleration | 1000 |
| Head angular rate | 60 |
| Neck forces | 1000 |
| Neck moments | 600 |
| **Nij** | **600** (neck force at 600 for the OC moment) |
| Chest acceleration | 180 |
| **Chest compression** | **600** (both H3 50M and H3 5F) |
| Pelvic acceleration | 1000 |
| Femur forces | 600 |
| Tibia forces / moments | 600 |
| Foot acceleration | 1000 |
| Vehicle accel, barrier loads, belt loads | 60 |

The NHTSA test report summary lists HIC15, Nij, neck tension, neck compression, chest 3 ms clip,
chest displacement (mm), and left and right femur force (N), with FMVSS 208 thresholds alongside
[S25].

## Injury risk functions (AIS3+ unless noted) [S4 App. C, Eq. 1–9]

Let Φ = standard normal CDF.

### Head (both dummies) — Eq. 1

```
P_head = Φ( (ln(HIC15) − 7.45231) / 0.73998 )
```

### Neck — Eq. 2–5

```
P_neck,Nij = 1 / (1 + exp(3.227 − 1.969·Nij))                 # H3 50M and H3 5F
P_neck,F   = 1 / (1 + exp(10.9745 − 2.375·F))   # H3 50M, F = peak tension OR compression, kN
P_neck,F   = 1 / (1 + exp(10.958  − 3.770·F))   # H3 5F
P_neck     = max(P_neck,Nij, P_neck,tension, P_neck,compression)
```

Nij uses the FMVSS 208 in-position intercepts (S4 Table 1: 50M 6806/6160 N, 310/135 Nm; 5F
4287/3880 N, 155/67 Nm). Nij = max over the four modes. The same tension curve applies to
compression ("NHTSA assumed that the tensile neck tolerance is approximately equal to the
compressive neck tolerance"). The neck risk is "the greater of the injury risk for Nij, neck
tension, and neck compression" [S4]. The FR text's "kV" is a misprint for kN. Peak forces come
from the CFC 1000 neck force channels [S24].

### Chest — Eq. 6–7 (Laituri, normalized to age 35)

```
P_chest(50M) = 1 / (1 + exp(12.597 − 0.05861·35 − 1.568·(D)^0.4612))
P_chest(5F)  = 1 / (1 + exp(12.597 − 0.05861·35 − 1.568·(D/0.817)^0.4612))
```

- D = peak chest deflection (compression magnitude) in **mm**, from the CFC 600 chest compression
  channel [S24]. The unit follows from NHTSA's mm reporting [S25] and from the curve only giving
  realistic risks in mm (23.2 mm → 2.1 %).
- `12.597 − 0.05861·35 = 10.5456`. The FR PDF text layer reads "0.0586135", which is the
  concatenation of the coefficient 0.05861 and the age 35. Consistent with "normalized to the
  average age of the driving population which is approximately 35 years" [S4].

### Knee-thigh-hip (AIS2+) — Eq. 8–9

```
P_femur(50M) = 1 / (1 + exp(5.7949 − 0.5196·F))      # F = peak femur axial compression, kN
P_femur(5F)  = 1 / (1 + exp(5.7949 − 0.7619·F))
```

Left and right femur forces are both measured and reported [S25]. ⚠ Neither the 2008 notice nor
the 2018 lab procedure states how they are combined for P_femur. Use the **greater** of the two
compressive forces (conservative and conventional). This remains unconfirmed against NHTSA's
published star calculation.

### Joint probability (per occupant) [S4]

```
P_joint = 1 − (1 − P_head)(1 − P_neck)(1 − P_chest)(1 − P_femur)
```

## Star rating [S4 §d]

1. Round P_joint to the nearest 0.1 % (ASTM E29).
2. RRS = P_joint / 0.15, rounded to the nearest 0.01 (ASTM E29).
3. Stars:

| Stars | RRS | Equivalent P |
|---|---|---|
| 5 | RRS < 0.67 | P < 0.100 |
| 4 | 0.67 ≤ RRS < 1.00 | 0.100 ≤ P < 0.150 |
| 3 | 1.00 ≤ RRS < 1.33 | 0.150 ≤ P < 0.200 |
| 2 | 1.33 ≤ RRS < 2.67 | 0.200 ≤ P < 0.400 |
| 1 | RRS ≥ 2.67 | P ≥ 0.400 |

**Combined frontal rating:** "the simple average of the probability of injury to the driver and
front passenger" [S4], then RRS and stars as above. The frontal result also feeds the overall
Vehicle Safety Score as a weighted average with side and rollover [S4].

## Calculation checklist (agent)

1. Filter per the table above, then crop to T0 … T0 + 300 ms.
2. Per occupant: HIC15, Nij (4 modes, CFC 600), peak neck tension and compression (CFC 1000), peak
   chest compression (CFC 600, mm), and peak femur compression L/R (CFC 600, kN).
3. Evaluate the risk functions with the dummy-specific coefficients → P_joint → RRS → stars.
   Average driver and passenger P → frontal combined stars.

```python
from math import log, exp
from scipy.stats import norm

def ncap_frontal(dummy, hic15, nij, ften_kN, fcomp_kN, defl_mm, femur_kN):
    ph = norm.cdf((log(hic15) - 7.45231) / 0.73998)
    a, b = (10.9745, 2.375) if dummy == "50M" else (10.958, 3.770)
    pn = max(1/(1+exp(3.227 - 1.969*nij)),
             1/(1+exp(a - b*ften_kN)), 1/(1+exp(a - b*fcomp_kN)))
    d = defl_mm if dummy == "50M" else defl_mm/0.817
    pc = 1/(1+exp(12.597 - 0.05861*35 - 1.568*d**0.4612))
    pf = 1/(1+exp(5.7949 - (0.5196 if dummy == "50M" else 0.7619)*femur_kN))
    pj = 1 - (1-ph)*(1-pn)*(1-pc)*(1-pf)
    return dict(head=ph, neck=pn, chest=pc, femur=pf, joint=pj, rrs=round(round(pj, 3)/0.15, 2))
```
