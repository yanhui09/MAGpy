### 10 minute install

(not taking into account downloading and building the databases)

## Assuming we are in a brand new Ubuntu instance

# 1 Update ubuntu
```sh
sudo apt-get update
sudo apt install gcc g++ make
```

# 2 install usearch 

Make sure you download VERSION 5.2.32!

I can't do anything else to help you here - you need to register and you will be sent a link by email.

[link](https://www.drive5.com/usearch/download.html)

Make sure the executable is in your PATH

# 3 download and install conda
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

# 4 clone this repo
```sh
git clone https://github.com/WatsonLab/MAGpy.git
```


