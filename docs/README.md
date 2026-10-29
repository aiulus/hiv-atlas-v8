# HIV blood atlas — Cellucid viewer (public, de-identified)

Interactive single-cell viewer data for the amfAR HIV Immune Atlas (Theis lab, Helmholtz Munich),
as Cellucid-ready AnnData Zarr stores (Cellucid >= 0.9.5; https://docs.cellucid.com). Open it in the browser:

    https://www.cellucid.com/?data=<URL-encoded address of this folder, ending in />&dataset=<dataset id>

The datasets in this catalog are listed in `datasets.json` (schema 2); each `<id>.zarr/` is an
ordinary AnnData that `anndata.read_zarr` also opens.

## What is in it

- Cells from **public deposits only**: Frouard et al. 2026 (Nat Commun), Geretz et al. 2023
  (Sci Transl Med), Weymar et al. 2022 (Cell Rep), Peterson et al. 2026 (J Clin Invest, blood
  libraries); from v7 on also Wei et al. 2023 and Zhang et al. 2023; v8 adds Peterson's colon libraries.
- UMAP of the scVI integration (batch key = sequencing library), 2D and 3D, and the scVI latent space.
- Expression: log1p(counts per 10k) for the 3,000 highly variable genes plus a standard immune
  marker panel (not the full transcriptome), rounded to 7 float32 mantissa bits (at most 0.39%
  relative error, about 4x finer than the earlier 8-bit export) so each build fits GitHub Pages; marked as log1p
  (`uns['log1p']`) so the viewer's differential expression reports fold changes correctly.
- Per-cell fields, v6 and v7: study, cohort, chemistry, sorted population (`cell_scope`), ex-vivo
  treatment, study state (viremic / suppressed / uninfected), infection stage, timepoint group, HIV
  RNA status, cell-type labels (`ann_level_1/2`, CellTypist), QC and doublet flags, depth / mito /
  mixing scores; from v7 on, Leiden clusters (`leiden`).
- Per-cell fields, v8 (names and levels agreed with the source studies' authors, 2026-10-29):
  `Dataset`, `Donor`, `Individual_10X_well`, `Chemistry`, `Compartment` (Blood / Colon),
  `Cell_input_type` (Whole PBMCs / CD4 T cells / Memory CD4 T cells / Whole colon cells),
  `Ex_vivo_treatment` (Unstimulated / PMA_iono_IL2), `Primer_condition`, `HIV_status`
  (Viremic / Suppressed / Uninfected), `Treatment_initiation_stage` (Viremic / Acute-treated /
  Chronic-treated / Unknown / Uninfected), `Sex`, `Age_group` (0-20 / 21-50 / 51+),
  `HIV_RNA_detection`, `HIV_UMI_bin`, `Leiden_cluster`, `Cell_type_annotation_level_1/2`,
  `CellTypist_label`, `CellTypist_confidence`, `QC_exclude_flag`, `Doublet_flag`, `Doublet_score`,
  `Log10_total_UMI`, `Percent_mito`, `Donor_mixing_entropy`, `Well_mixing_entropy`.
- The library (v8: `Individual_10X_well`) is a study-scoped code (e.g. `frouard2026_L01`, v8
  `Frouard2026_L01`), not the deposit's library name.
- Cells are named `c0000001`.., not by their sequencing barcode.
- Neighbour graph: each cell's 10 strongest neighbours (for display only).

## What is NOT in it

- v6 and v7: no donor or specimen identifiers, sex, age, CD4 count or viral load.
- v8: a pseudonymous `Donor` code per study (e.g. `Peterson2026_D03`, not the deposit's donor ID),
  and `Sex` and `Age_group` only where the source paper itself publishes them (at the same or a
  finer grain); "Missing information" elsewhere. No deposit donor or specimen IDs, exact ages, CD4
  count or viral load.

## Read before interpreting

- **Preview.** The integration is not final; the atlas will change.
- "HIV RNA detected" = at least one HIV UMI against the deposit's own reference. "Not detected"
  includes latently infected cells: RNA is measured, not provirus.
- Peterson cells were all cultured overnight; one arm also got PMA/ionomycin (`ex_vivo_treatment`).
  In v8, "Unstimulated" therefore means "not stimulated", not "never cultured".
- v8 `Treatment_initiation_stage`: Frouard from the cohort (SCOPE = chronic, Treat Acute = acute);
  Peterson from its Supplementary Table 1; Wei 2023 "Unknown" (immediate and deferred-ART
  participants share anonymous hashtags); Weymar and Zhang "Unknown" (not reported).
- v8 Wei 2023 `Donor` codes are per hashtag: one participant's viremic and suppressed draws carry
  two codes, because the deposit does not link them.
- QC and doublet fields are flags only; no cell was removed for them.
- PWOH (people without HIV) = Frouard's 3 control donors in v6; v7 adds Wei et al. 2023's donors
  without HIV.
- v7 and v8: Wei et al. 2023 cells without a per-cell HIV call are shown as "not evaluable".
- v8: colon cells come from Peterson et al. 2026 only, so blood-vs-colon differences cannot be
  separated from that study's own handling.
- Cite the original studies above when using these data.
