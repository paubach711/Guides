# How do I import a VM from one system to another

This guide explains how to import virtual machines into a Proxmox environment. In my case, I moved VMs from an old `virsh` server to a newer Proxmox server.

## Rsync between servers

Rsync is a very basic tool used to move files or folders between two machines.

```
rsync -avz -vv --progress /path/to/qcow2 root@newserver:/path/to/import/
```

We need to move the image inside the import folder on the Proxmox server.

## Creation and import of the virtual machine

Once the files have been moved, inside the Proxmox web environment we follow these instructions:

```
1. Create VM
2. Basic machine info
3. OS
  3.1. Do not use any media
4. System
5. Disk
  5.1. Delete disk
  5.2. Import
  5.3. Select image
    5.3.1. Select the image you want to import
6. CPU
7. Memory
8. Network (CHECK IF YOUR IMPORTED MACHINES REQUIRE SPECIFIC NETWORK SETTINGS)
9. Confirm
  9.1. Finish
```

Once we do this, the machines will be ready to use.

## RAW images

Sometimes we encounter RAW images that can be hard to import. Follow these steps:

```
rsync -avz -vv --progress /path/to/image root@newserver:/path/to/import/
```

```
1. Create VM
2. Basic machine info
3. OS
  3.1. Do not use any media
4. System
5. Disk
  5.1. Delete disk
6. CPU
7. Memory
8. Network (CHECK IF YOUR IMPORTED MACHINES REQUIRE SPECIFIC NETWORK SETTINGS)
9. Confirm
  9.1. Finish
```

Basically, we create an empty machine. Now, inside the console/terminal:

```
qm importdisk <virt ID> machine.img VM
```

This will allow the VM to see the disk. It will appear as "Unused disk 0".

Then:

```
Double-click on "Unused Disk 0"
Add
```

After doing that, we have successfully imported a RAW image into a Proxmox VM.

If you want to make this disk bootable, go to:

```
Options
Boot Order
Enable: scsi0
Drag it to the first boot option
```

---

Copyright © 2026 Pau Ubach

This document is licensed under the GNU Free Documentation License v1.3 or later.

SPDX-License-Identifier: GFDL-1.3-or-later
