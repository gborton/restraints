# Head Criteria

## Inputs

| Signal | ISO MME (filtered) | CFC |
|---|---|---|
| Head CG accel X, Y, Z | `??HEAD0000[H3|HF]AC[X,Y,Z]A` | 1000 (all protocols) [S1, S2, S3, S6, S9, S10] |

Resultant (filter each component first) [S6 §3.1.1]:

```
a_R(t) = sqrt(a_x(t)^2 + a_y(t)^2 + a_z(t)^2)      # in g
```

## HIC (Head Injury Criterion), HIC15 / HIC36

FMVSS 208 [S1 S6.2, S15.3.2], Euro NCAP [S6 §3.1.2]:

```
HIC = max over t1 < t2, (t2 - t1) <= W of
      (t2 - t1) * [ (1/(t2 - t1)) * ∫_{t1}^{t2} a_R(t) dt ] ^ 2.5
```

- `a_R` in g, `t` in **seconds**.
- `W = 15 ms` for HIC15; `W = 36 ms` for HIC36.

### HPC (UN R94 / R137)

Same expression, labelled the Head Performance Criterion, with a 36 ms maximum interval. **It is
computed only if there is head contact with a vehicle component.** With no contact, the criterion
is satisfied. If the start of contact can be determined, t1/t2 are limited to the interval from
contact onset to end of record [S9 Annex 4 §1; S10 Annex 4 §1].

### Reference implementation

```python
import numpy as np

def hic(a_res_g, dt, window_s=0.015):
    """a_res_g: filtered resultant head accel (g); dt: sample period (s)."""
    v = np.concatenate([[0.0], np.cumsum((a_res_g[1:] + a_res_g[:-1]) * 0.5 * dt)])  # ∫a dt
    n = len(a_res_g); w = int(round(window_s / dt))
    best, t1b, t2b = 0.0, 0, 0
    for i in range(n - 1):
        j = np.arange(i + 1, min(i + w, n - 1) + 1)
        T = (j - i) * dt
        avg = (v[j] - v[i]) / T
        val = T * np.clip(avg, 0, None) ** 2.5
        k = np.argmax(val)
        if val[k] > best:
            best, t1b, t2b = val[k], i, j[k]
    return best, t1b * dt, t2b * dt
```

Report HIC together with t1 and t2. Restrict the data to the protocol window before computing
(see `conventions/sign-conventions-units.md`).

## Head 3 ms exceedance ("3 ms clip")

There are two definitions:

1. **Cumulative 3 ms**: the highest level exceeded for a **total** of 3 ms (summed over all
   intervals).
2. **Continuous 3 ms**: the highest level sustained continuously for 3 ms. Equivalently, the max
   over all 3 ms windows of the min inside each window. It is always ≤ the cumulative value.

| Protocol / criterion | Definition | Evidence |
|---|---|---|
| UN R94 / R137 head 80 g | **Cumulative**, excluding rebound | "calculated cumulatively, excluding rebound movement of the head" [S23 §5.2.1.1; S9 §5.2.1.1.1] |
| Euro NCAP head Ares‑3ms (H3) | **Cumulative** | TB 021 v4.1 channel table: "Resultant 3ms cumulative exceedence" for H3 50M and 5F head and chest [S19 §2.1, §2.3]. CP 005 (2026) does not restate it |
| FMVSS 208 chest 60 g | Regulation text says **cumulative** ("intervals whose cumulative duration is not more than 3 milliseconds") [S1 S6.3]. **But NHTSA's Signal Analysis Software, consistent with SAE J1727, computes the 3 ms clip on a continuous basis** (NHTSA interpretation letter, 2 July 2002, which notes the result is "sometimes … lower (but never higher)") [S26] | Use continuous to match NHTSA-computed values. Report both if a compliance margin is close |
| US NCAP chest 3 ms (reported, not rated) | Continuous (NHTSA software) | [S26] |
| IIHS chest / head 3 ms clip | J1727-based: "All calculations comply with the recommendations of SAE … J1727 (2015a)" [S28]. Per [S26], J1727's clip is **continuous** | Inferred. IIHS does not define it explicitly |

```python
def exceed_cumulative(a, dt, dur_s=0.003):
    n = int(round(dur_s / dt))
    return np.sort(a)[::-1][n - 1]          # level exceeded for a cumulative n samples

def exceed_continuous(a, dt, dur_s=0.003):
    n = int(round(dur_s / dt))
    from numpy.lib.stride_tricks import sliding_window_view
    return sliding_window_view(a, n).min(axis=1).max()
```

## Peak resultant / hard-contact rules

| Protocol | Rule |
|---|---|
| Euro NCAP 2026 | "A hard contact is assumed if the peak resultant head acceleration exceeds 80 g or if there is other evidence of hard contact. Only where Ares exceeds 80 g or there is a hard contact, will the limits for HIC15 and Ares‑3ms be applied." [S5 §3.5.1]. "If there is no hard contact, full points are awarded to the head. If there is hard contact, a sliding scale is applied to HIC and Ares 3ms" [S21 §3] |
| IIHS | Peak resultant head accel > 70 g from contact with a hard surface → head/neck rating lowered one level (forward-excursion and rebound flow chart) [S7, S8] |
| UN R94/R137 | HPC only computed with head contact [S9, S10] |

## Limits by protocol (H3 50M / H3 5F)

| Protocol | Criterion | H3 50M | H3 5F | Source |
|---|---|---|---|---|
| FMVSS 208 (S14 vehicles) | HIC15 | ≤ 700 | ≤ 700 | [S1 S6.2(b), S15.3.2] |
| FMVSS 208 (legacy, non-S14) | HIC36 | ≤ 1000 | – | [S1 S6.2(a)(2)] |
| US NCAP | HIC15 → P(AIS3+) risk curve (same for 50M & 5F) | see `protocols/us-ncap-frontal.md` | same | [S4] |
| IIHS (50M driver/passenger; 5F rear only with contact) | HIC15 G/A/M/P | 560 / 700 / 840 | 560 / 700 / 840 (IARV 779) | [S7, S8, S27] |
| Euro NCAP 2026 | HIC15 HPL–LPL (capping) | 500–700 (700) | 500–700 (700) | [S5 §3.5.1] |
| Euro NCAP 2026 | Ares‑3ms HPL–LPL (capping) | 72–80 g (80) | 72–80 g (80) | [S5 §3.5.1] |
| UN R94 (05 series) | HPC36 / 3 ms | ≤ 1000 / ≤ 80 g | – (50M only) | [S23 §5.2.1.1] |
| UN R137 | HPC / 3 ms | ≤ 1000 / ≤ 80 g | ≤ 1000 / ≤ 80 g | [S9 §5.2.1.1.1, §5.2.1.2.1] |

Out of scope here: Euro NCAP DAMAGE (THOR only; needs angular rate) [S6 §3.1.3].
