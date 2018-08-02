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
wget -q https://s3-us-west-2.amazonaws.com/sourmash-databases/2018-03-29/genbank-d2-k31.tar.gz
gunzip < genbank-d2-k31.tar.gz | tar xvf -


# Pfam
wget ftp://ftp.ebi.ac.uk/pub/databases/Pfam/current_release/Pfam-A.hmm.gz
wget ftp://ftp.ebi.ac.uk/pub/databases/Pfam/current_release/Pfam-A.hmm.dat.gz
wget ftp://ftp.ebi.ac.uk/pub/databases/Pfam/current_release/active_site.dat.gz
gunzip Pfam-A.hmm.gz Pfam-A.hmm.dat.gz active_site.dat.gz
hmmpress Pfam-A.hmm

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

### 10 set the checkM data root

I have to say this has proven difficult for me, but this manual way appears to work

Run the command:

```sh
checkm data setRoot
```

You will see this output - please set this to the checkm folder where we unzipped data above:
```sh
It seems that the CheckM data folder has not been set yet or has been removed. Running: 'checkm data setRoot'.
Where should CheckM store it's data?
Please specify a location or type 'abort' to stop trying:
/home/ubuntu/checkm
```

You will then see this:

```sh
Path [/home/ubuntu/checkm] exists and you have permission to write to this folder.
(re) creating manifest file (please be patient).

You can run 'checkm data update' to ensure you have the latest data files.

```

For some reason I then see it **again**.  Just do the same again:

```sh
*******************************************************************************
 [CheckM - data] Check for database updates. [setRoot]
*******************************************************************************

Where should CheckM store it's data?
Please specify a location or type 'abort' to stop trying:
/home/ubuntu/checkm
```

Finally you wll see this:

```sh
Path [/home/ubuntu/checkm] exists and you have permission to write to this folder.
(re) creating manifest file (please be patient).

You can run 'checkm data update' to ensure you have the latest data files.

Data location successfully changed to: /home/ubuntu/checkm

```

What should happen now is that when snakemake makes the MAGpy-2.7 env during execution, it will link to this one, which already has the data root set by you :-)

### 11 edit config.json

The file config.json tells MAGpy where everything is.  On this installation on Ubuntu, it should (and does) look like this:

```sh
{
    "phylophlan_dir": "/home/ubuntu/phylophlan",
    "uniprot_sprot": "/home/ubuntu/uniprot_sprot",
    "sourmash_gbk": "/home/ubuntu/genbank-k31.sbt.json",
    "pfam_dir": "/home/ubuntu/"
}
```

### 12 make scripts executeable and add them to your PATH

```sh
chmod 755 MAGpy/scripts/*
```

Edit $HOME/.bashrc and add the MAGpy scripts dir to your PATH:

```
export PATH="/home/ubuntu/MAGpy/scripts/:$PATH"
```

And source it

```sh
source $HOME/.bashrc
```





