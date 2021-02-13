# Installation

In the following, we'll describe how to install the RBC and related software.

* Since the RBC project is under active development and to get the latest updates and bug fixes fastest, one should use the approach described in [Developers Corner](../../developers-corner/developing-rbc-and-omniscidb/for-developers.md).

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

* It is recommended to install the rbc and omniscidb conda packages to _separate_ conda environments.

```text
conda create -n omniscidb omniscidb
conda activate omniscidb

# or for CUDA enabled omniscidb, use

conda create -n omniscidb-cuda omniscidb=*_cuda
conda activate omniscidb-cuda
```

To check that omniscidb package is installed successfully, make sure that the omniscidb environment is activated and run:

```text
omnisci_server --version
```

To start omniscidb server with runtime UDF/UDTFs support enabled, run

```text
# Create DB, run only once
mkdir -p omnisci_data
omnisci_initdb omnisci_data

# Start server:
omnisci_server --data=omnisci_data --enable-runtime-udf --enable-table-functions
```

  

