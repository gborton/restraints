# Lower Extremity Criteria (Femur, Knee, Tibia)

## Inputs

| Signal | ISO MME | CFC | Source |
|---|---|---|---|
| Femur axial force Fz | `??FEMR[LE,RI]00[H3|HF]FOZB` | 600 | [S2, S3, S6, S9] |
| Knee slider displacement | `??KNSL[LE,RI]00[H3|HF]DSXC` | 180 | [S6, S10 Annex 3 §5.2.4.2] |
| Upper/lower tibia Mx, My | `??TIBI[LE,RI][UP,LO][H3|HF]MO[X,Y]B` | 600 | [S6, S10] |
| Upper/lower tibia Fz | `??TIBI[LE,RI][UP,LO][H3|HF]FOZB` | 600 | [S6, S10] |

## Femur axial compression

```
F_femur = |min(Fz(t))|         per leg; compression is negative (J211)
```

[S6 §3.6.4]. Evaluate left and right separately. Most protocols use the worse leg.

### Femur limits

| Protocol | H3 50M | H3 5F | Source |
|---|---|---|---|
| FMVSS 208 | ≤ 2250 lb (10.0 kN) | ≤ 6805 N | [S1 S6.5, S15.3.5] |
| US NCAP | Risk curve Eq. 8 | Eq. 9 (scale 0.68) | [S4] |
| IIHS original moderate overlap (2014 guidelines) G/A/M | ≤ 7.3 / 9.1 / 10.9 kN, plus duration corridors | – | [S7] |
| **IIHS small overlap v VII and MO 2.0 (H3 50M)** | **KTH risk 5 / 15 / 25 %**. Femur force is not a rated parameter in these tables | – | [S27 Table 2; S8 Table 1] |
| IIHS MO 2.0 (H3 5F rear) G/A/M | – | ≤ 4.9 / 6.2 / 7.4 kN | [S8 Table 4] |
| Euro NCAP 2026 HPL–LPL (single peak value) | 3.8–9.1 kN | 2.6–6.2 kN | [S5 §3.5.3] |
| Euro NCAP AOP v9.3 (2023–2025, superseded) | HPL 3.80 kN, LPL 9.07 kN @ 0 ms / 7.56 kN @ 10 ms ("cumulative exceedence plot") | – | [S20 §3.1.4.2] |
| UN R94 | Femur Force Criterion (force–duration, Fig. 3) | – | [S23 §5.2.1.6] |
| UN R137 | FFC ≤ 9.07 kN | FFC ≤ 7 kN | [S9 §5.2.1.1.5, §5.2.1.2.5] |

### Femur force–duration corridors

| Source | Corridor (force @ cumulative duration) |
|---|---|
| IIHS H3 50M [S7 Fig. 5] | Good limit: 7.3 kN @ 0 ms → 6.1 kN @ 10 ms. Acceptable limit: 9.1 kN @ 0 → 7.6 kN @ 10 ms. Marginal limit: 10.9 kN @ 0 → 9.1 kN @ 10 ms |
| **UN R94 Fig. 3** [S10, S23] | **9.07 kN @ 0 ms → 7.58 kN @ ≥ 10 ms** (read from the figure image). Note that Euro NCAP AOP v9.3 printed 7.56 kN @ 10 ms for its LPL [S20] |

Use the cumulative `exceedance_duration` function in `criteria/neck.md` with the compression
magnitude.

### IIHS Knee-Thigh-Hip (KTH) (small overlap v VII and Moderate Overlap 2.0, H3 50M)

Based on Rupp et al. (2009). It combines **peak femur compressive force** and **impulse** to
estimate AIS2+ knee/distal-femur and AIS3+ hip fracture risk [S8 "Thigh and hip"; S27 "Thigh and
hip"]. "The thigh/hip and leg/foot ratings are based on the lowest rating scored from either the
left or right limb" [S27].

- Impulse = ∫ F_femur dt from the start of compression (the last time force = 0 before the peak)
  to the time after the peak when compressive force last falls below **4,050 N**.
- Rating boundaries: KTH risk 5 % (G/A), 15 % (A/M), 25 % (M/P) [S8 Table 1].
- The force–impulse corridors are in S8 Appendix C (Figures C4, C5) and S27 (Figures 13, 14). The
  risk function itself is not reproduced in this KB (it comes from Rupp et al. 2009, not
  retrieved). Take the corridor curves from those figures rather than deriving them.

## Knee slider (tibia–femur) displacement

```
D_knee = |min(D(t))|           per knee
```

[S6 §3.6.3]

| Protocol | H3 50M | H3 5F | Source |
|---|---|---|---|
| IIHS (H3 50M) G/A/M | ≤ 12 / 15 / 18 mm | – | [S7, S8] |
| Euro NCAP 2026 HPL–LPL | 6–15 mm | 6–15 mm | [S5 §3.5.3] (not used in virtual tests) |
| UN R94 | ≤ 15 mm | – | [S23 §5.2.1.9] |

## Tibia Index (TI)

Euro NCAP [S6 §3.6.5] and UN R94 [S10 Annex 4 §5]:

```
M_R(t) = sqrt(Mx(t)^2 + My(t)^2)
TI(t)  = |M_R(t) / (M_C)_R| + |Fz(t) / (F_C)_Z|      evaluated while Fz(t) < 0 (compression)
TI(t)  = 0                                            when Fz(t) >= 0   (Euro NCAP)
TI     = max TI(t)     computed separately at upper and lower tibia, each leg
```

UN R94: "F_z may be measured at either location. The value obtained is used for the top and
bottom TI calculations" [S10 Annex 4 §5.2].

| Constant | H3 50M | H3 5F | Source |
|---|---|---|---|
| (M_C)_R | 225 Nm | 114 Nm | 50M: [S6 §3.6.5; S10]. 5F: [S6 §3.6.5] |
| (F_C)_Z | 35.9 kN | 22.9 kN | 50M: [S6 §3.6.5; S10]. 5F: [S6 §3.6.5] |

### IIHS tibia moment adjustment (applied before TI)

The Hybrid III lower leg geometry produces forward bowing moments under pure axial load. IIHS
corrects My before computing TI (SAE J1733 polarity, Nm and N) [S7 Eq. 2–3, S8 Eq. 2–3, S27 Eq. 2–3]:

```
My_upper_adj = My_upper_meas − Fz_tibia × 0.02832
My_lower_adj = My_lower_meas + Fz_tibia × 0.006398
```

Operators confirmed from the 2014 IIHS PDF's Symbol-font glyphs: U+F02D ("−") in Eq. 2 and U+F02B
("+") in Eq. 3 [S7]. The 2024 documents keep the same "+" in Eq. 3 [S8, S27].

```python
def tibia_index(mx, my, fz_N, Mc, Fc_N):
    mr = np.hypot(mx, my)
    ti = np.abs(mr/Mc) + np.abs(fz_N/Fc_N)
    return np.max(np.where(fz_N < 0, ti, 0.0))
```

### Tibia limits

| Protocol | Criterion | H3 50M | H3 5F | Source |
|---|---|---|---|---|
| IIHS (H3 50M) | TI (upper, lower) G/A/M | ≤ 0.80 / 1.00 / 1.20 | – | [S7, S8] |
| IIHS (H3 50M) | Tibia axial force G/A/M | ≤ 4.0 / 6.0 / 8.0 kN | – | [S7, S8] |
| IIHS (H3 50M) | Foot accel G/A/M | ≤ 150 / 200 / 260 g | – | [S7, S8] |
| Euro NCAP 2026 | TI HPL–LPL | 0.4–1.3 | LPL 1.3 only | [S5 §3.5.4] |
| Euro NCAP 2026 | Tibia compression HPL–LPL | 2.0–8.0 kN | LPL 8.0 kN only | [S5 §3.5.4] |
| UN R94 | TI | ≤ 1.3 (top and bottom) | – | [S23 §5.2.1.8] |
| UN R94 | TCFC | ≤ 8 kN | – | [S23 §5.2.1.7] |

Euro NCAP: tibia criteria are not applied to virtual tests. For sled/virtual load cases, lower-leg
points are awarded by default unless a physical test exceeds the TI or tibia-force LPL
[S5 §3.3].

## Pelvis / acetabulum

Euro NCAP acetabulum force (3.3–4.1 kN) applies to THOR only (out of scope) [S5 §3.5.3]. H3 iliac
and lumbar channels are recorded for monitoring and submarining modifiers [S6 §2.3].
