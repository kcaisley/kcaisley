---
marp: true
theme: gaia
---

<!-- _class: lead -->

# ASICLab IT Upgrades 🖳

#### Kennedy Caisley
#### Marco Vogt

26 July 2023


---

### The physical network...

<!-- _backgroundColor: white -->
<!-- _class: lead -->

![](../images/network.png)

---

### ...with services running on top

<!-- _backgroundColor: white -->
<!-- _class: lead -->
![](../images/network_half.png)

---

### Motivations

❌ CentOS 7 reaching EOL with no upgrade path

❌ Drive failures & low storage in file server

❌ Workstations failing to boot & softwares outdated

❌ Config management opaque and slow

❌ Docs not maintain-ed | able

---
<!-- _class: lead -->

### Project #1: An OS Upgrade

<!-- The first three categories are:
How much work would need to be invested initially + overtime in running these classes of software? This includes the mindshare, documentation, software availability, and ease software configuration.

*Questions: How much did RHEL cost?
How much does SLES cost?
What is the real support for SUSE/Open Suse for EDA?* -->

|Distribution | Design | Services | Desktop | Pricing | Future |
|---|:---:|:---:|:---:|:---:|:---:|
|RHEL       | ✅ | ✅ | 🆗 | 💰 | ✅ |
|Rocky/Alma | ✅ | ✅ | 🆗 | ✅ | 😬 |
|SUSE       | 🆗 | ✅ | 🆗 | 💰 | ✅ |
|OpenSUSE   | 🆗 | 🆗 | 🆗 | ✅ | ✅ |
|Ubuntu     | ❌ | 🆗 | ✅ | ✅ | ✅ |
|Fedora     | 🆗 | ✅ | ✅ | ✅ | ✅ |

---

### `fedora`, I choose you!

✅ Installed across 14 workstations and 2 servers

🆗 Some EDA tools run; work-around for the rest

✅ Enterprise services are well-supported & documented (RHEL)

✅ Desktop apps mostly available (`zoom`, `slack`, `code`)

✅ Automatic driver & firmware upgrades

✅ Successful upgrades since: **36 -> 37 -> 38 -> 39** *(soon)*

---
<!-- _class: lead -->

### Project #2: Workstation Setup

`clonezilla` isn't the right tool for *Configuration Management*:

> The art of setting and maintaining a machine in a desired state.




<!-- - Now that we have a bunch of fresh machines:
    - How to take a machine from a fresh install -> desired state (and keep it that way)
    - Remember the old way was to configure one, and use Clonezilla
- Used for workstations only (just make root account, enable SSH), as we want the 12+8 machines to be the same
- Now using Ansible, whenever it make sense
    - State based or 'idempotent', rather than action based
        - Example: Write line to file
    - Replaces monolithic Clonezilla; force us to know our stack -->

- `ansible`

---

1. `dnf install ansible` on a machine not being configured
2. `ssh-copy-id ` to all target machines 
3. List target machines in `inventory.yaml`:

    ```yaml
    workstations:
    hosts:
        asiclab001.physik.uni-bonn.de:
        mac: 54:BF:64:98:25:D4
        asiclab002.physik.uni-bonn.de:
        mac: 54:BF:64:98:25:CC
        asiclab003.physik.uni-bonn.de:
        mac: 54:BF:64:98:25:BAs
    ``````
---

4. List desired state in `playbook.yaml`:
    ```yaml
    - name: Send a wake-on-LAN magic packet                                       
        community.general.wakeonlan:
            mac: '{{ mac }}'
    - name: Ensure client.conf exists & contains CUPS hostname
        ansible.builtin.lineinfile:
            path: /etc/cups/client.conf
            line: ServerName cups.physik.uni-bonn.de
            create: yes
    - name: Check development tools are installed
        ansible.builtin.apt:
            name:
            - gcc
            - tmux
            - git-lfs
            state: latest
    ```
---

5. Run playbook on target inventory:

    ```bash
    [asiclab@penelope ~]$ ansible -K playbook.yaml -i inventory.yaml
    ```

Some additional useful arguments:

- `--limit` to specific subset of machines
- Only run playbook tasks with certain `--tag`
- Be more `--verbose`

---

Ansible output log:

```log
PLAY [Workstation Configuration] *********************************************

TASK [Send a wake-on-LAN magic packet] ***************************************
ok: [asiclab001.physik.uni-bonn.de]
ok: [asiclab002.physik.uni-bonn.de]
ok: [asiclab003.physik.uni-bonn.de]

TASK [Ensure client.conf exists & contains CUPS hostname] ********************
ok: [asiclab001.physik.uni-bonn.de]
ok: [asiclab002.physik.uni-bonn.de]
changed: [asiclab003.physik.uni-bonn.de]

TASK [Check development tools are installed] *********************************
ok: [asiclab001.physik.uni-bonn.de]
changed: [asiclab002.physik.uni-bonn.de]
changed: [asiclab003.physik.uni-bonn.de]
```

<!-- - Config managment is a super complex field, and is mostly overboard for our group
- But in cases where you have a list of repeated, common tasks, that need to be done
on many machines, it's a very userful tool -->


---

<!-- _class: lead -->
# Project 3: Fixing the File Server
1.  Copied `/tools` and `/users` (~3 days)
2.  Built `raid6` array with **5** new 16 TB drives:
    - Capacity: 48 TB
    - Speed gain: 3x read, but no write
    - Fault tolerance: 2-drive failure (double parity)
3. Copied back data (another ~3 days)

---

Raid array details, seen from `penelope` server

```
[asiclab@penelope ~]$ sudo mdadm --detail /dev/md127

/dev/md127:
     Creation Time : Sat Jan 14 14:46:26 2023
        Raid Level : raid6
        Array Size : 46877242368 (43.66 TiB 48.00 TB)
     Used Dev Size : 15625747456 (14.55 TiB 16.00 TB)                            
      Raid Devices : 5
        Chunk Size : 512K

    Number   Major   Minor   RaidDevice State
       0       8       16        0      active sync   /dev/sdb
       1       8       32        1      active sync   /dev/sdc
       2       8       48        2      active sync   /dev/sdd
       3       8       64        3      active sync   /dev/sde
       4       8       80        4      active sync   /dev/sdf
```
---
4. Enable new incremental backups with HRZ using IBM `dsmc`
5. Enable automatic array checks with `raid-check.timer`
5. Enable `nfs4` server for `/users` and `/tools`.

A sanity check from `asiclab001` workstation:

```log
[asiclab@asiclab008 ~]$ showmount -e penelope.physik.uni-bonn.de                 

Export list for penelope.physik.uni-bonn.de:

/export/disk/tools   asiclab*,juno.physik.uni-bonn.de,noyce.physik.uni-bonn.de,
jupiter.physik.uni-bonn.de,faust02.physik.uni-bonn.de,apollo.physik.uni-bonn.de

/export/disk/users   asiclab*,juno.physik.uni-bonn.de,noyce.physik.uni-bonn.de,
jupiter.physik.uni-bonn.de,faust02.physik.uni-bonn.de,apollo.physik.uni-bonn.de
```

---

7. Use `ansible` to auto-mount NFS shares on all workstations:

```yaml
- name: Create mount points and mount /users
    ansible.posix.mount:
        src: penelope.physik.uni-bonn.de:/export/disk/users
        path: /users
        opts: rw
        state: mounted
        fstype: nfs4
- name: Create mount points and mount /tools
    ansible.posix.mount:
        src: penelope.physik.uni-bonn.de:/export/disk/tools
        path: /tools
        opts: ro
        state: mounted
        fstype: nfs4
```

---

# Changes: Identity Management
- User data on `rw` NFS share
- Ported old LDAP data to modern FreeIPA distribution (LDAP, SSSD, NSS)
- Data protected using groups `base`, `icdesign`, `tsmc65`, etc
- Downside: No IdM or NFS = no login or crashing


Don't just manually add users

```bash
freeipa command to list users
```

Also available on GUI:
![FreeIPA GUI](img.png)

Then on client client
```bash
SHOULD PUT THE ANSIBLE VERSION
sudo realm join penelope.physik.uni-bonn.de
```

```bash
$ ls /home
asiclab

$ ls /users
dschuechter  kcaisley  krueger  mvogt  skahn  szhang  ydieter  ...
```

# Changes: EDA Tools
- Living on the read only NFS mount `tools`, executed on workstation
    - Must query against FlexLM and SOS to start
- EDA tools typically only certified on a handful of OSes (RHEL, Suse) [see here](https://www.cadence.com/content/dam/cadence-www/global/en_US/documents/support/2021-2024-cadence-compute-platform-roadmap-v1-public.pdf) 
- We can't easily use RHEL equivalents with RHEL rebuilds due to CentOS EOL, and RHEL source now being closed
- Turns out FPGA tools (ISE & Vivado) just work on Fedora
- In other, what would be hands would be to be able to run the software inside of a complete `OS virtual environment`, so that the tools sees all the right package versions: i.e. we want Containerization
- There are several choices (Docker, Podman, etc) but the best for our high-performance + GUI needs is `apptainer` best


```bash
$ ls /tools
cadence  clio  containers  designs  kits  mentor  synopsys  xilinx ...
```

1. Create a `.def` file, for target application. Add the following:

2. Select a starting OS base image (Docker compatible!)

    ```docker
    Bootstrap: docker
    From: centos:7
    ```
3. Create mount points for external locations; `$HOME` is already done

    ```docker
    %setup
    mkdir ${APPTAINER_ROOTFS}/tools
    mkdir ${APPTAINER_ROOTFS}/users
    ```

4. List packages to install on top of OS base image

    ```docker
    %post
    yum -y update && yum clean all
    yum install -y csh tcsh glibc gdb ... etc
    ```

5. Compile the container image:

```bash
[kcaisley@asiclab008]$ sudo apptainer build demo.img demo.def

INFO:    Starting build...
Getting image source signatures
INFO:    Running setup scriptlet
INFO:    Running post scriptlet
...
Complete!
INFO:    Adding environment to container
INFO:    Creating SIF file...
...
Done!
```

```bash
[kcaisley@asiclab008]$ cat /etc/redhat-release
Fedora release 38 (Thirty Eight)

[kcaisley@asiclab008]$ bash --version
GNU bash, version 5.2.15(1)-release (x86_64-redhat-linux-gnu)
Copyright (C) 2022 Free Software Foundation, Inc.

[kcaisley@asiclab008]$ apptainer shell demo.sif

Apptainer>

Apptainer> cat /etc/redhat-release 
CentOS Linux release 7.9.2009 (Core)

Apptainer> bash --version
GNU bash, version 4.2.46(2)-release (x86_64-redhat-linux-gnu)
Copyright (C) 2011 Free Software Foundation, Inc.
```

---

# Project #5: Write the docs
<!-- _class: lead-->

Decision making framework:

### `confluence.atlassian.com` is 📉

### `.md` + `github.com` is 📈

---
<!-- _backgroundColor: white -->
<!-- _class: lead -->

![h:1000](../images/docs1.png)

---

<!-- _class: lead -->
<!-- _backgroundColor: white -->

![h:300](../images/docs2.png)

---
# Remaining Work + Problems
- 6/20 workstations still on CentOS7
- 3/5 Servers (Faust02/Jupiter/Juno) still on CentOS7
- Apollo decommission (is on CentOS 6)
- faust02 renamed to -> faust
- How to organize the lab? Still a mess.
- Bandwidth of access to NFS shares doesn't work well
- SSH keys don't work with LDAP users

- FreeIPA instance transfer
- Discussion: Should we port any of these changes elsewhere in the lab

# Up Next
- In two weeks
- GUI-based design Cadence and the alternative of Python-based Circuit generators

