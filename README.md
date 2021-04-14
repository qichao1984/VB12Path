# VB12Path
# VB12Path: A Curated Functional Gene Database for Metagenomic Profiling of cobalamin synthesis pathways
Cobalamin (VB12) is of critical importance to both natural ecosystems and host health, but can only be synthesized by a small group of microorganisms. Shotgun metagenome sequencing has greatly facilitated profiling VB12 biosynthesis gene families and associated microbial taxa. However, there are many challenges for accurate profiling of functional gene families with public ortholog databases, such as inefficient database searching, difficulties in distinguishing homologous genes, and low coverage of VB12 synthesis genes and/or gene (sub)families. A comprehensive and accurate database for characterizing VB12 synthesis microbial communities from different environments is therefore in urgent need for metagenomic analysis. 

The developed VB12Path contains 60 key gene families and 519,827 representative sequences, and 21,154 homologous orthology groups were also included to reduce false positive sequence assignments.

Four files are included in VB12Path:

<b>1. VB12_2020.zip</b>: fasta format representative sequences obtained by clustering curated sequences at 100% sequence identity. This file can be used for "BLAST" searching VB12Path genes in shotgun metagenomes. Due to the file size limit of GitHub, we splited this file into three files. Windows users can directly decompress VB12_2020.zip using programs like winrar or the built in winzip program in windows explorer. For linux users, please follow the following steps to decompress this file: mv VB12_2020.zip VB12_2020.z03; cat VB12_2020.z* > VB12_2020.zip; zip -FF VB12_2020.zip --out VB12_2020-full.zip; unzip VB12_2020-full.zip

<b>2. id2gene.map</b>: a mapping file that maps sequence IDs to gene names, only sequences belonging to VB12Path gene families are included. Sequences for VB12Path homologs are not included. This file is used to generate VB12Path profiles from BLAST-like results against the VB12Path database. 

<b>3. VB12Path_FunctionProfiler.PL</b>: a perl script for functional profiling of VB12 synthesis genes.

<b>4. VB12Path_TaxonomyProfiler.PL</b>: a perl script for taxonomical profiling of VB12 synthesis microbial communities

<b>DOWNLOAD/INSTALLATION</b>

git clone https://github.com/qichao1984/VB12Path.git

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

kraken2: https://github.com/DerrickWood/kraken2.git

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
