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
* `fdisk /dev/devicename` #create partition with 250MB
* `mkswap /dev/devicename`
* make entry in `/etc/fstab` 
* `swapon -a`
* `free -m`
