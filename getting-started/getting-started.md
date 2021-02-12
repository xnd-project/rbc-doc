# Getting started

## Installation

In the following, we'll describe various ways to install the RBC and related software. Notice that the RBC project is under active development and to get the latest updates and bug fixes fastest, it is recommended to use the approach described under Create development environments, see below. Otherwise, use the instructions below.

### Install RBC using conda

```text
conda install -c conda-forge rbc

# or to install rbc to a new environemnt, run
conda create -n rbc -c conda-forge rbc
conda activate rbc

# check if rbc installed succesfully
python -c 'import rbc; print(rbc.__version__)'
```

### Install RBC using pip

```text
pip install rbc-project
# check if rbc installed succesfully
python -c 'import rbc; print(rbc.__version__)'
```

### Install OmniSciDB using conda

It is recommended to install rbc and omniscidb conda packages to _separate_ conda environments.

```text
conda create -n omniscidb omniscidb
conda activate omniscidb

# or for CUDA enabled omniscidb, use

conda create -n omniscidb-cuda omniscidb=*_cuda
conda activate omniscidb-cuda
```

To check that omniscidb package installed succesfully, run

```text
omnisci...
```

To start omniscidb server for RBC, run

```text
omn
```

  

## For developers

We are developing and testing the RBC software under Linux using a Conda environment.  
  
To create the RBC development environment, follow the instructions below:

* Install conda \(if needed\):

```text
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh -b -p $HOME/miniconda3
source $HOME/miniconda3/bin/activate
conda init bash  # or zsh when on macOS Catalina
```

* Create rbc-dev environment:

```text
conda env create -n rbc-dev \
  -f https://raw.githubusercontent.com/Quansight/pearu-sandbox/master/conda-envs/rbc-dev.yaml
```

* Create omniscidb-dev environment

```text
conda env create -n omniscidb-dev \
  -f https://raw.githubusercontent.com/Quansight/pearu-sandbox/master/conda-envs/omniscidb-dev.yaml
```

* Checkout rbc sources

```text
# If you are a member of xnd-project organization, use

git clone git@github.com:xnd-project/rbc.git

# else fork https://github.com/xnd-project/rbc and clone your rbc fork,
# or use

git clone https://github.com/xnd-project/rbc.git
```

* Checkout omniscidb sources

```text
# If you are a member of omnisci organization, use

git clone git@github.com:omnisci/omniscidb-internal.git

# else for https://github.com/omnisci/omniscidb and clone your omniscidb fork,
# or use

git clone https://github.com/omnisci/omniscidb.git
```

