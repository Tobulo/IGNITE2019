# IGNITE2019
A practical on genome assembly proposed during the 2019 IGNITE training event in Banyuls-sur-Mer.

First, let us install the miniconda3 environment using `wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh` then `sh Miniconda3-latest-Linux-x86_64.sh`.
To activate the new bash variables: `source .bashrc` (alternatively, one can just open a new terminal window).
We need to add the bioconda channel: `conda config --add channels bioconda`. Let us add two other channels as well: `conda config --add channels conda-forge` and `conda config --add channels r`.

Bioconda is a great resource of automatic recipes to build bioinformatic packages (https://doi.org/10.1038/s41592-018-0046-7; to see the current list of available recipes, go to https://bioconda.github.io/conda-recipe_index.html).
Now we will use bioconda to install a first (meta)genome assembler: IDBA_UD (https://bioconda.github.io/recipes/idba/README.html). IDBA_UD is a DBG(De Bruijn Graph)-based assembler that is able to use successively several k values (something very important when dealing with data with unequal coverage, such as metagenomic or single-cell datasets).

In addition to the assembler IDBA_UD, the package idba contains also some useful tools that we will be using during our practical: sim_reads (to simulate Illumina read pairs from a reference sequence), fq2fa (to convert FASTQ files into FASTA and merge them), and raw_n50 (to quickly estimate the N50 statistics of a genome assembly). To see a short help message including the options and switches of each of these programs, just type their name.

Let us install another useful programme, KAT: https://bioconda.github.io/recipes/kat/README.html. KAT allows you to perform all sorts of kmer-based analyses, and we will use it to assess the quality of our assemblies in a finer fashion than just by looking at N50s (type `kat comp` to get a short help message regarding this).

Other useful assemblers available in Bioconda that we will use during our practical are bwise on the one hand (a DBG-based assembler specially developped to deal with heterozygous diploid genomes in a computationally efficient way) and the minimap2-miniasm-racon series of tools for OLC (overlap-layout-consensus) assembly; let us install them as well.

## Beginner's walkthrough using an _Escherichia coli_ reference sequence
Create a folder called "AssemblyPractical", then enter it.

Download the sequence of E. coli strain Sakai: `wget http://seqphase.mpg.de/Sakai.fasta`. It is now in your folder, and you can look at the first 10 lines of it using `head Sakai.fasta` (to look at the first 100 lines, try `head -100 Sakai.fasta`). To find out the number of lines of Sakai.fasta, try `wc -l Sakai.fasta` (`wc` means "word count", and with -l it actually counts lines rather than words). For our practical to proceed faster, we will only use the first 10% of the genome: `head -7886 Sakai.fasta > toy.fasta`.

To simulate 50X coverage reads using sim_reads: `sim_reads --paired --depth 50 toy.fasta  toyD50.fasta`.

To assemble the reads using a single k value of 50: `idba_ud -r toyD50.fasta -o outputD50k50 --mink 50 --maxk 50`.

To make a kat PDF plot comparing the kmers in the reads and in the assembly you obtained (see https://kat.readthedocs.io/en/latest/walkthrough.html#genome-assembly-analysis-using-k-mer-spectra), first do `cd outputD50k50` then `kat comp -t 1 -p pdf ../toyD50.fasta contig.fa`. You may want to replot the graph using a diffent maximal k value of e.g. 80 using `kat plot spectra-cn -x 80 -p pdf kat-comp-main.mx`.

One interesting statistics that KAT outputs is the k-mer completeness percentage. You can also compute some basic assembly statistics on the contig file by running `raw_N50 contig.fa`. When the "true" genome sequence is known (as is the case here since we simulated our reads), a very useful way to assess the correctness of a given assembly is to map it on the starting genome and look for potential misassemblies (using minimap2 then minidot from the minasm package).

Let us assemble the reads using the default idba_ud series of kmer values, then compare with the previous result: `idba_ud -r toyD50.fasta -o outputD50`.

Now with Bwise: `Bwise -x toyD50.fasta -o outputBwise`
Among the ouputs of Bwise you can find GFA (graph fragment assembly) files. These can be visualized using Bandage (https://doi.org/10.1093/bioinformatics/btv383, also available in bioconda).

Finally, let us try assembly our toy dataset with minimap2 (overlap computation step) then miniasm (layout step) then racon (consensus step).


## Now it is your turn to play!
Pick up a biological or bioinformatic question you are interested in and try to solve it using simulation. Here are some suggestions:

- are fully random DNA sequences easier or more difficult to assemble compared with real biological sequences? To find it out,
you may want to try and simulate a random DNA sequence of the same length as our toy genome from above, then simulate reads from this random sequence and try assembling them. A useful resource to generate a random DNA sequence is http://www.navinlab.com/bioperl2/bioperl2/random_DNA.html.

- what is the effet of various levels of heterozygosity on the performance of genome assemblers? To find it out, you can use the programme msbar in the emboss package to mutate your toy DNA sequence, then generate an equal coverage of reads from both your original sequence and the mutated one, mix the two read datasets into one and assemble it.

- what is the effect of read length/error rate/library insert size (in the case of paired reads)/coverage depth on the performance of genome assemblers?

- what about long reads (Nanopore, PacBio)? Try one of the many simulators available, e.g. https://github.com/rrwick/Badread (you may want to use a longer reference genome, e.g. one entire bacterial genome) then assemble the reads using e.g. minimap2-miniasm-racon.





