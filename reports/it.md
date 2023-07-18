---
title:
- Updating the IT Infrastructure in ASICLab
author:
- Kennedy Caisley
colortheme:
- dove
fonttheme:
- serif
date:
- 26 July, 2023
---



# ASICLab IT Infrastructure Presentation

# The Before Times

- State before: Backups, hardware Machines, What and how: software/OS we run, machine config ()
- Issues:
    - Harddrives failing in NFS server
    - Desktops were randomly crashing and not booting on CentOS7
    - Configuration management strategy was monolitic: couldn't understand it
    - CentOS7 was released in 2014, [EOL in 2024](https://en.wikipedia.org/wiki/Red_Hat_Enterprise_Linux#RHEL_7).
        - Repos very outdated for key software: gcc, Firefox, etc.
        - Workaround have existed for tools, but aren't perfect
        - [There's no CentOS8/9](https://arstechnica.com/gadgets/2020/12/centos-shifts-from-red-hat-unbranded-to-red-hat-beta/)
        - Alternative clones like AlmaLinux8/9 and Rocky Linux8/9 are facing issues as RH [closed access to RHEL source](https://www.redhat.com/en/blog/furthering-evolution-centos-stream)
    - Documentation was outdates and difficult to work with


# The Changes
- Fedora Server
    - Continous updates make apps like Slack, Zoom, desktop usage, more streamlined
- NFS server
    - Raid6 config (show diagram)
    - `tools`` and `users` shares
- Identities
    - LDAP, FreeIPA, sssd
- Fedora Workstations
    - Updates x2 per year
    - The software core to our work, we self vendor, the auxilary stuff we want to be easy
- Ansible (only introduce after covering server infrastructure)
    - Only these steps are needed on Fedora workstations (make root account, enable SSH)
    - Replace monolitic Clonezilla; force us to know our stack
- Docs
    - Markdown, git repo (show screenshots)
- Ansible, NFS shares/RAID array, fedora linux, containers, DNS automatically assigns IP/name now

# Remaining Issues
- How to organize the lab? 
- Bandwidth of access to NFS shares doesn't work well
- SSH keys don't work with LDAP users

# Up Next
- In two weeks
- GUI-based design Cadence and the alternative of Python-based Circuit generators

# Notes

- Compile with `pandoc -t beamer it.md -o it.pdf`
- how do I make it two columns?
