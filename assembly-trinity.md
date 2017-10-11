# De novo transcriptome assembly with Trinity

This tutorial will use mRNAseq reads from a small subset of data from [Nematostella vectensis](https://en.wikipedia.org/wiki/Starlet_sea_anemone) [(Tulin et al., 2013)](https://evodevojournal.biomedcentral.com/articles/10.1186/2041-9139-4-16). 

Original RNAseq workflow protocol [here](https://khmer-protocols.readthedocs.io/en/ctb/mrnaseq/), more updated protocol [here](http://eel-pond.readthedocs.io/en/latest/).

## Installation

On a Jetstream instance, run the following commands to update the base
software:

```
sudo apt-get update && \
sudo apt-get -y install screen git curl gcc make g++ python-dev unzip \
  default-jre pkg-config libncurses5-dev r-base-core r-cran-gplots \
  python-matplotlib python-pip python-virtualenv sysstat fastqc \
  trimmomatic bowtie samtools blast2 wget bowtie2 openjdk-8-jre \
  hmmer ruby
```

Install Trinity:

```
cd ${HOME}

wget https://github.com/trinityrnaseq/trinityrnaseq/archive/Trinity-v2.3.2.tar.gz \
    -O trinity.tar.gz
tar xzf trinity.tar.gz
cd trinityrnaseq*/
make |& tee trinity-build.log
```

Assuming it succeeds, modify the path appropriately:

```
echo export PATH=$PATH:$(pwd) >> ~/.bashrc
source ~/.bashrc
cd
```

You will also need to set the default Java version to 1.8

```
sudo update-alternatives --set java /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java
```

## Then, let's check we still have our reads from yesterday's QC lesson
```
set -u
printf "\nMy trimmed data is in $PROJECT/quality/, and consists of $(ls -1 ${PROJECT}/quality/*.qc.fq.gz | wc -l) files\n\n"
set +u

```
where set -u should let you know if you have any unset variables, i.e. if the `$PROJECT` variable is not defined. 

If you see `-bash: PROJECT: unbound variable`, then you need to set the $PROJECT variable.  

```
export PROJECT=/mnt/work
```
and then re-run the `printf` code block.

NOTE: if you do not have files, please rerun quality trimming steps [here](quality-trimming.html)


## Running the Assembly!

Let's make another working directory for the assembly

```
cd ${PROJECT}
mkdir -p assembly
cd assembly
```

For paired-end data, Trinity expects two files, 'left' and 'right':

```
zcat ${PROJECT}/quality/*R1*.qc.fq.gz > ${PROJECT}/assembly/left.fq
zcat ${PROJECT}/quality/*R2*.qc.fq.gz > ${PROJECT}/assembly/right.fq
```

### Assembling with Trinity

Here is the assembly command!

```
cd ${PROJECT}/assembly
Trinity --left left.fq \
  --right right.fq --seqType fq --max_memory 14G \
  --CPU 2
```

Note that these last two parts (`--max_memory 14G --CPU 2`) configure the maximum amount of memory and CPUs to
use.  You can increase (or decrease) them based on what machines you are running on.

Once this completes, you'll have an assembled transcriptome in
`${PROJECT}/assembly/trinity_out_dir/Trinity.fasta`.

