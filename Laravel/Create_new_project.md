# Create a new Laravel project

## Basics

* Run `laravel new` command will create a fresh Laravel installation in the directory you specify.
    * For instance, `laravel new blog` will create a directory named `blog` containing a fresh Laravel installation with all of Laravel's dependencies already installed.

## Git

* Setup a git repo on Github and `git init` the local Laravel project. Push the code the Github and pull it down on server.
    * run `composer install` on server after the first `git pull`!
* Update `readme.md`
* Update `.gitignore`
    * add `.DS_Store` and `.idea`
* Create `development` branch for development. Master branch is only for merging and that's what the users see (production server).
    * Use `crontab` to do automatic update on development site
        * run `crontab -e` and add
            * `* * * * * cd /var/www/html/Beta-Laravel && git pull origin master`
            * `* * * * * cd /var/www/html/Beta-Laravel && composer install`
            * `* * * * * cd /var/www/html/Beta-Laravel && php artisan migrate`
    * Run `grep CRON /var/log/syslog` to see crontab output
    * Enable crontab logging
        * run `vim /etc/rsyslog.d/50-default.conf`, remove `#` which is in front of `#cron.*`
        * run `service rsyslog restart && service cron restart`
        * Go to `/var/log/cron.log` to see log

## Change Folder Permission

* Run `sudo chmod -R 775 storage/ bootstrap/cache/ vendor/`
    * If web viewing fails, make sure these are run beforehand
    ```
    sudo adduser <username> www-data
    sudo chown -R www-data:www-data /var/www
    sudo chmod -R g+rwX /var/www
    ```

> After installing Laravel, you may need to configure some permissions. Directories within the storage and the bootstrap/cache directories should be writable by your web server or Laravelwill not run. If you are using the Homestead virtual machine, these permissions should already be set.

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
## `.env` file

* Update .env files on both local and production server, that's the only file not version controlled by git!
    * run `php artisan key:generate`
    * Database connection
        * use 127.0.0.1 on Mac

## Change timezone

* Edit `config/app.php`, change timezone from `UTC` to `Asia/Taipei`

# Notes

## Local testing

* `cd` into the project folder, and run `php -S localhost:8888 -t public` to start the local development server.
* `php artisan serve`

## Syncing projects

Using [Git to sync files](http://stackoverflow.com/questions/20383871/laravel-deployment-there-is-a-standard-way) between local machine and production server. Don't use sftp.

    1. `git pull` the repository
    2. Execute `composer update` to update your vendor folder
    3. Execute `bower update` to download whatever js or css you've installed
    4. Execute `php artisan migrate` to upgrade your database schema
    5. Execute `chmod` and `chown` to fix whatever permission mess those commands might have made to your directories while downloading files
