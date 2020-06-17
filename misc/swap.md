# Regarding swap and disabling it

## Disclaimer
```
There are varying opinions on whether to use swap on Linux.
This article is thus written on, to my best knowledge, what I have read and know, and by extension, my opinions on swap usage.
This may not account for logical fallacies, outdated information, etc.

Additional disclaimer:
I am not responsible if you muck up your installation, accidentally delete your data, or lose your job from this article.
```
## Preface

Being that most, if not all Bay Trail devices are low-end, it is almost guaranteed that the device contains a eMMC chip for storage.
For those not in the know, an eMMC chip is basically an SD Card + Controller in a single IC.

Generally, the main purpose of swap is to move unused data in RAM when you need more memory onto the disk so the OOMK (Out of Memory Kiler) does not have to kick in and exit your program.
This, however, dramatically increases reads and writes to your storage, in this case, an eMMC chip.

Why is this relavant? Well, eMMC chips usually contain a single lower quality flash memory chip that can endure less write cycles.
Coincidentally, RAM is used as high throughput temporary storage, requiring many read/write operations (Basically high I/O usage). 
When a program's data in RAM is swapped onto disk, it generally means it is not actively using the RAM. This means that the high I/O nature of RAM is not as well pronounced in swap.

This is generally not a problem, as many distros and programs are lightweight in memory usage, and thus mostly won't even use swap on 4GB of RAM.
However, with our lower-end Bay Trail devices, most come with only 1/2 GB of RAM as standard. This is where we run into the problem.
Since our devices come with little RAM and use eMMC as storage, it means that the swap is likely to be highly utilized, which dramatically shortens the life of the eMMC chip.

To combat this, we can either reduce or outright disable swap to preserve the lifespan of the eMMC chip.

## Should I reduce swappiness or disable swap?

There are some considerations to be made, however.

On devices with 1 GB of RAM, just the DE & kernel alone can take up to 500 MB of RAM. Should you require just a measly 600 MB of RAM to use, the OOMK will kick in if you disable swap.
Thus, here, we need to sacrifice some lifespan of the eMMC to prevent OOMK from kicking in.
But, we can decrease the impact of using swap by reducing the swappiness (systemd var to control frequency of swap use).
This will keep swap use to an absolute minimum while still having it available.

On devices with 2 GB of RAM, swap can be disabled entirely, as being such low-end computers, it is practically impossible to use up all 2 GB of RAM at once. (What the hell are you doing using up 2 GB of RAM on an Atom SoC?)

## Reducing swappiness
```
$ su -
# nano /etc/sysctl.conf
```
To the end of the file, append the following: `vm.swappiness=10`

## Outright disabling swap
Instructions for Debian & devrivatives:

1. Open terminal and run the following:

```
$ su -
# apt install gnome-disk-utility
# swapoff -all
# nano /etc/fstab/
Comment out or delete the line containing UUID for the swap partition.
# nano /etc/initramfs-tools/conf.d/resume
Comment out or delete the line containing UUID for the swap partition.
# exit
```
2. Open Gnome Disks, it should ask for your password.
3. Delete your swap partition.
4. Resize your data partition to utilize the extra space left behind by the swap partition.
5. Back to the terminal:
```
$ su -
# update-initramfs -u -k all
# update-grub
# reboot
```
