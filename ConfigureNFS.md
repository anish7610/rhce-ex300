### Install NFS utilities

- sudo yum install -y nfs-utils

### Create the Shared Directory

- sudo mkdir -p /shared

### Set appropriate permissions

- chmod 775 -R /shared
- chown -R nobody:nobody /shared

### Configure NFS exports

- sudo nano /etc/exports

/shared 192.168.64.0/24(rw,sync,no_root_squash,no_subtree_check)

### Apply the Export Configuration

- sudo exportfs -r

### Start the necessary services

- sudo systemctl start nfs-server

### Enable them to start on boot

- sudo systemctl enable nfs-server

### On the NFS Client

- sudo yum install -y nfs-utils

### Create a Mount Point

- sudo mkdir -p /mnt/nfs/shared

### Mount the NFS Share

- sudo mount -t nfs server-ip:/shared /mnt/nfs/shared

### Verify the Mount

- df -h

### Make the Mount Permanent

- sudo nano /etc/fstab

server-ip:/shared /mnt/nfs/shared nfs defaults 0 0

### Test Read/Write access to the shared directory

- echo "Testing NFS Share" > /mnt/nfs/shared/testfile.txt
- cat /mnt/nfs/shared/testfile.txt

- sudo systemctl status nfs-server
- sudo journalctl -xe
