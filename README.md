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

Note: The actual Samtools Coverage output was inserted here. The actual Samtool coverage result is empty so I used the results of Samtool idxstats and Samtool flagstats along with the Chromosome group CSV as a basis for this result. 

## R Analysis

The R analysis compares mean sequencing depth between chromosome groups and generates a coverage figure.

Required Packages

install.packages("ggplot2")
install.packages("dplyr")

Complete R Code

Save this code as "analysis.R".

# Panthera leo Sex Chromosome Dosage Analysis

library(ggplot2)
library(dplyr)

# Create results folder
dir.create("results", showWarnings = FALSE)

# Read input files
coverage <- read.delim(
  "data/coverage.tsv",
  check.names = FALSE,
  comment.char = ""
)

chromosome_groups <- read.csv(
  "data/chromosome_groups.csv",
  stringsAsFactors = FALSE
)

# Standardize column names
names(coverage)[names(coverage) == "#rname"] <- "sequence_name"

# Check required columns
if (!"sequence_name" %in% names(coverage)) {
  stop("The coverage file must contain a sequence name column.")
}

if (!"meandepth" %in% names(coverage)) {
  stop("The coverage file must contain a meandepth column.")
}

if (!all(c("sequence_name", "group") %in%
         names(chromosome_groups))) {
  stop("The chromosome group CSV needs sequence_name and group columns.")
}

if (nrow(coverage) == 0) {
  stop("Coverage table is empty. Check the Samtools Coverage output.")
}

# Match chromosome names with their groups
merged_data <- merge(
  coverage,
  chromosome_groups,
  by = "sequence_name"
)

if (nrow(merged_data) == 0) {
  stop("No matching chromosome names. Check sequence accessions.")
}

# Calculate average coverage by chromosome group
group_summary <- merged_data %>%
  group_by(group) %>%
  summarise(
    mean_coverage = mean(meandepth, na.rm = TRUE),
    chromosome_count = n(),
    .groups = "drop"
  )

# Save coverage summary
write.csv(
  group_summary,
  "results/coverage_summary.csv",
  row.names = FALSE
)

# Generate chromosome coverage plot
coverage_plot <- ggplot(
  merged_data,
  aes(x = sequence_name, y = meandepth, fill = group)
) +
  geom_col() +
  labs(
    title = "Chromosome Coverage in Panthera leo",
    x = "Chromosome",
    y = "Mean Sequencing Depth",
    fill = "Chromosome Group"
  ) +
  theme_minimal() +
  theme(
    axis.text.x = element_text(
      angle = 90,
      hjust = 1,
      size = 7
    )
  )

# Save figure
ggsave(
  "results/chromosome_coverage.png",
  plot = coverage_plot,
  width = 12,
  height = 7,
  dpi = 300
)

print(group_summary)
print(coverage_plot)

## Results

The results will be based on the actual chromosome-level coverage obtained from Samtools Coverage.

## Coverage Summary

Insert the generated "coverage_summary.csv" here.

### Figure

The R script generates:

"results/chromosome_coverage.png"

Insert the generated figure here.

### Interpretation

Compare the mean sequencing depth of the autosomes with the X and Y chromosomes.

Differences in coverage may provide evidence of sex-chromosome dosage. However, interpretation must consider the biological sex of the sequencing donor, mapping quality, and the completeness of the reference assembly.

## Answers to the Six Assignment Questions

Question 1. What animal species was selected?

The selected animal species is Panthera leo.

Question 2. What genome assembly was used?

The selected genome assembly is GCA_046255005.1.

Question 3. What is the sex-determination system?

Panthera leo has an XX/XY sex-determination system.

Question 4. Were the assembly and WGS samples obtained from the same donor?

The assembly BioSample and WGS BioSample are both SAMN40727535. Therefore, they are identified as the same BioSample.

Question 5. How does chromosome coverage help identify sex-chromosome dosage?

Chromosome coverage measures how many sequencing reads align to a particular chromosome. Comparing X and Y coverage with autosomal coverage can help identify differences associated with sex-chromosome dosage.

Question 6. What are the limitations of this analysis?

The analysis may be affected by sequencing quality, alignment accuracy, incomplete genome assemblies, and differences in chromosome length. The biological sex of the WGS donor must also be established before interpreting the expected X and Y coverage patterns.

## Limitations

- Sequencing coverage may vary because of differences in sequencing depth.
- Some reads may not align correctly to the reference genome.
- Repetitive DNA sequences may affect mapping accuracy.
- Missing or incomplete chromosome sequences may affect coverage calculations.
- The biological sex of the WGS donor must be confirmed.
- Chromosome coverage alone cannot establish biological sex without appropriate supporting information.

## References

1. National Center for Biotechnology Information (NCBI). Genome Assembly: GCA_046255005.1. https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_046255005.1/

2. National Center for Biotechnology Information (NCBI). BioSample: SAMN40727535. https://www.ncbi.nlm.nih.gov/biosample/SAMN40727535

3. National Center for Biotechnology Information (NCBI). Sequence Read Archive: SRR28532998. https://www.ncbi.nlm.nih.gov/sra/SRR28532998

4. Galaxy Project. Galaxy Platform. https://usegalaxy.org/

5. Samtools. SAMtools Documentation. https://www.htslib.org/

6. R Core Team. R: A Language and Environment for Statistical Computing. https://www.r-project.org/

 documentation, analysis code, chromosome group information, coverage input table, and generated results and figures.

Large sequencing and genome reference files are excluded from the repository.
