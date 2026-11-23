# Changelog

All notable changes to the APPN Aerial Standard Operating Procedures are
recorded here. The SOPs are versioned as a **single set** using the
`MAJOR.YYx` scheme described in the
[repository README](https://github.com/aus-plant-phenomics-network/APPN-Field-Protocols-and-Pipelines#versioning):

- **MAJOR** — annual baseline, locked at the start of the season.
- **MINOR** (`.YY`) — an in-season change to **what you do** or **new scope**.
- **PATCH** (`x`, a letter) — an editorial-only change with no procedural impact.

Each released revision below corresponds to a
[GitHub release](https://github.com/aus-plant-phenomics-network/APPN-Field-Protocols-and-Pipelines/releases)
tagged `vMAJOR.YYx`. Every **minor** release is also announced at a Field EWG
meeting.

## [1.04] — 2026-11-25

In-season procedure correction raising the CALViS SWIR side-overlap floor
to ≥ 40% and tightening Standard Mission line spacing, resolving issue
[#12](https://github.com/aus-plant-phenomics-network/APPN-Field-Protocols-and-Pipelines/issues/12).
Also the first revision minted with a DOI, with the author list and
affiliations updated for registry (ROR) matching.

### Changed

- **CALViS Fieldbook** — side overlap is now planned at **≥ 40% on the
  SWIR** (previously > 30%), giving ~50% or more on the VNIR (#12). In
  October 2026 a Type 2 flight at the old 13 m spacing (~32% SWIR side
  overlap) produced unrecoverable no-data holes between flight lines,
  traced by GRYFN to gust-induced roll. Standard Mission line spacing is
  tightened accordingly (Type 1: 8 → 6 m, Type 2: 13 → 11 m, Type 3:
  21 → 18 m), and the Standard Mission Parameters table now lists the
  resulting SWIR/VNIR side overlap and GSD.
- **Citation** — added Bryn Collyer and Neelesh Sharma as authors, added
  missing ORCID iDs, and simplified affiliations to organisations that
  resolve in the ROR registry used for DOI metadata.

## [1.03] — 2026-10-19

Adds a formal citation for the protocol suite, crediting the 22 contributors
who approved authorship, and updates the spectral QC extraction workflow to
the current QA scripts.

### Added

- **Citation** — the repository now ships a `CITATION.cff` (Citation File
  Format) listing the authors, version and release date of the suite, with
  contributions recorded using the CRediT taxonomy. GitHub's **Cite this
  repository** button exports it as APA or BibTeX.
- **Home** — new *How to Cite* section pointing to the citation.

### Changed

- **QA Process** — the pixel-extraction script is now
  `QA00_SpectralValidation.py` (per-run extraction, QC report and spectra
  figure), with extracted tables written to `QC_data/QC_Spectral_Tables/`
  under a documented `QC_{ELM|VAL}[_{TargetIdentifier}]_spectra_{VNIR|SWIR}…`
  naming pattern. The `type` column of the extraction table is replaced by
  `EM_Region` (`VNIR`/`SWIR`), and the extra columns recorded by QA00
  (`target_type`, `target_id`, `panel_set`, `Valid_Range`) are documented.
  Added a *Comparing Runs (multi-run QC)* section for the new
  `QA02_SpectralRunComparison.py` cross-run comparison script, including
  date filtering and sharing tables between nodes (`--save-dir` /
  `--load-dir`). Fixed a merged heading that hid the *Extracting Pixels into
  a Table* section.

## [1.02] — 2026-08-13

In-season procedure corrections making GOBI side-overlap planning
VNIR-driven, resolving issue
[#11](https://github.com/aus-plant-phenomics-network/APPN-Field-Protocols-and-Pipelines/issues/11),
plus a rework of the QC panel naming convention for validation panels and
dual-ELM flights.

### Changed

- **GOBI M350 Fieldbook** — side overlap is now driven by the VNIR
  (hyperspectral) data-quality requirement rather than the RGB camera
  alone (#11). Hyperspectral missions (Types 1–3) are planned at **80% RGB
  side overlap** (≈ 37% VNIR sidelap — the practical minimum), with ≥ 40%
  VNIR preferred and ≥ 50% where two-line redundancy is required; RGB-only
  Type 4 missions may stay at 75%. Added notes on buffering the capture
  polygon perpendicular to the flight direction (more when lines cross the
  boundary at an angle), on frame-period differences between the GRYFN
  Flight Calculator and the WebUI (negligible; calculator values used), and
  on why Type 1 (30 m) uses 20% oversampling instead of the 30% default
  (30% would push the VNIR frame rate too close to the sensor maximum).
  Clarified that frame period (along-track sampling) is separate from side
  overlap.
- **GOBI IF1200 Fieldbook** — same VNIR-driven overlap rework as the M350
  book (#11): flight-line spacings are now derived from 80% RGB side
  overlap (Type 1: 9 m, Type 2: 15 m, Type 3: 24 m), aligning the IF1200
  with the M350 recommendations. Same capture-polygon buffering,
  calculator-vs-WebUI, and Type 1 20%-oversampling notes. Added a warning
  that the IF1200 flies without RTK (± 1 m positional error), which erodes
  effective overlap at low altitude — favour extra overlap or tighter
  spacing on low-altitude flights. Added a QA note that missing VNIR edge
  pixels can come from restrictive KML boundaries or processing extents —
  process with no fixed extent where practical.
- **QA Process** — QC vector naming convention reworked around the
  documented `QC_{TargetType}_{TargetIdentifier}_{Descriptor}.geojson`
  pattern, with target identifiers now optional and `groundtruth` reserved
  for the surveyed GCP reference layer. `QC_VAL_Panels.geojson` is now
  reserved for the GRYFN dedicated 2-panel validation set; other
  non-ELM panel sets use `QC_VAL_{PanelName}_Panels.geojson` (e.g.
  `QC_VAL_Grfyn4P_Panels.geojson`). Dual-ELM flights name each 4-panel set
  with a target identifier (e.g. `QC_ELM_blue_Panels.geojson` /
  `QC_ELM_yellow_Panels.geojson`); per-product GCP layers
  (`QC_GCP_{Product}_points.geojson`) are strongly recommended when data
  products do not co-align. Added a worked naming example for the most
  common field setup (two GRYFN 4-panel sets, the 2-panel validation set,
  and GCPs).

## [1.01] — 2026-07-23

In-season procedure corrections to the flight-planning overlap guidance,
resolving issues [#7](https://github.com/aus-plant-phenomics-network/APPN-Field-Protocols-and-Pipelines/issues/7),
[#8](https://github.com/aus-plant-phenomics-network/APPN-Field-Protocols-and-Pipelines/issues/8) and
[#9](https://github.com/aus-plant-phenomics-network/APPN-Field-Protocols-and-Pipelines/issues/9),
plus field-experience edits from the early-season deployments.

### Changed

- **GOBI M350 Fieldbook** — side overlap is now unambiguously planned from
  the RGB camera (75% default, 80% in wind > 5 m/s), with an explanatory
  note under Standard Mission Parameters covering the GRYFN 70/70-minimum
  recommendation and why RGB percentages must not be applied to the VNIR
  sensor (#7). Added links to the GRYFN custom camera settings guide and a
  requirement to use the latest GRYFN Flight Calculator. WebUI
  altitude-normalisation time corrected to ~30–60 s from power-on.
- **GOBI IF1200 Fieldbook** — removed the incorrect SWIR side-overlap
  reference (the GOBI has no SWIR sensor); flight-line spacing is now
  stated as derived from 75% RGB side overlap, with the same explanatory
  note as the M350 book (#7, #8). Added the latest-Flight-Calculator
  requirement. WebUI altitude-normalisation time corrected to ~30–60 s.
- **CALViS Fieldbook** — side-overlap guidance corrected to > 30% for the
  SWIR sensor (> 40% for the VNIR), with a note explaining that the SWIR's
  narrower swath makes it the limiting sensor for flight-line spacing (#9).
  Added the latest-Flight-Calculator requirement. Solar-noon check link
  updated to suncalc.org; per-sensor 95%-saturation count targets split for
  SWIR (≈ 61,750) and VNIR (≈ 3,890); dynamic-alignment speed corrected to
  6 m/s.
- **Processing Pipelines** — RGB orthomosaic resolution is now set from the
  GRYFN Flight Calculator (previous fixed value was a placeholder); added
  notes that the ELM panel-selection Y-axis shows radiance units, not raw
  counts.
- **Standard Data Products** — RGB orthomosaic typical resolution now
  references the GRYFN Flight Calculator instead of a fixed 0.6 cm.

## [1.00] — 2026-06-01

First locked revision of the suite.

### Adopted

- GOBI M350 Fieldbook
- GOBI IF1200 Fieldbook
- HiRes Fieldbook
- CALViS Fieldbook
- Standard Flight Procedure
- Data Folder Structure
- Processing Pipelines
- QA Process — *positional thresholds and the spectral/LiDAR accuracy
  baselines still to be developed against APEx data*
- Plot Delineation — *the three delineation methods still being
  cross-validated*

### In progress

- Validation Flight — being restructured as part of early-season APEx work.
- Stubs being drafted: M3M Fieldbook, Spectral Panel Cleaning and Calibration,
  M3M Processing Pipeline, HiRes Processing Pipeline.
