### Mounting file systems using systemd

- cd /usr/lib/systemd/system
- ls -lhtr *mount
- more tmp.mount
- cd /etc/systemd/system
- ls -lhtrc
- cp /usr/lib/systemd/system/tmp.mount data.mount

### Edit Mount Service File

<pre>
[Unit]
Description=Data Directory /data
Documentation=https://systemd.io/TEMPORARY_DIRECTORIES
Documentation=man:file-hierarchy(7)
Documentation=https://www.freedesktop.org/wiki/Software/systemd/APIFileSystems
ConditionPathIsSymbolicLink=!/tmp
DefaultDependencies=no
Conflicts=umount.target
Before=local-fs.target umount.target
After=swap.target

[Mount]
What=/dev/sdb
Where=/data
Type=ext4
Options=defaults

# Make 'systemctl enable tmp.mount' work:
[Install]
WantedBy=multi-user.target
</pre>

### Create mount directory

- mkdir /data

### Reload Systemctl

- systemctl daemon-reload
- systemctl start data.mount
- systemctl status data.mount

### Check if the mount exists

- df -h

### Enable mount at boot time

- systemctl enable data.mount

### Reboot and verify if it's mounted

- systemctl reboot
