### Configure iSCSI Target with FileIO

### Install targetcli

- sudo yum install targetcli

### Create a file to serve as the iSCSI virtual disk:

On the iSCSI target machine, create a file that will be used to simulate a block device.

- sudo mkdir -p /var/iscsi_disks
- dd if=/dev/zero of=/var/iscsi_disks/targetfile.img bs=1M count=512

### Start targetcli to configure the iSCSI target:

- sudo targetcli
- /backstores/fileio create myfileio /var/iscsi_disks/targetfile.img 512M

### Create an iSCSI target:

- /iscsi create iqn.2024-11.com.example:target
- /iscsi/iqn.2024-11.com.example:target/tpg1/luns create /backstores/fileio/myfileio

### Create an ACL rule for the initiator

On iSCSI client

- cat /etc/iscsi/initiatorname.iscsi

ON iSCSI target

- cd /iscsi/iqn-target/tpg1
- acls/ create initiator-name

### Configure the Initiatorc

### Install iSCSI client tools

- sudo yum install iscsi-initiator-utils

### Discover the iSCSI target:

- sudo iscsiadm -m discovery -t sendtargets -p target_IP
- sudo iscsiadm -m node --login

### Verify the Connection

Check the iSCSI session:

- sudo iscsiadm -m session

Check the newly created disk:

- fdisk -l
- lsblk

### Formatting and Mounting the New Disk (Optional)


Format the new disk (e.g., /dev/sda):

- sudo mkfs.ext4 /dev/sda

Create a mount directory

- sudo mkdir -p /mnt/iscsi

Mount the new disk:

- sudo mount /dev/sda /mnt
