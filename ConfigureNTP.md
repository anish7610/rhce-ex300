### Configure NTP Service

- sudo dnf list installed chrony
- sudo dnf install chrony -y
- sudo systemctl enable --now chronyd
- sudo systemctl status chronyd

### Update NTP servers
- sudo nano /etc/chrony/chrony.conf

<pre>
server time.google.com iburst
server 0.centos.pool.ntp.org iburst
server 1.centos.pool.ntp.org iburst
</pre>

### Restart chronyd
- sudo systemctl restart chronyd

### Verify Synchronization

- chronyc sources -v

### To force immediate synchronization with servers

- sudo chronyc makestep

### Allow NTP Requests from Other Systems

- allow <network>

### Check logs

- journalctl -u chronyd

### Confirm Time Synchronization

- timedatectl status
