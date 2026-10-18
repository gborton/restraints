# Sources

Compiled 2026-10-18 (initial pass, then a verification pass the same day). Every `[S#]` tag in this
knowledge base points to an entry below. Values were read from these documents directly unless the
entry says "secondary" or "not retrieved".

## Primary — United States

**[S1] 49 CFR 571.208 — FMVSS No. 208, Occupant Crash Protection.**
https://www.law.cornell.edu/cfr/text/49/571.208 (LII mirror of the eCFR; official:
https://www.ecfr.gov/current/title-49/subtitle-B/chapter-V/part-571/subpart-B/section-571.208).
Used: S4.11, S4.13, S5.1–S5.2, S6.1–S6.7, S13.2, S14.5, S15.1–S15.3.7, S16.1, S17–S18, S25.3–S25.4,
S26.

**[S2] 49 CFR 572.36 — Hybrid III (Subpart E) test conditions and instrumentation.**
https://www.law.cornell.edu/cfr/text/49/572.36. Used: §572.36(i) channel classes, §572.36(j).

**[S3] 49 CFR 572.137 — Hybrid III 5th Female (Subpart O) test conditions and instrumentation.**
https://www.law.cornell.edu/cfr/text/49/572.137. Used: §572.137(m) channel classes (sternum
deflection 600), §572.137(n) SAE J1733.

**[S4] NHTSA, "Consumer Information; New Car Assessment Program," 73 FR 40016–40050, July 11,
2008 (FR Doc. E8‑15620).** PDF: https://www.govinfo.gov/content/pkg/FR-2008-07-11/pdf/E8-15620.pdf
· Text: https://www.federalregister.gov/documents/full_text/text/2008/07/11/E8-15620.txt
Used: Appendix C risk curves Eq. 1–9, joint probability, Table 1, §d star bands, driver/passenger
averaging, age-35 normalisation.

**[S24] NHTSA Office of Crashworthiness Standards, "Laboratory Procedure for New Car Assessment
Program Full Frontal Rigid Barrier Impact Testing," May 2018** (docket NHTSA‑2015‑0046‑0014).
https://downloads.regulations.gov/NHTSA-2015-0046-0014/attachment_1.pdf
Used: data acquisition (not pre-filtered, ≥ 10 kHz, −50 to 300 ms, time-zero bias removal),
§I filtering requirements table (neck forces 1000, Nij 600, chest compression 600, etc.), App. A
Part 572 E filter list incl. "use filter class 600 for the neck force" for the OC moment, Table 1A
Hybrid III sign convention.

**[S25] NHTSA NCAP test report NCAP‑KAR‑24‑006, 2024 Tesla Cybertruck, 56.3 km/h full frontal,
test date Dec 18, 2024.** https://nrd-static.nhtsa.dot.gov/reports/vehdb/v10000/v15300/v15312R001.pdf
Used: confirms the May 2018 lab procedure is in current use, H3 50M driver / H3 5F passenger,
ISO-MME-style channel codes and SAE classes on data plots (e.g. `11CHST0000H3DSXB` "SAE Class
600", `13NECKUP00HFFOZA` "SAE Class 1000"), reported injury summary and units.

**[S26] NHTSA Chief Counsel interpretation letter 24207.jeg (to Exponent), dated 7/2/2002.**
https://www.nhtsa.gov/interpretations/24207jeg (retrieved via
http://web.archive.org/web/20250729171811/https://www.nhtsa.gov/interpretations/24207jeg).
Used: "NHTSA's Signal Analysis Software, consistent with SAE J1727, calculates chest g's with the 3
millisecond clip limit determined on a continuous basis … chest g's will sometimes be lower (but
never higher)."

**[S31] NHTSA NCAP notices, 2023 RFC (88 FR, May 26, 2023, FR Doc. 2023‑11201) and Final Decision
(Nov 25, 2024, FR Doc. 2024‑27446)** —
https://www.federalregister.gov/documents/full_text/text/2023/05/26/2023-11201.txt and
https://www.federalregister.gov/documents/full_text/text/2024/11/25/2024-27446.txt
Used only to confirm that these notices address crashworthiness pedestrian protection, not frontal
dummies or risk curves.

## Primary — IIHS

**[S7] IIHS, "Moderate Overlap Frontal Crashworthiness Evaluation — Guidelines for Rating Injury
Measures," September 2014.**
https://iihs-prod.iihs.org/media/dcc9d5d6-c301-4f92-a7b5-dad13ffc7766/6h8G8A/Ratings/Protocols/current/measures_frontal.pdf
Used: Table 1–2, Fig. 1–5, tibia moment adjustment Eq. 2–3. The operator signs were confirmed from
the PDF's Symbol glyphs U+F02D (−) and U+F02B (+).

**[S8] IIHS, "Moderate Overlap 2.0 Rating Guidelines (Version II)," May 2024.**
https://iihs.org/media/6e36bcef-fcdd-4a22-a2b2-cf9390b6ff6c/zVAeaQ/Ratings/Protocols/current/Moderate%202.0%20Rating%20guidelines%20Phase%202_FINAL_May2024.pdf
Used: Tables 1, 2, 4–7, Chest Index Eq. 4–5, KTH impulse definition, tibia adjustment.

**[S27] IIHS, "Small Overlap Frontal Crashworthiness Evaluation Rating Protocol, Version VII,"
April 2024.**
https://iihs-prod.iihs.org/media/4ff6d6ee-2dc9-459c-a588-1cdcae448531/S7n0Kg/Ratings/Protocols/current/small_overlap_rating_protocol.pdf
Used: 64.4 km/h / 25 % overlap, Table 2 (with KTH, no femur force row), left/right worst-limb rule,
Fig. 9–14, passenger-side appendix (H3 50M driver and passenger, same Table 2).

**[S28] IIHS, "Small Overlap Frontal Crashworthiness Evaluation Crash Test Protocol, Version VII,"
May 2021.**
https://www.IIHS.org/media/ec54a7ea-1a1d-4fb2-8fc3-b2e018db2082/1A5oYw/Ratings/Protocols/current/small_overlap_test_protocol.pdf
Used: data acquisition (10 kHz, 3 kHz prefilter, offset −50 to −40 ms), J211/1 (2014) filtering,
foot accel CFC 180, deflection rate from CFC 60, J1727 (2015) calculations, Nij per 65 FR 30680,
J1733 (2018).

**[S16] IIHS small overlap rating protocol Version V (July 2017, archive).** Superseded by [S27].

## Primary — Euro NCAP

**[S5] Euro NCAP, "Crash Protection — Frontal Impact Protocol," Version 1.2, July 2026.**
https://cdn.euroncap.com/cars/assets/Euro_NCAP_Protocol_Crash_Protection_Frontal_Impact_v1_2_7f33c197c2.pdf
(v1.1, Sept 2025, was used in the first pass:
https://cdn.euroncap.com/cars/assets/euro_ncap_protocol_crash_protection_frontal_impact_v11_9659fff24e.pdf
A diff shows the injury limit tables are unchanged, while scoring/colour text, belt instrumentation
and seat-setting wording changed.)
Used: scoring tables, Figures 1–2 (MPDB 50 ± 1 km/h vehicle and trolley, 50 % ± 25 mm overlap;
FWDB 35 ± 1 km/h; dummy seats), §1.1–1.4, §3.3.1, §3.4 colour band scheme, §3.5.1–3.5.5 limits and
rules, §4.3.1 modifiers.

**[S6] Euro NCAP, Technical Bulletin CP 005, "Data Acquisition and Injury Calculation," Version
1.4.3, June 2026.**
https://cdn.euroncap.com/cars/assets/CP_005_Data_Acquisition_and_Injury_Calculation_v1_4_3_fcd5e5866f.pdf
Used: §1 ISO-MME 1.6 / SI units, MME header fields, §2.1/§2.3 channel names and CFCs, §3
calculations.

**[S19] Euro NCAP, TB 021 "Data Acquisition and Injury Calculation," Version 4.1, November 2023
(superseded by CP 005 for 2026).**
https://cdn.euroncap.com/cars/assets/tb_021_data_acquisition_and_injury_calculation_v41_18245a243a.pdf
Used: channel table labels "Resultant 3ms cumulative exceedence" (head, chest) and "continuous
exceedence" (neck tension/shear, femur) for H3 50M and 5F, plus the loading-phase definition.

**[S20] Euro NCAP, "Assessment Protocol — Adult Occupant Protection," Version 9.3, 5 Dec 2023
(superseded for 2026).**
https://cdn.euroncap.com/cars/assets/euro_ncap_assessment_protocol_aop_v93_f17b8fd21c.pdf
Used: §2.1 linear-interpolation scoring, §2.1.1 capping definition ("loss of all points related to
the tests"), pre-2026 force–duration HPL/LPL curves (neck, femur 9.07 @ 0 / 7.56 @ 10 ms).

**[S21] Euro NCAP, Technical Bulletin CP 007, "Assessment Modifiers," Version 1.2, July 2026.**
https://cdn.euroncap.com/cars/assets/CP_007_Assessment_Modifiers_v1_2_e05b3e9fd1.pdf
Used: §3 head hard-contact rule ("sliding scale is applied to HIC and Ares 3ms"), frontal
modifiers.

**[S22] Euro NCAP, Technical Bulletin CP 101, "MPDB Face Specification," Version 2.0.**
https://cdn.euroncap.com/cars/assets/cp_101_euro_ncap_mobile_progressive_deformable_barrier_face_specification_v20_aedf2e32bd.pdf
Used: MPDB trolley mass incl. face 1400 ± 20 kg.

**[S32] Euro NCAP, "Full Width Frontal Impact Testing Protocol," Version 1.2.1, November 2021
(superseded).**
https://cdn.euroncap.com/cars/assets/euro_ncap_frontal_fw_test_protocol_v121_975675fbcf.pdf
Used: §7.2 target speed 50 ± 1 km/h (pre-2026 comparison only).

## Primary — UNECE

**[S9] UN Regulation No. 137, consolidated to the 02 series, OJ L 392, 5.11.2021, pp. 130–177
(Spanish edition).** https://www.boe.es/doue/2021/392/L00130-00177.pdf
Used: §5.1, §5.2.1.1–5.2.1.2, §5.2.2, Annex 3 (impact speed 50 −0/+1 km/h; CFCs), Annex 4.

**[S23] UN Regulation No. 94, consolidated text incorporating Supplement 1 to the 05 series (entry
into force 12 June 2025), EU OJ L 2026/1075.**
https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=OJ%3AL_202601075
Used: §5.2.1.1–5.2.1.9 (ThCC 42 mm), transitional provisions, Annex 3 (56 −0/+1 km/h, 40 % ±
20 mm), Annex 4 (HPC, TI constants, VC procedure), Annex 8 §1.5 and Figure 1 table (CFC = F_H; F_N
values).

**[S10] UN/ECE Regulation No. 94, OJ L 254, 20.9.2012 (CELEX 42012X0920(02)).**
https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:42012X0920(02)
Older series (ThCC 50 mm). Used for **Figures 1–3**, which were decoded from the embedded JPEG
images and read visually: neck tension, neck shear, femur force–duration curves.

## Code tables and implementations

**[S29] ISO MME code database `mme_code.mdb` (data version 1.6.1, 2014‑06‑25), redistributed in
GitHub repo WuglyakBolgoink/iso-location-code (`out/mme_code.mdb`).**
https://github.com/WuglyakBolgoink/iso-location-code
Read with the `access-parser` Python package (no repo code executed). Used: tables
MME_TEST_OBJECTS, MME_POSITIONS, MME_FILTER_CLASSES (with remarks), MME_DIRECTIONS,
MME_PHYSICAL_DIMENSIONS (default SI units), MME_FINE_LOCATIONS_3 (H3, HF, HM). This is a
third-party copy of the ISO/TS 13499 code list. It is 2014 data. Newer ISO/TS 13499:2019 code
lists may add codes.

**[S30] Open-source J211 Appendix C implementations (corroboration only).**
GTBiomech `CFC.m` — https://github.com/GTBiomech/HEADSport-Filter-Method/blob/main/CFC.m (Tierney
et al., BMJ Open SEM 2024). LS-PrePost-MCP test docstring ("J211 App. C forward + backward … f_d =
2.0775 CFC") — https://github.com/lwz20210407/LS-PrePost-MCP. DADiSP CFC module description (analog
prototype cutoff 5/3 × CFC) — https://www.dadisp.com/cfc1.htm

**[S33] NI DIAdem 2021 documentation, Crash Analysis Toolkit, "CFC Filters."**
https://www.ni.com/docs/en-US/bundle/diadem/page/crash/misc_cfc.html (content retrieved via the
documentation backend `https://docs-be.ni.com/api/bundle/diadem/page/crash/misc_cfc.html`. Formula
images `f_cfc.gif`, `f_cfcconst.gif`, `f_cfcconstwd.gif` were read visually). Vendor documentation
(secondary). It cites SAE J211 §8.4.1 and ISO 6487 §4.5, §5.8. Used: per-CFC 3-dB frequency,
stop damping and **minimum sampling frequency** (600 Hz / 1.8 kHz / 6 kHz / 10 kHz), J211 difference
equation and coefficient formulas, and the ISO 6487 example-code constant (1.25·5/3 = 2.0833).

## Not retrieved / superseded

**[S11] SAE J211‑1** (MAR95 per FMVSS 208; 2014 per IIHS; 2022 per Euro NCAP) — paywalled. Filter
coefficients corroborated via [S30]. CFC frequencies from [S23].
**[S12] ISO/TS 13499** — paywalled. Code tables taken from [S29], [S6] and [S25].
**[S13] Eppinger et al. 1999/2000 (NHTSA Docket 1999‑6407)** — referenced via [S4], [S7], [S8].
**[S14] SAE J1733** — polarity confirmed via the NCAP lab procedure Table 1A [S24].
**SAE J1727** (calculation guidelines) — not retrieved. Its continuous 3 ms clip is characterised by
[S26].
**[S15], [S17], [S18]** (secondary summaries from the first pass) — superseded by [S23], [S5]
Figures 1–2 and [S9] Annex 3 respectively.

## Remaining open items

1. **Euro NCAP 2026 sliding-scale ↔ colour-band mapping** and the **capping consequence**:
   specified in CP 006 "Colour band and test data", listed by Euro NCAP as "coming soon". Capping
   is assumed to follow AOP v9.3 [S20].
2. **US NCAP left/right femur combination** for P_femur: not stated in [S4] or [S24]. Max of L/R
   assumed.
3. **FMVSS 208 compliance (TP‑208) filter for 50M sternum deflection**: Part 572 says 180, while
   NCAP lab practice uses 600 [S24]. TP‑208 not retrieved.
4. **Cumulative vs continuous** exceedance for UN R94 neck/femur force–duration curves: not stated
   in the regulation. Euro NCAP's own pre-2026 documents disagree [S19, S20].
5. **SAE J211‑1 filter details**: the 2.0775 constant is confirmed by three implementations including
   DIAdem [S33]. ISO 6487 example code uses 2.0833 (≈ 0.3 % shift). Still open: J211's "special
   starting conditions", and the per-CFC sampling minimums taken directly from J211 §8.4.1 / ISO
   6487 (currently from [S33] only).
6. **KTH risk function** (Rupp et al. 2009) is not reproduced. Use IIHS corridor figures.
7. **IIHS 3 ms clip**: inferred continuous from J1727 compliance [S28] + [S26]. IIHS does not state
   it explicitly.
