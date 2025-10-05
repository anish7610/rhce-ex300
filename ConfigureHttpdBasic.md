### Create new document rootdir

- sudo mkdir -p /newSite

### Modify Httpd Config

- sudo vi /etc/httpd/conf/httpd.conf

DocumentRoot "/newSite"

<pre>
<Directory "/newSite">
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
</pre>

### Set Permissions for the new document root

- echo "\<h1>This is a sample page\</h1>" | tee /newSite/index.html
- sudo chown -R apache:apache /newSite
- sudo chmod -R 755 /newSite

### Test and Restart Apache

- sudo apachectl configtest
- sudo systemctl restart apache2

### Update SELinux File Context

- sudo semanage fcontext -a -t httpd_sys_content_t "/newSite(/.*)?"
- sudo restorecon -Rv /newSite
