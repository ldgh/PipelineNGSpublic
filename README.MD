# NGSpublic


### Summary


- [Introduction](#introduction)
- [Programs](#programs)
- [Python Libraries](#python-libraries)
- [Example of the execution of the command line](#example-of-the-execution-of-the-command-line)
- [Optional parameters](#optional-parameters)
- [Mandatory input files](#Files)
- [Bibliografy](#bibliografy)

## Introduction

Sequencing machines output files that must be processed before they can be used in biological analyses. This is a  automated pipeline for joint and incremental variant calling, developed by [TOU, R.](http://lattes.cnpq.br/5429240008797663) for sequencing data. This script optimizes the time spent by the bioinformatician behind the analyses, allowing greater flexibility for passing parameters to generate the final product of this work: a high-quality VCF file.

![workflow](https://github.com/lcsfaria/NGS_TOU/assets/73356412/77ffaab4-7902-4639-b169-74700d144820)


## Programs

[Trimmomatic](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4103590/)

[BWA](https://github.com/lh3/bwa)

[Samtools](https://www.htslib.org/)

[GATK](https://gatk.broadinstitute.org/hc/en-us)

[Picard](https://broadinstitute.github.io/picard/)

[FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)

[MultiQC](https://multiqc.info/)


## Python Libraries

- os
- argparse
- sys

## Example of the execution of the command line

```
usage: main.py [-h] [--ram RAM] [--workdir WORKDIR] [--input INPUT] [--threads THREADS] [--txts TXTS] [--step STEP] [--lista_idvs LISTA_IDVS] [--tmp TMP] [--joint JOINT] [--mode MODE] [--fastqc FASTQC]

```

## Parameters

```
  -h, --help            Show this help message and exit
  --ram RAM             RAM usage
  --workdir WORKDIR     Work directory
  --input INPUT         Input folder
  --threads THREADS     Number of threads to use
  --txts TXTS           Folder with text files for optinal flags
  --step STEP           Step you want start
  --lista_idvs LISTA_IDVS  Sndividuals targed list
  --tmp TMP             Keep temporary files
  --joint JOINT         If joint "true". Else "false"
  --mode MODE           For exome use "exome". For wgs use "wgs"
  --fastqc FASTQC       Running FastQC and MultiQC"
```

## Files:

In the parameter --lista_idvs, a list with the names of the samples must be provided. This is important because the program expects the standard sample name+"_L008_R1.fastq.gz" and sample name+"_L008_R2.fastq.gz" in the initial fastq, and all other files are named based on the individual's name. We differentiate the step from the output folder name, but except for the fastqs, the files will be named like this: sample name+file extension (e.g. joão_S1.bam).

The parameter '--txts' indicates the folder where some very important text files are located. They are:

**cmdtrim.txt**: In the first column, which should not be modified, we have the names of the programs we use, and in the second column, separated by semicolons, we have the command used to call this program. See the template file.

**Intervals.txt**: The genome intervals where we will call the variants. It can be replaced by the bed file with the targets of the kit used. Be careful, the more intervals, the larger the space occupied by the temporary files folder.

**obligatory_flags.txt**: Mandatory flags that can be modified. The number will increase in future versions of the program. Same logic as the cmdtrim.txt file.

**optional_flags.txt**: Same as the previous file, but for optional flags.

**ref_loc.txt**: Location of the genome reference files used.

**samples.map**: Will be automatically created by the program.

### Flag --step
This flag is used to continue the run from somewhere you stopped. In other words: it tells the program how far it has already gone and not where to start from. The program starts from the step following the one indicated. Its content is related to the last successfully executed step, and the options coincide with the names of the output folders. The possibilities, in order, are:

nothing: For the untrimmed fastq

bwa: When individuals have already been mapped and sorted

markduplicates: Duplicates marked and removed

paired_only:

leftaligned: bam left aligned

baserecalibrator1: First run of baserecalibrator

bqsr_applied: bam after base recalibration

baserecalibrator2: Second run of baserecalibrator to compare with the first

analyzecovariates: comparison before and after running baserecalibrator

gvcf: when the step of creating GVCFs has been run

dbimport: when genomicdb has already been created. Attention: if you want to update the genomicdb, you must place the GVCFs of the individuals to be added in the /output/gvcf folder of your work directory (you do not need the gvcfs that are already in the genomicdb), with a list of individuals compatible with the new individuals, and the --step flag should be 'gvcf' and not dbimport.

rawvcf: the raw VCF has already been created from the genomicdb

variantrecalibrator: variant recalibration files have already been generated

recalibrated: recalibration has already been completed and there are two VCF files in the folder. It means that the program has already run and is here only for future updates.


## Bibliografy

ANDREWS, S. FastQC A Quality Control tool for High Throughput Sequence Data. 2010. Available at: https://www.bioinformatics.babraham.ac.uk/projects/fastqc/. Accessed on: 28 Aug. 2023.

BAILEY, S. F.; ALONSO MORALES, L. A.; KASSEN, R. Effects of Synonymous Mutations beyond Codon Bias: The Evidence for Adaptive Synonymous Substitutions from Microbial Evolution Experiments. Genome biology and evolution, vol. 13, no. 9, 1 Sep. 2021. DOI 10.1093/gbe/evab141. Available at: http://dx.doi.org/10.1093/gbe/evab141.

BOLGER, A. M.; LOHSE, M.; USADEL, B. Trimmomatic: a flexible trimmer for Illumina sequence data. Bioinformatics , vol. 30, no. 15, p. 2114–2120, 1 Aug. 2014. .

BRAVERMAN, J. M.; HUDSON, R. R.; KAPLAN, N. L.; LANGLEY, C. H.; STEPHAN, W. The hitchhiking effect on the site frequency spectrum of DNA polymorphisms. Genetics, vol. 140, no. 2, p. 783–796, Jun. 1995. 

BROAD INSTITUTE. Picard toolkit. Broad Institute, GitHub repository, 2019. Available at: https://broadinstitute.github.io/picard/.

CAETANO-ANOLLES, D. Base Quality Score Recalibration (BQSR). Mar. 2024. Available at: https://gatk.broadinstitute.org/hc/en-us/articles/360035890531-Base-Quality-Score-Recalibration-BQSR. Accessed on: 29 Mar. 2024.

CAETANO-ANOLLES, D. GVCF - Genomic Variant Call Format. 9 Jun. 2023. Available at: https://gatk.broadinstitute.org/hc/en-us/articles/360035531812-GVCF-Genomic-Variant-Call-Format. Accessed on: 28 Mar. 2024.

DANECEK, P.; AUTON, A.; ABECASIS, G.; ALBERS, C. A.; BANKS, E.; DEPRISTO, M. A.; HANDSAKER, R. E.; LUNTER, G.; MARTH, G. T.; SHERRY, S. T.; MCVEAN, G.; DURBIN, R. The variant call format and VCFtools. Bioinformatics , vol. 27, no. 15, p. 2156–2158, 7 Jun. 2011. . Accessed on: 13 Nov. 2023.

DANECEK, P.; BONFIELD, J. K.; LIDDLE, J.; MARSHALL, J.; OHAN, V.; POLLARD, M. O.; WHITWHAM, A.; KEANE, T.; MCCARTHY, S. A.; DAVIES, R. M.; LI, H. Twelve years of SAMtools and BCFtools. GigaScience, vol. 10, no. 2, p. giab008, 16 Feb. 2021. . Accessed on: 28 Aug. 2023.

DE CONTI, L.; BARALLE, M.; BURATTI, E. Exon and intron definition in pre-mRNA splicing. Wiley interdisciplinary reviews. RNA, vol. 4, no. 1, p. 49–60, 1 Jan. 2013. . Accessed on: 31 Mar. 2024.

EWELS, P.; MAGNUSSON, M.; LUNDIN, S.; KÄLLER, M. MultiQC: summarize analysis results for multiple tools and samples in a single report. Bioinformatics , vol. 32, no. 19, p. 3047–3048, 16 Jun. 2016. . Accessed on: 28 Aug. 2023.

FRIEDMAN, J. M.; JONES, K. L.; CAREY, J. C. Exome Sequencing and Clinical Diagnosis. JAMA: the journal of the American Medical Association, 31 Jul. 2020. DOI 10.1001/jama.2020.11126. Available at: http://dx.doi.org/10.1001/jama.2020.11126.

GILISSEN, C.; HOISCHEN, A.; BRUNNER, H. G.; VELTMAN, J. A. Disease gene identification strategies for exome sequencing. European journal of human genetics: EJHG, vol. 20, no. 5, p. 490–497, 18 Jan. 2012. . Accessed on: 31 Mar. 2024.
GRIFFITHS, A. J. F.; WESSLER, S. R.; CARROLL, S. B.; DOEBLEY, J. Introduction to Genetic Analysis. [S. l.]: W. H. Freeman, 2010.

LANDRUM, M. J.; LEE, J. M.; RILEY, G. R.; JANG, W.; RUBINSTEIN, W. S.; CHURCH, D. M.; MAGLOTT, D. R. ClinVar: public archive of relationships among sequence variation and human phenotype. Nucleic acids research, vol. 42, no. Database issue, p. D980–5, Jan. 2014. .

LI, H. Aligning sequence reads, clone sequences and assembly contigs with BWA-MEM. 16 Mar. 2013. Available at: http://arxiv.org/abs/1303.3997. Accessed on: 28 Aug. 2023.

MARTH, G. T.; CZABARKA, E.; MURVAI, J.; SHERRY, S. T. The allele frequency spectrum in genome-wide human variation data reveals signals of differential demographic history in three large world populations. Genetics, vol. 166, no. 1, p. 351–372, Jan. 2004.

MCKENNA, A.; HANNA, M.; BANKS, E.; SIVACHENKO, A.; CIBULSKIS, K.; KERNYTSKY, A.; GARIMELLA, K.; ALTSHULER, D.; GABRIEL, S.; DALY, M.; DEPRISTO, M. A. The Genome Analysis Toolkit: A MapReduce framework for analyzing next-generation DNA sequencing data. Genome research, vol. 20, no. 9, p. 1297–1303, 1 Sep. 2010. . Accessed on: 10 Jan. 2024.

MOLECULAR GENETICS OF HEMOPHILIA A: CLINICAL PERSPECTIVES. Egyptian Journal of Medical Human Genetics, vol. 11, no. 2, p. 105–114, 1 Nov. 2010. . Accessed on: 1 Apr. 2024.

ROSS, J. P.; DION, P. A.; ROULEAU, G. A. Exome sequencing in genetic disease: recent advances and considerations. F1000Research, vol. 9, no. 336, p. 336, 6 May 2020. . Accessed on: 31 Mar. 2024.

SAUNA, Z. E.; KIMCHI-SARFATY, C. Understanding the contribution of synonymous mutations to human disease. Nature reviews. Genetics, vol. 12, no. 10, p. 683–691, 31 Aug. 2011. .

VAN ROSSUM, G.; DRAKE, F. L. Python 3 Reference Manual: (Python Documentation Manual Part 2). [S. l.]: CreateSpace Independent Publishing Platform, 2009.

WANG, K.; LI, M.; HAKONARSON, H. ANNOVAR: functional annotation of genetic variants from high-throughput sequencing data. Nucleic acids research, vol. 38, no. 16, p. e164–e164, 3 Jul. 2010. . Accessed on: 29 Mar. 2024.

ZAHA, A.; FERREIRA, H. B.; PASSAGLIA, L. M. P. Biologia Molecular Básica - 5.ed. [S. l.]: Artmed Editora, 2014.

