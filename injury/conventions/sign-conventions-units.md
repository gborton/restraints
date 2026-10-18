# Sign Conventions, Units and Evaluation Windows

## Polarity standard

| Source | Convention referenced |
|---|---|
| 49 CFR 572 Subpart E (H3 50M) | Document incorporated by § 572.31(a)(5) [S2 §572.36(j)]. NCAP lab procedure: Figure 2A / Table 1A, other transducers per SAE J1733 DEC94 [S24 App. A] |
| 49 CFR 572 Subpart O (H3 5F) | SAE J1733 [S3 §572.137(n)]. NCAP lab procedure Figure 1B [S24] |
| IIHS | SAE J1733 (2018) [S28] |
| Euro NCAP 2026 | SAE J211 (2022) [S5 §1.1] |

## Hybrid III load polarities (NCAP lab procedure Table 1A) [S24 App. A]

"Directions are defined in relation to a seated dummy." Positive output direction:

| Transducer | Positive output |
|---|---|
| Neck Fx (shear) | Head rearward or chest forward |
| Neck Fy (shear) | Head leftward, chest rightward |
| Neck Fz (axial) | **Head upward, chest downward (tension)** |
| Neck Mx | Left ear toward left shoulder |
| Neck My | **Chin toward sternum (flexion)** |
| Neck Mz | Chin toward left shoulder |
| Femur Fz (axial) | **Knee forward, pelvis rearward (tension)**. So compression is negative |
| Knee clevis Fz | Tibia downward (tension), femur upward |
| Upper tibia Mx / My | Ankle leftward / ankle forward (knee held) |
| Lower tibia Fx / Fy / Fz | Ankle forward / ankle rightward / ankle downward (tension) |
| Chest displacement | **Chest compressed = negative** |
| Knee shear displacement | **Push on front of tibia = negative** |

Consistent statements elsewhere:

- IIHS "Tension (+FZ) / Flexion (+MY); Compression (−FZ) / Extension (−MY)" [S7, S8].
- Euro NCAP femur `abs(min(F))`, knee `abs(min(D))`, TI only while Fz < 0 [S6 §3.6].
- Euro NCAP shear: "positive shear shall indicate head-rearwards motion" [S6 §3.2.5].

Accelerometer axis directions are given in the NCAP lab procedure Figure 2A (an image, not
extracted). The usual J211 dummy convention is +X forward, +Y right, +Z down. ⚠ Only the sign of
head Ax matters for the criteria here (Euro NCAP loading-phase end). Resultants are sign-free.

**Agent rule:** in a frontal crash, femurs go into compression and the sternum deflects toward the
spine. If those peaks are positive in your data, the channel polarity is inverted relative to
J211/J1733.

## Units

ISO MME default units are SI: acceleration **m/s²**, displacement **m** [S29]. Convert as needed.

| Criterion | Inputs | Units |
|---|---|---|
| HIC / HPC | Resultant head acceleration, time | **g** and **seconds** [S1 S6.2; S23 Annex 4] |
| 3 ms value | Resultant acceleration | g, ms |
| Nij | Fz, M_OCy and critical values | N, Nm [S1 S6.6] |
| M_OCy | My (Nm), Fx (N), D (m) | Nm |
| VC | Deflection D(t) in **metres**, Δt in s → m/s [S10 Annex 4 §6] |
| Tibia Index | Mx, My in Nm; Fz consistent with (F_C)_Z | – |
| US NCAP neck tension/compression risk | Force in **kN** (FR text misprints "kV") [S4 App. C] |
| US NCAP femur risk | Force in **kN** [S4 Eq. 8–9] |
| US NCAP chest risk | Deflection in **mm**. NHTSA reports chest compression in mm [S25], and only mm gives plausible risks (23.2 mm → 2.1 %, vs 0.01 % if entered as 0.91 in) |

## Evaluation time windows

| Protocol | Window | Source |
|---|---|---|
| FMVSS 208 barrier tests | Data for **300 ms after the vehicle strikes the barrier** | [S1 S4.11(a)] |
| FMVSS 208 driver low-risk deployment (5F) | 125 ms after initiation of final-stage deployment | [S1 S4.11(d)] |
| US NCAP frontal | Data recorded from at most **−50 ms to 300 ms**. Time-zero bias removed | [S24 §E, §H] |
| IIHS | Offset = mean of −50 to −40 ms. Head/neck rated in forward excursion; rebound contacts downgrade per flow chart | [S28; S7 Fig. 1; S27 Fig. 9] |
| Euro NCAP frontal | **Loading phase only**: usually ends when filtered head Ax crosses 0 g after its minimum peak. Euro NCAP decides the actual end | [S6 §3] |
| Euro NCAP (rebound) | "Injury parameter assessments will not be evaluated during the rebound phase" | [S5 §3.5.5] |
| UN R94 / R137 head 3 ms | Cumulative, **excluding rebound movement of the head** | [S23 §5.2.1.1; S9 §5.2.1.1.1] |

## Time zero

T0 = first vehicle-to-barrier contact. US NCAP: "a system that identifies the precise instant of
barrier contact will be incorporated with the time reference signal" [S24 §D]. Use the contact /
event channel. Do not use the first sample.
