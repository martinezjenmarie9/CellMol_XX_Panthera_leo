# *Panthera leo* – Sex Chromosome Dosage Analysis

## Project Overview

This project investigates sex-chromosome dosage in Panthera leo (lion) using whole-genome sequencing (WGS) data. The goal is to examine chromosome coverage and determine how sex-chromosome dosage can be used to distinguish between sex chromosome groups.

## Study Question

How does sequencing coverage of the sex chromosomes compare with autosomal chromosome coverage in Panthera leo, and what can this tell us about sex-chromosome dosage?

## Selected Species and Accession Information

**Animal species:** *Panthera leo*

**Assembly accession:** GCA_046255005.1

**Assembly BioSample:** SAMN40727535

**Sex system:** XX/XY

**WGS run accession** SRR28532998

**WGS BioSample:** SAMN40727535

**Same donor:** Yes

**Status:** Verified

## NCBI Accession Links

- "NCBI Genome Assembly – GCA_046255005.1" (https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_046255005.1/)
- "NCBI BioSample – SAMN40727535" (https://www.ncbi.nlm.nih.gov/biosample/SAMN40727535)
- "NCBI SRA – SRR28532998" (https://www.ncbi.nlm.nih.gov/sra/SRR28532998)

## Study Design

The analysis uses whole-genome sequencing reads from Panthera leo.

The WGS reads are aligned against the selected genome assembly. The resulting BAM file is used to calculate chromosome-level sequencing coverage.

Coverage is then compared between autosomes and sex chromosomes.

## Galaxy Workflow

Platform: "Galaxy" (https://usegalaxy.org/)

History name: "Sex_Dosage_Panthera_leo"

## Galaxy Steps

1. Downloaded the genome assembly GCA_046255005.1 from NCBI.
2. Imported the genome FASTA reference into Galaxy.
3. Retrieved the WGS sequencing data using SRA accession SRR28532998.
4. Imported the paired-end FASTQ reads into Galaxy.
5. Performed FastQC to examine the quality of the sequencing reads.
6. Used BWA-MEM to align the sequencing reads against the selected reference genome.
7. Used Samtools Sort to sort the resulting BAM file.
8. Used Samtools Index to index the sorted BAM file.
9. Used Samtools Flagstat to examine alignment statistics.
10. Used Samtools idxstats to examine chromosome-level alignment information.
11. Used Samtools Coverage to calculate sequencing coverage.

## Galaxy Screenshots

Insert screenshots of the following:

- [ ] Galaxy history showing the imported reference and WGS reads.
- [ ] FastQC quality reports.
- [ ] BWA-MEM alignment result.
- [ ] Samtools Sort output.
- [ ] Samtools Flagstat result.
- [ ] Samtools idxstats result.


## Galaxy Workflow/History Link

"[INSERT GALAXY HISTORY LINK]"

## Chromosome Groups

Chromosomes are classified into two main groups:

### Autosomes

Autosomes are chromosomes that are not involved in determining biological sex. They serve as the comparison group for chromosome coverage.

### Sex Chromosomes

The sex chromosomes in lions follow the mammalian XX/XY system.

- X chromosome: Sex chromosome.
- Y chromosome: Sex chromosome.

## Chromosome Group CSV

The chromosome groups are identified using the NCBI Sequence Report and chromosome assembly information.

[The CSV file]

> The chromosome classification is based on the chromosome assignments in the NCBI assembly report. Unplaced and unlocalized sequences should not automatically be classified as autosomes.

## Why These Groups Were Chosen

Autosomes provide a baseline for comparison because they are not sex chromosomes. The X and Y chromosomes are analyzed separately because their sequencing coverage can differ depending on the biological sex of the sample.

## Coverage Table

File: "data/coverage.tsv"


## R Analysis




## References

1. National Center for Biotechnology Information (NCBI). Genome Assembly: GCA_046255005.1. https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_046255005.1/

2. National Center for Biotechnology Information (NCBI). BioSample: SAMN40727535. https://www.ncbi.nlm.nih.gov/biosample/SAMN40727535

3. National Center for Biotechnology Information (NCBI). Sequence Read Archive: SRR28532998. https://www.ncbi.nlm.nih.gov/sra/SRR28532998

4. Galaxy Project. Galaxy Platform. https://usegalaxy.org/

5. Samtools. SAMtools Documentation. https://www.htslib.org/

6. R Core Team. R: A Language and Environment for Statistical Computing. https://www.r-project.org/

 documentation, analysis code, chromosome group information, coverage input table, and generated results and figures.

Large sequencing and genome reference files are excluded from the repository.
