#### `penelope`

Currently runs CentOS7 and hosts the ~7TB of user and design software and serves it via the NFS shares `/cadence` and `/faust`, which is used by our Linux machines, but not Windows machines. Planned maintenance work, starting in January:

* Final incremental back up, shutdown the machine, remove the 4 data spinning drives, and 2 OS solid-state drives
* Install Fedora 37 Server (no GUI) on one of the solid state drives, which will then be re-racked. 
* Rack new 16TB drives in the 5 remaining drive bay, and configure them with the BTRFS file system (default on Fedora), in a Raid 6 configuration. With 5x16TB drives, and 2 parity strips, this yield 48 TB of effective storage space
* Copy backup data from `apollo` to new `penelope` drives.
* Re-enable NFS file shares, potentially renaming `/cadence` to `/tools` and renaming `/faust` to `/users`?

#### `asiclab###`

Desktop machines will be slowly transitioned to Fedora, as situations permit. The idea is to gradually complete this transition in the next 4-5 months, such that we can identify all the configuration necessary on the machines for PCB, ASIC, and TCAD work without having to disrupt people's work flows. Then this knowledge, captured in a simple Ansible configuration script, will be used to initially setup and maintain updates on the family of machines:

1. *(First step is manual)* Install latest Fedora Desktop edition, create local admin account, ensure correct hostname and IP have been inferred from DNS server, enable SSH access.
2. *(Subsequent steps are automated)* Run system-wide update of all software and DELL motherboard firmware (BIOS)
3. Install drivers for PCIe Nvidia GPU and aux Ethernet card
4. Mount remote user and tools directory (will be hopefully renamed to `/users` and `/tools`)
5. Create local user accounts and groups, with accounts tailored to each machine, with `$HOME` living on mounted `/users` NFS share
6. Copy Cadence and TCAD containers built on CentOS7 to local scratch storage, for improved performance
7. Configure network printers
8. Enable VNC server and RDP access
9. Install additional applications: Apptainer, VSCode, GIMP, Inkscape, Slack, etc

#### `faust02`

Currently runs CentOS7 with FLEX LM license server, and LDAP/SMB server inside of a container called `noyce`. The SMB service is rarely used, and the LDAP configuration is powerful but very complicated. If we accept the restriction of only using NFS and SSHFS for remote file system access, we can drastically simplify this setup for a group our size by just using Linux's build-in User and Group permissions system. These permissions are already appropriately configured on our files and user accounts, and so no work is required to enable this. As desktop (`asiclab###`) machines are upgraded to Fedora 37 over the coming months, they will no longer depend on LDAP for file access and user accounts. Therefore the planned work is:

* Copy relevent user and group permissins to Ansible configuration script for desktop machines
* Once all `asiclab###` desktops Fedora, then turn off `noyce` machine with LDAP server
* Simplify `faust02` server name to `faust`
* By 30.06.2024, evalute if FLEM LM license server can run on Fedora, as this is the end of CentOS7 security updates.
  * If so update to Fedora, otherwise, consider Rocky/AlmaLinux.


#### `jupiter/juno`

Currently running CentOS7, and used only for simulation. Virtuoso IC6.1.8 currently depends on a CentOS7 environment, so switching the OS at this time is not advisable. Similar to `faust02`:

* By 30.06.2024, evaluate if Virtuoso can be run on Fedora, as this is the end of CentOS7 security updates.
  * If so update to Fedora, otherwise, consider Rocky/AlmaLinux.

#### `apollo`

After backup data has been transferred to new `penelope` setup, `apollo` can be retired, or at the very least powered down.