# ngs-fastq-to-vcf

A reproducible Nextflow pipeline that takes paired-end Illumina FASTQ files to annotated, normalised VCFs. Built as part of an 8-week programme to move hands-on next-generation sequencing (NGS) bioinformatics from proficient to mastered, with an emphasis on clean architecture, testing and reproducibility.

> **Status:** 🚧 Week 1 — environment and quality-control module in progress.

## Pipeline overview

```
FASTQ ──► QC (FastQC, fastp) ──► Alignment (bwa-mem) ──► Sort / index (samtools)
      ──► Duplicate marking ──► Coverage (mosdepth, BED targets)
      ──► Variant calling ──► Normalisation (bcftools norm) ──► Annotation (VEP) ──► Report
```

| Stage | Tools | Status |
|---|---|---|
| Quality control and trimming | FastQC, fastp | 🚧 In progress |
| Alignment | bwa-mem, samtools | ⏳ Planned (Week 2) |
| Duplicate marking and coverage | samtools markdup, mosdepth | ⏳ Planned (Week 3) |
| Containerisation and profiles | Docker, Nextflow profiles | ⏳ Planned (Week 4) |
| Variant calling and normalisation | bcftools | ⏳ Planned (Week 5) |
| Annotation and report | Ensembl VEP, Python | ⏳ Planned (Week 6) |
| Testing and continuous integration | nf-test, pytest, GitHub Actions | ⏳ Planned (Week 7) |
| v1.0 release | — | ⏳ Planned (Week 8) |

## Quick start

### 1. Create the environment

Requires [Miniforge](https://github.com/conda-forge/miniforge) (includes mamba).

```bash
mamba env create -f environment.yml
mamba activate ngs
```

### 2. Run the pipeline

```bash
nextflow run main.nf \
  --reads 'data/*_{1,2}.fastq.gz' \
  --outdir results
```

## Repository structure

```
.
├── main.nf               # Pipeline entry point
├── nextflow.config       # Parameters and profiles
├── modules/              # One process per tool
├── data/                 # Small test FASTQs (not committed if large)
├── results/              # Pipeline outputs (git-ignored)
├── environment.yml       # Conda/mamba environment
├── mistakes-log.md       # Errors, root causes and rules learned
└── README.md
```

## Design principles

- **Reproducible:** every tool version is recorded; the same inputs give the same outputs.
- **Modular:** one Nextflow process per tool, so stages can be tested and swapped independently.
- **Tested:** each module gets a test on small public data before it is wired into the pipeline.
- **Documented:** decisions and mistakes are logged as the pipeline grows.

## Test data

Small paired-end human FASTQ files from the [nf-core test-datasets](https://github.com/nf-core/test-datasets) repository.

## Author

Kudakwashe Nyamupangedengu, PhD — [github.com/Kudashian](https://github.com/Kudashian)