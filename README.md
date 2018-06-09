# redhat-learning
RHEL 7 tips and tricks

1. Set SElinux to enforcing mode, Persistent across reboot.
- info- SELinux stands for Security-Enhanced Linux. It is a way to improve the server security.The `getenforce` command returns Enforcing, Permissive, or Disabled.
- vim /etc/selinux/config
- set `SELINUX=enforcing`
- reboot (to make it persistent)
- Confirm that `getenforce` returns `Enforcing`
