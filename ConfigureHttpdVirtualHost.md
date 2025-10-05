### Create Webroot Directories

- sudo mkdir -p /var/www/example.com/html
- sudo mkdir -p /var/www/test.com/html

### Set Ownership and Permissions
Ensure the Apache user (usually apache or www-data) has access to the webroot directories.

- sudo chown -R apache:apache /var/www/example.com
- sudo chown -R apache:apache /var/www/test.com
- sudo chmod -R 775 /var/www

### Add Website Content
Place your website files in the html subdirectories of the webroot.

- echo "\<h1>Welcome to example.com!\</h1>" | sudo tee /var/www/example.com/html/index.html
- echo "\<h1>Welcome to test.com!\</h1>" | sudo tee /var/www/test.com/html/index.html


### Configure Apache to Use the Webroot Directories
Create Virtual Host Files

### For example.com:

- sudo nano /etc/httpd/conf.d/example.com.conf
Add the following:

<pre>
<VirtualHost *:80>
    ServerAdmin webmaster@example.com
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com/html
    ErrorLog /var/log/httpd/example.com_error.log
    CustomLog /var/log/httpd/example.com_access.log combined
</VirtualHost>
</pre>

### For test.com:

- sudo nano /etc/httpd/conf.d/test.com.conf

<pre>
<VirtualHost *:80>
    ServerAdmin webmaster@test.com
    ServerName test.com
    ServerAlias www.test.com
    DocumentRoot /var/www/test.com/html
    ErrorLog /var/log/httpd/test.com_error.log
    CustomLog /var/log/httpd/test.com_access.log combined
</VirtualHost>
</pre>

### Restart Apache
Restart the Apache service to apply the new configurations.

- sudo systemctl restart httpd

### Test the Configuration

Access the Websites

Open your browser and navigate to:

- http://example.com
- http://test.com

Local Testing
If testing locally, update /etc/hosts:

- sudo nano /etc/hosts

<pre>
127.0.0.1 example.com
127.0.0.1 test.com
</pre>

### Monitor Apache Logs
Check logs to ensure everything is working correctly:

- sudo tail -f /var/log/httpd/example.com_error.log
- sudo tail -f /var/log/httpd/test.com_error.log
