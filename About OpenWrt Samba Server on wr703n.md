## 硬件
1. usb存储设备
2. wr703n
3. 可以使用的linux系统

## 步骤
###### 备份路由配置
ref: [generic.backup](http://wiki.openwrt.org/doc/howto/generic.backup)

```
ssh root@openwrt "cd /etc; tar cvz config" > openwrt.tar.gz
```
###### 存储设备分区

先分区

```
$ fdisk device_name
```

Disable journal, enable extent for large file, 2048k per block, one inode per 8192bytes

```
$ mkfs -t ext4 -O ^has_journal,extent -b 2048 -i 8192 partition_name
```

###### 配置overlay

ref: [extroot](http://wiki.openwrt.org/doc/howto/extroot)

```
opkg update
opkg install kmod-fs-ext4
opkg install block-mount
```

```
/etc/init.d/fstab enable
```

```
mkdir /mnt/sda1
mount /dev/sda1 /mnt/sda1
```

```
tar -C /overlay -cvf - . | tar -C /mnt/sda1 -xf -
```

```
config mount
        option target        /overlay
        option device        /dev/sda1
        option fstype        ext3
        option options       rw,sync
        option enabled       1
        option enabled_fsck  0
```

```
reboot
```

###### 安装配置samba

```
opkg update
opkg install kmod-usb2
opkg install kmod-usb-storage
opkg install samba36-server
```

`/etc/samba/smb.conf.template`

```
[global]
        netbios name = |NAME|
        display charset = |CHARSET|
        interfaces = |INTERFACES|
        server string = |DESCRIPTION|
        unix charset = utf-8             #-
        workgroup = |WORKGROUP|
        browseable = yes
        deadtime = 30
        domain master = yes
        encrypt passwords = true
        enable core files = no
        guest account = nobody
        guest ok = yes
#-      invalid users = root
        local master = yes
        load printers = no
        map to guest = Bad User
        max protocol = SMB2
        min receivefile size = 16384
        null passwords = yes
        obey pam restrictions = yes
        os level = 20
        passdb backend = smbpasswd
        preferred master = yes
        printable = no
        security = user
        smb encrypt = disabled
        smb passwd file = /etc/samba/smbpasswd
        socket options = TCP_NODELAY IPTOS_LOWDELAY
        syslog = 2
        use sendfile = yes
        writeable = yes
```

`/etc/config/samba`

```
config samba
        option 'name'                   'OpenWrt'
        option 'workgroup'              'WORKGROUP'
        option 'description'            'OpenWrt'
        option 'homes'                  '1'
config 'sambashare'
        option 'name'           'Shares'
        option 'path'           '/mnt/sda2'
#       option 'users'          'root'
        option 'read_only'      'no'
        option 'guest_ok'       'no'
```

```
smbpasswd -a root # add user 'root' and set password
/etc/init.d/samba start
```

###### 安装配置aria2

ref: [恩山](http://www.right.com.cn/forum/thread-116088-1-1.html)

```
opkg update
opkg install aria2
```

`/etc/config/aria2`

```
#Aria2 configuration

# RPC Setting
#
enable-rpc=true
rpc-listen-all=true
rpc-allow-origin-all=true
rpc-listen-port=6800
#rpc-user=rpc_user
#rpc-passwd=rpc_passwd


# General Setting
#
dir=/mnt/sda1/download/aria2
input-file=/mnt/sda1/aria2/aria2.session
save-session=/mnt/sda1/aria2/aria2.session
save-session-interval=60
log=/mnt/sda1/aria2/aria2.log
log-level=warn
#event-poll=select
disk-cache=8M
#enable-mmap=true
file-allocation=trunc
user-agent=uTorrent/2210(25130)

# Connection Setting
#
continue=true
max-connection-per-server=5
max-concurrent-downloads=3
min-split-size=1M
split=5
max-overall-download-limit=0
auto-save-interval=120
check-certificate=false
```

```
aria2c --conf-path=/etc/config/aria2 -D
```

`/etc/init.d/aria2`

```
#!/bin/sh /etc/rc.common

START = 70

start() {
        echo start
        aria2c --conf-path=/etc/config/aria2 -D
}

stop() {
        echo stop
        aria2c --stop=1
}
```

###### 配置远程访问

## Modules

* kmod-usb-core (default installed)
* kmod-usb-ohci (default installed)
* kmod-usb2
* kmod-usb-storage
* kmod-fs-ext4
* block-mount
* samba36-server
* aria2
