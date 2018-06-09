# redhat-learning
RHEL 7 tips and tricks

- SELinux stands for Security-Enhanced Linux. It is a way to improve the server security.
1. Set SElinux to enforcing mode, Persistent across reboot.
- vim /etc/selinux/config
- set `SELINUX=enforcing`
- reboot (to make it persistent)
- Confirm that `getenforce` returns `Enforcing`
