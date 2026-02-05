# Bioscripts

A collection of Python scripts (not only) for processing and analyzing bisulfite sequencing (Bismark) data.

## Overview

BioScripts provides tools for annotating DNA methylation calls with sequence context and merging forward/reverse strand methylation data from Bismark coverage files.

## Scripts

### get_CpG_from_genome

Annotates Bismark coverage files with sequence context information (±1 base around methylated sites).

**Usage:**
```bash
python get_CpG_from_genome/get_CpG_from_genome.py -f <fasta_file> -b <bed_file> -o <output_bed> [-c <chunk_size>] [-m]
```

**Arguments:**
- `-f, --fasta_file`: Path to the input FASTA file (required)
- `-b, --bed_file`: Path to the input BED/coverage file (required)
- `-o, --output_bed`: Path to the output BED file (required)
- `-c, --chunk_size`: Chunk size for reading FASTA file
- `-m, --methylkit`: Input is methylKit format

**Output:**
Generates an annotated BED file with additional columns:
- `REF_-1+1`: Sequence context (base -1 and +1 relative to the CpG site)

### merge_reverse_strand_calls

Merges bookended forward and reverse strand methylation calls from the output of `get_CpG_from_genome`.

**Usage:**
```bash
python merge_reverse_strand_calls/merge_reverse_strand_calls.py -b <bed_file> -o <output_bed>
```

**Arguments:**
- `-b, --bed_file`: Path to the input BED file (required)
- `-o, --output_bed`: Path to the output BED file (required)

**Output:**
BED file with merged methylation calls containing:
- `seqname`: Chromosome/sequence name
- `start`: Start position
- `end`: End position
- `perc_mCpG`: Percentage of methylated cytosines
- `numCs`: Number of methylated cytosines
- `numTs`: Number of unmethylated cytosines

### split_rename_fasta

Splits a large FASTA file into individual sequence files based on regex pattern matching.

**Usage:**
```bash
python split_rename_fasta/split_rename_fasta.py -i <input_file> -p <pattern> -o <output_dir>
```

**Arguments:**
- `-i, --input_file`: Path to the input FASTA file
- `-p, --pattern`: Regex pattern to match sequence names
- `-o, --output_dir`: Directory for output files

### nf-samplesheet

Generates a nextflow samplesheet from FASTQ files in a directory.

**Usage:**
```bash
python nf-samplesheet/get_samplesheet.py <sample1> [<sample2> ...] -d <fastq_dir> [--single]
```

**Arguments:**
- `samples`: One or more sample names (positional)
- `-d, --dir`: Directory with FASTQ files
- `--single`: Single-end reads flag

**Output:**
CSV samplesheet with columns:
- `sample`: Sample name
- `single`: Single-end flag (true/false)
- `fastq_1`: Path to R1 FASTQ file
- `fastq_2`: Path to R2 FASTQ file (if paired-end)
- `genome`: Genome reference (empty by default)

## Installation

This project uses conda for environment management:

```bash
conda env create -f environment.yaml
```

Or use the Apptainer container:

```bash
apptainer run bioscripts.sif <script> [arguments]
```

## Requirements

- Python 3.x
- pandas
- numpy
- BioPython

## License

MIT License - See [LICENSE](LICENSE) for details.

Copyright (c) 2025 Fritjof Lammers