# Hybrid III 5th Percentile Adult Female (H3 5F)

- US definition: **49 CFR Part 572 Subpart O** [S3]. FMVSS 208 S15.3 criteria reference it
  [S1 S15.3.7].
- Euro NCAP specification: Technical Bulletin CP 103, which requires conformance to Part 572
  Subpart O with Euro NCAP modifications [S5 §1.2].
- ISO MME dummy code: **`HF`** [S6 §2.3].
- Polarity per SAE J1733 [S3 §572.137(n)].

## Dummy-specific constants

| Constant | Value | Used in | Source |
|---|---|---|---|
| OC offset D | 0.01778 m ("all HIII series") | M_OCy = My − D·Fx | [S6 §3.2.1] |
| Nij Fzc tension / compression (in-position) | 4287 N / 3880 N | Nij | [S1 S15.3.6(a)(3)] |
| Nij Myc flexion / extension (in-position) | 155 Nm / 67 Nm | Nij | [S1 S15.3.6(a)(3)] |
| Nij OOP (driver low-risk deployment) | Fzc 3880 / 3880 N, Myc 155 / 61 Nm | Nij | [S1 S25.4(a)(3)] |
| VC D_const / sf | 0.187 m / 1.3 | VC | [S6 §3.4.6; S9 Annex 4 §5.2] |
| TI (M_C)_R / (F_C)_Z | 114 Nm / 22.9 kN | Tibia Index | [S6 §3.6.5] |
| US NCAP chest scale | 0.817 (chest depth ratio 5F/50M) | Eq. 7 | [S4] |
| US NCAP femur scale | 0.68 (thigh circumference ratio) | Eq. 9 | [S4] |

## Channel set

Position codes [S29]: `1` driver (LHD), `3` front passenger (LHD), `4` 2nd-row left, `6` 2nd-row
right. Examples: US NCAP passenger `13…` [S25]. Euro NCAP FWDB driver `11…`, FWDB rear passenger
(behind front passenger) `16…` [S5 Fig. 2]. IIHS MO 2.0 rear-left `14…` [S8].

| Region | Channels (shown for seat 3) | CFC Part 572 O [S3] | CFC US NCAP lab [S24] | CFC Euro NCAP [S6] |
|---|---|---|---|---|
| Head | `13HEAD0000HFACXA`, `…ACYA`, `…ACZA` (NHTSA also `…ACRA`) | 1000 | 1000 | 1000 |
| Upper neck forces | `13NECKUP00HFFO[X,Y,Z]?` | 1000 | 1000 (600 for Nij/M_OC) | 1000 (Fx at 600 for M_OC) |
| Upper neck moments | `13NECKUP00HFMO[X,Y,Z]B` | 600 | 600 | 600 |
| Chest accel | `13CHST0000HFAC[X,Y,Z]C` | 180 | 180 | 180 |
| Chest (sternum) deflection | Euro NCAP `13CHST0003HFDSXC`. NHTSA `13CHST0000HFDSXB` | **600** | **600** | **180** |
| Pelvis accel | `13PELV0000HFAC[X,Y,Z]?` | 1000 | 1000 | 600 |
| Iliac force / moment | `13ILAC[LE,RI]00HFFOX?` / `…MOY?` | 180 (iliac wing forces) | – | 600 |
| Lumbar Fx, Fz / My | `13LUSP0000HFFO[X,Z]?` / `…MOY?` | 1000 / 600 | – | 600 / 600 |
| Femur | `13FEMR[LE,RI]00HFFOZB` | 600 | 600 | 600 |
| Knee slider | `13KNSL[LE,RI]00HFDSXC` | – | – | 180 |
| Upper / lower tibia | `13TIBI[LE,RI][UP,LO]HFMO[X,Y]B`, `…FO[X,(Y),Z]B` | – | 600 | 600 |

**Sternum deflection CFC differs by program:** 600 in US practice (Part 572 O [S3 §572.137(m)(3)(iii)]
and the NCAP lab procedure [S24]); 180 in Euro NCAP [S6 §2.3] and UN R137 [S9 Annex 3 §5.2.3].
ISO MME default units are SI (m/s², m) [S29].

## Where the H3 5F is used (frontal)

| Program | Test | Seat(s) | Details |
|---|---|---|---|
| FMVSS 208 | Belted rigid barrier 0° ±5°, up to 48 km/h (S14.1/14.2) or 56 km/h (S14.6/14.7) | Driver, RF passenger | `protocols/fmvss-208.md` |
| FMVSS 208 | Unbelted rigid barrier 32–40 km/h ±5° | Driver, RF passenger | |
| FMVSS 208 | Offset deformable barrier, 40 km/h, 40 % overlap, left side, belted | Driver, RF passenger | |
| FMVSS 208 | Driver out-of-position low-risk deployment (chin-on-module, chin-on-rim) | Driver | |
| US NCAP | 56.3 km/h (35 mph) full frontal rigid barrier | RF passenger | `protocols/us-ncap-frontal.md` |
| IIHS | Moderate Overlap 2.0 | Rear left (behind driver) | `protocols/iihs-frontal.md` |
| Euro NCAP 2026 | MPDB, vehicle 50 km/h vs trolley 50 km/h, 50 % overlap | Front passenger | `protocols/euro-ncap-frontal.md` |
| Euro NCAP 2026 | **FWDB 35 km/h** | Driver; rear passenger (behind front passenger) | |
| Euro NCAP 2026 | Sled & virtual (35 km/h and 56 km/h load cases) | Driver / passenger | |
| UN R137 | 50 km/h full width rigid | Front passenger | `protocols/un-r94-r137.md` |
