# Neck Criteria (Upper Neck Load Cell)

## Inputs

| Signal | ISO MME | CFC (see `conventions/filtering-cfc.md`) |
|---|---|---|
| Upper neck shear Fx | `??NECKUP00[H3|HF]FOX?` | 600 for Nij and any M_OC calc (FMVSS/NCAP/IIHS/Euro NCAP); 1000 for force peaks |
| Upper neck axial Fz | `??NECKUP00[H3|HF]FOZ?` | 600 for Nij; **1000 for peaks** (Part 572; US NCAP lab procedure "Neck Forces 1000" [S24]; Euro NCAP; UN) |
| Upper neck moment My | `??NECKUP00[H3|HF]MOY?` | 600 everywhere |

Polarity: +Fz = tension; +My = flexion (see `conventions/sign-conventions-units.md`).

## Moment at the occipital condyle (OC)

The upper neck load cell sits below the OC. Moment is transferred to the OC with:

```
M_OCy(t) = My(t) - D * Fx(t)
D = 0.01778 m   (all Hybrid III series, i.e. both H3 50M and H3 5F)
```

[S6 §3.2.1: "d 0.01778m for all HIII series"]. FMVSS 208 uses "Mocy" in Nij and points to Part 572
for "moment calculations" [S1 S6.6(a)(2), S6.7, S15.3.7]. US NCAP lab procedure: "For calculation of
the moment about the occipital condyle, use filter class 600 for the neck force" [S24 App. A]. IIHS
computes Nij "using formulas presented in 65 Fed. Reg. 30680 [2000]" (the FMVSS 208 advanced air
bag final rule) [S28].

## Nij

FMVSS 208 [S1 S6.6(a), S15.3.6(a)]; used unchanged by US NCAP [S4 App. C Eq. 2] and IIHS [S7, S8]:

```
Nij(t) = Fz(t)/Fzc + M_OCy(t)/Myc
```

- Use Fx, Fz and My filtered at **CFC 600**, then compute M_OCy.
- At each instant **exactly one** of four modes applies, based on the signs of Fz and M_OCy. The
  other three are set to zero:
  - Nte: tension (Fz > 0) + extension (M_OCy < 0)
  - Ntf: tension + flexion (M_OCy > 0)
  - Nce: compression (Fz < 0) + extension
  - Ncf: compression + flexion
- Use magnitudes against the mode's critical values, so each Nij ≥ 0.
- Report the peak of each mode, plus the overall maximum. FMVSS limit: "None of the four Nij
  values shall exceed 1.0 at any time during the event" [S1 S6.6(a)(5)].

### Critical intercepts

| Dummy / condition | Fzc tension (N) | Fzc compression (N) | Myc flexion (Nm) | Myc extension (Nm) | Source |
|---|---|---|---|---|---|
| H3 50M (in-position) | 6806 | 6160 | 310 | 135 | [S1 S6.6(a)(3)] |
| H3 5F (in-position) | 4287 | 3880 | 155 | 67 | [S1 S15.3.6(a)(3)] |
| H3 5F (driver low-risk deployment, OOP) | 3880 | 3880 | 155 | 61 | [S1 S25.4(a)(3)] |

```python
import numpy as np
CRIT = {
  "H3_50M":     dict(Ft=6806, Fc=6160, Mf=310, Me=135),
  "H3_05F":     dict(Ft=4287, Fc=3880, Mf=155, Me=67),
  "H3_05F_OOP": dict(Ft=3880, Fc=3880, Mf=155, Me=61),
}
def nij(fx_N, fz_N, my_Nm, dummy="H3_50M", D=0.01778):
    """Inputs already CFC600-filtered, J211 polarity. Returns dict of mode peaks."""
    c = CRIT[dummy]
    moc = my_Nm - D*fx_N
    ten, flex = fz_N >= 0, moc >= 0
    n = np.abs(fz_N)/np.where(ten, c["Ft"], c["Fc"]) + np.abs(moc)/np.where(flex, c["Mf"], c["Me"])
    out = {
      "Nte": np.max(n, where=ten & ~flex, initial=0),
      "Ntf": np.max(n, where=ten & flex,  initial=0),
      "Nce": np.max(n, where=~ten & ~flex, initial=0),
      "Ncf": np.max(n, where=~ten & flex, initial=0),
    }
    out["Nij_max"] = max(out.values())
    return out
```

## Peak tension / compression

FMVSS 208 [S1 S6.6(b)(c), S15.3.6(b)(c), S25.4(b)(c)], measured at the upper neck load cell:

| Dummy | Peak tension (N) | Peak compression (N) |
|---|---|---|
| H3 50M | 4170 | 4000 |
| H3 5F | 2620 | 2520 |
| H3 5F OOP (driver low-risk deployment) | 2070 | 2520 |

## Neck tension / shear / extension (Euro NCAP, UN)

| Protocol | Criterion | H3 50M | H3 5F | Source |
|---|---|---|---|---|
| Euro NCAP 2026 | Fx shear HPL–LPL (cap) | 1.9–3.1 kN (3.10) | 1.2–2.0 kN (2.7, driver only) | [S5 §3.5.1] |
| Euro NCAP 2026 | Fz tension HPL–LPL (cap) | 2.7–3.3 kN (3.30) | 1.70–2.6 kN (2.9, driver only) | [S5 §3.5.1] |
| Euro NCAP 2026 | My extension at OC HPL–LPL (cap) | 42–57 Nm (57) | 36–49 Nm (57, driver only) | [S5 §3.5.1] |
| UN R94 | Tension & shear: force–duration curves (Fig. 1, 2) | see below | – | [S23 §5.2.1.2] |
| UN R94 | Extension moment | ≤ 57 Nm | – | [S23 §5.2.1.3] |
| UN R137 | Tension | ≤ 3.3 kN | ≤ 2.9 kN | [S9 §5.2.1.1.2, §5.2.1.2.2] |
| UN R137 | Fore/aft shear | ≤ 3.1 kN | ≤ 2.7 kN | [S9] |
| UN R137 | Extension moment | ≤ 57 Nm | ≤ 57 Nm | [S9] |

Euro NCAP 2026 gives **single-value** limits for shear and tension, which are evaluated as peaks
over the loading phase [S5 §3.5.1; S6 §3.2.5, §3]. Before 2026, Euro NCAP assessed them on
force–duration exceedance curves. AOP v9.3 (2023) used LPL = the UN R94 curves below, and HPL =
1.90 kN @ 0 / 1.20 kN @ 25–35 / 1.10 kN @ 45 ms (shear) and 2.70 @ 0 / 2.30 @ 35 / 1.10 @ 60 ms
(tension). It described them as "cumulative exceedence plots" [S20 §3.1.6.2], whereas TB 021 v4.1
labelled them "continuous exceedence" [S19 §2.1]. The 2026 HPL/LPL values equal the 0 ms
breakpoints of those curves.

Euro NCAP extension: `M_OCy = My − Fx·d` with My and Fx at CFC 600. Extension = magnitude of the
most negative value [S6 §3.2.1].

## Force–duration (exceedance) corridors

A curve gives the maximum force allowed for a cumulative duration t. For each level F, compute the
time the signal spends above F, then compare it with the corridor.

```python
def exceedance_duration(f, dt, levels, continuous=False):
    """Time (s) the signal is >= each level: cumulative (summed) or longest continuous run."""
    out = []
    for L in levels:
        m = f >= L
        if not continuous:
            out.append(m.sum() * dt)
        else:
            best = run = 0
            for v in m:
                run = run + 1 if v else 0
                best = max(best, run)
            out.append(best * dt)
    return out
```

Cumulative vs continuous: UN R94 does not say which in the figure captions (⚠ open). Euro NCAP
pre-2026 documents disagree (AOP v9.3 "cumulative", TB 021 v4.1 "continuous") [S19, S20].

| Source | Channel | Breakpoints (force @ duration) |
|---|---|---|
| IIHS "limit for Good" (H3 50M) [S7 Fig. 2–4; S27 Fig. 10–11] | Tension | 3.3 kN @ 0 ms → 2.9 kN @ 35 ms → 1.1 kN @ 45 ms |
| | Compression | 4.0 kN @ 0 ms → 1.1 kN @ 30 ms |
| | Shear | 3.1 kN @ 0 ms → 1.5 kN @ 25–35 ms → 1.1 kN @ 45 ms |
| **UN R94 Fig. 1** (H3 50M) [S10, S23] | Tension | **3.3 kN @ 0 ms → 2.9 kN @ 35 ms → 1.1 kN @ ≥ 60 ms** (read from the figure image) |
| **UN R94 Fig. 2** (H3 50M) [S10, S23] | Fore/aft shear | **3.1 kN @ 0 ms → 1.5 kN @ 25–35 ms → 1.1 kN @ ≥ 45 ms** (read from the figure image) |

Linear interpolation between breakpoints. The curve is flat after the last breakpoint. Note that
IIHS's tension corridor reaches 1.1 kN at 45 ms, but R94's reaches it at 60 ms.

IIHS: a head/neck rating that is otherwise good is lowered to acceptable if tension, compression
or shear fall outside these corridors [S7, S27].

## IIHS rating boundaries

| Dummy | Parameter | Good | Acceptable | Marginal | Poor | Source |
|---|---|---|---|---|---|---|
| H3 50M | Nij | ≤ 0.80 | ≤ 1.00 | ≤ 1.20 | > 1.20 | [S7, S8 Table 1, S27 Table 2] |
| H3 50M | Tension (kN) | ≤ 2.6 | ≤ 3.3 | ≤ 4.0 | > 4.0 | |
| H3 50M | Compression (kN) | ≤ 3.2 | ≤ 4.0 | ≤ 4.8 | > 4.8 | |
| H3 5F rear (MO 2.0) | Nij (only with contacts) | ≤ 0.80 | ≤ 1.00 | ≤ 1.20 | > 1.20 | [S8 Table 4] |
| H3 5F rear | Tension (kN) | ≤ 2.0 | ≤ 2.4 | ≤ 2.8 | > 2.8 | |
| H3 5F rear | Compression (kN) | ≤ 2.0 | ≤ 2.5 | ≤ 3.0 | > 3.0 | |

IIHS: "Injury results that round to the values shown … will receive the better of the two ratings
they separate" [S7].
