# Bag 3 Install

# Container
## Container .def file:

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



## Building Container:

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

Final build

(in /scratch and as kcaisley)

```bash
sudo apptainer build --force bag3++_centos7.sif bag3++_centos7.def
```

then just

```bash
apptainer shell -B /tools,/users,/run/media bag3++_centos7.sif
```

I left out the idea of building a sandbox, and just debugged what I needed before building



# Server Environment

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
./miniconda3/bin/conda env create -f environment.yml --force -p ./miniconda3/envs/bag_python37
```

Okay, this works!

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

# Workspace

`.bashrc_bag`

```bash
export BAG_WORK_DIR=$(pwd)
export BAG_TOOLS_ROOT=/run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c
export BAG_FRAMEWORK=${BAG_WORK_DIR}/BAG_framework
export BAG_TECH_CONFIG_DIR=${BAG_WORK_DIR}/cds_ff_mpt
export BAG_TEMP_DIR=/run/media/kcaisley/scratch/space
export IPYTHONDIR=${BAG_WORK_DIR}/.ipython
# disable hash-salting. We need stable hashing across sessions for caching purposes.
export PYTHONHASHSEED=0
# set program locations
export BAG_PYTHON=${BAG_TOOLS_ROOT}/bin/python3

# set location of BAG configuration file
export BAG_CONFIG_PATH=${BAG_WORK_DIR}/bag_config.yaml

# setup pybag
export PYBAG_PYTHON=${BAG_PYTHON}
```

`.bashrc`

```bash
export PYTHONPATH=""

### Setup BAG
source .bashrc_bag

# location of various tools
export CDS_INST_DIR=/tools/cadence/IC618
export SPECTRE_HOME=/tools/cadence/2020-21/RHELx86/SPECTRE_20.10.073
export QRC_HOME=/tools/cadence/EXT191
export CMAKE_HOME=/run/media/kcaisley/scratch/cmake-3.17.0

export CDSHOME=${CDS_INST_DIR}
export MMSIM_HOME=${SPECTRE_HOME}
export PVS_HOME=/tools/cadence/2019-20/RHELx86/PVS_19.10.000

# OA settings
export OA_CDS_ROOT=${CDS_INST_DIR}/oa_v22.60.063
export OA_PLUGIN_PATH=${OA_CDS_ROOT}/data/plugins:${OA_PLUGIN_PATH:-}
export OA_BIT=64

# PATH setup
export PATH=${PVS_HOME}/bin:${PATH}
export PATH=${QRC_HOME}/bin:${PATH}
export PATH=${CDS_INST_DIR}/tools/plot/bin:${PATH}
export PATH=${CDS_INST_DIR}/tools/dfII/bin:${PATH}
export PATH=${CDS_INST_DIR}/tools/bin:${PATH}
export PATH=${MMSIM_HOME}/bin:${PATH}
export PATH=${CMAKE_HOME}/bin:${PATH}
export PATH=${BAG_TOOLS_ROOT}/bin:${PATH}

# LD_LIBRARY_PATH setup
export LD_LIBRARY_PATH=${BAG_WORK_DIR}/cadence_libs:${LD_LIBRARY_PATH}
export LD_LIBRARY_PATH=${BAG_TOOLS_ROOT}/lib:${LD_LIBRARY_PATH}
export LD_LIBRARY_PATH=${BAG_TOOLS_ROOT}/lib64:${LD_LIBRARY_PATH}

# Virtuoso options
export SPECTRE_DEFAULTS=-E
export CDS_Netlisting_Mode="Analog"
export CDS_AUTO_64BIT=ALL

# License setup
# source /tools/flexlm/flexlm.sh  #May still need to update this?

export LM_LICENSE_FILE=8000@faust02.physik.uni-bonn.de

# This is for cluseter computing
# Setup LSF
# source /tools/support/lsf/conf/profile.lsf  #May still need to update this?
# export LBS_BASE_SYSTEM=LBS_LSF

# Enable devtoolset
source /opt/rh/devtoolset-8/enable
source /opt/rh/rh-git218/enable
source /opt/rh/httpd24/enable

# pybag compiler settings
export CMAKE_PREFIX_PATH=${BAG_TOOLS_ROOT}
```


in `.cdsenv` add line:

```
asimenv.startup	simulator	string	"spectre"
asimenv.startup projectDir string "/run/media/kcaisley/scratch/space"
```

then fixed the symbolic link at:
`/users/kcaisley/cadence/bag3_ams_cds_ff_mpt/cds_ff_mpt/workspace_setup`

```
lrwxrwxrwx.  1 kcaisley base    37 Jun 13 17:12 PDK -> /tools/kits/CADENCE/cds_ff_mpt_v_1.1/
```




# Starting Container/Workspace:

```
xhost +
```

```
apptainer shell -B /tools,/users,/run/media /tools/containers/bag_centos7.sif
```

```
cd ~/cadence/bag3_ams_cds_ff_mpt
```

```
source .bashrc
```

```
virtuoso &
```

## Deprecated starting steps:

Start container
```
apptainer shell -B /tools,/users,/run/media /run/media/kcaisley/scratch/bag3++_centos7.sif
```

Then to enable scl environments (not necessary, as it's added to path in bashrc script)

```
scl enable devtoolset-8 bash
```

```
scl enable rh-git218 bash
```

clearing and issue with: (not an issue if you don't have conda init put stuff in bashrc)

```
__vte_prompt_command() { true; }
```

To activate this environment, use (not necessary, as it's added to path in bashrc script)


```
source ~/.bashrc    #not necessary if done correctly
conda activate /run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c
```

To deactivate

```
conda deactivate
```

But I don't want to have to have conda init or conda in my path, so this is my case:

[Link to source of material below](https://stackoverflow.com/posts/37230019/timeline)

If `conda` is on your path: (not necessary, as it's added to path in bashrc script)


`source activate <env name> && python xxx.py && source deactivate`

If `conda` isn't on your path but is installed: (not necessary, as it's added to path in bashrc script)


`source /path/to/conda/bin/activate /path/to/desired/env_name/ && python xxx.py && source deactivate`

The following command will activate the environment: (not necessary, as it's added to path in bashrc script)

```
source /run/media/kcaisley/scratch/miniconda3/bin/activate /run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c/
```

```
cd ~/cadence/bag3_ams_cds_ff_mpt/
```


# Debugging Generator

First warning that was noticed was that rpm stopped working. This is because the rpm library depending on liblzma, and the LD_LIBRARY_PATH had some paths added to it in the conda install, which constains a incompatible version.

### RPM and LibLZMA issue

```
rpm: /tools/packages/miniconda3/envs/bag_py3d7_c/lib/liblzma.so.5: version `XZ_5.1.2alpha' not found (required by /lib64/librpmio.so.3)
```

```
Apptainer> ldd /lib64/librpmio.so.3
/lib64/librpmio.so.3: /tools/packages/miniconda3/envs/bag_py3d7_c/lib/liblzma.so.5: version `XZ_5.1.2alpha' not found (required by /lib64/librpmio.so.3)
	linux-vdso.so.1 =>  (0x00007fffa8be8000)
	libnss3.so => /lib64/libnss3.so (0x00007fd35c800000)
	libbz2.so.1 => /lib64/libbz2.so.1 (0x00007fd35c400000)
	libz.so.1 => /tools/packages/miniconda3/envs/bag_py3d7_c/lib/libz.so.1 (0x00007fd35d356000)
	libelf.so.1 => /lib64/libelf.so.1 (0x00007fd35c000000)
	libpopt.so.0 => /lib64/libpopt.so.0 (0x00007fd35bc00000)
	liblzma.so.5 => /tools/packages/miniconda3/envs/bag_py3d7_c/lib/liblzma.so.5 (0x00007fd35d32c000)
	liblua-5.1.so => /lib64/liblua-5.1.so (0x00007fd35b800000)
	libm.so.6 => /lib64/libm.so.6 (0x00007fd35b400000)
	libaudit.so.1 => /lib64/libaudit.so.1 (0x00007fd35b000000)
	libdl.so.2 => /lib64/libdl.so.2 (0x00007fd35ac00000)
	libpthread.so.0 => /lib64/libpthread.so.0 (0x00007fd35a800000)
	libc.so.6 => /lib64/libc.so.6 (0x00007fd35a400000)
	libnssutil3.so => /lib64/libnssutil3.so (0x00007fd35a000000)
	libplc4.so => /lib64/libplc4.so (0x00007fd359c00000)
	libplds4.so => /lib64/libplds4.so (0x00007fd359800000)
	libnspr4.so => /lib64/libnspr4.so (0x00007fd359400000)
	/lib64/ld-linux-x86-64.so.2 (0x00007fd35d000000)
	librt.so.1 => /lib64/librt.so.1 (0x00007fd359000000)
	libcap-ng.so.0 => /lib64/libcap-ng.so.0 (0x00007fd358c00000)
```
This is where `liblzma.so.5` should be coming from:
```
Apptainer> ldconfig -p | grep liblzma   
	liblzma.so.5 (libc6,x86-64) => /lib64/liblzma.so.5
	liblzma.so.5 (libc6) => /lib/liblzma.so.5
```

This isn't an issue for BAG, but it does mean that Virtuoso will issue a warning, as RPM doesn't work, and therefor the checksystem version script fails. To temporarily make RPM work (at the expense of BAG), simply remove the BAG conda env locations from $LD_LIBRARY_PATH.


### Qt Plugin Issue

When running `./meas_cell.sh` I'm getting some issues with a Qt GUI starting:

```bash
[2023-06-16 10:17:12.535] [sim_db] [info] Returning previous simulation data
This application failed to start because it could not find or load the Qt platform plugin "xcb"
in "".

Available platform plugins are: eglfs, linuxfb, minimal, minimalegl, offscreen, vnc, xcb.

Reinstalling the application may fix this problem.
[2023-06-16 10:17:12.544] [cbag] [critical] 	stack dump [1]  /lib64/libpthread.so.0+0xf630 [0x7f08ce00f630]
```

From [this page](https://askubuntu.com/posts/1054691/timeline), before you start (dangerously) messing around with symlinks to shared libraries, I strongly suggest that you run

`export QT_DEBUG_PLUGINS=1`

and then run your failing executable again in the Terminal. Read the actual error message thrown by QT, since none of the above solutions addressed the cause of this error in my case. This gave me:
```bash
Got keys from plugin meta data ("xcb")
QFactoryLoader::QFactoryLoader() checking directory path "/run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c/bin/platforms" ...
Cannot load library /run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c/lib/python3.7/site-packages/PyQt5/Qt/plugins/platforms/libqxcb.so: (/run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c/lib/libQt5XcbQpa.so.5: symbol _ZNK15QPlatformWindow15safeAreaMarginsEv, version Qt_5_PRIVATE_API not defined in file libQt5Gui.so.5 with link time reference)
QLibraryPrivate::loadPlugin failed on "/run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c/lib/python3.7/site-packages/PyQt5/Qt/plugins/platforms/libqxcb.so" : "Cannot load library /run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c/lib/python3.7/site-packages/PyQt5/Qt/plugins/platforms/libqxcb.so: (/run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c/lib/libQt5XcbQpa.so.5: symbol _ZNK15QPlatformWindow15safeAreaMarginsEv, version Qt_5_PRIVATE_API not defined in file libQt5Gui.so.5 with link time reference)"
This application failed to start because it could not find or load the Qt platform plugin "xcb" in "".
```

The relevant part is: `symbol _ZNK15QPlatformWindow15safeAreaMarginsEv, version Qt_5_PRIVATE_API not defined in file libQt5Gui.so.5 with link time reference`

Googling this, lead me to this: https://superuser.com/questions/1397366/what-is-wrong-with-my-qt-environment-it-reports-could-not-find-or-load-the-qt

> It looks like you have a Qt libraries version mismatch.
> I see Qt libraries coming from both `/usr/bin/platforms/libqeglfs.so` and `/usr/lib/x86_64-linux-gnu/qt5/plugins/platforms`, which are probably not consistent.
> You might need to specify the path like: `export LD_LIBRARY_PATH:/path/to/right/libs:$LD_LIBRARY_PATH`

Sure enough, I'm checking my error log, and I see references to both:

```bash
/run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c/lib/python3.7/site-packages/PyQt5/Qt/plugins/platforms/
/run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c/bin/platforms
```

But check it, there is also:

```bash
/run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c/lib/python3.7/site-packages/PyQt5/Qt/lib/libQt5XcbQpa.so.5
```

The solution was modifying the LD_LIBRARY_PATH:

```
export LD_LIBRARY_PATH=/run/media/kcaisley/scratch/miniconda3/envs/bag_py3d7_c/lib/python3.7/site-packages/PyQt5/Qt/lib:$LD_LIBRARY_PATH
```

But I think I should just fix the bashrc file? Maybe add things to the end of the path.

### XDG_RUNTIME_PATH issue

```
QStandardPaths: XDG_RUNTIME_DIR points to non-existing path '/run/user/2002', please create it with 0700 permissions.
```

These were fixed by:

https://unix.stackexchange.com/questions/162900/what-is-this-folder-run-user-1000

```
export XDG_RUNTIME_DIR=/run/media/kcaisley/scratch/space
```


### Session Manager Issue

```
Qt: Session management error: None of the authentication protocols specified are supported
```

https://stackoverflow.com/questions/59057653/qt-session-management-error-none-of-the-authentication-protocols-specified-are

```
unset SESSION_MANAGER
```

Now, nothing is appearing graphically, but I see:

```python
{'delay_fall': array([[1.44678203e-11, 2.12270059e-11, 2.79859170e-11],
       [1.90778681e-11, 2.83911787e-11, 3.75218904e-11],
       [1.19660825e-11, 1.76068935e-11, 2.31339914e-11]]),
 'delay_rise': array([[1.84293439e-11, 2.77868968e-11, 3.69646259e-11],
       [2.38278466e-11, 3.62280028e-11, 4.84479263e-11],
       [1.69927221e-11, 2.59050590e-11, 3.46498162e-11]]),
 'pwr_avg': array([[1.62404387e-05, 2.72441252e-05, 3.80507290e-05],
       [1.30442844e-05, 2.19406111e-05, 3.05181711e-05],
       [2.07834708e-05, 3.39967189e-05, 4.71144087e-05]]),
 'sim_data': {'ff_125': <bag.simulation.data.SimData object at 0x7f14efdebb90>,
              'ss_m40': <bag.simulation.data.SimData object at 0x7f14efdd7990>,
              'tt_25': <bag.simulation.data.SimData object at 0x7f14efdd7890>},
 'sim_envs': ['tt_25', 'ss_m40', 'ff_125'],
 'trans_fall': array([[1.68837592e-11, 2.92186103e-11, 4.21613994e-11],
       [2.26158479e-11, 3.93678371e-11, 5.56614052e-11],
       [1.42056520e-11, 2.44467640e-11, 3.53796199e-11]]),
 'trans_rise': array([[2.44673691e-11, 4.21319603e-11, 5.96973391e-11],
       [3.17268196e-11, 5.53270956e-11, 7.88278387e-11],
       [2.25445377e-11, 3.87765963e-11, 5.49322180e-11]])}
```

```python
{'beta': 1.375,
 'delay_error_avg': 0.000919131708073776,
 'seg_n': 8,
 'seg_p': 11}
```

I suppose it looks like it's working now?

# Generator Workflow Flow


`./meas_cell.sh`, and it's kind are simple wrapper scripts, which just call their corresponding python equivalents, in `BAG_framework/run_scripts/`

```
dsn_cell.py
extract_cell.py
gen_cell.py
import_sch_cellview.py
meas_cell.py
sim_cell.py

There are also additional .py file there:

export_layout.py
copy_pytest_outputs.py
gds_filter.py
gds_import.py
generate_verilog.py
gen_wrapper.py
import_layout.py
meas_cell_old.py
netlist_config.py
reformat_schematic.py
run_em_cell.py
setup_submodules.py
verify.py
write_tech_info.py
```


```
├── data    (process specific data, in submodules, for corresponding generators)
│   ├── aib_ams
│   ├── bag3_digital
│   └── bag3_testbenches



├── gen_libs    (init empty, holds generated OA cell views)
│   └── AAA_INV
├── gen_outputs (initially empty)
│   ├── inv3
│   └── mos_char_nch_18n
├── gen_outputs_scratch -> /run/media/kcaisley/scratch/space/simulations/gen_outputs

```

