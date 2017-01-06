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
* The `.conf` file should look something like this (change `[domian name to use]` according to your needs):
```
<VirtualHost *:80>
  ServerAdmin admin@[domian name to use]
  ServerName [domian name to use]
  ServerAlias www.[domian name to use]
  DocumentRoot /var/www/[domian name to use]/public

  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
    * The DocumentRoot for Laravel must point to `public` folder!
    * Comment out `RewriteCond` and `RewriteRule`
    ```
    RewriteEngine on
    # RewriteCond %{SERVER_NAME} =[domian name to use]
    # RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,QSA,R=permanent]
    ```

# Enable/Disable the New Virtual Host Files

* `sudo a2ensite [domian name to use].conf`
* `sudo a2dissite 000-default.conf`

# Restart

* `sudo systemctl restart apache2`
