# redhat-learning
RHEL 7 tips and tricks

1. Set SElinux to enforcing mode, Persistent across reboot.
- vim /etc/selinux/config
- set `SELINUX=enforcing`
- reboot (to make it persistent)
- Confirm that `getenforce` returns `Enforcing`
