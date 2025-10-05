### Configuring CGroups

- mount | grep cgroup
- systemd-cgtop

### Start http service

- yum install -y httpd
- systemctl status httpd

- cat /proc/<httpd-pid>/cgroup

CPU/Memory not configured yet

### Create a new service file for httpd

- mkdir -p /etc/systemd/system/http.service.d
- sudo vi /etc/systemd/system/http.service.d/httpd.service/override.conf

<pre>
[Service]
CPUSWeight=1500
MemoryMax=512M
</pre>

- sudo systemctl daemon-reload
- sudo systemctl restart httpd
- systemd-cgtop
