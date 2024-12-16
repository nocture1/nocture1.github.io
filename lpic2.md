# ファイルシステム
```tree
/
├─/dev
|   |
|   ├─/tty1
|   ├─/sda
|   ├─/sdb
|   ├─/sdc
|   ├─/sdd
|   ├─/sr0
|   └─/st0
|
├─/var
|   |
|   ├─/cache
|   ├─/lock
|   ├─/run
|   |   |
|   |   └─/log
|   |       |
|   |       └─journal
|   |       
|   ├─/log
|   |   |
|   |   ├─messages
|   |   ├─dmesg
|   |   ├─journal
|   |   └─boot.log
|   |
|   ├─/run
|   └─/spool
|       |
|       └─cron
|
├─/tmp
|   |
|
├─/lib
|   |
|   └─/systemd
|       |
|       └─system
|
├─/proc
|   |
|   ├─/cpuinfo
|   ├─/interrupts
|   ├─/ioports
|   ├─/meminfo
|   ├─/modules
|   └─/bus
|       |
|       ├─usb
|       └─pci

|
├─/etc
|   |
|   ├─/updatedb.conf
|   ├─/init.d
|   |   |
|   |   ├─ntpd
|   |   └─sshd
|   |
|   ├─/hosts
|   ├─/services
|   ├─/ntp.conf
|   ├─/crontab
|   ├─/cron.*
|   ├─/cron.deny
|   ├─/cron.allow
|   ├─/chrony.deny
|   ├─/chrony.allow
|   ├─/hosts.deny
|   ├─/hosts.allow
|   ├─/nologin
|   ├─/false
|   ├─/rsyslog.d
|   ├─/rsyslog.conf
|   ├─/at.deny
|   ├─/at.allow
|   ├─/resolv.cnf
|   ├─/fstab
|   ├─/mtab
|   ├─/sudoers
|   ├─/ld.so.conf
|   ├─/ld.so.cache
|   ├─/yum.conf
|   ├─/inittab
|   ├─/yum.repos.d
|   ├─/machine-id
|   ├─/passwd
|   ├─/shadow
|   ├─/skel
|   ├─/aliases
|   ├─/aliases.db
|   ├─/logrotate.conf
|   ├─/default
|   |   |
|   |   └─grub
|   |
|   ├─/udev
|   |   |
|   |   └─rules.d
|   |
|   ├─/systemd
|   |   |
|   |   ├─system
|   |   └─journal
|   |
|   └─/x11
|       |
|       ├─xorg.conf
|       └─xorg.conf.d
|
├─/opt
|
├─/boot
|   |
|   └─/grub
|       |
|       ├─menu.lst
|       ├─grub.cfg
|       └─grub.conf
|
├─/bin
|
├─/sbin
|   |
|   └─/nologin
|
└─/usr
    |
    ├─/bin
    ├─/local
    ├─/sbin
    ├─/share
    └─/src

~
└─/.ssh
    |
    ├─/known.host
    └─/authorized_key
```