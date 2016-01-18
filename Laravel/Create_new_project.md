# Create a new Laravel project

## Basics

* Run `laravel new` command will create a fresh Laravel installation in the directory you specify.
    * For instance, `laravel new blog` will create a directory named `blog` containing a fresh Laravel installation with all of Laravel's dependencies already installed.
* Run `php artisan serve` in the folder to start the local development server

## Change Folder Permission

* Run `sudo chmod -R 775 storage/ bootstrap/cache/ vendor/`
    * If web viewing fails, make sure these are run beforehand
    ```
    sudo adduser <username> www-data
    sudo chown -R www-data:www-data /var/www
    sudo chmod -R g+rwX /var/www
    ```

> After installing Laravel, you may need to configure some permissions. Directories within the storage and the bootstrap/cache directories should be writable by your web server or Laravelwill not run. If you are using the Homestead virtual machine, these permissions should already be set.

## Change timezone

* Edit `config/app.php`, change timezone from `UTC` to `Asia/Taipei`

## Create Apache VirtualHost

Add a Virtual Host in Apache configuration file to access Laravel framework from web browser.

Don't forget to update DNS settings, too! Add `cname` in the DNS page.

Create Apache configuration file under `/etc/apache2/sites-available/` directory , name the file as `beta.henrybear327.com.conf`, and add the content below:
```
<VirtualHost *:80>
        ServerName beta.henrybear327.com
        DocumentRoot /var/www/html/beta/public

        <Directory /var/www/html/beta/public>
                AllowOverride All
                Options -Indexes +FollowSymLinks
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

## Notes for local testing

* `cd` into the project folder, and run `php -S localhost:8888 -t public` to start the local development server.
* `php artisan serve`
