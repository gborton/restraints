# ISO MME Channel Codes (ISO/TS 13499) — Hybrid III 50M / 5F

ISO MME (ISO/TS 13499, "Multimedia data exchange format for impact tests") is the common channel
naming system:

- **Euro NCAP** requires all test data in ISO-MME 1.6, fully compliant with ISO/TS 13499 [S6 §1].
- **NHTSA** NCAP test reports label dummy channels with the same 16-character codes (e.g.
  `11FEMRRI00H3FOZB`, `13NECKUP00HFFOZA`) [S25]. NHTSA's data deliverable format is its own Entree
  format [S24 §I.J].
- **IIHS** publishes in its own formats. Map its channels by location, axis and dummy.

The code tables below come from the ISO MME code database (`mme_code.mdb`, data version 1.6.1,
2014-06-25) [S29] and from the channel lists in Euro NCAP CP 005 [S6] and NHTSA reports [S25].

## 16-character code structure

```
 1  2  3456  78  9A  BC  DE  F  G
 T  P  MAIN  F1  F2  F3  PD  D  C
 │  │   │    │   │   │   │   │  └─ Filter class (1)
 │  │   │    │   │   │   │   └──── Direction (1)
 │  │   │    │   │   │   └──────── Physical dimension (2)
 │  │   │    │   │   └──────────── Fine location 3: dummy type (2)
 │  │   │    │   └──────────────── Fine location 2 (2)
 │  │   │    └──────────────────── Fine location 1 (2)
 │  │   └───────────────────────── Main location (4)
 │  └───────────────────────────── Position / seat (1)
 └──────────────────────────────── Test object (1)
```

Example: `11HEAD0000H3ACXA` = vehicle 1, front-left seat, head, H3 50M, acceleration, X, CFC 1000.

### Test object (char 1) [S29]

| Code | Meaning |
|---|---|
| `0` | Undefined / other |
| `1`–`9` | Vehicle 1–9 (vehicle under test = `1`; its occupants use the vehicle's code) |
| `B` | Fixed barrier |
| `M` | Mobile barrier |
| `S` | Sled |
| `D` | Dummy / dummy impactor (component tests) |
| `C`, `H`, `P`, `T` | Component, linear impactor, pendulum, test rig |

### Position / seat (char 2) [S29]

| Code | Seat |
|---|---|
| `0` | Undefined |
| `1` | Front left (driver in LHD) |
| `2` | Front middle |
| `3` | Front right (front passenger in LHD) |
| `4` | 2nd row left |
| `5` | 2nd row middle |
| `6` | 2nd row right |
| `7` / `8` / `9` | 3rd row left / middle / right |
| `A`–`Z` | Seat A–Z (free assignment) |

Euro NCAP MME header: "Driver position object 1: LHD=1, RHD=3" [S6 §1.2]. NHTSA NCAP: driver
H3 50M = `11…`, passenger H3 5F = `13…` [S25].

### Fine location 3: dummy type [S29]

| Code | Dummy |
|---|---|
| `H3` | Hybrid III mid-sized adult male (50th) |
| `HF` | Hybrid III small adult female (5th) |
| `HM` | Hybrid III large adult male (95th) (out of scope) |

### Physical dimension (chars 13–14) and default units [S29]

| Code | Quantity | **ISO MME default unit** |
|---|---|---|
| `AC` | Acceleration | **m/s²** |
| `FO` | Force | N |
| `MO` | Moment | Nm |
| `DS` | Displacement | **m** |
| `DC` | Distance (IR-TRACC) | m |
| `AV` | Angular velocity | rad/s |
| `AN` | Angle | rad |
| `VE` | Velocity | m/s |
| `EN` | Energy | J |
| `TE` | Temperature | K |
| `EV` | Event | – |

**Agent rule:** the default units are SI. Euro NCAP: "All data shall be provided using SI units
unless specified otherwise" [S6 §1]. The injury equations in this KB expect head/chest acceleration
in **g** and deflection/knee displacement in **mm**. Read the unit field in each `.chn`/channel
header and convert (1 g = 9.81 m/s²) before evaluating limits. NHTSA report plots show g, N, Nm and
mm [S25].

### Direction (char 15) [S29]

`X` longitudinal, `Y` lateral, `Z` vertical, `R` resultant, `0` without direction. (`1`/`2`/`3` are
alternative longitudinal/lateral/vertical codes; `L` steering column is expired.)

### Filter class (char 16) [S29]

| Code | Meaning (database remark) |
|---|---|
| `0` | Unfiltered |
| `A` / `B` / `C` / `D` | CFC 1000 / 600 / 180 / 60 (ISO 6487 / SAE J211:MAR95) |
| `E` / `F` / `G` / `H` | CFC 1000 / 600 / 180 / 60 per the **older** ISO 6487:1987 / SAE J211:OCT88 |
| `P` | Prefiltered > CFC 1000 (filter described in the channel file) |
| `Q` | Prefiltered < CFC 60 |
| `S` | Special filter |
| `V` | Data from film/video analysis |
| `X` | Without (constant channel) |

Euro NCAP delivers raw dummy channels as `…P` and applies CFC filters in its analysis software
[S6 §2]. NHTSA reports label plotted channels with the applied class (`A`, `B`, `C`) [S25].

## Hybrid III channels used for injury criteria

`??` = test object + position. `dt` = `H3` or `HF`. `[LE,RI]` = left/right. `[UP,LO]` = upper/lower
tibia.

| Body region | Signal | Euro NCAP code (as delivered) [S6] | NHTSA NCAP code (as plotted) [S25] | Euro NCAP CFC | NHTSA NCAP CFC [S24] |
|---|---|---|---|---|---|
| Head | Accel X, Y, Z / resultant | `??HEAD0000dtAC[X,Y,Z]P` | `??HEAD0000dtAC[X,Y,Z,R]A` | 1000 | 1000 |
| Upper neck | Force Fx, Fy, Fz | `??NECKUP00dtFO[X,Y,Z]P` | `??NECKUP00dtFO[X,Z]A` | 1000 | 1000 (600 for Nij) |
| Upper neck | Moment Mx, My, Mz | `??NECKUP00dtMO[X,Y,Z]P` | `??NECKUP00dtMOYB` | 600 | 600 |
| Chest (spine box) | Accel X, Y, Z / resultant | `??CHST0000dtAC[X,Y,Z]P` | `??CHST0000dtAC[X,Y,Z,R]C` | 180 | 180 |
| Chest | Sternum deflection | `??CHST0003dtDSXP` | `??CHST0000dtDSXB` | 180 | **600** |
| Pelvis | Accel X, Y, Z | `??PELV0000dtAC[X,Y,Z]P` | – | 600 | 1000 |
| Iliac (5F) | Fx / My | `??ILAC[LE,RI]00HFFOXP` / `MOYP` | – | 600 | – |
| Lumbar | Fx, Fz / My | `??LUSP0000dtFO[X,Z]P` / `MOYP` | – | 600 | – |
| Femur | Axial Fz | `??FEMR[LE,RI]00dtFOZP` | `??FEMR[LE,RI]00dtFOZB` | 600 | 600 |
| Knee slider | Displacement | `??KNSL[LE,RI]00dtDSXP` | – | 180 | – |
| Upper tibia | Fx, Fz / Mx, My | `??TIBI[LE,RI]UPdtFO[X,Z]P` / `MO[X,Y]P` | – | 600 | 600 |
| Lower tibia | Fx, Fy, Fz / Mx, My | `??TIBI[LE,RI]LOdtFO[X,Y,Z]P` / `MO[X,Y]P` | – | 600 | 600 |
| Seat belt (B3 shoulder) | Force | `??SEBE0003B3FO0P` | – | 60 | 60 |

**Sternum deflection location code differs:** Euro NCAP uses fine location `0003`
(`CHST0003`) [S6]. NHTSA reports use `0000` (`CHST0000…DSX`) [S25]. Match on main location +
dimension + direction (`CHST…DSX`) rather than the exact string.

Fully expanded examples:

```
Euro NCAP FWDB driver (LHD), H3 5F, filtered for calculation:
11HEAD0000HFACXA  11NECKUP00HFFOZA  11NECKUP00HFMOYB  11CHST0003HFDSXC
11FEMRLE00HFFOZB  11KNSLLE00HFDSXC  11TIBILEUPHFMOXB  11TIBILEUPHFFOZB

US NCAP driver H3 50M / passenger H3 5F (as in NHTSA report plots):
11HEAD0000H3ACRA  11NECKUP00H3FOZA  11NECKUP00H3MOYB  11CHST0000H3DSXB  11FEMRLE00H3FOZB
13HEAD0000HFACRA  13NECKUP00HFFOZA  13NECKUP00HFMOYB  13CHST0000HFDSXB  13FEMRRI00HFFOZB
```

Euro NCAP's occipital-condyle moment uses shear force filtered at CFC 600
(`??NECKUP00??FOXB`) even though neck forces are otherwise CFC 1000 [S6 §3.2.1]. The NHTSA NCAP
lab procedure says the same: "For calculation of the moment about the occipital condyle, use
filter class 600 for the neck force" [S24 App. A §C.2].
