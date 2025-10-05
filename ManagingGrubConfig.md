### Grub Config file

- sudo cat /boot/grub2/grub.cfg
- sudo cat /etc/default/grub

### Modify Grub Default Timeout

- sudo -i
- vi /etc/default/grub

GRUB_TIMEOUT=20

- grub2-mkconfig > /boot/grub2/grub.cfg

### Reboot

- reboot

### Switch into single-user mode

Press 'e' to enter Grub command line editor and append the following parameter to switch to single-user mode
at the end of the line starting with 'linux'

systemd.unit=rescue.target

- runlevel
- systemctl isolate multi-user.target # switch to multi-user mode
- systemctl isolate graphical.target
