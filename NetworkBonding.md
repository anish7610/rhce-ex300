### Add a second network interface

Go to UTM -> Select the VM -> Edit -> + -> Network
Start the VM

- yum install -y ethtool
- ethtool --version
- ip link show

### Configure dhcp for the newly added interface via NetworkManager

- nmcli device status

###  Add 2 NIC's for bonding purposes so as to not mess with the VM's connectivity
- sudo nmcli connection add type ethernet ifname enp0s2 con-name enp0s2 autoconnect yes
- sudo nmcli connection add type ethernet ifname enp0s3 con-name enp0s3 autoconnect yes
- sudo nmcli connection up enp0s2
- sudo nmcli connection up enp0s3

### Configure Network Bonding

- nmcli device

### Use enp0s1 and enp0s3 devices as slave interfaces, since enp0s2 is assgined to System

<pre>
[anish@centos-2 ~]$ nmcli device

DEVICE  TYPE      STATE                   CONNECTION    
enp0s2  ethernet  connected               System enp0s2 
enp0s1  ethernet  connected               enp0s1        
enp0s3  ethernet  connected               enp0s3        
lo      loopback  connected (externally)  lo            
</pre>

### Create the Bond Interface
- sudo nmcli connection add type bond con-name bond0 ifname bond0 mode active-backup

### Add Slaves to the Bond
- sudo nmcli connection add type bond-slave ifname enp0s1 master bond0
- sudo nmcli connection add type bond-slave ifname enp0s3 master bond0

### Assign an IP Address to Bond Interface
- sudo nmcli connection modify bond0 ipv4.method auto

### Bring up the Bond Interface

- sudo nmcli connection up bond0
- sudo nmcli connection up bond-slave-enp0s1
- sudo nmcli connection up bond-slave-enp0s3

### Check Bond status

<pre>
[anish@centos-2 ~]$ nmcli device

DEVICE  TYPE      STATE                   CONNECTION        
enp0s2  ethernet  connected               System enp0s2     
bond0   bond      connected               bond0             
enp0s1  ethernet  connected               bond-slave-enp0s1 
enp0s3  ethernet  connected               bond-slave-enp0s3 
</pre>

<pre>
[anish@centos-2 ~]$ sudo cat /proc/net/bonding/bond0 
Ethernet Channel Bonding Driver: v5.14.0-529.el9.aarch64

Bonding Mode: fault-tolerance (active-backup)
Primary Slave: None
Currently Active Slave: enp0s1
MII Status: up
MII Polling Interval (ms): 100
Up Delay (ms): 0
Down Delay (ms): 0
Peer Notification Delay (ms): 0

Slave Interface: enp0s1
MII Status: up
Speed: Unknown
Duplex: Unknown
Link Failure Count: 0
Permanent HW addr: c6:e5:6f:f1:ad:2d
Slave queue ID: 0

Slave Interface: enp0s3
MII Status: up
Speed: Unknown
Duplex: Unknown
Link Failure Count: 0
Permanent HW addr: ba:9a:5a:36:82:2f
Slave queue ID: 0
</pre>

#### Turn Off Primary Slave Interface enp0s1

nmcli connection down bond-slave-enp0s1

### Verify failover takes place and enp0s3 is now the Currently active slave

<pre>
[anish@centos-2 ~]$ sudo cat /proc/net/bonding/bond0 
Ethernet Channel Bonding Driver: v5.14.0-529.el9.aarch64

Bonding Mode: fault-tolerance (active-backup)
Primary Slave: None
Currently Active Slave: enp0s3
MII Status: up
MII Polling Interval (ms): 100
Up Delay (ms): 0
Down Delay (ms): 0
Peer Notification Delay (ms): 0

Slave Interface: enp0s3
MII Status: up
Speed: Unknown
Duplex: Unknown
Link Failure Count: 0
Permanent HW addr: ba:9a:5a:36:82:2f
Slave queue ID: 0
</pre>
