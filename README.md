# Computational 16S practical — EMBO Microbial Communities 2026

This repository contains the participant material for a two-hour computational
16S practical. The experiment compares microbial profiles from axenic and
xenic *Chaetoceros neogracilis* cultures across time using Oxford Nanopore 16S
sequencing. Media controls are included as a separate condition.

## Setup

The following steps describe how you get an up-to-date copy of this repo onto
your VM and and define a variable to locate the path to your personal copy.

## Step 1: open a terminal on your VM

To open a terminal, use the keyboard shortcut **CTRL-Alt-T**. You can also open
your home folder in the file browser via the bookmark on your desktop, right
click below the folders and select "Open in terminal". Both alternatives open
the terminal such that your current path in the terminal is your home folder.

**All commands shown below must be copied and pasted in this terminal window.**
To **copy**, either highlight and then copy with CTRL+C or use the "Copy to
clipboard" button that might (does not always) show up on the right when you
hover over the code you want to copy. To **paste** in the terminal right click
and select "paste" in the terminal window.

### Step 2: get your own copy of the repository to work in

For you to get the newest version of the repo we will clone it, copy it to your
home folder that is.
```bash
git clone https://git.embl.org/grp-zimmermann-kogadeeva/EMBO_MCD2026_Practical5_16S.git
```

Now you will find a folder `EMBO_MCD2026_Practical5_16S` in your home folder.

### Step 3: open rstudio and the project

Next, we will open rstudio and open a project within rstudio.

1. Either click the button in the upper-left corner or press Windows key to open app menu and search for rstudio
2. Within rstudio, click in the upper-right corner on button labelled "Project (None)" and click on "Open Project..."
3. In the file-browser that popped-up, navigate to EMBO_MCD2026_Practical5_16S and click on file "EMBO_MCD2026_Practical5_16S.Rproj"

## Dataset used in this practical

The prepared analysis dataset contains 18 samples. Axenic and xenic cultures
were sampled at `T00`, `T02`, `T07`, and `T14`, with two biological replicates
per condition and timepoint. Two media controls are available at `T00` only.

The supplied NanoCLUST table contains classified relative-abundance profiles
at order, family, genus, and species rank. It does not contain raw sequencing
reads or per-sample read counts.

## Course workflow

The practical deliberately separates pipeline exposure from biological
interpretation. The NanoCLUST walkthrough is a separate pipeline-exposure
exercise. The core two-hour interpretation session starts from the prepared
complete dataset.

### 1. Pipeline exposure: run NanoCLUST on an assigned barcode

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

### 2. Core session: analyze the prepared complete dataset

Participants then work through
[`scripts/16S_analysis_and_visualization.Rmd`](scripts/16S_analysis_and_visualization.Rmd).
It uses the prepared combined NanoCLUST profiles for all 18 course samples,
allowing everyone to compare the full experiment even though each participant
ran only a subset through NanoCLUST.

The analysis covers:

- validation of the relative-abundance profiles and metadata;
- explicit completion of absent sample–taxon combinations with zero;
- condition-faceted species-composition plots;
- descriptive observed richness and Shannon diversity across time;
- Bray–Curtis dissimilarity and sample-level PCoA;
- axenic and xenic time courses with baseline media controls; and
- compositionality, host-associated signal, and taxonomic uncertainty.

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

The R workflow requires `tidyverse`, `vegan`, `scales`, `ggrepel`, `here`,
`RColorBrewer`, `knitr`, `rmarkdown`, and their rendering dependencies. The
command produces an HTML version of the practical next to the R Markdown
source.

## Interpretation limits

The supplied NanoCLUST table contains relative abundances rather than raw read
counts. It therefore cannot support sequencing-depth QC, mapping-rate checks,
rarefaction, count-based alpha diversity, or claims about absolute microbial
growth. Taxonomic names are classifier assignments, not confirmed species
identities.
