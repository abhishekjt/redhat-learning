# redhat-learning
RHEL 7 tips and tricks

1. Set SElinux to enforcing mode, Persistent across reboot.
  * info- SELinux stands for Security-Enhanced Linux. It is a way to improve the server security.The `getenforce` command returns Enforcing, Permissive, or Disabled.
  * vim `/etc/selinux/config`
  * set `SELINUX=enforcing`
  * reboot (to make it persistent)
  * Confirm that `getenforce` returns `Enforcing`
  
  2. List all lines which have string `enter` from `/usr/share/dict/words` file and copy the lines to `/root/words.found` file.
  * `grep "enter" /usr/share/dict/words >>/root/word.found`
  
  3. Download file from `"http://classroom.example.com/content/rhcsa/sample.txt"` Search lines which contains alpha-numeric words( combination of alphabets and number) and copy those lines is sorted order to `/root/samplelines` file.
  * `grep -Ei "[a-z][0-9]|[0-9][a-z]" sample.txt |sort >>/root/samplelines`
  
  4. Locate the files of owner `larry` and copy to the directory `/root/found` directory
* `mkdir -p /root/found`
* `find / -user larry -type f | xargs cp -rp {} /root/found`

5. Create the user "dax" with uid 4223.
* `useradd -u 4223 dax`

6. Compress `/etc` folder with `gun zip` technique under the directory `/opt`
* `cd /opt`
* `tar -cvf etc.tar /etc`
* `gzip etc.tar`

7. Create the SWAP space of "250 MB” dont remove the existing swap.
* `free -m`
* `fdisk /dev/sda` #create partition with 250MB
* `mkswap /dev/sda#`
* make entry in `/etc/fstab` 
* `mount -a`
* `swapon -a`
* `free -m`

8. Create the "LVM" with the name "store" by using 15PE's from the volume group "data". Consider the PE size as "16MB". Mount it on /mnt/store with filesystem vfat.
* Make sure extended partition is already created , if not create it by doing `fdisk`, `e` and select the default options and save it.
* `fdisk /dev/sda` with type LVM and part size +256M (since (16*15) = 240 , +16M(size of each PE))
* `pvcreate /dev/sda#`
* `vgcreate -s +16M vgdata /dev/sda#`
* `lvcreate -L +240M -n lvdata vgdata`
* `mkfs.vfat /dev/vgdata/lvdata`
* `mkdir /mnt/strore`
* `make the entry in fstab` (to make it persistent)
* mount -a (to check if mount details were right)
* `df -h` 

9. List all lines which have string "enter" from /usr/share/dict/words file and copy the lines to /root/words.found file.
* `grep enter /usr/share/dict/words > /root/words.found`

10. Create the "LVM" with the name "slab" of 160MB from the volume group "marvel". Consider the PE size as "8MB". Mount it on /mnt/slab with filesystem xfs. Resize the lvm "/dev/marvel/slab" so that after reboot size should be 320MB.
* use fdisk to create partition of 328M (because we need to resize it to 320M)
* `pvcreate /dev/sda#`
* `vgcreate -s +8M marvel /dev/sda#`
* `lvcreate -L +160M -n slab marvel`
* `mkfs.xfs /dev/marvel/slab`
* make the entry in /etc/fstab
* `lvextend -L +160M /dev/marvel/slab` (if it shows no space, add volume using vgextend ) (check `lvs`)
* `xfs_growfs /dev/marvel/slab` (check `df-h`)

11. Create a group named "stoogs".A user “curly” and “larry” should belongs to "stoogs" group as a secondary group . A user “moe” should not have access to interactive shell and he should not be a member of "stoogs" group. passwd for all user created should be "jenny".
* `groupadd stoogs`
* `useradd curly`
* `useradd larry`
* `usermod -aG stoogs curly`
* `usermod -aG stoogs larry`
* `useradd -s /sbin/nologin moe`
* `passwd curly`
* `passwd larry`
* `passwd moe`

12. Create the Directory "/home/manager" with the following characteristics. Group ownership of "/home/manager" should go to "stoogs" group. The directory should be have full permission for all members of "stoogs" group but not to any other users except "root". Files created under "/home/manager" should get the same group ownership is set to the "stoogs" group.
* `mkdir -p /home/manager`
* `usermod -aG stoogs /home/manager`
* `chmod 770 /home/manager`
* `usermod g+s /home/manager` (g+s means that all new files and subdirectories created within the current directory inherit the group ID of the directory)

13. Copy the file /etc/fstab to /var/tmp and configure the "ACL" as mention following. The file /var/tmp/fstab is owned by the "root". The file /var/tmp/fstab belongs to the group "root" The file /var/tmp/fstab should not be executable by other's. The user "curly" should able to read and write to the file. The user "larry" can neither read nor write to the file. other users (future and current) shuold be able to read /var/tmp/fstab.
* `cp /etc/fstab /var/tmp/`
* `chown root /var/tmp/fstab`
* `chgrp root /var/tmp/fstab`
* `setfacl -m o::r /var/tmp/fstab`
* `setfacl -m u:curly:rw /var/tmp/fstab`
* `setfacl -m u:larry:--- /var/tmp/fstab` (without rd/w , we cannot execute a file in linux)
