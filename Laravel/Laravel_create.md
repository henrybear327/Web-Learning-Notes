## [Laravel](https://laravel.com/docs/5.2#server-requirements)

### Installation
* Run `composer global require "laravel/installer"`
* Add `export PATH=$PATH:~/.composer/vendor/bin` to `.bashrc`

### Enable `mod rewrite`

* Run `a2enmod rewrite`
* service apache2 restart

## Create a new Laravel project

### Basics

Run `laravel new` command will create a fresh Laravel installation in the directory you specify.

For instance, `laravel new blog` will create a directory named `blog` containing a fresh Laravel installation with all of Laravel's dependencies already installed.

### Change Folder Permission

* Run `sudo chmod -R 775 storage/ bootstrap/cache/ vendor/`
    * If fails, make sure these are run beforehand
    ```
    sudo adduser <username> www-data
    sudo chown -R www-data:www-data /var/www
    sudo chmod -R g+rwX /var/www
    ```

> After installing Laravel, you may need to configure some permissions. Directories within the storage and the bootstrap/cache directories should be writable by your web server or Laravel will not run. If you are using the Homestead virtual machine, these permissions should already be set.

### Create Apache VirtualHost

Add a Virtual Host in Apache configuration file to access Laravel framework from web browser.

Don't forget to update DNS settings, too! Add `cname` in the DNS page.

Create Apache configuration file under `/etc/apache2/sites-available/` directory and add below content.
```
<VirtualHost *:80>
        ServerName beta.henrybear327.com
        DocumentRoot /var/www/html/Ecourse/public

        <Directory /var/www/html/Ecourse/public>
                AllowOverride All
                Options Indexes FollowSymLinks
                Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        LogLevel warn
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

After that, run
```
a2ensite beta.henrybear327.com
sudo service apache2 reload
```

Note that the file name must start with `beta.henrybear327.com`.

### Notes for local testing

* `cd` into the project folder, and run `php -S localhost:8888 -t public` to start the local development server.
