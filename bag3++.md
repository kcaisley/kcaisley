# Bag 3 Install

## here is the .def file:

```
Bootstrap: docker
From: centos:7

# build me with the command: apptainer build /tmp/virtuoso_centos7.sif ./bag3++_centos7.def
# and start me with: apptainer shell -B /tools,/users /tmp/bag3++_centos7.sif

%setup
    #run on the host system after base image. Filepaths are relative to host.
    mkdir ${APPTAINER_ROOTFS}/users
    mkdir ${APPTAINER_ROOTFS}/tools

    #used to mount scratch disks
    mkdir ${APPTAINER_ROOTFS}/run/media
%post
    #CentOS 7 image on dockerhub isn't updated, so run this first
    yum -y update && yum clean all
    
    # Extras repo needed to provide some packages below
    yum install -y centos-release-scl centos-release-scl-rh
    
    # rh-git29 isn't in official repos anymore, so use this instead
    yum install -y devtoolset-8 httpd24-curl httpd24-libcurl rh-git218

    # additional tools
    yum install -y curl make wget
```



## .def notes:

* rh-git218 (git with nice visual colors; newer git versions don't track symlinks)

Note: rh-git29 is not longer available, so update to rh-git218.

Note, these packages are available in the SCL repository, which must be installed: yum install centos-release-scl-rh centos-release-scl

rh-git29 didn't exist: https://www.softwarecollections.org/en/scls/rhscl/rh-git29/



## Immutable mode

Build the container, in an immutable mode:

```
sudo apptainer build /tmp/virtuoso_centos7.sif ./bag3++_centos7.def
```

Run the immutable container, and start a shell inside:

```
apptainer shell -B /tools,/users /tmp/bag3++_centos7.sif
```

## Sandbox mode

for prototyping, I make a container in sandbox mode:

```
sudo apptainer build --sandbox /tmp/bag3++_centos7_sandbox.sif ./bag3++_centos7.def
```

And start a shell inside this writable container, as root, so that you can install and modify contents:

```
apptainer shell -B /tools,/users --writable /tmp/bag3++_centos7_sandbox.sif/
```



## Some notes on building:

```
kcaisley > sudo apptainer build bag3++_centos7_test.sif bag3++_centos7.def
```

works, with permissions kcaisley:base

```
kcaisley > sudo apptainer build --sandbox bag3++_centos7_sandbox.sif bag3++_centos7.def
```

works, but makes with permissions root:root. Can this be run with sudo shell?



targeting the main .sif with this command seems to work?:

```
apptainer shell -B /tools,/users --writable-tmpfs bag3++_centos7.sif
```

okay let's try two things:

```
asiclab > apptainer build bag3++_centos7.sif bag3++_centos7.def
```

works and makes permissions asiclab:asiclab

```
asiclab > apptainer build --sandbox bag3++_centos7_sandbox.sif bag3++_centos7.def
```

works and makes with permissions asiclab:asiclab



```
WARNING: The sandbox contain files/dirs that cannot be removed with 'rm'.
WARNING: Use 'chmod -R u+rwX' to set permissions that allow removal with 'rm -rf'
WARNING: Use the '--fix-perms' option to 'apptainer build' to modify permissions at build time.
```



### Final build:

(in /scratch and as kcaisley)

```bash
sudo apptainer build --force bag3++_centos7.sif bag3++_centos7.def
```

then just

```bash
apptainer shell -B /tools,/users,/run/media bag3++_centos7.sif
```

I left out the idea of building a sandbox, and just debugged what I needed before building



# After starting container:

Then to enable scl environments


```
scl enable devtoolset-8 bash
```

```
scl enable rh-git218 bash
```

clearing and issue with:

```
__vte_prompt_command() { true; }
```

To activate this environment, use

```
source ~/.bashrc
conda activate /run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c
```

To deactivate

```
conda deactivate
```







# Building server environement

I need to get conda, which isn't easily available from pip, as it appears corrupted. Therefore:

From inside the container, and while at the working dir

```
Apptainer > wget https://repo.anaconda.com/miniconda/Miniconda3-py37_23.1.0-1-Linux-x86_64.sh
```

run the script, from working dir

```
Apptainer > bash Miniconda3-py37_23.1.0-1-Linux-x86_64.sh -b -f -p ./miniconda3
```

and manually set the install location

```
Miniconda3 will now be installed into this location:
/users/kcaisley/miniconda3

  - Press ENTER to confirm the location
  - Press CTRL-C to abort the installation
  - Or specify a different location below

[/users/kcaisley/miniconda3] >>> /tools/foss/bag/miniconda3
```



conda install doesn't work well: https://github.com/ContinuumIO/anaconda-issues/issues/9480



Run this from the working directory, where the conda environmental.yml file is.

```
./miniconda3/bin/conda env create -f environment.yml --force -p ./miniconda3/envs/bag_py3d7_c
```

Okay!



autoreconf fixed for libfyaml: https://askubuntu.com/questions/27677/cannot-find-install-sh-install-sh-or-shtool-in-ac-aux	

autreconf failing, needed libtool: https://www.cyberithub.com/solved-autoreconf-automake-failed-with-exit-status-1/

```
autoreconf -vif
```



For Bash, I add the line:

```
using python : 3.7 : /run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c : /run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c/include/python3.7m ;
```

notes for things to change in install instructions:

* fix typo 'many Python and C++ depe'
* fix alignment in point 4
* explain that devtoolset should be 'scl enable' into order to compile with it
* perhaps update git version, but does the version I picked track symlinks?
* write out the command to the conda install prefix in the conda env create
* clean up the confusion between /path/to/conda/env vs /path/to/programs

* It's HDF5, not the other way arround. Fix this in the install instructions
* add note about requiring openssl-devel package for building cmake



# Setting up workspace
