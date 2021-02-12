# Installation

## Installation

In the following, we'll describe various ways to install the RBC and related software. Notice that the RBC project is under active development and to get the latest updates and bug fixes fastest, it is recommended to use the approach described in [For developers](for-developers.md). Otherwise, use the instructions below.

### Install RBC using conda

```text
conda install -c conda-forge rbc

# or to install rbc to a new environemnt, run
conda create -n rbc -c conda-forge rbc
conda activate rbc

# check if rbc installed succesfully
python -c 'import rbc; print(rbc.__version__)'
```

### Install RBC using pip \(alternative\)

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

To check that omniscidb package installed successfully, run \(the omniscidb environment must be activated, see above\):

```text
omnisci_server --version
```

To start omniscidb server for RBC, run

```text
# Create DB (run only once)
mkdir -p omnisci_data
omnisci_initdb omnisci_data

# Start server:
omnisci_server --data=omnisci_data --enable-runtime-udf --enable-table-functions
```

  

