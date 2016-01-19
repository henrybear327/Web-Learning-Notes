# Web development workflow

Using [Git to sync files](http://stackoverflow.com/questions/20383871/laravel-deployment-there-is-a-standard-way) between local machine and production server. Don't use sftp.

    1. `git pull` the repository
    2. Execute `composer update` to update my vendor folder
    3. Execute `bower update` to download whatever js or css I've installed
    4. Execute `php artisan migrate` to upgrade my database schema
    5. Execute `chmod` and `chown` to fix whatever permission mess those commands might have made to my directories while downloading files

# Setup

* Setup a git repo on Github and create Laravel repo at local machine. Push the code the Github and pull it down on server.
    * run `composer install` on server at the first time!
* Create `development` branch for development. Master branch is only for merging and that's what the users see (production server).
    * Use `crontab` to do automation
        * run `crontab -e` and add
            * `* * * * * cd /var/www/html/Beta-Laravel && git pull origin master`
            * `* * * * * cd /var/www/html/Beta-Laravel && composer install`
            * `* * * * * cd /var/www/html/Beta-Laravel && php artisan migrate`
    * Run `grep CRON /var/log/syslog` to see crontab output
    * Enable crontab logging
        * run `vim /etc/rsyslog.d/50-default.conf`, remove `#` which is in front of `#cron.*`
        * run `service rsyslog restart && service cron restart`
        * Go to `/var/log/cron.log` to see log
* Update .env files on both local and production server, that's the only file not version controlled by git!
    * run `php artisan key:generate`
    * Database connection
        * use 127.0.0.1 on Mac
