# APPN Aerial SOP — Revision 1.0 Release Checklist

This document tracks the **remaining work to cut revision 1.0**. All
document-level content TODOs have either been completed or moved to the
[future-release backlog](TODO_FUTURE.md) (Rev 1.1+). What remains below are
the release-gate actions, not document content.

Document status is tracked in
[Protocols/STATUS.md](Protocols/STATUS.md) (auto-generated from
[publish.yaml](publish.yaml)). The image / figure backlog is tracked
separately in [IMAGE_TODO.md](IMAGE_TODO.md).

---

## 1.0 release gates

- [ ] **Confirm the Approved → Adopted transition.** The Approved
  documents have had all their in-scope content TODOs cleared. Mark each as
  `adopted` in [publish.yaml](publish.yaml) when the locked revision is cut.
  - [CALViS Fieldbook](Protocols/Sensors/CALVIS/CALViS_FieldBook.md)
  - [GOBI M350 Fieldbook](Protocols/Sensors/GOBI/GOBI_M350_FieldBook.md)
  - [GOBI IF1200 Fieldbook](Protocols/Sensors/GOBI/GOBI_IF1200_FieldBook.md)
  - [HiRes Fieldbook](Protocols/Sensors/HIRES/HIRES_FieldBook.md)
  - [Standard Flight Procedure](Protocols/FlightDesign/StandardFlight/Standard_Flight.md)
  - [QA Process](Protocols/QA/QAprocess/AerialDataQC.md)
  - [Plot Delineation](Protocols/PlotProtocols/PlotDelineation/Plot_Delineation.md)
  - [Processing Pipelines](Protocols/Pipelines/GryfnProcessingPipeline/Gryfn_Processing_Pipeline.md)
  - [Data Folder Structure](Protocols/DataManagement/DataFolderStructure/DataFolderStructure.md)
    _(already Adopted)_

- [ ] **Confirm 1.0 disposition for the in-progress / draft documents.** These
  ship in 1.0 as clearly-marked drafts or placeholders; their content work is
  deferred to [TODO_FUTURE.md](TODO_FUTURE.md):
  - [Validation Flight](Protocols/FlightDesign/ValidationFlight/Validation_Flight.md)
    — Modified; carries a "being restructured" caution.
  - [Platforms Overview](Protocols/Background/PlatformsOverview/Platforms_Overview.md)
    — Drafted (background, no approval gate).
  - Stubs (ship as "coming soon" placeholders): M3M Fieldbook, Spectral Panel
    Cleaning & Calibration, M3M Processing Pipeline, HiRes Processing Pipeline,
    Ground-Based Phenotyping & Environmental.

- [ ] **Run the publish + PDF render** for the cut:
  ```
  python Scripts/publish_to_wiki.py
  python Scripts/publish_to_wiki.py --pdf
  ```

- [ ] **Verify cross-links and figures** resolve in the published wiki copy
  (the figure backlog in [IMAGE_TODO.md](IMAGE_TODO.md) is expected to remain
  open for 1.0).

---

## Open decisions — GOBI overlap review (Aug 2026)

Raised in the GOBI overlap/sidelap review (Warin / Lleyton / Connor email
thread) and Lleyton's follow-up. These need a call before the revised GOBI
overlap guidance is locked into a release:

- [ ] **30 m (Type 1) VNIR oversampling — 20% or 30%?** Type 1 currently
  uses **20%** oversampling while Types 2–3 use **30%**. Lleyton queried the
  inconsistency and leans toward standardising on **30% everywhere** (at 30%
  the VNIR frame rate is still only ~215 Hz vs the 250 Hz sensor max; the
  calculator's flag is a warning, not a hard limit). Changing this updates
  the Type 1 `Frame Period (Hz)` cell (and possibly line spacing) in the
  Standard Mission Parameters tables of **both** GOBI fieldbooks — recompute
  the frame period in the GRYFN calculator / Gryfn WebUI.

---

## What was cleared for 1.0

The following in-scope content items were completed during release prep:

- **Standard Flight** — added the cross-link to the Processing Pipelines
  reflectance-target section; converted the inline APEx markers and status
  banner to clean backlog references.
- **Aerial Data QC** — wrote the **Accuracy reporting** section (report
  contents, interpretation, storage) with pass/fail thresholds left as a
  clearly-marked EWG placeholder; cleaned the GCP-conversion and Vector-Layer
  TODO markers.
- **Processing Pipelines** — converted the status banner and GOBI walkthrough
  TODO to backlog references.
- **Plot Delineation** — confirmed **Method 3 (GPT plot creation tool)** is
  fully written; removed the stale "to be written by Mickey" markers.
- **HiRes Fieldbook** — moved the stubbed Gimbal Balancing steps to the
  backlog with a clean placeholder note.

---

## Where everything else went

Everything that could not be completed for 1.0 — Field EWG approvals, APEx
parameter sweeps, operational-parameter values, field photos/diagrams,
sections owned by named contributors, and the five stub documents — is now
tracked in **[TODO_FUTURE.md](TODO_FUTURE.md)** (Rev 1.1+).
