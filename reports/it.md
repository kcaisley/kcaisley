---
title:
- Updating the IT Infrastructure in ASICLab
author:
- Kennedy Caisley
date:
- 26 July, 2023
---



# General Architecture

- Organization of System (block diagram)
- Shared network basics (not administered by us)
    - DNS server
    - Wireguard VPN server
    - Printer Network
    - Central switch, with fiber backhaul
    - Documentation via 
- 'Personal' Infrastructure (Workstations and Servers, self administered)
    - File storage: Raid array, NFS share, Samba share, shared read only data, user home directories, protected via groups, backup
    - User sync: NFS mount, LDAP, User Permissions, (no freeipa yet)
    - EDA tools: ASIC design, PDKs, TCAD, PCB, EM sim tools, plus associated License and (Cliosoft)

# The Before Times: Problems to be fixed

- Hard drives failing in NFS server
- Desktops were randomly crashing and not booting on CentOS7
- Configuration management strategy was monolithic: couldn't understand it
- CentOS7 was released in 2014, [EOL in 2024](https://en.wikipedia.org/wiki/Red_Hat_Enterprise_Linux#RHEL_7).
    - Repos very outdated for key software: gcc, Firefox, etc.
    - Workaround have existed for tools, but aren't perfect
    - [There's no CentOS8/9](https://arstechnica.com/gadgets/2020/12/centos-shifts-from-red-hat-unbranded-to-red-hat-beta/)
    - Alternative clones like AlmaLinux8/9 and Rocky Linux8/9 are facing issues as RH [closed access to RHEL source](https://www.redhat.com/en/blog/furthering-evolution-centos-stream)
- Documentation () was outdated and difficult to work with

# Change: OS Upgrade
- Philosophy: Can live with self-vendoring software core to work, but the auxillary stuff should be easy
- Ubuntu is a good choice too, but Fedora has:
    - more built-in enterprise features (FreeIPA, Ansible support, to be discussed)
    - Updates 2x per year make apps like Slack, Zoom, desktop usage, more streamlined
- Successful version upgrades 36 -> 37 -> 38 -> 39 (in Oct)


# Change: System Configuration
- How to take a machine from a fresh install -> desired state (and keep it that way)
- Used for workstations only (just make root account, enable SSH), as we want the 12+8 machines to be the same
- Now using Ansible, whenever it make sense
    - State based or 'idempotent', rather than action based
        - Example: Write line to file
    - 
    - Replaces monolithic Clonezilla; force us to know our stack
    - Will demonstrate code used to implement subsequent changes

- NFS server
    - Raid6 config (show diagram)
    - `tools`` and `users` shares
- Identities
    - LDAP, FreeIPA, sssd

# Changes: Fedora Workstations
    - Updates x2 per year
    - 

# Changes: Documentation
- Markdown, git repo (show screenshots)
- Ansible is more or less 'self-documenting', for the workstations

# Remaining Work + Problems
- 3 Servers (Faust02/Jupiter/Juno) still on CentOS7
- ~6 (of 20) workstations still on CentOS7
- Apollo decommision (is on CentOS 6)
- faust02 renamed to -> faust
- How to organize the lab? 
- Bandwidth of access to NFS shares doesn't work well
- SSH keys don't work with LDAP users

- FreeIPA instance transfer
- Discussion: Should we port any of these changes elsewhere in the lab

# Up Next
- In two weeks
- GUI-based design Cadence and the alternative of Python-based Circuit generators

