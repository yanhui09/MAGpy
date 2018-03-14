# 10 minute install

(not taking into account downloading and building the databases)

## Assuming we are in a brand new Ubuntu instance

### 1 Update ubuntu
```sh
sudo apt-get update
sudo apt install gcc g++ make
```

### 2 install usearch 

Make sure you download VERSION 5.2.32!

I can't do anything else to help you here - you need to register and you will be sent a link by email.

[link](https://www.drive5.com/usearch/download.html)

Make sure the executable is in your PATH

### 3 download and install conda
```sh
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh

# install it
sh Miniconda3-latest-Linux-x86_64.sh

# review license
# accept license
# accept or change home location
# yes to placing it in your path

# source .bashrc
source $HOME/.bashrc
```

### 4 clone this repo
```sh
git clone https://github.com/WatsonLab/MAGpy.git
```

### 5 create the main MAGpy environment
```sh
conda env create -f MAGpy/envs/MAGpy-3.5.yaml

# activate it
source activate MAGpy-3.5
```

### 6 download data and build indices
```sh
# Uniprot - for DEMO we use Swiss-Prot, but in reality
# you probably want TrEMBL
wget -q ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/complete/uniprot_sprot.fasta.gz 
diamond makedb --in uniprot_sprot.fasta.gz -d uniprot_sprot
rm uniprot_sprot.fasta.gz

# Sourmash
wget -q https://s3-us-west-1.amazonaws.com/spacegraphcats.ucdavis.edu/microbe-genbank-sbt-k31-2017.05.09.tar.gz 
gunzip < microbe-genbank-sbt-k31-2017.05.09.tar.gz | tar xvf -


# Pfam
wget ftp://ftp.ebi.ac.uk/pub/databases/Pfam/current_release/Pfam-A.hmm.gz
wget ftp://ftp.ebi.ac.uk/pub/databases/Pfam/current_release/Pfam-A.hmm.dat.gz
wget ftp://ftp.ebi.ac.uk/pub/databases/Pfam/current_release/active_site.dat.gz
gunzip Pfam-A.hmm.gz Pfam-A.hmm.dat.gz active_site.dat.gz
hmmpress Pfam-A.hmm
gzip Pfam-A.hmm Pfam-A.hmm.dat active_site.dat

# checkM
# set root to /home/ubuntu/checkm/
mkdir checkm
cd checkm
wget https://data.ace.uq.edu.au/public/CheckM_databases/checkm_data_2015_01_16.tar.gz
gunzip < checkm_data_2015_01_16.tar.gz | tar xvf -
cd ..
```

### 7 update ete3 database
```
python MAGpy/scripts/update_ete3.py
```

### 8 prep a second env based on python 2.7
```
source deactivate

conda env create -f MAGpy/envs/MAGpy-2.7.yaml

# activate it
source activate MAGpy-2.7
```

### 9 install phylophlan
```
hg clone https://bitbucket.org/nsegata/phylophlan
```

### 10 edit config.json






