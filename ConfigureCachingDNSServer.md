### Install unbound
- sudo dnf install unbound -y

### Configure unbound
- sudo nano /etc/unbound/unbound.conf

Add the following configuration

<pre>
server:
    # Listen on localhost
    interface: 127.0.0.1

    # Cache size
    msg-cache-size: 50m
    rrset-cache-size: 100m

    # Number of threads
    num-threads: 2

    # Upstream DNS servers
    forward-zone:
        name: "."
        forward-addr: 8.8.8.8
        forward-addr: 8.8.4.4
</pre>

- sudo systemctl enable --now unbound

### Point the System's DNS to unbound

- sudo nano /etc/resolv.conf

nameserver 127.0.0.1

### Make it immutable

- chattr +i /etc/resolv.conf

### Test DNS Caching

- dig example.com
- dig example.com


### Verify the Caching DNS

- dig example.com
- nslookup example.com

Check Logs

- sudo journalctl -u unbound

Monitor Queries

- sudo tcpdump -i interface_name port 53
