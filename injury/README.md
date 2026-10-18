# Injury Criteria Knowledge Base — Hybrid III 50th Male & 5th Female (Frontal)

Reference files that let an agent compute frontal-crash injury criteria from Hybrid III 50th
percentile male (H3 50M) and Hybrid III 5th percentile female (H3 5F) dummy data. Each file covers
equations, ISO MME channel names, filtering, and which regulatory or consumer test uses each
criterion and its limit.

Scope: H3 50M and H3 5F only. THOR, WorldSID, ES-2re, SID-IIs, Q-series and CRABI are out of scope.
Where a protocol uses other dummies in the same test (for example THOR in Euro NCAP MPDB), this KB
notes that and stops there.

## File map

| File | Use it for |
|---|---|
| [quick-reference.md](quick-reference.md) | One matrix of test → dummy → criterion → limit. Start here. |
| [conventions/iso-mme-channel-codes.md](conventions/iso-mme-channel-codes.md) | Building and parsing ISO MME channel codes |
| [conventions/sign-conventions-units.md](conventions/sign-conventions-units.md) | SAE J211 polarity, units, time windows |
| [conventions/filtering-cfc.md](conventions/filtering-cfc.md) | CFC filter algorithm and the required CFC for each channel in each protocol |
| [criteria/head.md](criteria/head.md) | HIC15, HIC36/HPC, 3 ms exceedance, peak resultant |
| [criteria/neck.md](criteria/neck.md) | Occipital condyle moment, Nij, tension/compression/shear, extension, duration corridors |
| [criteria/chest.md](criteria/chest.md) | Sternum deflection, chest 3 ms acceleration, VC, deflection rate, IIHS Chest Index |
| [criteria/lower-extremity.md](criteria/lower-extremity.md) | Femur force (incl. duration/KTH), knee slider, Tibia Index, tibia compression, IIHS moment adjustment |
| [dummies/hybrid-iii-50m.md](dummies/hybrid-iii-50m.md) | H3 50M channel list and dummy-specific constants |
| [dummies/hybrid-iii-05f.md](dummies/hybrid-iii-05f.md) | H3 5F channel list and dummy-specific constants |
| [protocols/fmvss-208.md](protocols/fmvss-208.md) | US FMVSS No. 208 compliance tests |
| [protocols/us-ncap-frontal.md](protocols/us-ncap-frontal.md) | US NCAP 35 mph (56 km/h) full frontal: risk curves and stars |
| [protocols/iihs-frontal.md](protocols/iihs-frontal.md) | IIHS small overlap, moderate overlap (original) and Moderate Overlap 2.0 |
| [protocols/euro-ncap-frontal.md](protocols/euro-ncap-frontal.md) | Euro NCAP 2026 Crash Protection: frontal MPDB, FWDB, sled/virtual |
| [protocols/un-r94-r137.md](protocols/un-r94-r137.md) | UN R94 (offset) and UN R137 (full width) type approval |
| [sources.md](sources.md) | Citations `[S#]` used in every file |

## How an agent should use this KB

1. Identify the **test protocol** (for example "US NCAP frontal") and **seat/dummy**. Open the protocol
   file to see which criteria apply and what the limits or scoring are.
2. For each criterion, open the `criteria/` file to get the equation, the input channels, and the
   protocol-specific filtering. **CFC choices differ between protocols for the same physical
   channel.** Example: H3 5F sternum deflection is CFC 600 under 49 CFR 572 Subpart O and CFC 180
   under Euro NCAP and UN R137. See `conventions/filtering-cfc.md`.
3. Resolve channels using `conventions/iso-mme-channel-codes.md` plus the dummy file.
4. Apply polarity rules from `conventions/sign-conventions-units.md` before using any
   "compression = negative" or "tension = positive" logic.
5. Evaluate over the protocol's time window. FMVSS 208 and US NCAP use 300 ms after barrier contact.
   Euro NCAP uses the loading phase only.

## Confidence markers

- Values without a marker were read directly from the cited primary source during compilation
  (2026-10-18). A second verification pass the same day resolved most open items. The pass read
  regulation figures from images, pulled the NHTSA NCAP lab procedure and a 2024 NCAP test report,
  the current IIHS small overlap protocols, Euro NCAP v1.2 / CP 007 / TB 021 / AOP v9.3, the 2026
  UN R94 consolidation, and the ISO MME code database.
- **⚠** marks the few items still not confirmed from a primary source. `sources.md` →
  "Remaining open items" lists them.

## Key gotchas found during verification

- **Euro NCAP 2026 full-width test is 35 km/h** (MPDB is 50 + 50 km/h, 50 % overlap).
- **US NCAP filters sternum deflection at CFC 600 for both H3 50M and H3 5F.** Euro NCAP and UN
  use 180.
- **FMVSS 208 says "cumulative" 3 ms, but NHTSA's software computes a continuous 3 ms clip.**
  Euro NCAP and UN head 3 ms are cumulative.
- **IIHS small overlap (v VII) and MO 2.0 rate thigh/hip by KTH risk**, not peak femur force.
- **ISO MME default units are SI** (m/s², m). Convert to g and mm before applying limits.
- **CFC number = F_H** (e.g. CFC 1000 → F_H 1000 Hz, F_N 1650 Hz).

Regulations and consumer protocols change. Check the version and date in `sources.md` against the
current issue before use.

## Disclaimer

This knowledge base is an independent engineering reference compiled from public documents. It is
**not affiliated with, endorsed by, or an official publication of** NHTSA, IIHS, Euro NCAP, UNECE,
SAE, ISO or any other organization named in it. Names of programs and organizations are used only
to identify the documents summarized.

The content is provided "as is", without warranty of any kind. It is **not certification-grade**:
values may be incomplete, out of date or mis-transcribed, and some items are explicitly marked ⚠ as
unconfirmed. Before using any value, equation or code here for compliance testing, consumer
ratings, product decisions or safety-critical work, verify it against the current official document
cited in `sources.md`. The authors accept no liability for any use of this material.

Regulatory values and limits are factual statements drawn from the cited sources. Copyright in
those source documents remains with their owners. Short excerpts are quoted for reference with
attribution, and the documents themselves are not redistributed here.

## License

- **Documentation** (all Markdown text and tables): [Creative Commons Attribution 4.0
  International (CC BY 4.0)](../LICENSE). You may share and adapt it, including commercially, with
  attribution.
- **Code** (the Python snippets in fenced code blocks): [MIT License](../LICENSE-CODE).

Suggested attribution: "injuryMD — Hybrid III frontal injury criteria knowledge base, CC BY 4.0".
