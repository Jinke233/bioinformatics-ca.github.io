---
layout: post2
permalink: /analysis_of_metagenomic_data_module6_lab_2016/
title: Analysis of Metagenomic Data 2016 Student Page
header1: Analysis of Metagenomic Data 2016
header2: Module 6 Lab
image: CBW_Metagenome_icon.jpg
---

# Module 6 Metatranscriptomics Lab

**This work is licensed under a [Creative Commons Attribution-ShareAlike 3.0 Unported License](http://creativecommons.org/licenses/by-sa/3.0/deed.en_US). This means that you are able to copy, share and modify the work, as long as the result is distributed under the same license.**

Overview
--------

This tutorial will take you through a pipeline processing metatranscriptomic data. The pipeline, developed by the Parkinson lab, consists of various steps which are as follows:

1.  Remove adaptor sequences and trim low quality sequences. These are added during library preparation and sequencing steps and can be generated during sequencing runs.
2.  Remove duplicate reads to reduce processing time for following steps.
3.  Remove abundant rRNA sequences which typically dominate metatranscriptomic datasets despite the use of rRNA removal kits.
4.  Remove host reads (if exploring a microbiome in which host is an issue).
5.  Add duplicated reads, removed in step 2, back to the data set to improve quality of assemblies.
6.  Assemble the reads into contigs to improve annotation quality.
7.  Annotate reads to known genes.
8.  Map identified genes to a "system" dataset for network visualization - here a protein-protein interaction map based on E. coli proteins.
9.  Generate normalized expression values associated with each gene.
10. Visualize the results using an E. coli map of protein-protein interactions as a scaffold in Cytoscape.

The whole metatranscriptomic pipeline includes existing bioinformatic tools and a series of Perl scripts that run these tools and provide input files in the correct format. We will go through these steps to illustrate the complexity of the process and the underlying tools and scripts.

New, faster, and/or more accurate tools are being developed all the time, and it is worth bearing in mind that any pipelines need to be flexible to incorporate these tools as they get adopted as standards by the community. For example, over the past two years, our lab has transitioned from cross\_match to Trimmomatic and from BLAST to DIAMOND. Also due to our historical lab culture, we rely on the use of Perl scripts. However, when building your own pipelines, you might readily adopt other scripting languages, such as Python.

To illustrate the process we are going to use sequence reads generated from the rumen of a cow. These are 100bp paired end reads - single end reads can also be used, but paired end reads can increase sequence length if there is significant overlap and consequently improve annotation quality.

Rather than use the entire set of 14 million which might take several days to process on a desktop, the tutorial will take you through processing a subset of 100,000 reads.

Preliminaries
-------------

### Amazon node

Read [these directions](http://bioinformatics-ca.github.io/logging_into_the_Amazon_cloud/) for information on how to log in to your assigned Amazon node.

### Work directory

Create a new directory that will store all of the files created in this lab.

```
rm -rf ~/workspace/module5
mkdir -p ~/workspace/module5
cd ~/workspace/module5
ln -s ~/CourseData/metagenomics/metatranscriptomics/* .
```

**Notes**:

-   The `ln -s` command adds symbolic links of all of the files contained in the (read-only) `~/CourseData/metatranscriptomics` directory.

### Input files

Our data set consists of 100 bp paired-end Illumina reads from cow rumen. To inspect their contents:

```
less cow1.fastq
less cow2.fastq   
```

**Notes**:

-   Type `q` to exit `less`.

### Checking read quality with FastQC

```
fastqc cow1.fastq
```

The FastQC report is generated in a HTML file, cow1\_fastqc.html. You'll also find a zip file which includes data files used to generate the report.

To open the HTML report file, please go to your workspace folder from your web browser with URL <http://cbwxx.dyndns.info/module5>, where xx is your unique CBW number. By double clicking on the HTML file, you can go through the report and find the following information:

-   Basic Statistics: Basic information of the cow RNA-seq data, e.g. the total number of reads, read length, GC content.
-   Per base sequence quality: An overview of the range of quality values across all bases at each position.
-   Per Base Sequence Content: A plot showing nucleotide bias across sequence length.
-   Overrepresented Sequences: Sequences which comprise >0.1% of all sequences provided.
-   Adapter Content: Provides information on the level of adaptor contamination in your sequence sample.

**Notes**:

-   As you look at the reports, try running BLAST via the [NCBI website](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE_TYPE=BlastSearch) on some of the overrepresented sequences, by copy/paste, to get an idea of what they might be.

***Question: What do overrepresented sequences map to?***

Processing the Reads
--------------------

A bulk of the work in building sequence processing pipelines is formating files generated by one tool to feed into the next tool. In the first step we need to reformat the headers of the paired-end reads such that the 5\` and 3\` ends are assigned appropriate matching sequence identifiers s e.g. 5\` reads are marked with a trailing '/1' while 3\` reads are marked with a trailing '/2'. This is so that downstream programs can correctly match paired reads.

```
perl main_add_subID_reads_fastq.pl  cow
```

**Notes**:

-   check input file: 'less cow1.fastq'
    -   @SRR594215.2 FCFC81EB6ABXX:7:1101:1495:2185 length=200/1
    -   TGTACCTTGAGAGGAAGCACCGGCAAACTTCGTGCCAGGAGCCGCGGTAATACGAGGGGTGCAAGCGTTGTTCGGAATTACTGGGCGGACAGGGAGAGGT

<!-- -->

-   check output file: 'less cow1\_new.fastq'
    -   @SRR594215.2/1
    -   TGTACCTTGAGAGGAAGCACCGGCAAACTTCGTGCCAGGAGCCGCGGTAATACGAGGGGTGCAAGCGTTGTTCGGAATTACTGGGCGGACAGGGAGAGGT

### Step 1. Remove adaptor sequences and trim low quality sequences. 

Trimmomatic can rapidly identify and trim adaptor sequences, as well as identify and remove low quality sequence data - you can download and install on your own computer from their project [website](http://www.usadellab.org/cms/?page=trimmomatic). As a reference database for identifying contaminating vector and adaptor sequences we rely on the UniVec\_Core dataset which is a fasta file of known vectors and sequencing adaptors derived from the NCBI Univec Database. Please download it into your working directory first.

```
wget ftp://ftp.ncbi.nih.gov/pub/UniVec/UniVec_Core
```

```
java -jar /usr/local/Trimmomatic-0.36/trimmomatic-0.36.jar PE cow1_new.fastq cow2_new.fastq cow1_qual_paired.fastq cow1_qual_unpaired.fastq cow2_qual_paired.fastq cow2_qual_unpaired.fastq ILLUMINACLIP:UniVec_Core:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:50
```

**Notes**:

-   The command line parameters are:
    -   `PE`: The input data are paired-end reads.
    -   `ILLUMINACLIP:UniVec\_Core:2:30:10`: remove the adaptors.
    -   `LEADING:3`: Trims bases at the beginning of a read if thery are below quality score of 3.
    -   `TRAILING:3`: Trims bases at the end of a read if thery are below quality score of 3.
    -   `SLIDINGWINDOW:4:15`: Scan with a window of size 4 for reads with local quality below a score of 15, and trim if found.
    -   `MINLEN:50`: Delete a sequence with a length less than 50.

***Question: How many low quality sequences have been removed?***

Checking read quality with FastQC:

```
fastqc cow1_qual_paired.fastq
```

Compare with the previous report to see changes in the following sections:

-   Basic Statistics
-   Per base sequence quality
-   Overrepresented sequences

Since this dataset is based on paired-end reads, we can identify pairs of sequence reads that overlap and can therefore be merged into a single sequence. For this we use the tool FLASH which can be found at this [website](http://ccb.jhu.edu/software/FLASH/):

```
flash -M 75 -p 64 -t 2 -o cow_qual -d out cow1_qual_paired.fastq cow2_qual_paired.fastq
cat out/cow_qual.extendedFrags.fastq out/cow_qual.notCombined_1.fastq > cow1_qual_all.fastq
cp out/cow_qual.notCombined_2.fastq cow2_qual_all.fastq
```

**Notes**:

-   The command line parameters are:
    -   `-M 75`: Maximum overlap length expected in approximately 90% of read pairs is 75.
    -   `-p 64`: The smallest ASCII value of the characters used to represent quality values of bases in FASTQ files. Here we set this to 64 consistent with the Illumina platform that was used to generate the data.
    -   `-t 2`: Sets the number of computing threads to use to 2.
    -   `-o`: Prefix of output files.
    -   `-d out`: Path to directory for output files.

<!-- -->

-   cow1\_qual\_all.fastq contains merged reads and un-merged reads from cow1\_qual\_paired.fastq, while cow2\_qual\_all.fastq has un-merged reads from cow2\_qual\_paired.fastq only. To allow compatibility for downstream programs, we maintain two files or 'paired reads' albeit cow1\_qual\_all.fastq contaains the additional merged reads not present in the cow2\_qual\_all.fastq file.

If you want to see the distribution of merged read length you can look at the histogram file:

```
less out/cow_qual.histogram
```

***Question: Can you find how many pairs have been merged?***

### Step 2. Remove duplicate reads

To significantly reduce the amount of computating time required for identification and filtering of rRNA reads, we perform a dereplication step to remove duplicated reads using the software tool USEARCH.

```
usearch -derep_fulllength cow1_qual_all.fastq -fastaout cow1_qual_all_unique.fasta -sizeout -uc cow1_qual_all_unique.uc

usearch -derep_fulllength cow2_qual_all.fastq -fastaout cow2_qual_all_unique.fasta  -sizeout -uc cow2_qual_all_unique.uc

perl main_get_derepli_IDs.pl cow
```

**Notes**:

-   The command line parameters are:
    -   `-derep\_fulllength`: dereplicates reads based on a match across the full-length of the sequences.
    -   `-fastaout`: output file in FASTA format.
    -   `-sizeout`: annotates the number of replicated reads associated with the specified sequence.
    -   `-uc`: Generates an additional USEARCH cluster (UC) format file for help with downstream de-dereplication.

***Question: Can you find how many unique reads there are?***

While the number of replicated reads in this small dataset is relatively low, with larger datasets, this step can reduce file size by as much as 50-80%

Check read quality with FastQC:

```
fastqc cow1_qual_all_unique.fastq
```

The results are found in cow1\_qual\_all\_unique\_paired\_fastqc.html.

### Step 3. Remove abundant rRNA sequences

rRNA genes tend to be highly expressed in all samples and must therefore be screened out to avoid lengthy downstream processing times for the assembly and annotation steps. You could use sequence similarity tools such as BWA or BLAST for this step, but we find Infernal (http://infernal.janelia.org/), albeit slower, is more sensitive as it relies on a database of hidden Markov models (HMMs) describing rRNA sequence profiles based on the Rfam database. Due to the reliance on HMMs, Infernal, can take as much as 26 hours on a single processor for ~100,000 reads on a single core. So we will skip this step and use two precomputed files - "cow1\_rRNA.infernalout" and "cow2\_rRNA.infernalout" from a tar file "files2out.tar.gz".

``` 
tar -xzf files4out.tar.gz cow1_rRNA.infernalout cow2_rRNA.infernalout
```

**Notes**:

-   The infernal commands you would use are given below:
    -   cmscan -o cow1\_rRNA.log --tblout cow1\_rRNA.infernalout --noali --notextw --rfam -E 0.001 Rfam.cm cow1\_qual\_all\_unique.fasta
    -   cmscan -o cow2\_rRNA.log --tblout cow2\_rRNA.infernalout --noali --notextw --rfam -E 0.001 Rfam.cm cow2\_qual\_all\_unique.fasta

<!-- -->

-   The command line parameters are:
    -   `--tblout`: save a simple tabular file.
    -   `--noali`: omit the alignment section from the main output. This can greatly reduce the output volume.
    -   `--rfam`: use a strict filtering strategy devised for large database. This will speed the search at a potential cost to sensitivity.
    -   `-E`: report target sequences with an E-value of 0.001.

Finally from all these output files we need to filter out the rRNA reads:

```
perl main_get_sequence_length.pl cow rRNA
perl main_get_infernal_fromfile_1tophit.pl cow pairs 1 0.001 90
perl main_select_reads_fromfile.pl cow rRNA infernal pairs
```

**Notes**:

-   The script parameters you would use are given below:
    -   1 - apply cutoff values.
    -   0.001 - maximal E-value is 0.001
    -   90 - percentage of identity is 90%

***Question: How many rRNA sequences were identified? How many reads are now remaining?***

-   clue - examine the files cow1\_qual\_unique\_n\_rRNA.fastq cow2\_qual\_unique\_n\_rRNA.fastq cow1\_qual\_unique\_rRNA.fastq cow2\_qual\_unique\_rRNA.fastq

There's a lot of rRNAs!!

Check read quality again with FastQC:

```
fastqc cow1_qual_unique_rRNA.fastq
fastqc cow1_qual_unique_n_rRNA.fastq
```

Again compare with the previous report to identify differences:

-   Basic Statistics
-   Per base sequence quality
-   Overrepresented sequences

### Step 4. Remove host reads

To identify and filter host reads (here reads of bovine origin) we use the Burrows Wheeler aligner (BWA) tool to search against a database of cow sequences. For our purposes we use a cow genome database, downloaded from Ensembl (ftp://ftp.ensembl.org/pub/release-80/fasta/bos\_taurus/cds/Bos\_taurus.UMD3.1.cds.all.fa.gz). Normally we would generate an index for these sequences using 'bwa index -a bwtsw cow\_cds.fa' and 'samtools faidx bow\_cds.fa', but we have already done that for you so you can perform the alignments for the reads using the following commands:

```
/usr/local/bwa-0.7.5a/bwa aln -t 4 cow_cds.fa cow1_qual_unique_n_rRNA.fastq > cow1_host.sai
/usr/local/bwa-0.7.5a/bwa aln -t 4 cow_cds.fa cow2_qual_unique_n_rRNA.fastq > cow2_host.sai
/usr/local/bwa-0.7.5a/bwa sampe cow_cds.fa cow1_host.sai cow2_host.sai cow1_qual_unique_n_rRNA.fastq cow2_qual_unique_n_rRNA.fastq > cow_host.sam
```

Then we use SAMTools to convert sam-formatted binary BWA output files and custom perl scripts to extract unmapped reads (which are now our set of putative bacterial mRNA's - congratulations!):

```
samtools view -bS cow_host.sam | samtools sort -n -o cow_host.bam
samtools view -F 4 cow_host.bam > cow_host.bwaout

perl main_read_samout.pl cow host bwa pairs
perl main_select_reads_fromfile.pl cow host bwa pairs
```

**Notes**:

-   This step does not actually identify any reads of bovine origin. However, the Infernal step did identify 1937 of 81955 rRNA reads that map to bovine LSU rRNAs!

### Step 5. De-dereplication

After removing rRNA and host reads, we need to replace the previously removed replicated reads back to our data set.

```
perl main_remove_derepli.pl cow
```

***Question: How many putative mRNA sequences were identified? How many unique mRNA sequences?***

-   clue - examine the files cow1\_mRNA.fastq, cow2\_mRNA.fastq, cow1\_qual\_unique\_n\_rRNA\_n\_host.fastq, cow2\_qual\_unique\_n\_rRNA\_n\_host.fastq

Again check read quality with FastQC:

```
fastqc cow1_mRNA.fastq
```

Please check if there are any changes from the following sections:

-   Basic Statistics
-   Per base sequence quality
-   Overrepresented sequences

### Step 6. Assembling reads

Previous studies have shown that assembling reads into larger contigs significantly increases our ability to annotate them through sequence similarity searches. Comparisons of various assembly methods have shown Trinity yields decent performance in terms of number of reads annotated after assembly (Celaj, A., Markle, J., Danska, J. and Parkinson, J. (2014) Comparison of assembly algorithms for improving rate of metatranscriptomic functional annotation. Microbiome. 2:39.). Here we will apply the Trinity pipeline to our set of putative mRNA's recovered at the end of Step 5.

1. Perform Trinity assembly:

For paired-end reads (which is why we kept two paired files throughout):

```
Trinity --seqType fq --left cow1_mRNA.fastq --right cow2_mRNA.fastq --CPU 8 --max_memory 10G --min_contig_length 75 --full_cleanup
```
**Notes**:

-   Trinity assembles reads into contigs which are placed into a file named "trinity\_out\_dir.Trinity.fasta". By entering "less trinity\_out\_dir.Trinity.fasta", you can see the format of contig sequences as follows:
    -   &gt;TRINITY\_DN179\_c0\_g1\_i1 len=284 path=\[523:0-283\] \[-1, 523, -2\]
    -   GACCGGCGCCTTAGCCCCTAAATTTTCATCCTGCCGTCGAGGCCCGACAAGACTATTTCC
    -   GATTTATCTGCACCGCTTGATCCTCAGATTCGGAACAACATCCTTGGAGCGATGTCTCGT
    -   CCCATCATCTGATGACGGGATTAAGCCAGTAATCTACTGTACTTCGATTAGGCAGCGAGA
    -   GCGTAATTGTTTTCGCCAATTAAATTTTTGTTCACTCAGATTAAAGAGCTAGCCAACGAG
    -   GCTCTGCGTGCTTACGTACCATCTCAGCCTGCTGTCAAATCCAG

<!-- -->

-   Because Trinity headers are not consistent between runs, we have to cheat here slightly to ensure that our named contigs are consistent with DIAMOND output that we have pregenerated (due to time constraints) in a subsequent annotation step and will rely on a pregenerated contig assembly file termed "cow\_contigs.fasta".

<!-- -->

-   The command line parameters are:
    -   `--seqType`: type of reads: ( fa, or fq ).
    -   `--CPU`: number of CPUs to use is 8.
    -   `--max_memory`: max memory to use by Trinity is 10GB.
    -   `--min_contig_length`: .
    -   `--full_cleanup`: remove the temporary folder and results.

2. Extract singleton reads to a fastq format file:

In order to extract unassembled reads, i.e. singletons, we need to map all putative mRNA reads to our set of assembled contigs by BWA. Unmapped reads represent our set of singletons.

First, we need to build an index to allow BWA to search against our set of contigs:

```
/usr/local/bwa-0.7.5a/bwa index -a bwtsw cow_contigs.fasta
samtools faidx cow_contigs.fasta
```

Next we attempt to map the entire set of putative mRNA reads to this contig database:

```
/usr/local/bwa-0.7.5a/bwa aln -t 4 cow_contigs.fasta cow1_mRNA.fastq > cow1_trinity.sai
/usr/local/bwa-0.7.5a/bwa aln -t 4 cow_contigs.fasta cow2_mRNA.fastq > cow2_trinity.sai
/usr/local/bwa-0.7.5a/bwa sampe cow_contigs.fasta cow1_trinity.sai cow2_trinity.sai  cow1_mRNA.fastq cow2_mRNA.fastq > cow_trinity.sam
samtools view -bS cow_trinity.sam | samtools sort -n -o cow_trinity.bam
samtools view -F 4 cow_trinity.bam > cow_trinity.bwaout
```

We then extract singletons into a fastq format file for subsequent processing:

```
perl main_read_samout.pl cow assembly bwa pairs
perl main_select_reads_fromfile.pl cow assembly bwa pairs
perl main_get_sequence_length.pl cow singletons
```

Finally we generate a mapping table in which each contig is associated with the number of reads used to assemble that contig. This table is useful for determining how many reads map to a contig and is used for determining relative expression (see Steps 6 and 8).

```
perl main_get_maptable_contig.pl cow assembly
```
**Notes**:

-   The format in the file “cow\_contigs\_IDs\_length.txt” is \[contigID \#reads length\].
-   From the following files we observe -
    -   cow\_contigs.fasta: 297 contigs = 972 reads
    -   cow1\_singletons.fastq: 5348 reads (these include the merged reads from the FLASH step above)
    -   cow2\_singletons.fastq: 953 reads

Your numbers may differ from these as the algorithm that BWA uses can alter mapping from run to run.

Note the file cow1\_singletons.fastq contains many more reads than cow2\_singletons.fastq - this is an artifact from the earlier step of merging reads, all merged reads were added to the file of unmerged 5` reads.

### Step 7. Annotate reads to known genes/proteins

This is the step where we attempt to infer the origins of the putative microbial mRNA reads. In our pipeline we rely on a tiered set of sequence similarity searches of decreasing accuracy - BWA, BLAT and DIAMOND. While BWA and BLAT provide high stringency, sequence diversity that occurs at the nucleotide level results in few matches observed for these processes. Nonetheless they are quick. To avoid the problems of diversity that occur at the level of nucleotide, particularly in the absence of reference microbial genomes, we use DIAMOND searches to provide more sensitive peptide-based searches, which are less prone to sequence changes between strains.

Since BWA and BLAT utilize nucleotide searches, we rely on a microbial genome database that we obtained from the NCBI, (ftp://ftp.ncbi.nlm.nih.gov/genomes/archive/old_refseq/Bacteria/all.ffn.tar.gz), which contains 5231 ffn files (maybe more now!). We then merge all 5231 ffn files into one fasta file "microbial\_all\_cds.fasta" and build indexes for this database to allow searching via BWA and BLAT. For DIAMOND searches we use the Non-Redundant (NR) protein database also from NCBI: (ftp://ftp.ncbi.nih.gov/blast/db/FASTA/nr).

**Notes**:

-   the commands used to build the indexed databases are as follows - you don't need to do these!
    -   bwa index -a bwtsw microbial\_all\_cds.fasta
    -   samtools faidx microbial\_all\_cds.fasta
    -   makeblastdb -in microbial\_all\_cds.fasta -dbtype nucl
    -   diamond makedb -p 8 --in nr -d nr

<!-- -->

-   If you got the error message: "Cannot allocate memory", or the running speed is very slow, especially while doing BWA or DIAMOND mapping, you can skip the steps and use our precomputed files from the tar file "files2out.tar.gz". DIAMOND in particular may take a long time to run.
    -   For example, to extract "cow\_contigs.sam" file, you can use the command "tar -xzf files4out.tar.gz cow\_contigs.sam".

**BWA searches against microbial genome database**

for contigs:

```
/usr/local/bwa-0.7.5a/bwa aln -t 4 $BLASTDB/microbial_all_cds.fasta cow_contigs.fasta > cow_contigs.sai
/usr/local/bwa-0.7.5a/bwa samse $BLASTDB/microbial_all_cds.fasta cow_contigs.sai cow_contigs.fasta > cow_contigs.sam
samtools view -bS cow_contigs.sam | samtools sort -n -o cow_contigs.bam
samtools view -F 4 cow_contigs.bam > cow_contigs_micro_cds.bwaout

perl main_read_samout.pl cow microgenes bwa contigs micro_cds
perl main_select_reads_fromfile.pl cow microgenes bwa contigs micro_cds
```

for singletons:

```
/usr/local/bwa-0.7.5a/bwa aln -t 4 $BLASTDB/microbial_all_cds.fasta  cow1_singletons.fastq > cow1_singletons.sai
/usr/local/bwa-0.7.5a/bwa aln -t 4 $BLASTDB/microbial_all_cds.fasta  cow2_singletons.fastq > cow2_singletons.sai
/usr/local/bwa-0.7.5a/bwa sampe $BLASTDB/microbial_all_cds.fasta cow1_singletons.sai  cow2_singletons.sai cow1_singletons.fastq cow2_singletons.fastq > cow_singletons.sam
samtools view -bS cow_singletons.sam | samtools sort -n -o cow_singletons.bam
samtools view -F 4 cow_singletons.bam > cow_singletons_micro_cds.bwaout

perl main_read_samout.pl cow microgenes bwa singletons micro_cds
perl main_select_reads_fromfile.pl cow microgenes bwa singletons micro_cds
```

**Notes**:

-   The contig searches rely on the 'single end' (samse) mode of searching, while the singleton searches rely on the 'paired end' (sampe) mode of searching. This is one reason why we have persisted with these two types of files through this pipeline.
-   Here we are only taking one gene per contig, but it is possible that contigs may have more than one genes (e.g. co-transcribed genes).

**BLAT searches against microbial genome database**

Because the microbial genome database is very large, we can run into "out-of-memory" features(!) when running BLAT. We therefore split the database into two sub-databases, i.e. "microbial\_all\_cds\_1.fasta" and "microbial\_all\_cds\_2.fasta". After building the corresponding indexed databases, we then issue the following commands:

for contigs:

```
/usr/local/bin/blat -noHead -minIdentity=90 -minScore=50 $BLASTDB/microbial_all_cds_1.fasta  cow_contigs_n_micro_cds.fasta -fine -q=rna -t=dna -out=blast8 cow_contigs_1.blatout
/usr/local/bin/blat -noHead -minIdentity=90 -minScore=50 $BLASTDB/microbial_all_cds_2.fasta  cow_contigs_n_micro_cds.fasta -fine -q=rna -t=dna -out=blast8 cow_contigs_2.blatout
cat cow_contigs_1.blatout cow_contigs_2.blatout > cow_contigs_n_micro_cds.blatout
```

for singletons:

```
/usr/local/bin/blat -noHead -minIdentity=90 -minScore=50 $BLASTDB/microbial_all_cds_1.fasta  cow1_singletons_n_micro_cds.fasta -fine -q=rna -t=dna -out=blast8 cow_singletons1_1.blatout
/usr/local/bin/blat -noHead -minIdentity=90 -minScore=50 $BLASTDB/microbial_all_cds_2.fasta  cow1_singletons_n_micro_cds.fasta -fine -q=rna -t=dna -out=blast8 cow_singletons1_2.blatout    
cat cow_singletons1_1.blatout cow_singletons1_2.blatout > cow1_singletons_n_micro_cds.blatout

/usr/local/bin/blat -noHead -minIdentity=90 -minScore=50 $BLASTDB/microbial_all_cds_1.fasta  cow2_singletons_n_micro_cds.fasta -fine -q=rna -t=dna -out=blast8 cow_singletons2_1.blatout
/usr/local/bin/blat -noHead -minIdentity=90 -minScore=50 $BLASTDB/microbial_all_cds_2.fasta  cow2_singletons_n_micro_cds.fasta -fine -q=rna -t=dna -out=blast8 cow_singletons2_2.blatout
cat cow_singletons2_1.blatout cow_singletons2_2.blatout > cow2_singletons_n_micro_cds.blatout
```

**Notes**:

-   The command line parameters are:
    -   `-noHead`: Suppresses .psl header (so it's just a tab-separated file).
    -   `-minIdentity`: Sets minimum sequence identity is 90%.
    -   `-minScore`: Sets minimum score is 50. This is the matches minus the mismatches minus some sort of gap penalty.
    -   `-find`: For high-quality mRNAs.
    -   `-q`: Query type is RNA sequence.
    -   `-t`: Database type is DNA sequence.

<!-- -->

-   The running speed of blat is relatively slow. To save your time, you can skip the blat mapping steps by extracting corresponding blatout files:
    -   tar -zxf files4out.tar.gz cow\_contigs\_n\_micro\_cds.blatout cow1\_singletons\_n\_micro\_cds.blatout cow2\_singletons\_n\_micro\_cds.blatout

We then use the following scripts to postprocess BLAT mapping results:

for contigs:

```
perl main_sort_blastout_fromfile.pl cow n_micro_cds blat contigs 10
perl main_get_blast_fromfile_1tophit.pl cow micro_cds blat contigs 1 100 85 65 60
perl main_select_reads_fromfile.pl cow microgenes_blat blat contigs micro_cds
```

for singletons:

```
perl main_sort_blastout_fromfile.pl cow n_micro_cds blat singletons 10
perl main_get_blast_fromfile_1tophit.pl cow micro_cds blat singletons 1 100 85 65 60
perl main_select_reads_fromfile.pl cow microgenes_blat blat singletons micro_cds
```

**Notes**:

-   The script parameters are:
    -   10 - maximal number of hits
    -   1 - apply cutoff values; if 0, there is no cutoffs but pick the first top hit
    -   100 - length of query is 100
    -   85 - percentage of identity is 85
    -   65 - percentage of overlap is 65
    -   60 - bit score is 60

**DIAMOND against the non-redundant (NR) protein DB**

DIAMOND is a BLAST-like local aligner for mapping translated DNA query sequences against a protein reference database (BLASTX alignment mode). The speedup over BLAST is up to 20,000 on short reads at a typical sensitivity of 90-99% relative to BLAST depending on the data and settings. However, for our dataset running time is still long (timing scales by size of reference database; not so much by number of reads) so please use the 3 precomputed files.

```
tar -zxf files4out.tar.gz cow_contigs_nr.diamondout  cow1_singletons_nr.diamondout cow2_singletons_nr.diamondout
```

The DIAMOND commands are provided below for your information:

First you need to make a temporary folder first by using command
-   mkdir dmnd_tmp

Then to run DIAMOND you might use the following commands
-   for contigs
    -   diamond blastx -p 8 -d $BLASTDB/nr -q cow\_contigs\_n\_micro\_cds\_rest.fasta -a cow\_contigs\_nr.matches -t dmnd\_tmp -e 10 -k 10
    -   diamond view -a cow\_contigs\_nr.matches.daa -o cow\_contigs\_nr.diamondout -f tab
-   for singletons:
    -   diamond blastx -p 8 -d $BLASTDB/nr -q cow1\_singletons\_n\_micro\_cds\_rest.fasta -a cow1\_singletons\_nr.matches -t dmnd\_tmp -e 10 -k 10
    -   diamond view -a cow1\_singletons\_nr.matches.daa -o cow1\_singletons\_nr.diamondout -f tab
    -   diamond blastx -p 8 -d $BLASTDB/nr -q cow2\_singletons\_n\_micro\_cds\_rest.fasta -a cow2\_singletons\_nr.matches -t dmnd\_tmp -e 10 -k 10
    -   diamond view -a cow2\_singletons\_nr.matches.daa -o cow2\_singletons\_nr.diamondout -f tab

**Notes**:

-   The command line parameters are:
    -   `-p`: Number of threads to use in the search is 8.
    -   `-q`: Input file name.
    -   `-d`: Database name.
    -   `-e`: Expectation value (E) threshold for saving hits.
    -   `-k`: Maximum number of aligned sequences to keep is 10.
    -   `-t`: Temporary folder.
    -   `-o`: Output file name.
    -   `-f`: Output file is in a tabular format.

From the output of these searches, you next need to extract the top matched proteins using the following scripts:

```
perl main_get_blast_fromfile_tophits.pl cow nr diamond contigs 1 100 85 65 60
perl main_sort_blastout_fromfile.pl cow nr diamond singletons 10
perl main_get_blast_fromfile_tophits.pl cow nr diamond singletons 1 100 85 65 60
```

**Notes**

-   Here we consider a match if 85% sequence identity over 65% of the read length - this can result in very poor e-values (E = 3!) but the matches nonetheless appear reasonable.
-   We see a lot of 'Errors' of Entries not being found in the database - this arises because our precomputed search was against a database of non-redundant proteins, many of which are not found in the more limited non-redundant database of bacterial proteins we provide here.

Because the non-redundant protein database contains entries from many species, including eukaryotes, we often find that sequence reads can match multiple protein with the same score. From these multiple matches, we currently select the first (i.e. 'top hit') that derives from a bacteria. As mentioned in the metagenomics lecture, more sophisticated algorithms could be applied, however our current philosophy is that proteins sharing the same sequence match are likely to possess similar functions in any event; taxonomy is a seperate issue however! Again, due to the size of the output file and the processing time, we will rely on the use of pre-computed files.

```
tar -zxf files4out.tar.gz cow_contigs_nr_diamond_hitsID_sub.txt  cow_contigs_nr_diamond_pairs_sub.txt cow_singletons_nr_diamond_hitsID_sub.txt  cow_singletons_nr_diamond_pairs_sub.txt
```

If you were going to perform these steps manually you would use the following commands:

-   perl main\_get\_blast\_fromfile\_1topbachit.pl cow nr diamond contigs
-   perl main\_get\_blast\_fromfile\_1topbachit.pl cow nr diamond singletons


We then generate a sequence file of mapped microbial *genes* from the BWA and BLAT searches:

```
perl main_get_microbial_cds_sub.pl cow
perl main_get_sequence_length.pl cow micro_cds_sub
```

As well as a sequence file of mapped *proteins* from the DIAMOND searches:

```
perl main_get_nr_sub.pl cow
perl main_get_sequence_length.pl cow nr_sub
```

**SUMMARY**:

In order to know the number of mapped reads at different processing steps, you can use the following commands:

```
perl main_get_maptable_contig.pl cow bwa
perl main_get_maptable_contig.pl cow blat
perl main_get_maptable_contig.pl cow diamond

grep ">"  microbial_cds_sub.fasta | wc -l
grep ">"  nr_all_sub.fasta | wc -l
```

**Note**:

-   BWA: Total number of mapped-reads = 11 reads
-   BLAT: Total number of mapped-reads = 609 reads
-   DIAMOND: Total number of mapped-reads = 1255 reads

<!-- -->

-   Total number of mapped micro\_cds genes = 390
-   Total number of mapped nr proteins = 966

The numbers can change from run to run due to the Trinity assembly feature noted earlier.

Thus of ~6100 reads of putative microbial mRNA origin, we can annotate only ~1800 of them!! This is not uncommon for many microbiome samples without good reference sequences.

### Step 8. Map identified genes to a "system" dataset for network visualization - here a protein-protein interaction map based on E. coli proteins.

To help interpret our metatranscriptomic datasets from a functional perspective, we rely on mapping our data to functional networks such as metabolic pathways and maps of protein complexes. Here we will use a previously published map of functional protein-protein interactions (PPI) constructed for E. coli ([Peregrín-Alvarez JM. *et al.*, PLoS Comput Biol. 2009](http://www.ncbi.nlm.nih.gov/pubmed/19798435)) as a proxy to get a systems-level view of annotated reads. While it would be nice to have access to a 'pan-bacterial' protein interaction network to account for complexes from different species, such datasets do not currently exist.

To begin, we need to first match our annotated genes (from Step. 7) to E. coli homologs.

For microbial *genes* identified through our BWA and BLAT searches:

```
diamond blastx -p 8 -d $BLASTDB/EcoliMG1655_std -q microbial_cds_sub.fasta  -a microbial_cds_sub_ecoli_ppi.matches -t dmnd_tmp -e 10 -k 10 
diamond view -a microbial_cds_sub_ecoli_ppi.matches.daa  -o microbial_cds_sub_ecoli_ppi.diamondout -f tab
perl main_get_blast_fromfile_1tophit.pl cow ecoli_ppi diamond genes 0
```

For *proteins* identified through our DIAMOND searches:

```
diamond blastp -p 8 -d $BLASTDB/EcoliMG1655_std -q nr_all_sub.fasta  -a nr_all_sub_ecoli_ppi.matches -t dmnd_tmp -e 10 -k 10 
diamond view -a nr_all_sub_ecoli_ppi.matches.daa -o nr_all_sub_ecoli_ppi.diamondout -f tab
perl main_get_blast_fromfile_1tophit.pl cow ecoli_ppi diamond proteins 0
```

**Notes**:

-   the output files are "microbial\_cds\_sub\_ecoli\_ppi\_pairs.txt" and 'nr\_all\_sub\_ecoli\_ppi\_pairs.txt"

We then need to generate a "PPI\_pairs.txt" mapping file which lists E. coli homolog (we use the E. coli 'b'-number as the sequence classifier) for each of our genes/proteins:

```
perl main_combine_PPI_results.pl cow
```

### Step 9. Generate normalized expression values associated with each gene

We have removed low quality, adaptors, rRNA and host sequences and annotated reads to the best of our ability - now lets summarize our findings. We do this by looking at the relative expression of each of our genes in our microbiome. First we generate a mapping table, which links our gene and proteins identified in our BWA, BLAT and DIAMOND mappings with their respective taxonomic information (NCBI taxon ID, species name and phylum). This enables us to identify which species are contributing which functions to the microbiome:

```
perl main_get_taxonID_microbial_cds.pl cow
perl main_get_phylum.pl cow micro_cds

perl main_get_taxonID_nr.pl cow
perl main_get_phylum.pl cow nr
```

Then for each gene and protein, we calculate a normalized expression value (Reads Per Kilobase of Sequence Mapped - RPKM):

```
perl main_get_mapped_genesID_counts.pl cow micro_cds
perl main_get_mapped_genesID_counts.pl cow nr

perl main_get_mapped_gene_table.pl cow micro_cds
perl main_get_mapped_gene_table.pl cow nr

perl main_get_mapped_gene_table_RPKM.pl cow
```

**Notes**:

-   The final output file is named "cow\_table\_RPKM\_all.txt" and has the following format:
    -   \[geneID/proteinID, length, \#reads, taxonID, specie, phylum, RPKM, PPI\]
    -   gi\|110832861\|ref\|NC\_008260.1\|:414014-415204 1191 1 393595 Alcanivorax borkumensis SK2 gammaproteobacteria 450.4456 b3339

<!-- -->

-   There are 1874 reads mapping to 1356 microbial genes.

***Question: have a look at this file, what are the most highly expressed genes? Which phylum appears most active?***

### Step 10. Visualize the results using an E. coli map of protein-protein interactions as a scaffold in Cytoscape.

To visualize our processed microbiome dataset in the context of the E. coli PPI network, we use the network visualization tool - Cytoscape together with the enhancedGraphics plugin. Some useful commands for loading in networks, node attributes and changing visual properties are provided below (there are many cytoscape tutorials available online).

**Open a Cytoscape session file (.cys)**

-   Select File -&gt; Open -&gt; Select the session file and click Open.

**Loading a node attribute text file (.txt) - this will map attributes to nodes in your network which you can subsequently visualize**

-   Select File -&gt; Import -&gt; Table -&gt; File -&gt; Select the node file and click Open
-   Select Key Column for network (shared name),
-   Select Show Mapping Opteins -&gt; Select the primary key column in table and click OK

**Changing node properties - this changes the visual properties of the nodes - here size**

-   Select Style on Control Panel -&gt; Select Node tag at the bottom -&gt; Select Size -&gt; Select Column as RPKM -&gt; Select Mapping Type as Continuous Mapping -&gt; Double click on the Current Mapping to open Continuous Mapping Editor for Node Size -&gt; Select your preferred values

**Changing edge properties - this changes visual properties of edges connecting nodes - here width of lines**

-   Select Style on Control Panel -&gt; Select Edge tag at the bottom -&gt; Select Width -&gt; Select Column as Scores -&gt; Select Mapping Type as Continuous Mapping -&gt; Double click on the Current Mapping to open Continuous Mapping Editor for Edge Width -&gt; Select your preferred values

**Installing Apps - Cytoscape features the ability to load in 3rd party applications that provide additional functionality**

-   Select Apps —&gt; select App Manager -&gt; Type in enhancedGraphics in the Search box -&gt; Select enhancedGraphics and click Install

**Basic Network Navigation**

-   Use the zooming buttons located on the toolbar to zoom in and out of the interaction network shown in the current network display.
-   Using the scroll wheel of your mouse, you can zoom in by scrolling up and zoom out by scrolling downwards.
-   Select nodes on the current network display, you will see the nodes' attributes from the Table Panel (Node Table). Same for edges.

Here we will skip the steps of generating the node attribute file to map onto the E. coli PPI network - [cow_PPI.nodes.txt](https://github.com/bioinformatics-ca/bioinformatics-ca.github.io/raw/master/2016_workshops/metagenomics/Cow_PPI.nodes.txt) from "cow\_table\_RPKM\_all.txt", however for your information the steps involve:

-   predefining taxonomic categories (here we use the following 12 phylum categories: archaea, protozoan, bacteria, actinobacteria, bacteroidetes, gammaproteobacteria, deltaproteobacteria, betaproteobacteria, alphaproteobacteria, clostridiales, leuconostocaceae, lactobacillaceae, but you could define these categories to fit your microbiome).

<!-- -->

-   calculate RPKM values of each ecoli protein, for every phylum category, by adding RPKM values of the protein's mapped genes/proteins.

<!-- -->

-   generate a node attribute file which is a tab-delimited table with a format as follows:
    -   the first line is the header - you could use:

```
ecoli_protein    b#    RPKM    piechart        archaea protozoan       bacteria        
actinobacteria  bacteroidetes   gammaproteobacteria     deltaproteobacteria     betaproteobacteria
alphaproteobacteria     clostridiales   leuconostocaceae        lactobacillaceae
```

-   subsequent lines then use the format, with the final numbers being the RPKM associated with each taxon:

```
tuf    b3339   106.98  piechart: attributelist="archaea,protozoan,bacteria,actinobacteria,bacteroidetes,
gammaproteobacteria,deltaproteobacteria,betaproteobacteria,alphaproteobacteria,clostridiales,
leuconostocaceae,lactobacillaceae" colorlist="#FFA500,#C0C0C0,#EDF252,#0000FF,#FF00FF,#2C94DE,#ED4734,
#00FFFF,#FFCCFF,#34C400,#A52A2A,#663366" showlabels=false  0   0   45.89   6.86    
20.77  7.35    2.3 0   4.63    19.18   0   0
```

Once the node attribute file has been generated, we provide two network files (one based on cell wall biogenesis proteins and one based on transporters) onto which these attributes can be mapped: [ecoli_PPI_cellwall.cys](https://github.com/bioinformatics-ca/bioinformatics-ca.github.io/raw/master/2016_workshops/metagenomics/Ecoli_PPI_cellwall.cys) or [ecoli_PPI_transporter.cys](https://github.com/bioinformatics-ca/bioinformatics-ca.github.io/raw/master/2016_workshops/metagenomics/Ecoli_PPI_transporter.cys). While we recommend using the precomputed cytoscape files listed below - you could use these attribute files by downloading them from your module5 directory onto your laptop via scp or winscp. Once downloaded these files can be opened using Cytoscape installed in your local computer. To import node attributes (note you need to have the Ecoli PPI cytoscape file loaded first!):

```
1) select File -> Import -> Table -> File, select "cow_PPI.nodes.txt" from your working folder,
click OK from the prompting window. 
2) from Control Panel, select Style -> Properties -> Paint -> Custom Paint 1 -> Custom Graphics 1, 
3) click Custom Graphics 1, select piechart for Column, and select Passthrough Mapping for Mapping Type. 
```

**Notes**:

-   Two cytoscape files with node attributes precalculated are provided for your convenience the first focuses on proteins involved in cell wall biogenesis, the second focuses on proteins involved in transport activities, [ecoli_PPI_cellwall_cow.cys](https://github.com/bioinformatics-ca/bioinformatics-ca.github.io/raw/master/2016_workshops/metagenomics/Ecoli_PPI_cellwall_cow.cys) and [ecoli_PPI_transporter_cow.cys](https://github.com/bioinformatics-ca/bioinformatics-ca.github.io/raw/master/2016_workshops/metagenomics/Ecoli_PPI_transporter_cow.cys), open them up and have a play with different visualizations and different layouts - compare the circular layouts with the spring embedded layouts for example. If you want to go back to the original layout (created manually - yes each node was selected and dragged into position to group e.g. proteins involved in the same transport activity!) then you will have to reload the file
-   Cytoscape can be tempermental. If you don't see piecharts for the nodes, they appear as blank circles, you can show these manually. Under the 'properties' panel on the left, there is an entry labelled 'Custom Graphics 1'. Double click the empty box on the left (this is for default behaviour) - this will pop up a new window with a choice of 'Images' 'Charts' and 'Gradients' - select 'Charts', choose the chart type you want (pie chart or donut for example) and select the different bacterial taxa by moving them from "Available Columns" to "Selected Columns". Finally click on 'Apply' in bottom right of window (may not be visible until you move the window).

Questions:
- Which genes are most highly expressed in these two systems?
- Which taxa are responsible for most gene expression?
- Can you identify sub-systems (groups of interacting genes) that display anomalous taxonomic profiles?
- Think about how you might interpret these findings; for example are certain taxa responsible for a specific set of genes that operate together to fulfill a key function?
- Can you use the gene annotations to identify the functions of these genes through online searches?
- Think about the implications of sequence homology searches, what may be some caveats associated with interpreting these datasets?
