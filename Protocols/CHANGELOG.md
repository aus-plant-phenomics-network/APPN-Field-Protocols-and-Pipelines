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
