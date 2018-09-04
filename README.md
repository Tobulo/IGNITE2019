# ICME2019
A practical on genome assembly proposed during the 2019 ICME

First, let us install the miniconda3 environment using `wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh` then `sh Miniconda3-latest-Linux-x86_64.sh`.
To activate the new bash variables: `source .bashrc` (alternatively, one can just open a new terminal window).
We need to add the bioconda channel: `conda config --add channels bioconda`. Let us add two other channels as well: `conda config --add channels conda-forge` and `conda config --add channels r`.

Bioconda is a great resource of bioinformatic packages (to see the current list, go to https://bioconda.github.io/recipes.html).
Now we will use bioconda to install our first (meta)genome assembler: IDBA_UD (https://bioconda.github.io/recipes/idba/README.html).
In addition to the assembler IDBA_UD, the package idba contains also some useful tools that we will be using during our practical: sim_reads (to simulate Illumina read pairs from a reference sequence), fq2fa (to convert FASTQ files into FASTA and merge them), and raw_n50 (to quickly estimate the N50 statistics of a genome assembly).
Let us install another useful programme, KAT: https://bioconda.github.io/recipes/kat/README.html


