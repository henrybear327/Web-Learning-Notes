# [Set up virtual host on apache](https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-virtual-hosts-on-ubuntu-16-04)

## Setup

* Setup a folder `sudo mkdir -p /var/www/[domian name to use]/html`
* Grant permission `sudo chown -R $USER:$USER /var/www/[domian name to use]/html`
* `sudo chmod -R 755 /var/www`

## Create Demo Page

Run `vim /var/www/[domian name to use]/html/index.html` and add
```
<html>
  <head>
    <title>Welcome to Example.com!</title>
  </head>
  <body>
    <h1>Success!  The example.com virtual host is working!</h1>
  </body>
</html>
```

# Create New Virtual Host Files

* `sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/[domian name to use].conf`
* `sudo vim /etc/apache2/sites-available/[domian name to use].conf`
  * We need to add two directives. The first, called ServerName, establishes the base domain that should match for this virtual host definition. This will most likely be your domain. The second, called ServerAlias, defines further names that should match as if they were the base name. This is useful for matching hosts you defined, like www
  ```
  ServerName [domian name to use]
  ServerAlias www.[domian name to use]
  ```
  * Add `DocumentRoot /var/www/[domian name to use]/html`
  * The file should look something like this:
  ```
  <VirtualHost *:80>
    ServerAdmin admin@example.com
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>
  ```

# Enable/Disable the New Virtual Host Files

* `sudo a2ensite [domian name to use].conf`
* `sudo a2dissite 000-default.conf`

# Restart

* `sudo systemctl restart apache2`
