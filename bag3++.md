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
%post
    #CentOS 7 image on dockerhub isn't updated, so run this first
    yum -y update && yum clean all
    
    # Extras repo needed to provide some packages below
    yum install -y centos-release-scl centos-release-scl-rh
    
    # rh-git29 isn't in official repos anymore, so use this instead
    yum install -y devtoolset-8 httpd24-curl httpd24-libcurl rh-git218
    
    # additional tools
    yum install -y curl make wget which
    
    # needed to install conda
    yum install -y python3-pip python3-devel
    
    # Installing as user doesn't work. But will it work?
    pip3 install conda
```

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









## Pkg install

* rh-git218 (git with nice visual colors; newer git versions don't track symlinks)

Note: rh-git29 is not longer available, so update to rh-git218.

Note, these packages are available in the SCL repository, which must be installed: yum install centos-release-scl-rh centos-release-scl

rh-git29 didn't exist: https://www.softwarecollections.org/en/scls/rhscl/rh-git29/

conda install doesn't work well: https://github.com/ContinuumIO/anaconda-issues/issues/9480

```
conda env create -f environment.yml --force
```

No don't make it a sandbox, as this will cause plocate to use so much space





