# Channel Filtering (SAE J211-1 / ISO 6487 Channel Frequency Class)

All protocols in this KB filter dummy channels to a Channel Frequency Class (CFC):

- **FMVSS 208** S4.13 requires "a phaseless digital filter, such as the Butterworth four-pole
  phaseless digital filter specified in appendix C of SAE Recommended Practice J211/1 MAR95" [S1].
- **US NCAP** lab procedure: filters "in accordance with the most recent version of SAE
  Recommended Practice J211/1" [S24 §I.F].
- **IIHS**: SAE J211/1 (2014) [S28].
- **Euro NCAP**: ISO 6487 or SAE J211/1 (2022) [S5 §1.4].

## CFC frequencies

"This number and the value of the frequency F_H in Hz are numerically equal" [S23 Annex 8 §1.5].
F_H is the upper edge of the ±0.5 dB passband corridor. F_N is the higher corridor breakpoint listed
in the same table. The US NCAP lab procedure tabulates F_N as the "cut-off frequency" [S24 §I].
Values come from UN R94 Annex 8 Figure 1 [S23].

| CFC | F_L (Hz) | F_H (Hz) | F_N (Hz) | ISO MME filter letter [S29] |
|---|---|---|---|---|
| 1000 | ≤ 0.1 | 1000 | 1650 | A |
| 600 | ≤ 0.1 | 600 | 1000 | B |
| 180 | ≤ 0.1 | 180 | 300 | C |
| 60 | ≤ 0.1 | 60 | 100 | D |

## Algorithm (SAE J211/1 Appendix C 4-pole phaseless Butterworth)

A second-order Butterworth is applied forward, then backward over the same data, giving a 4-pole,
zero-phase filter. With sample period T (s):

```
wd = 2*pi*CFC*2.0775
wa = sin(wd*T/2) / cos(wd*T/2)
a0 = wa**2 / (1 + sqrt(2)*wa + wa**2)
a1 = 2*a0
a2 = a0
b1 = -2*(wa**2 - 1) / (1 + sqrt(2)*wa + wa**2)
b2 = (-1 + sqrt(2)*wa - wa**2) / (1 + sqrt(2)*wa + wa**2)

Y[n] = a0*X[n] + a1*X[n-1] + a2*X[n-2] + b1*Y[n-1] + b2*Y[n-2]
```

The SAE text itself was not available (paywalled). The coefficient set above is corroborated by
three independent sources:

- NI DIAdem's CFC documentation, which shows identical formula images and states "In accordance
  with SAE J211, a 4-channel Butterworth low pass … with special starting conditions" [S33].
- GTBiomech `CFC.m` (identical formulas, MATLAB `filtfilt`) [S30].
- An LS-PrePost test-suite docstring ("f_d = 2.0775 CFC") [S30].

ISO 6487's example code uses 2.0833 instead (see "ISO 6487 vs SAE J211 constant" below).
⚠ Residual: J211's "special starting conditions" (initial values of the recursion) are not
specified in any retrieved source. Pad the ends or use `filtfilt`, and crop well inside the record.

```python
import warnings
import numpy as np

J211_K = 2.0775            # SAE J211 constant (ISO 6487 example code uses 1.25*5/3)
MIN_FS_FACTOR = 10         # recommended fs >= 10 x CFC (DIAdem table; NCAP/IIHS 10 kHz)

def cfc_filter(x, dt, cfc, strict=False):
    """SAE J211/1 App. C 4-pole phaseless Butterworth. x: 1-D array, dt: s.

    Raises ValueError if fs <= 2*J211_K*CFC (filter undefined: tan() argument >= pi/2).
    Warns (or raises, if strict=True) if fs < 10 x CFC (response distorted by warping).
    """
    fs = 1.0 / dt
    if fs <= 2 * J211_K * cfc:
        raise ValueError(f"fs={fs:.0f} Hz is below the hard limit {2*J211_K*cfc:.0f} Hz "
                         f"for CFC {cfc}; the J211 filter is undefined.")
    if fs < MIN_FS_FACTOR * cfc:
        msg = (f"fs={fs:.0f} Hz < {MIN_FS_FACTOR} x CFC ({MIN_FS_FACTOR*cfc} Hz) for CFC {cfc}; "
               "response will deviate from the J211 corridor.")
        if strict:
            raise ValueError(msg)
        warnings.warn(msg)
    wd = 2*np.pi*cfc*J211_K
    wa = np.sin(wd*dt/2)/np.cos(wd*dt/2)
    den = 1 + np.sqrt(2)*wa + wa**2
    a0 = wa**2/den; a1 = 2*a0; a2 = a0
    b1 = -2*(wa**2 - 1)/den
    b2 = (-1 + np.sqrt(2)*wa - wa**2)/den

    def one_pass(u):
        y = np.empty_like(u, dtype=float)
        y[0] = u[0]; y[1] = u[1]          # simple start; see "edge handling" below
        for n in range(2, len(u)):
            y[n] = a0*u[n] + a1*u[n-1] + a2*u[n-2] + b1*y[n-1] + b2*y[n-2]
        return y

    return one_pass(one_pass(np.asarray(x, float))[::-1])[::-1]
```

Equivalent with SciPy: `scipy.signal.filtfilt([a0, a1, a2], [1, -b1, -b2], x)`. `filtfilt` also
pads the ends, which reduces edge transients.

### Pre-processing that the protocols specify

| Step | US NCAP lab procedure [S24] | IIHS [S28] | Euro NCAP [S6] |
|---|---|---|---|
| Delivered data | **Not pre-filtered**, above Class 1000, ≥ **10,000 samples/s** | 10 kHz, 16-bit, 3 kHz analog anti-alias prefilter | Unfiltered or prefiltered (`P`) |
| Record window | Begins at most **−50 ms**, truncated at **300 ms** | – | – |
| Offset removal | "Time zero bias should be removed from all signals" | Mean of 100 points from **−50 to −40 ms**, subtracted | – |
| Time zero | Determined by engineer, verified with trigger signal | – | – |

General practice: filter the **whole record**, including pre-T0 data, then crop to the evaluation
window.

## Required CFC by channel and protocol

| Channel | 49 CFR 572 E (H3 50M) [S2] | 49 CFR 572 O (H3 5F) [S3] | FMVSS 208 Nij [S1] | **US NCAP lab procedure** [S24] | IIHS [S28] | Euro NCAP CP 005 [S6] | UN R94 [S23] / R137 [S9] |
|---|---|---|---|---|---|---|---|
| Head accel | 1000 | 1000 | – | 1000 | J211 | 1000 | 1000 |
| Head angular rate | – | – | – | 60 | J211 | – | – |
| Upper neck forces | 1000 | 1000 | **600** | **1000** (600 for M_OC / Nij) | J211 | 1000 (Fx 600 for M_OC) | 1000 |
| Upper neck moments | 600 | 600 | **600** | 600 | J211 | 600 | 600 |
| Nij | – | – | 600 | **600** | – | – | – |
| Chest (spine) accel | 180 | 180 | – | 180 | J211 | 180 | – |
| Sternum deflection | 180 | 600 | – | **600 (both dummies)** | J211; **rate from CFC 60** | 180 | 180 |
| Pelvis accel | – | 1000 | – | 1000 | – | 600 | – |
| Femur force | 600 | 600 | – | 600 | J211 | 600 | 600 |
| Knee slider | – | – | – | – | J211 | 180 | 180 (R94) |
| Tibia forces / moments | – | – | – | 600 / 600 | J211 | 600 | 600 (R94) |
| Foot accel | – | – | – | 1000 | **180** | – | – |
| Seat belt force | – | – | – | 60 | – | 60 | – |
| VC input deflection | – | – | – | – | J211 | 180 | 180 |

"J211" = IIHS states only that it uses the J211/1 (2014) recommended classes, with the explicit
exceptions shown [S28].

### Practical consequences

1. **Nij (FMVSS 208 / US NCAP / IIHS):** filter Fx, Fz, My at **CFC 600**, then compute M_OCy
   [S1 S6.6(a)(1); S24].
2. **Peak neck tension / compression (FMVSS 208 / US NCAP):** neck forces at **CFC 1000**. FMVSS
   208 defers to Part 572 (S6.7/S15.3.7) for classes not stated. The NCAP lab procedure lists
   "Neck Forces 1000", and NHTSA report plots of `NECKUP00…FOZ` carry filter letter `A` [S24, S25].
3. **Sternum deflection in US NCAP:** the NCAP lab procedure filters "Chest Compression" at **CFC
   600 for both the H3 50M driver and H3 5F passenger** [S24 §I]. NHTSA reports plot both as
   `…CHST0000…DSXB`, "SAE Class 600" [S25]. This differs from the Part 572 Subpart E dummy
   *certification* class (180) for the 50M. Use **600** to reproduce NCAP results. For FMVSS 208
   compliance, Part 572 says 180 for the 50M and 600 for the 5F. ⚠ Confirm with NHTSA's FMVSS 208
   compliance test procedure (TP-208), which was not retrieved.
4. **Euro NCAP neck:** tension and shear peaks use CFC 1000 channels. The extension moment at the
   OC uses My and Fx at CFC 600 [S6 §2, §3.2.1].
5. **IIHS sternum deflection rate** is computed from deflection filtered at **CFC 60** [S28].
6. **Resultants:** filter each component first, then compute the resultant [S6 §3.1.1].

## Minimum sampling rate per CFC

### Per-class minimums (NI DIAdem CFC documentation [S33])

DIAdem's crash-analysis documentation tabulates these values. It cites SAE J211 §8.4.1 and ISO 6487
§4.5 / §5.8 and the "Vehicle Safety Workgroup" criteria description. IIHS performs its filtering
in DIAdem [S28].

| CFC | "3-dB limit frequency" (= F_N) | Stop damping | **Minimum sampling frequency** |
|---|---|---|---|
| 60 | 100 Hz | −30 dB | **≥ 600 Hz** |
| 180 | 300 Hz | −30 dB | **≥ 1800 Hz** |
| 600 | 1000 Hz | −40 dB | **≥ 6 kHz** |
| 1000 | 1650 Hz | −40 dB | **≥ 10 kHz** |

Rule of thumb from the table: **sample at ≥ 10 × CFC.** One dummy data set is usually acquired at a
single rate for all channels, so the CFC 1000 head channels drive the requirement: ≥ 10 kHz.

⚠ This comes from vendor documentation (secondary). It paraphrases J211/ISO 6487, which were not
retrieved. It agrees with the protocol requirements below.

### Protocol requirements

| Program | Requirement | Source |
|---|---|---|
| US NCAP | Digitized at "a minimum rate of 10,000 samples per second", not pre-filtered, data above Class 1000 | [S24 §E] |
| IIHS | 10 kHz, 16-bit, 3 kHz analog anti-alias prefilter | [S28] |
| UN R94 / R137 | VC: δt ≤ 1.25 × 10⁻⁴ s (≥ 8 kHz) for the sternum deflection used in V*C | [S23 Annex 4 §6; S9 Annex 4 §5] |
| Euro NCAP | No rate stated in CP 005. Delivered unfiltered/prefiltered, filtered by Euro NCAP software | [S6] |

### Why the floor exists (derived, checked numerically)

- **Hard mathematical limit:** the bilinear-transform coefficients use `tan(wd·T/2)` with
  `wd = 2π·2.0775·CFC`. This is only valid while `2.0775·CFC < fs/2`, i.e. **fs > 4.155 × CFC**.
  Below that the filter is meaningless.
- **Frequency warping near the floor:** the digital response converges to the analog prototype only
  when fs ≫ CFC. Response of the forward+backward J211 filter, computed with
  `scipy.signal.freqz`:

  | CFC | fs | −3 dB point | Gain at F_H | Gain at F_N |
  |---|---|---|---|---|
  | 1000 | 5 kHz (5×) | 1980 Hz | −0.01 dB | −0.38 dB |
  | 1000 | 10 kHz (10×) | 1750 Hz | −0.28 dB | −2.34 dB |
  | 1000 | 20 kHz (20×) | 1686 Hz | −0.41 dB | −2.78 dB |
  | 180 | 10 kHz (≈56×) | 300 Hz | −0.45 dB | −2.89 dB |
  | 60 | 10 kHz (≈167×) | 100 Hz | −0.45 dB | −2.90 dB |

  At ≥ 10 × CFC the passband stays within ±0.5 dB at F_H, and the −3 dB point approaches F_N. At
  5 × CFC the roll-off is pushed well past F_N, and the filter barely attenuates at F_N. ⚠ A full
  check against every breakpoint of the J211 / R94 Annex 8 Figure 1 corridor (−9 dB/oct,
  −24 dB/oct, −30/−40 dB stop band) was not done. Only the ±0.5 dB passband and F_N breakpoints were
  extracted from [S23].
- **Anti-aliasing:** the analog prefilter must remove content above fs/2 without touching the CFC
  passband. That needs fs/2 well above F_N, e.g. IIHS's 3 kHz prefilter with 10 kHz sampling
  [S28].

`cfc_filter()` (above) enforces these checks: it raises `ValueError` below the hard limit, and
warns below 10 × CFC (or raises, with `strict=True`). To check a data set before filtering:

```python
def check_sampling(cfc, fs):
    """Return (ok_hard_limit, meets_10x_rule)."""
    return fs > 2 * 2.0775 * cfc, fs >= 10 * cfc
```

### ISO 6487 vs SAE J211 constant

The DIAdem documentation notes that the **ISO 6487 example code** computes
`wd = 2π · CFC · 1.25 · 5/3 = 2π · CFC · 2.0833`, deviating from SAE J211's `2.0775`. DIAdem uses
the J211 value [S33]. The difference shifts the response by about 0.3 % (−3 dB point 1750 → 1755 Hz
for CFC 1000 at 10 kHz). That is negligible for injury values, but it can explain tiny differences
between software packages.
