# Evaluating your transcriptome assembly

We will be using Busco!

## Install

```
sudo apt-get -y install python3-dev hmmer unzip \
    ncbi-blast+ liburi-escape-xs-perl emboss liburi-perl \
    build-essential libsm6 libxrender1 libfontconfig1 \
    parallel libx11-dev python3-venv last-align transdecoder
```

Activate your python environment:

```
virtualenv -p python3 ~/bin/py3
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

In a text editor, open config.ini, replace path to hmmsearch executable to `/usr/bin/`

Put busco scripts in the path
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

Make a new directory: 

```
#set PROJECT=/mnt/work # reset PROJECT variable, if you logged out since we last used $PROJECT
cd ${PROJECT}
mkdir -p evaluation
cd evaluation
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
-i ../assembly/Trinity.fasta \
-o nema_busco_metazoa -l ~/busco/metazoa_odb9 \
-m transcriptome --cpu 2
```

Check the output:

```
cat run_nema_busco_metazoa/short_summary_nema_busco_metazoa.txt
```

Grab the full transcriptome, so we can compare
```
curl -O https://s3.amazonaws.com/public.ged.msu.edu/trinity-nematostella-raw.fa.gz
gunzip trinity-nematostella-raw.fa.gz
```

Run busco on the full transcriptome:
```
run_BUSCO.py \
-i trinity-nematostella-raw.fa \
-o nema_full_busco_metazoa -l ~/busco/metazoa_odb9 \
-m transcriptome --cpu 2
```

How does the full transcriptome compare?

Deactivate your virtual environment
``` 
deactivate
```

