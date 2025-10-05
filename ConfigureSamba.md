### Install Samba Server

- sudo yum update -y
- sudo yum install samba -y
- samba -V

### Create a Directory for the share

- sudo mkdir -p /srv/samba/share

### Set the appropriate permissions

- sudo chmod -R 775 /srv/samba/share
- sudo chown -R nobody:nobody /srv/samba/share

### Configure Samba

- sudo nano /etc/samba/smb.conf

<pre>
[SambaShare]
path = /srv/samba/share
writable = yes
browsable = yes
guest ok = yes
create mask = 0775
directory mask = 0775
</pre>

### Adjust firewall rules

- sudo firewall-cmd --permanent --add-service=samba
- sudo firewall-cmd --reload
- sudo firewall-cmd --list-all

### Allow Samba to share files

- sudo setsebool -P samba_enable_home_dirs on
- sudo setsebool -P samba_export_all_rw on

### Start and Enable Samba Services

- sudo systemctl enable smb
- sudo systemctl start smb
- sudo systemctl status smb

### Test the Configuration from Client VM

- sudo yum install samba-client -y
- testparm

### Access the share from another machine

- smbclient //server-ip/SambaShare
