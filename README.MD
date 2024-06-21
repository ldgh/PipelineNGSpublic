# NGSpublic


### Summary


- [Introduction](#introduction)
- [Programs](#programs)
- [Python Libraries](#python-libraries)
- [Usage](#Usage)
- [Mandatory input files](#Files)
- [Bibliografy](#bibliografy)

## Introduction

Sequencing machines output files that must be processed before they can be used in biological analyses. This is a  automated pipeline for joint and incremental variant calling, developed by [TOU, R.](http://lattes.cnpq.br/5429240008797663) for variant calling from NGS data. This script optimizes the time spent by the bioinformatician behind the analyses, allowing greater flexibility for passing parameters to generate the final product of this work: a high-quality VCF file.

![flowchat pipeline](https://github.com/ldgh/PipelineNGSpublic/assets/58569730/0514689a-9c41-45d6-8454-3217c19cf629)


## Programs
[Python 3](https://www.python.org/)

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

## Usage
```
main.py [-h] [--ram RAM] [--workdir WORKDIR] [--input INPUT] [--threads THREADS] [--txts TXTS] [--step STEP] [--lista_idvs LISTA_IDVS] [--tmp TMP] [--joint JOINT] [--mode MODE] [--fastqc FASTQC]

```

### Parameters

```
  -h, --help            Show this help message and exit
  --ram RAM             RAM usage
  --workdir WORKDIR     Work directory
  --input INPUT         Input folder
  --threads THREADS     Number of threads to use
  --txts TXTS           Folder with text files for optinal flags
  --step STEP           Step you want start
  --lista_idvs LISTA_IDVS  Individuals targed list
  --tmp TMP             Keep temporary files
  --joint JOINT         If joint "true". Else "false"
  --mode MODE           For exome use "exome". For wgs use "wgs"
  --fastqc FASTQC       Running FastQC and MultiQC"
```
### Example
```
python3 main.py --ram 4G --workdir dados/pipelineNGS-main/teste/rodando/ --input dados/pipelineNGS-main/teste/merged/ --threads 8 --txts dados/pipelineNGS-main/txts/ --step nada --lista_idvs dados/pipelineNGS-main/teste/idvs.txt --tmp ./tmp --joint true --mode exome --fastqc true
```

## Files:

In the parameter --lista_idvs, a list with the names of the samples must be provided. This is important because the program expects the standard sample name+"_L008_R1.fastq.gz" and sample name+"_L008_R2.fastq.gz" in the initial fastq, and all other files are named based on the sample's identifiers. We differentiate the step from the output folder name, but except for the fastqs, the files will be named like this: sample name+file extension (e.g. joão_S1.bam).

The parameter '--txts' indicates the folder where the config files are located. They are:

**cmdtrim.txt**: In the first column, which should not be modified, the names of the programs used are located, and in the second column, separated by semicolons, the command used to call this program. See the template file.

**Intervals.txt**: The genome intervals where variants will be called.

**obligatory_flags.txt**: Mandatory flags that can be modified. The number will increase in future versions. Same logic as the cmdtrim.txt file.

**optional_flags.txt**: Same as the previous file, but for optional flags.

**ref_loc.txt**: Location of the genome reference files used.

**samples.map**: Will be automatically created by the program.

### Flag --step
This flag is used to continue the run from somewhere you stopped. In other words: it tells the program how far it has already gone and not where to start from. The program starts from the step following the one indicated. Its content is related to the last successfully executed step, and the options coincide with the names of the output folders. The possibilities, in order, are:

`nada`: For the untrimmed fastq

`bwa`: When individuals have already been mapped and sorted

`markduplicates`: Duplicates marked and removed

`paired_only`:

`leftaligned`: bam left aligned

`baserecalibrator1`: First run of baserecalibrator

`bqsr_applied`: bam after base recalibration

`baserecalibrator2`: Second run of baserecalibrator to compare with the first

`analyzecovariates`: comparison before and after running baserecalibrator

`gvcf`: when the step of creating GVCFs has been run

`dbimport`: when genomicdb has already been created. Attention: if you want to update the genomicdb, you must place the GVCFs of the individuals to be added in the /output/gvcf folder of your work directory (you do not need the gvcfs that are already in the genomicdb), with a list of individuals compatible with the new individuals, and the --step flag should be 'gvcf' and not dbimport.

`rawvcf`: the raw VCF has already been created from the genomicdb

`variantrecalibrator`: variant recalibration files have already been generated

`recalibrated`: recalibration has already been completed and there are two VCF files in the folder. It means that the program has already run and is here only for future updates.

## Acknowledgements
Main developer: Rafael Pereira Tou

## Bibliografy

ANDREWS, S. FastQC A Quality Control tool for High Throughput Sequence Data. 2010. Available at: https://www.bioinformatics.babraham.ac.uk/projects/fastqc/. Accessed on: 28 Aug. 2023.

BOLGER, A. M.; LOHSE, M.; USADEL, B. Trimmomatic: a flexible trimmer for Illumina sequence data. Bioinformatics , vol. 30, no. 15, p. 2114–2120, 1 Aug. 2014.

BROAD INSTITUTE. Picard toolkit. Broad Institute, GitHub repository, 2019. Available at: https://broadinstitute.github.io/picard/.

DANECEK, P.; BONFIELD, J. K.; LIDDLE, J.; MARSHALL, J.; OHAN, V.; POLLARD, M. O.; WHITWHAM, A.; KEANE, T.; MCCARTHY, S. A.; DAVIES, R. M.; LI, H. Twelve years of SAMtools and BCFtools. GigaScience, vol. 10, no. 2, p. giab008, 16 Feb. 2021. Accessed on: 28 Aug. 2023.

EWELS, P.; MAGNUSSON, M.; LUNDIN, S.; KÄLLER, M. MultiQC: summarize analysis results for multiple tools and samples in a single report. Bioinformatics , vol. 32, no. 19, p. 3047–3048, 16 Jun. 2016. Accessed on: 28 Aug. 2023.

LI, H. Aligning sequence reads, clone sequences and assembly contigs with BWA-MEM. 16 Mar. 2013. Available at: http://arxiv.org/abs/1303.3997. Accessed on: 28 Aug. 2023.

MCKENNA, A.; HANNA, M.; BANKS, E.; SIVACHENKO, A.; CIBULSKIS, K.; KERNYTSKY, A.; GARIMELLA, K.; ALTSHULER, D.; GABRIEL, S.; DALY, M.; DEPRISTO, M. A. The Genome Analysis Toolkit: A MapReduce framework for analyzing next-generation DNA sequencing data. Genome research, vol. 20, no. 9, p. 1297–1303, 1 Sep. 2010. Accessed on: 10 Jan. 2024.

VAN ROSSUM, G.; DRAKE, F. L. Python 3 Reference Manual: (Python Documentation Manual Part 2). CreateSpace Independent Publishing Platform, 2009.

