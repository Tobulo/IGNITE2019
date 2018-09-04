# ICME2019
A practical on genome assembly proposed during the 2019 ICME

First, let us install the miniconda3 environment using `wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh` then `sh Miniconda3-latest-Linux-x86_64.sh`.
To activate the new bash variables: `source .bashrc` (alternatively, one can just open a new terminal window).
We need to add the bioconda channel: `conda config --add channels bioconda`. Let us add two other channels as well: `conda config --add channels conda-forge` and `conda config --add channels r`.

Bioconda is a great resource of bioinformatic packages (to see the current list, go to https://bioconda.github.io/recipes.html).
Now we will use bioconda to install our first (meta)genome assembler: IDBA_UD (https://bioconda.github.io/recipes/idba/README.html). IDBA_UD is a DBG(De Bruijn Graph)-based assembler that is able to use successively several k values (something very important for metagenomics).

In addition to the assembler IDBA_UD, the package idba contains also some useful tools that we will be using during our practical: sim_reads (to simulate Illumina read pairs from a reference sequence), fq2fa (to convert FASTQ files into FASTA and merge them), and raw_n50 (to quickly estimate the N50 statistics of a genome assembly). To see a short help message including the options and switches of each of these programs, just type their name.

Let us install another useful programme, KAT: https://bioconda.github.io/recipes/kat/README.html. KAT allows you to perform all sorts of kmer-based analyses, and we will use it to assess the quality of our assemblies in a finer fashion than just by looking at N50s.

There are many other assemblers available in Bioconda. For instance you may want to install minimap2, minimap and racon for OLC (overlap-layout-consensus) assembly.

## First walkthrough using an _Escherichia coli_ reference sequence
Go to $CINECA_SCRATCH and create a folder called "AssemblyPractical", then enter it.
Download the sequence of E. coli strain Sakai: `wget http://seqphase.mpg.de/Sakai.fasta`. It is now in your folder, and you can look at the first 10 lines of it using `head Sakai.fasta` (to look at the first 100 lines, try `head -100 Sakai.fasta`). To find out the number of lines of Sakai.fasta, try `wc -l Sakai.fasta` (`wc` means "word count", and with -l it actually counts lines rather than words). For our practical to proceed faster, we will only use the first 10% of the genome: `head -7886 Sakai.fasta > toy.fasta`.
To simulate 50X coverage reads using sim_reads: `sim_reads --paired --depth 50 toy.fasta  toyD50.fasta`.
To assemble the reads using a single k value of 50: `idba_ud -r toyD50.fasta -o outputD50k50 --mink 50 --maxk 50`.
To make a kat PDF plot comparing the kmers in the reads and in the assembly you obtained (see https://kat.readthedocs.io/en/latest/walkthrough.html#genome-assembly-analysis-using-k-mer-spectra), first do `cd outputD50k50` then `kat comp -t 1 -p pdf ../toyD50.fasta contig.fa`.
You may want to replot the graph using a diffent maximal k value of e.g. 80 using `kat plot spectra-cn -x 80 -p pdf kat-comp-main.mx`.

## Now it is your turn to play!
Pick up a biological or bioinformatic question you are interested in and try to solve it using simulation.
If you want to try and simulate a random DNA sequence (to see how well reads generated from a random sequence assemble compared with reads generated from a real sequence), see http://www.navinlab.com/bioperl2/bioperl2/random_DNA.html.





