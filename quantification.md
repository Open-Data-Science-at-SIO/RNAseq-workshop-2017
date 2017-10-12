# Analyzing RNAseq expression with Salmon

Based on the Eel-Pond RNAseq workflow protocol [here](http://eel-pond.readthedocs.io/en/latest/).

## Getting Started

If needed, start up a fresh Ubuntu 16.04 instance on Jetstream using the instructions [here](jetstream/boot.html).

On a Jetstream instance, run the following commands to update the base software:

```
sudo apt-get update && \
sudo apt-get -y install screen git curl gcc make g++ python-dev unzip \
        default-jre pkg-config libncurses5-dev r-base-core r-cran-gplots \
        python-matplotlib python-pip python-virtualenv sysstat fastqc \
        trimmomatic bowtie samtools blast2 wget bowtie2 openjdk-8-jre \
        hmmer ruby
```

---

## Installation

We will use [Salmon](http://salmon.readthedocs.org/en/latest/) to
quantify expression. Salmon is a new breed of software for quantifying RNAseq reads that is both really fast and takes
transcript length into consideration ([Patro et al. 2015](http://dx.doi.org/10.1038/nmeth.4197)).

For further reading, see

  * Intro blog post: http://robpatro.com/blog/?p=248
  * A 2016 blog post evaluating and comparing methods [here](https://cgatoxford.wordpress.com/2016/08/17/why-you-should-stop-using-featurecounts-htseq-or-cufflinks2-and-start-using-kallisto-salmon-or-sailfish/)
  * Salmon github repo [here](https://github.com/COMBINE-lab/salmon)


## Download and make Salmon available in the path 

```
cd
wget https://github.com/COMBINE-lab/salmon/releases/download/v0.8.2/Salmon-0.8.2_linux_x86_64.tar.gz
tar xvfz Salmon-0.8.2_linux_x86_64.tar.gz
export PATH=$PATH:$HOME/Salmon-0.8.2_linux_x86_64/bin 
```



## Then, let's check we still have our reads from yesterday's QC work
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

## Download or link an assembly

We can download a full assembly to use for mapping. This assembly was made with all Nematostella vectensis reads, rather than the subset we used in the [assembly](assembly-trinity.html) tutorial.

```
   cd ${PROJECT}
   mkdir -p quant
   cd quant
```

```
   curl -O https://s3.amazonaws.com/public.ged.msu.edu/trinity-nematostella-raw.fa.gz
   gunzip trinity-nematostella-raw.fa.gz
   ln -s trinity-nematostella-raw.fa trinity.nema.full.fasta
```

Note: if you prefer, you can download an annotated assembly we previously generated with these read subsets
```
   #ln -s ${PROJECT}/annotation/trinity.nema.fasta.dammit/trinity.nema.fasta.dammit.fasta ./trinity.nema.annot.fasta 
```

## Run Salmon

Build an index for your new transcriptome:

```
   salmon index --index nema --transcripts trinity.nema.full.fasta --type quasi
```
Next, link in the QC reads (produced in [quality](quality-trimming.html):

```
   ln -s ../data/*R1*.fastq.gz .
   ln -s ../data/*R2*.fastq.gz .
```

Then, run the salmon command:

```
  for R1 in *R1*.fastq.gz
  do
    sample=$(basename $R1 extract.fastq.gz)
    echo sample is $sample, R1 is $R1
    R2=${R1/R1/R2}
    echo R2 is $R2
    salmon quant -i nema -p 2 -l IU -1 <(gunzip -c $R1) -2 <(gunzip -c $R2) -o ${sample}quant
  done
```

This will create a bunch of directories named something like
`0Hour_ATCACG_L002001.quant`, containing a bunch of files. Take a
look at what files there are:

```
    find 0Hour_ATCACG_L002_R1_001* -type f
```

You should see::
```
    0Hour_ATCACG_L002_R1_001.extract.quant/quant.sf
    0Hour_ATCACG_L002_R1_001.extract.quant/aux_info/observed_bias.gz
    0Hour_ATCACG_L002_R1_001.extract.quant/aux_info/observed_bias_3p.gz
    0Hour_ATCACG_L002_R1_001.extract.quant/aux_info/fld.gz
    0Hour_ATCACG_L002_R1_001.extract.quant/aux_info/expected_bias.gz
    0Hour_ATCACG_L002_R1_001.extract.quant/aux_info/meta_info.json
    0Hour_ATCACG_L002_R1_001.extract.quant/cmd_info.json
    0Hour_ATCACG_L002_R1_001.extract.quant/logs/salmon_quant.log
    0Hour_ATCACG_L002_R1_001.extract.quant/libParams/flenDist.txt
    0Hour_ATCACG_L002_R1_001.extract.quant/lib_format_counts.json
    0Hour_ATCACG_L002_R1_001.extract.quant.counts
```

The two most interesting files are `salmon_quant.log` and
`quant.sf`. The latter contains the counts; the former contains the
log information from running things.

# Working with the counts

The `quant.sf` files actually contain the relevant information about
expression -- take a look

```
    head -20 0Hour_ATCACG_L002_R1_001.quant/quant.sf
```

The first column contains the transcript names, and the
fifth column is what DESeq2, edgeR, etc will want - the "raw counts".

# Other useful tutorials and references
https://github.com/ngs-docs/2015-nov-adv-rna/blob/master/salmon.rst
http://angus.readthedocs.io/en/2016/rob_quant/tut.html
https://2016-aug-nonmodel-rnaseq.readthedocs.io/en/latest/quantification.html

