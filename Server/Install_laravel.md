# [Laravel](https://laravel.com/docs/5.3/installation)

## Install [Composer](https://getcomposer.org/download/)

* Make sure that you are at `$HOME` directory!
* `mv composer.phar /usr/local/bin/composer`

## Enable `mod rewrite`

* Run `a2enmod rewrite`
* `service apache2 restart`

## Install Laravel
* Install `sudo apt-get install php7.0-zip`
* Run `composer global require "laravel/installer"`
* Add `export PATH=$PATH:$HOME/.config/composer/vendor/bin` to `.bashrc`
    * Add to `.bash_profile` if you are on Mac
    * PATH may be `~/.composer/vendor/bin`

## Create new project

* `laravel new project-name`
* This following step is CRUCIAL
```
chmod -R 777 storage/
chmod -R 777 bootstrap/cache/
```
* Set up [virtual host](./VirtualHost.md), point it directly to `/public/` folder
    * [Add the following code](https://www.dev-metal.com/enable-mod_rewrite-ubuntu-14-04-lts/) in `<VirtualHost *:80>` of the virtual host setting
    ```
    <Directory "/var/www/[Your Directory]">
        AllowOverride All
    </Directory>
    ```
* Create `404 page`, and add `Google analytics`
