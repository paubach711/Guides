# How do I import VM from one system to another

This guide will be used to know how to import virtual machines to a proxmox enviroment, in my case I moved VM from an old virsh server to a newer proxmox server

## Rsync between servers

Rsync is a very basic tool to move files or folders between two machines

```
rsync -avz -vv --progress  /path/to/qcow2 root@newserver:/path/to/import/
```
We need to move it inside the import folder from the proxmox server

## Creation and import of the virtual machine 

Once the files move, inside the proxmox web-env we follow these instructions:

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
8. Network (CHECK IF YOUR IMPORTED MACHINES REQUIERE SPECIFIC NETWORK SETTINGS)
9. Confirm
  9.1. Finish
```

Once we do this the machines will be ready to use

## RAW images

Sometimes we encounter this RAW images that can be hard to import, follow the steps:

```
rsync -avz -vv --progress  /path/to/qcow2 root@newserver:/path/to/import/
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
8. Network (CHECK IF YOUR IMPORTED MACHINES REQUIERE SPECIFIC NETWORK SETTINGS)
9. Confirm
  9.1. Finish
```
Basically we craete an empty machine, now inside the console/terminal:

```
qm importdisk <virt ID> machine.img VM
```
This will allow the VM to see the disk, it will appear as "Unused disk 0"

Then:

```
Click two times on "Unused Disk 0"
Add
```
Once doing that we have succesfully imported a RAW image to a proxmox VM

If you want to make this disk bootable go to:
```
Options
Boot Order
Enable: scsi0
Draw it to the first boot option
```

