> [!IMPORTANT]
> **Welcome to the APPN Standard Operating Procedures (SOPs) Wiki.**
>
> This space provides the authoritative, version-controlled reference for all
> APPN-approved operating procedures related to UAV-based and ground-based
> plant phenomics, with a primary focus on field data acquisition, calibration,
> quality assurance, and reproducibility. These SOPs are designed to support
> consistent, transparent, and comparable phenotyping outcomes across APPN
> nodes and partner organisations.

## 🌿 Purpose of this Wiki

The SOPs hosted here aim to:

- Ensure consistent application of best practice across all standard APPN
  flights and field campaigns
- Provide transparent documentation of methodologies used in data acquisition
  and processing
- Enable cross-node comparability of UAV- and ground-derived datasets
- Support error reporting, uncertainty quantification, and quality assurance
- Facilitate shared learning and continuous improvement across the network

All procedures published in this wiki underpin APPN’s commitment to rigorous
standards, open science, and FAIR data principles.

## 🌿 Scope

These SOPs apply to:

- Standard APPN UAV operations, including calibration and experimental flights
- APPN-supported projects undertaken by internal and external researchers
- Training and onboarding of APPN technical staff, students, and collaborators
- Where flights or workflows fall outside standard operating procedures, this
  wiki defines the minimum documentation and reporting requirements expected
  to ensure transparency and traceability

## 🌿 What These SOPs Cover

The SOPs in this wiki collectively cover the full end-to-end lifecycle of
APPN field phenotyping activities, including:

- Planning and preparation for APPN-supported UAV operations, including
  experimental intent, site readiness, and pre-flight considerations
- UAV flight execution, covering standard operating conditions, deviations
  from standard practice, and required metadata capture
- Calibration and validation activities, including the use of reference
  materials, alignment approaches, and reporting of calibration outcomes
- Sensor operation and handling, encompassing configuration, warm-up
  procedures, operational constraints, and known limitations
- Experimental design considerations relevant to UAV phenotyping, such as
  plot layout, timing, and integration with complementary measurements
- Data processing and pipeline use, including approved APPN workflows,
  versioning expectations, and traceability requirements
- Quality assurance, error metrics, and reporting, defining minimum checks,
  uncertainty considerations, and standards for data delivery

Individual SOPs may span more than one of these areas, reflecting the
integrated nature of field phenomics workflows.

## 🌿 Governance & Versioning

- Each SOP is versioned and timestamped
- Material is reviewed and updated through APPN Field EWG-led governance
  processes
- Superseded procedures will be archived and remain accessible for transparency
- The most recent version of each SOP should always be used for standard APPN
  flights

The SOPs are versioned as a **single set**. One version applies to the whole
protocol suite rather than to individual pages, written `MAJOR.YYx` — for
example `1.00`, `1.00a`, `1.01`. The current value is stamped on every
published wiki page and PDF.

- **MAJOR** and **MINOR** are **numbers**. The minor is always written as two
  digits with a leading zero (`YY`): `00`, `01`, …, `10`.
- **PATCH** is a **letter** appended directly to the minor with no separator
  (`a`, `b`, `c`, …). No letter means no patch (e.g. `1.01` is the base of that
  minor; `1.01a` is its first patch).

| Part | When it changes |
| ---- | --------------- |
| **MAJOR** | Once per year, **locked at the start of the season**, establishing the stable, citable baseline field teams follow for that season. Bumping the major resets the minor to `00` and drops any patch letter. |
| **MINOR** | An in-season change that alters **what you do** or **adds new scope** — finishing a stub, adding a page, resolving a provisional parameter, or correcting a procedure found to be wrong. |
| **PATCH** | An editorial change that does **not** alter the procedure — spelling, formatting, clarifying wording, or replacing an illustrative figure. Safe to adopt without re-checking already-collected data. |

> [!IMPORTANT]
> Every **minor** release is announced at a Field EWG meeting so all nodes are
> aware of the change in scope or procedure, and is documented on this Home
> page, in the repository README, and in a note at the top of each impacted
> page. Patch releases do not require an announcement.

## 🌿 Contribution & Feedback

APPN strongly encourages:

- Openness about challenges and limitations — please use the issues and
  discussion tabs on the main GitHub repository
- Constructive feedback from node staff and collaborators
- Shared learning to improve protocols over time

## 🌿 How to Cite

If you use these SOPs, please cite the revision you followed. The author list
and release metadata are maintained in
[`CITATION.cff`](https://github.com/aus-plant-phenomics-network/APPN-Field-Protocols-and-Pipelines/blob/main/CITATION.cff);
use **Cite this repository** on the
[main repository](https://github.com/aus-plant-phenomics-network/APPN-Field-Protocols-and-Pipelines)
page to copy the citation as APA or BibTeX.

## 🌿 Licensing

- Protocols and documentation are released under Creative Commons Attribution
  4.0 (CC BY 4.0)
- Code and pipelines referenced within SOPs are released under the MIT
  License, unless otherwise stated

_These SOPs underpin APPN’s collective commitment to high-quality,
reproducible, and trusted plant phenomics data._
