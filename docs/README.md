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
- Per-cell fields: study, cohort, chemistry, sorted population (`cell_scope`), ex-vivo treatment,
  study state (viremic / suppressed / uninfected), infection stage, timepoint group, HIV RNA status,
  cell-type labels (`ann_level_1/2`, CellTypist), QC and doublet flags, depth / mito / mixing scores;
  from v7 on, Leiden clusters (`leiden`); in v8, `compartment` (blood / colon).
- `library` is a study-scoped code (e.g. `frouard2026_L01`), not the deposit's library name.
- Cells are named `c0000001`.., not by their sequencing barcode.
- Neighbour graph: each cell's 10 strongest neighbours (for display only).

## What is NOT in it

No donor or specimen identifiers, sex, age, CD4 count or viral load.

## Read before interpreting

- **Preview.** The integration is not final; the atlas will change.
- "HIV RNA detected" = at least one HIV UMI against the deposit's own reference. "Not detected"
  includes latently infected cells: RNA is measured, not provirus.
- Peterson cells were all cultured overnight; one arm also got PMA/ionomycin (`ex_vivo_treatment`).
- PWOH (people without HIV) = Frouard's 3 control donors in v6; v7 adds Wei et al. 2023's donors
  without HIV.
- v7 and v8: Wei et al. 2023 cells without a per-cell HIV call are shown as "not evaluable".
- v8: colon cells come from Peterson et al. 2026 only, so blood-vs-colon differences cannot be
  separated from that study's own handling.
- Cite the original studies above when using these data.
