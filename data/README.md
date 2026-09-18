# Teaching inputs

## `metadata.tsv`

Cleaned experimental sample sheet containing group, condition, timepoint, and
replicate annotations. `Sample_ID` contains `barcode01` through `barcode24`,
derived from the numeric suffixes of the 24 unique source `EMBL_ID` values.
The current participant analysis excludes the six unresolved `Vincent_*`
records from both metadata and abundance profiles. They can be restored once
their experimental role has been clarified.

## `combined_relative_abundance.csv`

Combined NanoCLUST output supplied for the 2026 course. It contains relative
abundances at order, family, genus, and species rank for 24 barcode-labelled
samples. Source SHA-256:
`48b9bd9f3baca79ec49aab583d46901767a254d48553c056d27bdf1d3a5ebaff`.

This is a relative-abundance table, not a raw count table. It therefore cannot
support library-size QC, rarefaction, or count-based diversity estimates.
