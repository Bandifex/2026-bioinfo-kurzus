# 2026-elixir-bioinfo-kurzus

Itt vannak a kurzus óráin tanult anyagok.

# Usage

For both projects you will need the `bioinfo` environment active.
You may use the follwoing bash line in a UNIX terminal in order to install the enivronment.˛This code will make a new folder in you working directory called bioinfo. 
```bash
curl -fsSL https://data.biostarhandbook.com/setup/biostar.setup.2026.sh | bash
```
Close and reopen the terminal.
With the shorcut `bioinfo` in your terminal you can activate the environment.

With this active you may follow with the following chapters. 
If it's not working please refer to [the original Biostar handbook's webpage](https://www.biostarhandbook.com/courses/2026/elixir/setup/bioinfo-env/#activating-the-environment) (You may need log in credentials)

## 2026-ebola-surveillance

Use the Makefile to do the ebola surveillance.
Using the line `make` in you terminal you will see:
```text
Makefile for downloading and processing the Ebola virus genome and associated sequencing data.

Available targets:
  download   - Download the reference genome FASTA and GFF files.
  fastq      - Download the FASTQ files for the specified SRR accession.
  align      - Align the FASTQ reads to the reference genome and create a sorted BAM file.
  clear      - Remove all downloaded and generated files (refs, fastq, bam).
``` 

Now with the forementioned environment you may use the Makefile with the following line.
> Please note that you need the `bioinfo` environment!
```bash
make download fastq align
```

Now the made files are ready to be viewed in a genome viewer program like [IGV](https://igv.org)

To delete every data and directory created by the Makefile you should use `make clear`


## 2026-UHR-HBR-rnaseq
For this to work you may additionally need a few more envorinments.
Make sure your terminal working directory is the directry you wish to install the environment. 
```bash
# Initialize the stats project
pixi init
# Add the bioconda channel
pixi workspace channel add bioconda
# Install additional software
pixi add hisat2 subread
# Get the bioinformatics toolbox
bio code
# Download the Makefile
curl https://data.biostarhandbook.com/make/rnaseq.hisat.2026.mk > Makefile

# The following is needed only on MacOS
# Uncomment if you are using an M based Apple Silicon CPU
# pixi project platform add osx-64

# Add the necessary packages to the stats project
pixi add r-optparse r-tibble r-dplyr r-gplots r-pacman  python\
    bioconductor-proper bioconductor-biomart bioconductor-edger \
    bioconductor-deseq2 bioconductor-tximport \
    jq python 'biopython<1.86' bio parallel
```

You may activate the `stats` environment with
```bash
# Activate the stats environment
stats
```
or 
```bash
# Activate the stats environment
pixi shell -m <STATS-PATH>/stats
```

___

Now start with downloading UHR and HBR patient data.
```bash
#(bioinfo)
make download index
```
The contents of `reads` and `refs` folders has been created along with their indexes.

Now you may use 
```bash
seqkit stats refs/*.fa
```
and you will see statistics regarding the made FASTA files.

To make the alignments use the following
```bash
#(bioinfo)
make align SAMPLE=HBR_1
make align SAMPLE=HBR_2
make align SAMPLE=HBR_3
make align SAMPLE=UHR_1
make align SAMPLE=UHR_2
make align SAMPLE=UHR_3
```
This module will provide you with every .BAM and .BW files that you will need to visualize the variances of the samples extracted from patients with cancer.

___

Now to make statistical report. You will need to change to `stats`
```bash
stats
```
Use the 
```bash
make to_csv add_names
```
With this program used in the `csv/counts.csv` will 