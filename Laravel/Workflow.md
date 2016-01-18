# Web development workflow

Using Git to sync files between local machine and production server. Don't use sftp.

# Setup

* Setup a git repo on Github
* Create Laravel repo at local machine and push the code the Github
* Create `development` branch for development. Master branch is only for merging and that's what the users see.
    * Use `crontab` to do automatic `git pull`
        * run `crontab -e` and add `*/5 * * * * cd /var/www/html/Beta-Laravel/ && git pull -q origin master && composer install`
    * Go to `grep CRON /var/log/syslog` to see crontab output
* Update .env files on both local and production server, that's the only file not version controlled by git!
    * run `php artisan key:generate`
    * Database connection
