Day 13 – Linux Volume Management (LVM)

************Task 1: Check Current Storage*****

Ran command lsblk - Used to list block devices on the system, such as disks, partitions, and loop devices, in a tree format , df-h  Used to display disk space usage of mounted filesystems in a human-readable format.

<img width="1918" height="1070" alt="image" src="https://github.com/user-attachments/assets/800a4a19-c5b4-40d3-bab9-3b69f3e98dc3" />

****Task 2: Created Physical Volume pvcreate******

Ran the command sudo pvcreate /dev/loop17 to create a physical volume, and used the pvs command to verify it


<img width="1918" height="1070" alt="image" src="https://github.com/user-attachments/assets/4a0c0789-6b33-4b72-a298-388a7619a950" />

***Task 3: Created Volume Group****

Ran the command vgcreate devops-vg /dev/loop17 to create a Volume Group (VG), and verified it using the vgs command

<img width="1863" height="227" alt="image" src="https://github.com/user-attachments/assets/d7b75b3f-5a5f-43ae-9fbc-da87fb3652db" />

****Task 4: Created Logical Volume***

Ran the command lvcreate -L 500M -n app-data devops-vg to create a Logical Volume (LV), and verified it using the lvs command

<img width="1864" height="335" alt="image" src="https://github.com/user-attachments/assets/78565c64-9f9d-4975-a06f-86339e869703" />

********Task 5: Format and Mount*************

mkfs.ext4 /dev/devops-vg/app-data is used to create an EXT4 filesystem, mkdir -p /mnt/app-data creates the mount point directory, mount /dev/devops-vg/app-data /mnt/app-data mounts the Logical Volume, and df -h /mnt/app-data verifies the mounted filesystem and displays disk usage in human-readable format.

<img width="1864" height="737" alt="image" src="https://github.com/user-attachments/assets/6a8b7c58-30df-44e0-963e-0db4f7820ccf" />

*******Task 6: Extend the Volume**********

lvextend -L +200M /dev/devops-vg/app-data is used to extend the Logical Volume by 200 MB, resize2fs /dev/devops-vg/app-data resizes the EXT4 filesystem to use the new space, and df -h /mnt/app-data verifies the updated filesystem size and disk usage in human-readable format.


<img width="1861" height="327" alt="image" src="https://github.com/user-attachments/assets/b57c61e8-4c1e-4aab-a366-cc60aa2f3eb6" />






