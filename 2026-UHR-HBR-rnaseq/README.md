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