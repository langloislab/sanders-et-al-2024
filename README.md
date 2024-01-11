# Comparison of mouse models of microbial experience reveals differences in microbial diversity and response to vaccination

Autumn E. Sanders, Henriette Arnesen, Frances K. Shepherd, Dira S. Putri, Jessica K. Fiege, Mark J. Pierson, Shanley N. Roach, Harald Carlsen, David Masopust, Preben Boysen, Ryan A. Langlois

## Background

This repository contains code for analyzing metatranscriptomic data from wild-caught Norwegian mice. All raw sequencing data for the 6 mice is deposited under NCBI BioProject number PRJNA1062843. Pipeline trims raw reads with Trimmomatic, maps to the Mus musculus host genome with STAR, de novo assembles unmapped (i.e. non-host) reads with Trinity, performs a BLAST search to identify contig lineage, and counts reads per contig using Salmon. The output files can be graphed using the Rmarkdown file in this respository.

## Running pipeline

Pipeline requires the following in order to run:

1. Reference genome for Mus musculus, indexed by STAR version 2.7.1a. This paper utilized the Ensembl reference genome release 109 for Mus musculus, GRCm39.109. 
2. A BLAST nt database and taxonomy files `names.dmp` and `nodes.dmp` from NCBI. The latest be downloaded from https://ftp.ncbi.nih.gov/blast/db/FASTA/nt.gz and https://ftp.ncbi.nih.gov/pub/taxonomy/taxdump.tar.gz.
3. A config file edited by the user to point to the correct input directories for raw fastq files, the indexed reference genome, blast nt database, taxonomy names.dmp and nodes.dmp files, and a scratch directory for Trinity to place intermediate files.
4. A conda environment with Snakemake and pandaa, i.e. `mamba create -c conda-forge -c bioconda -n snakemake snakemake`.

Pipeline can then be run using `snakemake --cores 28` (or however many cores you want). The Snakemake pipeline will output two csv files of read counts at the species and family level. To reproduce heatmaps, read in the `config/sample.csv` file and the output file which will be produced in `final/sp_counts.csv` into `norway_rnaseq_graphing.Rmd`. Note that the Rmd file will filter out viral families that are phages, plant- or insect-infecting viruses to focus on mammalian-infecting viruses. 