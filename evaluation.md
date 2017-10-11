# Evaluating your transcriptome assembly

We will be using Transrate and Busco!

## Install

```
sudo apt-get -y install python3-dev hmmer unzip \
    ncbi-blast+ liburi-escape-xs-perl emboss liburi-perl \
    build-essential libsm6 libxrender1 libfontconfig1 \
    parallel libx11-dev python3-venv last-align transdecoder
```

BUSCO requires python 3. 
Create a virtual environment with python 3 and enter into it:

```
virtualenv -p python3 ~/bin/py3
source ~/bin/py3/bin/activate
```

## Transrate

[Transrate](http://hibberdlab.com/transrate/getting_started.html) serves two main purposes. It can compare two assemblies to see how similar they are. Or, it can give you a score which represents proportion of input reads that provide positive support for the assembly. We will use transrate to get a score for the assembly. Use the trimmed reads. For a further explanation of metrics and how to run the reference-based transrate, see the [documentation](http://hibberdlab.com/transrate/metrics.html) and the paper by [Smith-Unna et al. 2016](http://genome.cshlp.org/content/early/2016/06/01/gr.196469.115). 

### Install Transrate

```
cd 
sudo curl -SL https://bintray.com/artifact/download/blahah/generic/transrate-1.0.3-linux-x86_64.tar.gz | tar -xz
cd transrate-1.0.3-linux-x86_64 
./transrate --install-deps ref
rm -f bin/librt.so.1
echo 'export PATH=$PATH:"$HOME/transrate-1.0.3-linux-x86_64"' >> ~/bin/py3/bin/activate
source ~/bin/py3/bin/activate
```

### Install BUSCO

```
cd
git clone https://gitlab.com/ezlab/busco.git
pushd busco && python setup.py install && popd
```

```
cd ~/busco/config/
cp config.ini.default config.ini
```

Open the config file in a text editor (e.g. nano, vim) and replace path to hmmsearch executable with `/usr/bin/`

```
export PATH=$HOME/busco/scripts:$PATH
echo 'export PATH=$HOME/busco/scripts:$PATH' >> $HOME/.bashrc
```

Download the BUSCO databases
```
cd ~/busco/
curl -OL http://busco.ezlab.org/datasets/metazoa_odb9.tar.gz
tar -xvzf metazoa_odb9.tar.gz
```

Make a new directory and get the reads together:

```
cd ${PROJECT}
mkdir -p evaluation
cd evaluation

cat ${PROJECT}/quality/*R1*.qc.fq.gz > left.fq.gz
cat ${PROJECT}/quality/*R2*.qc.fq.gz > right.fq.gz
```

Transrate doesn't like pipes in sequence names. This version of Trinity doesn't output pipes into the sequence names, but others do. Let's just fix to make sure.

```
sed 's_|_-_g' ${PROJECT}/assembly/trinity_out_dir/Trinity.fasta > Trinity.fixed.fasta
```

Now, run the actual command:

```
transrate --assembly=Trinity.fixed.fasta --threads=2 \
--left=left.fq.gz \
--right=right.fq.gz \
--output=${PROJECT}/evaluation/nema
```

Questions:
* What is the transrate score?
* When you run the command above again with this transcriptome assembled from all of the reads in the Nematostella data set, does the score improve?

```
curl -O https://s3.amazonaws.com/public.ged.msu.edu/trinity-nematostella-raw.fa.gz
gunzip trinity-nematostella-raw.fa.gz
```

* How do the two transcriptomes compare with each other?

```
transrate --reference=Trinity.fixed.fasta --assembly=trinity-nematostella-raw.fa --output=full_v_subset
transrate --reference=trinity-nematostella-raw.fa --assembly=Trinity.fixed.fasta --output=subset_v_full
```

## BUSCO

* Eukaryota database used with 429 genes
* "Complete" lengths are within two standard deviations of the BUSCO group mean length

* Useful links:
  * Website: [http://busco.ezlab.org/](http://busco.ezlab.org/)
  * Paper: [Simao et al. 2015](http://bioinformatics.oxfordjournals.org/content/31/19/3210)
  * [User Guide](http://gitlab.com/ezlab/busco/raw/master/BUSCO_v2.0_userguide.pdf)

### Run the actual command:

```
run_BUSCO.py \
-i Trinity.fixed.fasta \
-o nema_busco_metazoa -l ~/busco/metazoa_odb9 \
-m transcriptome --cpu 2
```

Check the output:

```
cat run_nema_busco_metazoa/short_summary_nema_busco_metazoa.txt
```

How does the full transcriptome compare?


When you're finished, exit out of this virtual environment
```
deactivate
```

