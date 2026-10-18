# Hybrid III 50th Percentile Male (H3 50M)

- US definition: **49 CFR Part 572 Subpart E** [S2]. FMVSS 208 S6 criteria reference it [S1 S6.7].
- Euro NCAP specification: Technical Bulletin CP 104 [S5 §1.2].
- ISO MME dummy code: **`H3`** [S6 §2.1].
- Ankle: UN R94/R137 specify the H3 with a 45° ankle [S9 §5.1].

## Dummy-specific constants

| Constant | Value | Used in | Source |
|---|---|---|---|
| OC offset D | 0.01778 m | M_OCy = My − D·Fx | [S6 §3.2.1] |
| Nij Fzc tension / compression | 6806 N / 6160 N | Nij | [S1 S6.6(a)(3)] |
| Nij Myc flexion / extension | 310 Nm / 135 Nm | Nij | [S1 S6.6(a)(3)] |
| VC D_const / sf | 0.229 m / 1.3 | VC | [S6 §3.4.6; S10 Annex 4 §6] |
| TI (M_C)_R / (F_C)_Z | 225 Nm / 35.9 kN | Tibia Index | [S6 §3.6.5; S10 Annex 4 §5] |
| IIHS tibia adj. arms | upper 0.02832 m, lower 0.006398 m | Adjusted My | [S7, S8] |

## Channel set for frontal injury assessment (driver, LHD → prefix `11`)

| Region | Channels (filtered for calculation) | CFC Part 572 E (certification) [S2] | CFC US NCAP lab [S24] | CFC Euro NCAP [S6] |
|---|---|---|---|---|
| Head | `11HEAD0000H3ACXA`, `…ACYA`, `…ACZA` (NHTSA also `…ACRA`) | 1000 | 1000 | 1000 |
| Upper neck forces | `11NECKUP00H3FOX?`, `…FOY?`, `…FOZ?` | 1000 | 1000 (600 for Nij/M_OC) | 1000 (Fx at 600 for M_OC) |
| Upper neck moments | `11NECKUP00H3MOYB` | 600 | 600 | 600 |
| Chest accel | `11CHST0000H3AC[X,Y,Z]C` | 180 | 180 | 180 |
| Chest deflection | Euro NCAP `11CHST0003H3DSXC`. NHTSA `11CHST0000H3DSXB` | 180 | **600** | 180 |
| Pelvis accel | `11PELV0000H3AC[X,Y,Z]?` | – | 1000 | 600 |
| Femur | `11FEMRLE00H3FOZB`, `11FEMRRI00H3FOZB` | 600 | 600 | 600 |
| Knee slider | `11KNSLLE00H3DSXC`, `11KNSLRI00H3DSXC` | – | – | 180 |
| Upper tibia | `11TIBI[LE,RI]UPH3MO[X,Y]B`, `…FO[X,Z]B` | – | 600 | 600 |
| Lower tibia | `11TIBI[LE,RI]LOH3MO[X,Y]B`, `…FO[X,Y,Z]B` | – | 600 | 600 |

ISO MME delivers acceleration in m/s² and displacement in m by default. Convert to g and mm
[S29].

Front passenger in LHD: replace position `1` with `3` (e.g. `13HEAD0000H3ACXA`).

## Where the H3 50M is used (frontal)

| Program | Test | Seat(s) | Details |
|---|---|---|---|
| FMVSS 208 | Belted rigid barrier 0°, up to 56 km/h (S14.3/S14.4 certified vehicles) | Driver, RF passenger | `protocols/fmvss-208.md` |
| FMVSS 208 | Unbelted rigid barrier 32–40 km/h, 0 to ±30° | Driver, RF passenger | |
| US NCAP | 56.3 km/h (35 mph) full frontal rigid barrier | Driver | `protocols/us-ncap-frontal.md` |
| IIHS | Small overlap 64.4 km/h, 25 % (driver side: driver; passenger side: driver **and** passenger) | Driver / passenger | `protocols/iihs-frontal.md` |
| IIHS | Moderate overlap (original) and Moderate Overlap 2.0 | Driver | |
| Euro NCAP 2026 | Sled & virtual load cases only (driver: 50 km/h R137 pulse and 35 km/h FWDB pulse; passenger: 35 km/h FWDB pulse) | Driver / passenger | `protocols/euro-ncap-frontal.md` |
| UN R94 | 56 km/h 40 % offset deformable barrier | Both front outboard | `protocols/un-r94-r137.md` |
| UN R137 | 50 km/h full width rigid | Driver | |
