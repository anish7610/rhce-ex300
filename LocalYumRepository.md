### Create a YUM repository

- sudo yum install createrepo -y
- sudo mkdir -p /var/local-repo
- chmod -R 755 /var/local-repo
- cd /var/local-repo

### Download RPM's from here: https://rpmfind.net/linux/rpm2html/search.php

E.g hello

- sudo wget https://rpmfind.net/linux/fedora/linux/development/rawhide/Everything/aarch64/os/Packages/h/hello-2.12.1-5.fc41.aarch64.rpm

### Create Repository Metadata
- sudo createrepo /var/local-repo

### Confingure the local YUM repository

- sudo vi /etc/yum.repos.d/local.repo

[local-repo]
name=Local Repository
baseurl=file:///var/local-repo
enabled=1
gpgcheck=0

### Update YUM cache

- sudo yum clean all
- sudo yum makecache

### Verify the repository

- yum repolist

###  Test installing a package

- sudo yum install package-name

### Serve the repository over HTTP

- sudo yum install httpd -y
- sudo systemctl enable httpd
- sudo systemctl start httpd

### Move the repository to the web directory

- sudo mv /var/local-repo /var/www/html/

### Make the repository accessible

Update the .repo file to use baseurl=http://server-ip/local-repo.

### Verify

Access the URL in a browser to ensure it's readable
