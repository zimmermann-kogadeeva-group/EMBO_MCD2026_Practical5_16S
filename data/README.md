# Teaching inputs

## `metadata.tsv`

Cleaned experimental sample sheet containing group, condition, timepoint, and
replicate annotations. `Sample_ID` contains `barcode01` through `barcode18`,
derived from the numeric suffixes of the 18 course-sample `EMBL_ID` values.

## `combined_relative_abundance.csv`

Combined NanoCLUST output supplied for the 2026 course. It contains relative
abundances at order, family, genus, and species rank for 18 barcode-labelled
samples. Source SHA-256:
`904bed30a5e3dadc69763b887a82cd26f1dec158df00344ec081e155a4d89c09`.

This is a relative-abundance table, not a raw count table. It therefore cannot
support library-size QC, rarefaction, or count-based diversity estimates.
