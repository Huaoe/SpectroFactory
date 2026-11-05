---
title: 'technical research: 3D-printable open-source modular smartphone visible spectrophotometer + React guide site'
type: 'technical'
topic: '3D-printable open-source modular smartphone visible spectrophotometer + React guide site'
decision: 'What the guide site must cover and which base design + in-browser tech to build it on'
source: 'native run (bmad-deep-recon, sequential, web search + full-page fetch)'
status: complete
preset: 'standard'
validation: 'normal'
created: '2026-11-05'
updated: '2026-11-05'
claims_verified: 11
claims_unverified: 1
claims_disputed: 1
---

# Technical research: 3D-printable open-source modular smartphone visible spectrophotometer + React guide site

**Decision this research serves:** What the guide site must cover and which base design + in-browser tech to build it on.

## Executive summary

**Base the guide on the Winters et al. modular double-beam spectrophotometer (DBS, HardwareX 2021).** It is the only candidate found that is at once explicitly modular, documented part by part (DBS01–DBS25 as STL/STEP, BOM, Onshape CAD), openly licensed for derivatives (CC BY-SA 4.0 hardware), validated quantitatively against a commercial instrument, and already designed for low-end consumer printers and multiple cameras [1][2][3]. Smith Lab's DualSpec is the strongest alternative (standard 1 cm cuvettes, removable 0.1–5 mm slits) [4]; Public Lab and DVD/"toy" designs suit a beginner "explore" track, not quantitative use [21][23].

**Three findings drive the product shape:**
1. **Fit and light-tightness are the main build risks, not optics theory.** The DBS authors had to trim burrs, test-print fit pieces and reprint binding parts [2]; black PLA/PETG/ABS is opaque at ≥1 mm while some black filaments leak in red/NIR [8][9]. The guide needs a test-print/fit step and a light-leak check before calibration.
2. **Calibration must be repeated after every phone re-attachment.** Two independent vendors/educators say so [10][11]. Fluorescent-lamp Hg lines (436/488/546/611 nm) are the standard, cheap reference [10][11]; DBS used two-point calibration with LEDs, lasers or CFL lines [2].
3. **Browser capture is fine for qualitative spectra but quantitative absorbance is fragile.** Manual exposure/white-balance constraints exist in Chrome on Android but iOS support is disputed and must be feature-detected [12][13][14]; smartphone JPEG/video output is non-linear (RAW needed for rigorous work) [15][16]. Absorbance as a sample/reference ratio at one wavelength (the DBS method) partly cancels this [2].

**Biggest caveat:** costs, licenses and file availability come from 2021–2022 publications and were not re-checked against current OSF contents beyond the file listing; browser camera-control support is the fastest-ageing claim and must be tested on real devices before architecture is fixed.

## 1. Existing designs (select: base design)

**Frame (inferred from the request, logged as an assumption):** hard gates — printable on consumer FDM, open license allowing derivatives, smartphone camera, visible range. Preferences — modularity, documentation completeness, quantitative capability.

| Candidate | License | Modularity | Docs | Quantitative evidence | Verdict |
|---|---|---|---|---|---|
| Winters DBS (HardwareX 2021) | CC BY-SA 4.0 hardware; CC BY 4.0 article [1] | Explicit: 25 parts, swappable camera mounts (iPhone SE–X, centre-camera phones, USB cam, blank plate for any case) [1][3] | Step-by-step build, BOM, STEP/STL, Onshape [1][2][3] | CuSO₄ Beer's-law series comparable to PerkinElmer Lambda XLS [2] | **Recommended base** |
| Smith Lab SpecPhone / DualSpec (J Chem Educ 2016/2019) | Files on Thingiverse; license not confirmed this run [4] | Removable slits 0.1–5 mm, multiplatform phone adaptor [4] | Papers + project page [4] | Dual beam to improve S/N [4] | Strong alternative / second preset |
| Bruininks & Juurlink periscope (J Chem Educ 2022) | Not confirmed [5] | 3 parts + optional sample/LED module; mounts on the phone's own back cover so calibration survives [5] | Paper [5] | Emission/absorption/fluorescence, < $40 [5] | Good "compact" idea (back-cover mount) |
| Cokley et al. modular absorption + fluorimeter (J Chem Educ 2024) | Not confirmed [7] | Modular, absorption ↔ fluorescence by orientation [7] | Paywalled [7] | Not assessed (medium confidence) | Watch |
| Public Lab foldable/desktop | CERN OHL [21] | Low | Wiki, community | "Not meant for quantitative research" [21] | Beginner "explore" track only |
| Printables community (e.g. Tiny Pocket Spectrometer) | Varies | Usually single-phone | Minimal | Self-described "toy" [23] | Inspiration only |

Licensing note: CC BY-SA on DBS means derived STL files the site distributes (e.g. new phone mounts) must stay CC BY-SA with attribution [1].

## 2. Optics and 3D printing constraints

- **Grating.** DBS slides a film grating into the grating mount with grooves horizontal [2]; Smith Lab uses 1000 lines/mm film (d = 1000 nm) [4]. A DVD (0.74 µm pitch ≈ 1350 lines/mm) out-resolves a CD and can separate the Hg 577/579 nm doublet [6]; the periscope design uses a DVD wedge [5]. Guide implication: offer "film grating (recommended)" and "DVD fallback" as a BOM choice, with the grating equation \( m\lambda = d(\sin\alpha + \sin\beta) \) shown as background only.
- **Slit.** DBS builds an adjustable slit from two utility-knife blades in printed wedges (DBS22/DBS23) [2]; Smith Lab uses removable printed slits 0.1–5 mm [4]; razor slits can go below 0.05 mm [25]. Narrower slit → better resolution, less light (trade-off the guide should explain interactively).
- **Material and light leaks.** Black PLA, PETG, ABS and PA had ~0% transmittance at 1 mm; black PC and TPU transmitted in IR; light colours transmit substantially [8]. Some black PLA brands leaked at 689/828 nm in a probe setup [9]. Rule for the guide: black PLA/PETG, walls ≥ 1–2 mm, no light colours for optical-path parts, plus a dark-frame leak test.
- **Printer tolerances.** DBS: entry-level MakerBot, PLA only, 20% infill (raise for rigidity); remove burrs/raft roughness; fits vary by printer, so test-print small mating parts first; binding sliders → reprint in another orientation or sand [2].
- **Phone fit.** DBS covers iPhone SE–X with a dedicated mount, centre-camera phones with another, and everything else via a blank plate glued to the user's own phone case [1]. The periscope design attaches to the phone's back cover for repeatable alignment [5]. Neither covers current multi-camera phones explicitly — a per-phone mount generator or adapter library is a real gap.

## 3. Calibration and measurement

- **Wavelength calibration:** CFL/fluorescent tube lines — 436, 546, 611 nm [10]; GoSpectro uses four (436, 488, 546, 611 nm) [11]. Both require a new calibration each time the spectroscope is re-attached, and no zooming [10][11]. DBS used two-point linear calibration with LEDs, laser pointers or CFL lines, and published a reference-spectra table for field calibration [2].
- **Absorbance:** DBS computed \( A = -\log_{10}(P/P_0) \) from sample vs reference peak intensity at one wavelength (800 nm) on a 200-px-tall image region, with ImageJ "plot profile" [2]. An 11-point CuSO₄·5H₂O dilution series gave results "remarkably similar" to a PerkinElmer Lambda XLS once each instrument was restricted to its linear range [2]. This is the template for the site's "first quantitative experiment".
- **Camera pipeline limits:** rendered JPEG/RGB is non-linear (demosaicing, white balance, gamma, compression) and often unrecoverable; RAW/"Pro mode" is needed for rigorous spectral work [15][16]. Lab characterisation used DNG at ISO 100 with fixed settings [15].
- **In-browser control:** the spec exposes `exposureMode`, `exposureTime`, `whiteBalanceMode`, `colorTemperature`, `iso`, `focusMode`, `torch` [12]. The W3C status lists Chrome (incl. Android) as implementing them and iOS as unsupported (notes dated Feb 2023) [13]; WebKit merged `whiteBalanceMode` support in Sept 2023 [14]. **Status: disputed / unknown per Safari version** — the app must feature-detect with `getCapabilities()` and degrade to "lock in the native camera app, then upload a photo".
- **Existing software:** Spectral Workbench (Public Lab) website is offline for lack of funds [26]; its `spectral-workbench.js` library is GPL-3.0 [17]. Browser tools already do live capture + calibration + CSV export entirely client-side: spectro-web (WIP) [23], WebSpectrometer (two-point calibration, ROI, CSV) [24], Digital-Spectroscope (printable model + web capture) [25]. These prove feasibility; GPL/LGPL licensing affects whether code can be reused vs reimplemented.

## 4. Web tooling

- **3D preview:** React Three Fiber loads STL via `STLLoader` with `useLoader`, returning a `BufferGeometry` for a mesh [18] — low effort, suitable for per-part viewers and exploded assembly views.
- **Parametric generation in the browser:** options are OpenSCAD compiled to WASM (React component with customizer and STL/3MF export; ~10 MB payload, unverified figure) [19], Manifold (WASM, guaranteed-manifold meshes, GLB/3MF) [20], and replicad (OpenCascade, STEP export, fillets) [22]. DBS's source CAD is Onshape/STEP [1], so it is not directly parametric in any of these — parametric phone mounts would have to be re-modelled.
- **Implication:** v1 should ship pre-generated STLs from the DBS repository with a 3D viewer; a parametric "phone mount generator" (OpenSCAD-WASM or replicad) is a v2 feature with re-modelling cost.

## 5. Glossary and safety

Evidence gathered this run is thin; treated as an open dimension. What sources show: DBS calibration and fluorescence work uses laser pointers and flashlights [2]; the periscope module includes a UV LED [5]; the DBS validation uses copper sulfate solutions up to 1.2 M [2]. The guide therefore needs at minimum laser/UV eye-safety and chemical-handling notices; specific limits and wording were not researched (see Open questions). Glossary seeds from sources: slit, diffraction grating (lines/mm), dispersion, double beam (sample/reference), cuvette / path length, absorbance, Beer–Lambert law, wavelength calibration, linear dynamic range, ROI, RAW vs JPEG, white balance, exposure.

## Cross-dimension insights

- **The physical calibration rule drives the UX.** Because calibration is lost whenever the phone is re-attached [10][11] and browsers may not lock exposure/white balance [13][14], the site's measure flow must start with "calibrate now" every session, not as a one-time setup step.
- **Modularity and licensing reinforce each other.** DBS's swappable mounts [1] plus CC BY-SA [1] mean the site can legally host community-contributed phone mounts — the biggest coverage gap — as long as they remain share-alike.
- **Double beam partly compensates for weak browser camera control.** Ratioing sample vs reference in the same frame [2] reduces dependence on absolute exposure, which matters given the JPEG non-linearity [15][16].

## Contrary evidence

- iOS camera control: W3C status says unsupported [13], but WebKit has merged at least `whiteBalanceMode` [14]. Neither confirms current shipping behaviour.
- DBS quantitative results used images processed in ImageJ, not a browser pipeline [2]; whether a browser JPEG/video stream reproduces them is untested.
- Light-tightness: lab measurements say black PLA is opaque at 1 mm [8], while one study found brand-specific NIR leakage [9] — material advice must stay brand-agnostic and test-based.

## Recommendations

| # | Recommendation | Feeds | Confidence basis |
|---|---|---|---|
| R1 | Adopt Winters DBS as the reference build; keep DualSpec as a documented alternative preset. | Brief (scope), PRD | High — verified license, files, validation [1][2][3]; DualSpec license not confirmed [4]. |
| R2 | Structure the guide as: choose build → BOM → test-print fit → print all → assemble (slit, grating, mount) → light-leak check → calibrate (CFL) → measure (Beer's law CuSO₄ exercise). | Brief, UX | High for steps from [2][10][11]. |
| R3 | v1: pre-generated STL downloads + R3F viewer; v2: parametric phone-mount generator. | Architecture | High for viewer [18]; generator libraries unverified on payload/perf [19][20][22]. |
| R4 | Browser capture with feature detection of camera controls; fallback to photo upload; label results "qualitative" unless the double-beam ratio method is used. | Architecture, PRD (NFR) | Rests on a **disputed** compat claim [13][14] — validate on devices first. |
| R5 | Mandatory recalibration at the start of each measurement session. | UX, PRD | High — two independent sources [10][11]. |
| R6 | Host derived parts under CC BY-SA with attribution to Winters et al.; check GPL/LGPL before reusing any existing web spectrometer code. | PRD (legal), Architecture | High [1][17][25]. |

## Open questions

1. Current iOS Safari support for `exposureMode` / `whiteBalanceMode` / `exposureTime` — answer with a small device test page on iPhone + Android.
2. Does the OSF repository still contain all STL files and the BOM, and what do parts cost today? — enumerate OSF pages 3+ and re-price the BOM.
3. DualSpec / periscope design licenses — check the Thingiverse/Printables listings.
4. Safety wording for laser pointers, UV LEDs and copper sulfate in schools — targeted domain research or an existing school-lab safety reference.
5. Which current phones (multi-camera bumps) fit the DBS blank-plate approach — community testing or a mount generator.
6. Can a browser JPEG/video pipeline reproduce the DBS Beer's-law linearity? — prototype spike.

## Source appendix

| [n] | Supports | Publisher | Pub date | Accessed | Confidence |
|---|---|---|---|---|---|
| [1] | DBS design, license, parts, costs, mounts | [HardwareX (Elsevier) — Winters et al.](https://www.hardware-x.com/article/S2468-0672(21)00061-4/fulltext) | 2021-10 | 2026-11-05 | high |
| [2] | DBS build instructions, print settings, calibration, Beer's law validation | [Europe PMC — PMC9123456 full text](https://europepmc.org/article/PMC/PMC9123456) | 2022-05 | 2026-11-05 | high |
| [3] | DBS STEP files and BOM listing | [OSF registration 7ewb3](https://osf.io/7ewb3/) | 2021-08 | 2026-11-05 | high |
| [4] | SpecPhone/DualSpec cuvettes, slits, grating | [Smith Lab, University of Akron](https://www.adamsmithlab.org/specphone) | 2019-06 | 2026-11-05 | high |
| [5] | Periscope DVD design, back-cover mount, UV LED module | [J. Chem. Educ. (ACS) — Bruininks & Juurlink](https://pubs.acs.org/jceda8/article/99/5/2168/801097/An-Inexpensive-3D-Printed-Periscope-Type) | 2022-05 | 2026-11-05 | high |
| [6] | DVD vs CD grating resolution | [J. Chem. Educ. (ACS) — Wakabayashi & Hamada](https://doi.org/10.1021/ed083p56) | 2006-01 | 2026-11-05 | high |
| [7] | Modular absorption + fluorimeter | [J. Chem. Educ. (ACS) — Cokley et al.](https://pubs.acs.org/doi/10.1021/acs.jchemed.3c01021) | 2024-06 | 2026-11-05 | medium |
| [8] | Filament optical transmittance | [NSF Public Access Repository — Optical transmittance of 3D printing materials](https://par.nsf.gov/biblio/10298258) | 2021-01 | 2026-11-05 | high |
| [9] | Black PLA NIR leakage | [PLOS One](https://doi.org/10.1371/journal.pone.0253181) | 2021-06 | 2026-11-05 | high |
| [10] | CFL calibration lines, recalibrate on re-attach | [Royal Society of Chemistry — Education](https://edu.rsc.org/download?ac=15350) | n.d. | 2026-11-05 | medium |
| [11] | 4-line CFL calibration, recalibrate after removal | [Goyalab — GoSpectro Getting Started](https://www.goyalab.com/wp-content/uploads/2018/10/gospectro-getting-started-vus_20181205.pdf) | 2018-12 | 2026-11-05 | high |
| [12] | Image-track constraints list | [MDN Web Docs — MediaTrackConstraints](https://developer.mozilla.org/en-US/docs/Web/API/MediaTrackConstraints) | 2025 | 2026-11-05 | high |
| [13] | Implementation status Chrome vs iOS | [W3C mediacapture-image — implementation-status.md](https://github.com/w3c/mediacapture-image/blob/main/implementation-status.md) | 2023-02 | 2026-11-05 | medium |
| [14] | WebKit whiteBalanceMode support | [WebKit — PR #17814](https://github.com/WebKit/WebKit/pull/17814) | 2023-09 | 2026-11-05 | medium |
| [15] | Smartphone camera characterisation with DNG, ISO 100 | [MDPI Sensors — Tominaga et al.](https://www.mdpi.com/1424-8220/21/15/4985) | 2021-07 | 2026-11-05 | high |
| [16] | JPEG non-linear; RAW required | [PMC — Compressive recovery of smartphone RGB spectral sensitivity](https://pmc.ncbi.nlm.nih.gov/articles/PMC8237928/) | 2021-06 | 2026-11-05 | high |
| [17] | spectral-workbench.js GPL-3.0 | [Public Lab — GitHub](https://github.com/publiclab/spectral-workbench.js) | 2026-11 | 2026-11-05 | high |
| [18] | R3F STLLoader usage | [pmndrs — React Three Fiber docs](https://r3f.docs.pmnd.rs/tutorials/loading-models) | 2026 | 2026-11-05 | high |
| [19] | OpenSCAD WASM React component | [Reiss-Cashmore — openscad-component](https://github.com/Reiss-Cashmore/openscad-component) | 2025 | 2026-11-05 | low |
| [20] | Manifold WASM geometry | [ManifoldCAD docs](https://manifoldcad.org/docs/jsuser/) | 2025 | 2026-11-05 | medium |
| [21] | Public Lab foldable: CERN OHL, not quantitative | [Public Lab wiki — foldable spectrometer](https://publiclab.org/wiki/revisions/foldable-spec) | 2017-11 | 2026-11-05 | high |
| [22] | replicad browser CAD, STEP export | [replicad](https://replicad.xyz/) | 2025 | 2026-11-05 | medium |
| [23] | spectro-web in-browser tool; Tiny Pocket "toy" | [gheja — spectro-web](https://gheja.github.io/spectro-web/src/) | 2025-01 | 2026-11-05 | medium |
| [24] | WebSpectrometer calibration + CSV | [photon-cat — WebSpectrometer](https://github.com/photon-cat/webspectrometer) | 2024 | 2026-11-05 | medium |
| [25] | Digital-Spectroscope: razor slit < 0.05 mm, web capture | [joshbrew — Digital-Spectroscope](https://github.com/joshbrew/Digital-Spectroscope) | 2024 | 2026-11-05 | medium |
| [26] | Spectral Workbench site offline | [spectralworkbench.org](https://spectralworkbench.org/) | 2026-11 | 2026-11-05 | high |

## Staleness map

Computed with `recon_kit.py staleness` (windows: compat 1 mo, ecosystem 6 mo, cost/license/design 12 mo, patterns 24 mo).

| Claim | Class | Published | Re-check by | Stale |
|---|---|---|---|---|
| [13] Image Capture controls Chrome Android, not iOS | compat | 2023-02 | 2023-03-01 | yes |
| [14] WebKit whiteBalanceMode merged | compat | 2023-09 | 2023-10-01 | yes |
| [19] openscad-playground WASM ~10 MB | compat | 2026-11 | 2026-12-01 | no |
| [1] DBS costs $25.53 / $175.73 / $64.26 | cost | 2021-10 | 2022-10-01 | yes |
| [1] DBS hardware CC BY-SA 4.0 | license | 2021-10 | 2022-10-01 | yes |
| [3] DBS STEP + BOM on OSF | design | 2021-10 | 2022-10-01 | yes |
| [21] Public Lab CERN OHL, not quantitative | license | 2017-11 | 2018-11-01 | yes |
| [26] Spectral Workbench offline | ecosystem | 2026-11 | 2027-05-01 | no |
| [2] DBS Beer's law vs PerkinElmer | patterns | 2022-05 | 2024-05-01 | yes |
| [8] black PLA/PETG opaque at 1 mm | patterns | 2021-01 | 2023-01-01 | yes |
| [11] CFL lines 436/488/546/611 nm | patterns | 2018-12 | 2020-12-01 | yes |
| [6] DVD resolves Hg 577/579 nm | patterns | 2006-01 | 2008-01-01 | yes |

The physics claims ([6], [11] Hg emission lines, [8]) do not age in practice; the tool flags them only by publication date. **Act-on items before architecture:** the two compat claims [13][14] (stale now) and the OSF file/BOM/cost claims [1][3]. The earliest computed re-check date is 2008-01-01 ([6]), which is a date artifact; the earliest meaningful one is [13] (2023-03-01, already overdue).
