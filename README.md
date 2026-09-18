# Computational 16S practical — EMBO Microbial Communities 2026

This repository contains the participant material for a two-hour computational
16S practical. The experiment compares microbial profiles from axenic and
xenic *Chaetoceros neogracilis* cultures across time using Oxford Nanopore 16S
sequencing. Media controls are included as a separate condition.

## Course workflow

The practical deliberately separates pipeline exposure from biological
interpretation.

### 1. Run NanoCLUST on an assigned barcode

Participants follow
[`scripts/NanoCLUST_16S_analysis.md`](scripts/NanoCLUST_16S_analysis.md) and run
NanoCLUST for one assigned barcode or a small subset of samples. This provides
hands-on experience with the sequencing workflow without waiting for every
sample to finish.

The NanoCLUST walkthrough retains the established course commands. Its
teaching-server root, hub, group, assigned barcode, and prepared Conda
environment must be confirmed by the trainers before the session. Participants
should not run the all-sample post-processing step: a trainer prepares the
combined output in advance.

### 2. Analyze the prepared complete dataset

Participants then work through
[`scripts/16S_analysis_and_visualization.Rmd`](scripts/16S_analysis_and_visualization.Rmd).
It uses the prepared combined NanoCLUST profiles for all currently identified
course samples, allowing everyone to compare the full experiment even though
each participant ran only a subset through NanoCLUST.

The analysis covers:

- validation of the relative-abundance profiles and metadata;
- explicit completion of absent sample–taxon combinations with zero;
- condition-faceted species-composition plots;
- Bray–Curtis dissimilarity and sample-level PCoA;
- axenic, xenic, and media comparisons across time; and
- compositionality, host-associated signal, and taxonomic uncertainty.

The six unresolved `Vincent_*` samples are excluded from both metadata and
abundance profiles before analysis. They can be restored once their
experimental role is clarified.

## Repository structure

```text
.
├── README.md
├── data/
│   ├── README.md
│   ├── combined_relative_abundance.csv
│   └── metadata.tsv
└── scripts/
    ├── NanoCLUST_16S_analysis.md
    └── 16S_analysis_and_visualization.Rmd
```

The input contract and provenance are documented in
[`data/README.md`](data/README.md).

## Render the analysis

Run from the repository root:

```bash
Rscript -e 'rmarkdown::render("scripts/16S_analysis_and_visualization.Rmd")'
```

The R workflow requires `tidyverse`, `vegan`, `scales`, `ggrepel`,
`RColorBrewer`, `rmarkdown`, and their rendering dependencies.

## Interpretation limits

The supplied NanoCLUST table contains relative abundances rather than raw read
counts. It therefore cannot support sequencing-depth QC, mapping-rate checks,
rarefaction, count-based alpha diversity, or claims about absolute microbial
growth. Taxonomic names are classifier assignments, not confirmed species
identities. In particular, dominant cyanobacterial assignments in axenic
samples may represent host-associated chloroplast signal or reference-database
limitations and should be discussed rather than silently removed.
