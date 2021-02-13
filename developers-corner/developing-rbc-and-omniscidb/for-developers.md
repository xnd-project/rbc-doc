# Setup development environments

We are developing and testing the RBC software under Linux using [Conda](https://docs.conda.io/en/latest/) packaging system as it conveniently provides all the necessary dependencies for the RBC package as well as for the OmniSciDB software.

In the following, we explain how to setup Conda environments for developing RBC as well as OmniSciDB software, how to get the software, and how to run and test the software.

## Setting up a development environments

To create the needed development environments, follow the instructions below.

* Install and setup conda \(unless it is already installed\):

```text
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh -b -p $HOME/miniconda3
source $HOME/miniconda3/bin/activate
conda init bash  # or zsh when on macOS Catalina
```

* Create `rbc-dev` environment:

```text
conda env create -n rbc-dev \
  -f https://raw.githubusercontent.com/Quansight/pearu-sandbox/master/conda-envs/rbc-dev.yaml
```

* Create `omniscidb-dev` environment:

```text
conda env create -n omniscidb-dev \
  -f https://raw.githubusercontent.com/Quansight/pearu-sandbox/master/conda-envs/omniscidb-dev.yaml
```

It is recommended to keep the rbc and omniscidb development environments separate to minimize the risk for any software version conflict.

### CUDA

OmniSciDB can be built for CPU-only or CUDA-enabled mode.

For CUDA-enabled OmniSciDB development, create `omniscidb-cuda-dev` environment:

```text
conda env create -n omniscidb-cuda-dev \
  -f https://raw.githubusercontent.com/Quansight/pearu-sandbox/master/conda-envs/omniscidb-dev.yaml
```

In addition, make sure that your system has [CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit) installed and the CUDA driver functional \(run `nvidia-smi` to verify\). The highest CUDA Toolkit version that OmniSciDB supports is currently 11.0

Here follow the instructions for installing the CUDA Toolkit version 11.0.3:

```text
sudo mkdir -p /usr/local/src/cuda-installers
cd /usr/local/src/cuda-installers
sudo wget https://developer.download.nvidia.com/compute/cuda/11.0.3/local_installers/cuda_11.0.3_450.51.06_linux.run
sudo bash cuda_11.0.3_450.51.06_linux.run --toolkit --toolkitpath=/usr/local/cuda-11.0.3/ --installpath=/usr/local/cuda-11.0.3/ --override --no-opengl-libs --no-man-page --no-drm --silent

sudo sh /usr/local/src/cuda-installers/cuda_11.0.3_450.51.06_linux.run
┌──────────────────────────────────────────────────────────────────────────────┐
│ CUDA Installer                                                               │
│ - [X] Driver                                                                 │
│      [X] 450.51.06                                                           │
│ + [ ] CUDA Toolkit 11.0                                                      │
│   [ ] CUDA Samples 11.0                                                      │
│   [ ] CUDA Demo Suite 11.0                                                   │
│   [ ] CUDA Documentation 11.0                                                │
│   Options                                                                    │
│   Install                                                                    │
...
#   ^--- X-select only Driver and PRESS Install

```

## Getting software

* Checkout `rbc` sources:

```text
mkdir -p ~/git/xnd-project
cd ~/git/xnd-project

# If you are a member of xnd-project organization, use

git clone git@github.com:xnd-project/rbc.git

# else fork https://github.com/xnd-project/rbc and clone your rbc fork,
# or use

git clone https://github.com/xnd-project/rbc.git
```

* Checkout `omniscidb` sources:

```text
mkdir -p ~/git/omnisci
cd ~/git/omnisci

# If you are a member of omnisci organization, use

git clone git@github.com:omnisci/omniscidb-internal.git

# Otherwise, fork https://github.com/omnisci/omniscidb and clone the fork.
#
# Or use

git clone https://github.com/omnisci/omniscidb.git
```

* Although the `omniscidb-dev` environment contains all the prerequisites for building and running OmniSciDB server within the conda environment, we slightly adjust the conda environment to make the management of the building process for different build targets easier. For that, use the following script for activating the `omniscidb-dev` environment:

```text
cd ~/git/omnisci/
wget https://raw.githubusercontent.com/Quansight/pearu-sandbox/master/working-envs/activate-omniscidb-internal-dev.sh
```

The script `activate-omniscidb-internal-dev.sh` must be sourced \(_not_ run via `bash` or another shell\) to the existing terminal session. The activate script will show various information about how to develop the OmniSciDB software within a Conda environment.

* The activate script above uses a custom script `/usr/local/cuda/env.sh` for setting CUDA environment variables for conda environment. Use the following commands to install the `env.sh` script:

```text
cd ~/git/omnisci/
wget https://raw.githubusercontent.com/Quansight/pearu-sandbox/master/set_cuda_env.sh
sudo ln -s ~/git/omnisci/set_cuda_env.sh /usr/local/cuda/env.sh
```

## Development

### RBC

The basic workflow for developing and testing RBC package contains the following commands:

```text
cd ~/git/xnd-project/rbc
conda activate rbc-dev
python setup.py develop

# Possible commands for running rbc tests

pytest -sv -r A rbc/tests
pytest -sv -r A rbc/tests -x -k <test function>
```

The omniscidb related rbc tests are run only when the omniscidb server is running. The server must be started with the options `--enable-runtime-udf --enable-table-functions` , see OmniSciDB development section below.

By default, it is assumed that the omniscidb server can be accessed behind the localhost port 6274. Only in the case, the server is running elsewhere, one needs to configure the rbc testing environment as follows:

* Create a configuration file with the following content \(update credentials as needed\):

```text
# File: client.conf

[user]
# OmniSciDB user name
name: admin
# OmniSciDB user password
password: HyperInteractive

[server]
# OmniSciDB server host name or IP
host: localhost
# OmniSciDB server port
port: 6274
```

* The default location for the configuration file depends on the system but one can set an environment variable `OMNISCI_CLIENT_CONF` that should contain a full path to the configuration file. Default locations of the configuration file are shown below:

```text
# Linux, macOS:
OMNISCI_CLIENT_CONF=$HOME/.config/omnisci/client.conf
# Windows:
OMNISCI_CLIENT_CONF=%UserProfile/.config/omnisci/client.conf
OMNISCI_CLIENT_CONF=%AllUsersProfile/.config/omnisci/client.conf
```

### OmniSciDB

The basic workflow for developing and testing OmniSciDB software in the context of RBC development is as follows:

```text
cd ~/git/omnisci/omniscidb-internal  # or git/omnisci/omniscidb
source ~/git/omnisci/activate-omniscidb-internal-dev.sh

mkdir -p build && cd build
cmake -Wno-dev $CMAKE_OPTIONS_CUDA ..
make -j $NCORES
```

that will build CUDA-enabled omniscidb server. See the instruction from the activate script about how to build CPU-only omniscidb server, for instance.

To execute the omniscidb test-suite, make sure that the current working directory is the build directory and run:

```text
mkdir tmp && bin/initdb tmp
make sanity_tests
```

To start omniscidb server with runtime UDF/UDTFs support enabled, run:

```text
mkdir data && bin/initdb data
bin/omnisci_server --enable-runtime-udf --enable-table-functions
```

