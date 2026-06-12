# Installation

For both projects you will need the `bioinfo` environment active.
You may use the follwoing bash line in a UNIX terminal in order to install the enivronment.˛This code will make a new folder in you working directory called bioinfo. 
```bash
curl -fsSL https://data.biostarhandbook.com/setup/biostar.setup.2026.sh | bash
```
Close and reopen the terminal.
With the shorcut `bioinfo` in your terminal you can activate the environment.

With this active you may follow with the following chapters. 
If it's not working please refer to [the original Biostar handbook's webpage](https://www.biostarhandbook.com/courses/2026/elixir/setup/bioinfo-env/#activating-the-environment) (You may need log in credentials)


# Usage

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

