# koala-gut-microbiome-16S
R analysis code for a 16S rRNA study of class 1 integron presence and gut microbiome composition in wild koalas (Phascolarctos cinereus), Belair National Park, South Australia.

# Class 1 integrons and the koala gut microbiome

R analysis code for a 16S rRNA study examining whether class 1 integron presence
is associated with shifts in the gut microbiome of wild koalas (*Phascolarctos
cinereus*) in Belair National Park, South Australia.

## Overview

Faecal samples were collected from 62 wild-caught koalas (38 integron-negative,
24 integron-positive). Integron status was determined by prior PCR screening for
the *intI1* integrase gene. This code covers microbial community profiling, alpha
and beta diversity, differential abundance, and analysis of host and environmental
predictors.

This repository contains the **R analysis only**. Upstream sequence processing
(demultiplexing, denoising with deblur, taxonomic assignment against SILVA 138.1)
was performed in QIIME 2; the code here takes the resulting feature table,
taxonomy, and rooted tree as inputs.

## Required inputs

The following files are expected in the working directory:

| File | Description |
|---|---|
| `taxonomy-deblur.csv` | QIIME 2 taxonomy export |
| `feature-table-deblur.csv` | QIIME 2 feature table export |
| `rooted-tree-deblur.nwk` | QIIME 2 rooted tree export |
| `BNP_Koala_Metadata copy.csv` | Sample metadata |
| `Location_coords_koalaBNP.csv` | Site coordinates (map figure only) |

## Data availability

Raw sequence data are deposited in the NCBI Sequence Read Archive under BioProject
[PRJNA1402904](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1402904).

## Analysis notes

**Object flow**

- `ps` — full unrarefied object (raw deblur counts); used for DESeq2
- `ps_corrected` — copy of `ps` with BLAST-refined taxonomy applied
- `ps_rarefied` — rarefied to 14,555 reads (seed 123); used for diversity and
  relative abundance

Seven ASVs were manually reclassified following BLAST refinement (5 to genus,
2 to family). These corrections are applied once to the full object so they
propagate to both rarefied and unrarefied analyses.

**Reproducibility**

- Rarefaction depth: 14,555 reads (minimum sample depth), `set.seed(123)`
- All permutation tests (PERMANOVA, betadisper) are seeded with `set.seed(123)`
- DESeq2 is run on unrarefied counts with `sfType = "poscounts"` throughout
- Relative abundance at each taxonomic level is calculated as the mean of
  per-sample proportions

Run with workspace auto-restore **disabled** (RStudio: Tools → Global Options →
uncheck "Restore .RData into workspace at startup"). The script is designed to run
top-to-bottom from a clean session.

## Software

Analysis was performed in R (version 2025.08.0-daily+88). Key packages: phyloseq, vegan, DESeq2,
microbiome, ape, tidyverse, patchwork, FSA.

Note that `microbiome` and `phyloseq` mask several dplyr verbs (`select`, `rename`,
`arrange`); base R equivalents are used in places where this caused issues.

## Citation

[Manuscript citation once published]

## Contact

Laura Marshall / laura.marshall1@hdr.mq.edu.au

## License

MIT
