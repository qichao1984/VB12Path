# VB12Path
# VB12Path: A Curated Functional Gene Database for Metagenomic Profiling of cobalamin synthesis pathways
Cobalamin (VB12) is of critical importance to both natural ecosystems and host health, but can only be synthesized by a small group of microorganisms. Shotgun metagenome sequencing has greatly facilitated profiling VB12 biosynthesis gene families and associated microbial taxa. However, there are many challenges for accurate profiling of functional gene families with public ortholog databases, such as inefficient database searching, difficulties in distinguishing homologous genes, and low coverage of VB12 synthesis genes and/or gene (sub)families. A comprehensive and accurate database for characterizing VB12 synthesis microbial communities from different environments is therefore in urgent need for metagenomic analysis. 

The developed VB12Path contains 60 key gene families and 519,827 representative sequences, and 21,154 homologous orthology groups were also included to reduce false positive sequence assignments.

The following files are included in VB12Path:

<b>1. VB12Path_2020.zip</b>: fasta format representative sequences obtained by clustering curated sequences at 100% sequence identity. This file can be used for "BLAST" searching VB12Path genes in shotgun metagenomes. Due to the file size limit of GitHub, we splited this file into three files. Windows users can directly decompress VB12_2020.zip using programs like winrar or the built in winzip program in windows explorer. For linux users, please follow the following steps to decompress this file: mv VB12_2020.zip VB12_2020.z03; cat VB12_2020.z* > VB12_2020.zip; zip -FF VB12_2020.zip --out VB12_2020-full.zip; unzip VB12_2020-full.zip

<b>2. id2gene.map.2021 and annotations.txt</b>: a mapping file that maps sequence IDs to gene names, only sequences belonging to VB12Path gene families are included. Sequences for VB12Path homologs are not included. This file is used to generate VB12Path profiles from BLAST-like results against the VB12Path database. 

<b>3. VB12Path_FunctionProfiler.PL</b>: a perl script for functional profiling of VB12 synthesis genes.

<b>4. VB12Path_TaxonomyProfiler.PL</b>: a perl script for taxonomical profiling of VB12 synthesis microbial communities

<b>5. Example files</b>: We provided three testing fastq files and corresponding sampleinfo file in the example folder so that users can test the scripts before running real large data.

<b>Cutoffs</b>: cutoffs (e.g. e-values and identities) can be modified at the beginning lines of the scripts. The default parameters (e.g. e-value of 1e-4) were relatively conserved, users can make modification according to their data. Usually, relaxed parameters may introduce false positives, while strict parameters may increase false negatives. To our best knowledge, no standard criteria is available in metagenomics. The parameters shall also be modified according to the data types. For example, for short reads at 36bp, an e-value cutoff of 10 are usually used. 

<b>DOWNLOAD/INSTALLATION</b>

The database and scripts can directly downloaded and used by git clone https://github.com/qichao1984/VB12Path.git. However, users need to install the following perl modules and third party softwares. 

<p><b>Dependencies and Tools</b></p>
<i>Perl modules that can be easily installed via cpan:</i>

List::Util

Getopt::Long

<i>Dependencies for VB12Path_FunctionProfiler.PL, currently supported database searching tools are:</i>

usearch: https://www.drive5.com/usearch/download.html

diamond: https://github.com/bbuchfink/diamond/releases

blast: ftp://ftp.ncbi.nlm.nih.gov/blast/executables/legacy.NOTSUPPORTED/2.2.26/blast-2.2.26-x64-linux.tar.gz

<i>Dependencies for VB12Path_TaxonomyProfiler.PL:</i>

seqtk: https://github.com/lh3/seqtk.git;

kraken2: https://github.com/DerrickWood/kraken2.git. Several options are available when building Kraken2 databases. We recommend to build a standard Kraken2 database. 

<b>Selection of Database Searching Tools</b>:
Three database searching tools, including the legacy ncbi blast, diamond, and usearch, are currently employed in the scripts. However, we recommend users to use diamond, which is faster than blast and supports large input files. 

<b>USAGE</b>

Before getting started, please modify both scripts (VB12Path_FunctionProfiler.PL, VB12Path_TaxonomyProfiler.PL) at lines 6-18 to specify the locations of third party tools and their parameters. If the tools are already in the system path, no revision is needed. By default, basic parameters are used for these tools. Users are encouraged to make revisions in cases of short reads and/or expecting more strict/relaxed results. We also encourage users to develop useful implementations based on VB12Path.

Note: Kraken2 database could be downloaded from https://ccb.jhu.edu/software/kraken2/index.shtml?t=downloads, or built locally. 

<b>Example for using VB12Path_FunctionProfiler.PL:</b>

perl VB12Path_FunctionProfiler.PL -d \<workdir\> -m \<diamond|usearch|blast\> -f \<filetype\> -s \<seqtype\> -si \<sample size info file\> -rs \<random sampling size\> -o \<outfile\>
  
Detailed explanations: 

-d : specify the directory where your fasta/fastq (or gzipped) files are located. 

-m : specify the database searching program you plan to use, currently diamond, usearch and blast are supported. 

-f : specify the extensions of your sequence files, e.g. fastq, fastq.gz, fasta,fasta.gz, fq, fq.gz, fa, fa.gz

-s : sequence type, nucl or prot

-si: a tab delimited file containing the sample/file name and the number of sequences they have, note that no file extensions should be included here.

-rs: specify the number of sequences for random subsampling, if not specified, the lowest number in -si will be used.

-o : the output file for VB12 synthesis gene profiles.   


<b>Example for using VB12Path_TaxonomyProfiler.PL:</b>

perl VB12Path_TaxonomyProfiler.PL -d \<workdir\> -m \<diamond|usearch|blast\> -f \<filetype\> -s \<seqtype\> -si \<sample size info file\> -rs \<random sampling size\> 
  
Detailed explanations: 

-d : specify the directory where your fasta/fastq (or gzipped) files are located. 

-m : specify the database searching program you plan to use, currently diamond, usearch and blast are supported. 

-f : specify the extensions of your sequence files, e.g. fastq, fastq.gz, fasta,fasta.gz, fq, fq.gz, fa, fa.gz

-s : sequence type, nucl or prot

-si: a tab delimited file containing the sample/file name and the number of sequences they have, note that no file extensions should be included here.

-rs: specify the number of sequences for random subsampling, if not specified, the lowest number in -si will be used.

<b>Example for sampleinfo.txt:</b>

sample_name1	\<the number of sequences\>

sample_name2	\<the number of sequences\>

sample_name3	\<the number of sequences\>

Reference tools:seqkit https://bioinf.shenwei.me/seqkit/usage/#seqkit

In the example folder, we provided three fastq files each containing 50000 sequences for users to test if the script and database run correctly. The output files were also provided in the example/output folder. 
Command for functional profiling: perl VB12Path_FunctionProfiler.PL -d example -m diamond -f fastq -s nucl -si example/sampleinfo.txt -rs 50000 -o example/VB12_functional_profile.txt
Command for taxonomic profiling: perl VB12Path_TaxonomyProfiler.PL -d example -m diamond -f fastq -s nucl -si example/sampleinfo.txt -rs 50000


<b>Output Files</b>

Functional gene profiles and taxonomic profiles at different levels are respectively generated by VB12Path_FunctionProfiler.PL and VB12Path_TaxonomyProfiler.PL. 

While we encourage users to perform sophisticated statistical analysis using these tables, the following basic statistical analyses are recommended before doing so. 

1. Ordination analyses (NMDS, PCoA, DCA etc. ) and visualization of the functional gene and taxonomic compositions related with VB12 biosynthesis.
2. Relative abundances of different gene families and taxonomic groups at phylum or somewhat high level. The relative abundances can be summarized in two different ways, i.e. based on random sampling depth and the total number of reads mapped to VB12Path. We recommend to calculate relative abundances based on random sampling depth. 
3. Correlate the compositions of VB12 gene families and taxonomic groups to environmental factors to reveal the potential environmental drivers affecting VB12 communities.
