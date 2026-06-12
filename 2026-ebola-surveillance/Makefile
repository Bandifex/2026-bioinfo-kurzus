FASTA_URL=https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/848/505/GCF_000848505.1_ViralProj14703/GCF_000848505.1_ViralProj14703_genomic.fna.gz
GFF_URL=https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/848/505/GCF_000848505.1_ViralProj14703/GCF_000848505.1_ViralProj14703_genomic.gff.gz

GENOME_NAME=ebola-mayinga

GENOME_FASTA=refs/$(GENOME_NAME).fna
GENOME_GFF=refs/$(GENOME_NAME).gff

SRR=SRR1553425

REF=refs/$(GENOME_NAME).fna
GFF=refs/$(GENOME_NAME).gff

# The name of the alignem bam file 

BAM=bam/$(SRR).bam
SORTED_BAM=bam/$(SRR).sorted.bam

LIMIT=10000

R1=fastq/$(SRR)_1.fastq
R2=fastq/$(SRR)_2.fastq


VCF=vcf/$(SRR).vcf.gz

# Flags to customize mpileup command
PILEUP_FLAGS=-d 100 --annotate 'INFO/AD,FORMAT/DP,FORMAT/AD,FORMAT/ADF,FORMAT/ADR,FORMAT/SP'
# Flags for the call command
CALL_FLAGS=--ploidy 2 --annotate 'FORMAT/GQ' 

# -------- NO CHANGES BELOW THIS LINE --------
help:
	@echo "Makefile for downloading and processing the Ebola virus genome and associated sequencing data."
	@echo ""
	@echo "Available targets:"
	@echo "  download   - Download the reference genome FASTA and GFF files."
	@echo "  fastq      - Download the FASTQ files for the specified SRR accession."
	@echo "  align      - Align the FASTQ reads to the reference genome and create a sorted BAM file."
	@echo "  clear      - Remove all downloaded and generated files (refs, fastq, bam)."

download:
	mkdir -p refs
	wget -O - $(FASTA_URL) | gunzip > $(GENOME_FASTA)
	wget -O - $(GFF_URL) | gunzip > $(GENOME_GFF)
	bwa index $(GENOME_FASTA)

fastq:
	mkdir -p fastq
	fastq-dump -X ${LIMIT} --outdir fastq --split-files ${SRR}

align:
	mkdir -p bam
	bwa mem -t 4 $(GENOME_FASTA) $(R1) $(R2) | samtools view -bS - > ${BAM}
	samtools sort -o ${SORTED_BAM} ${BAM}
	samtools index ${SORTED_BAM}
	samtools flagstat ${SORTED_BAM}

vcf:
	bcftools mpileup ${PILEUP_FLAGS} -O u -f refs/$(GENOME_FASTA) BAM=$(SORTED_BAM) | \
	bcftools call ${CALL_FLAGS} -mv -O u | \
	bcftools norm -f refs/$(GENOME_FASTA) -d all -O u | \
	bcftools sort -O z > ${VCF}

clear:
	rm -rf refs fastq bam


.PHONY: download fastq index align  vcf