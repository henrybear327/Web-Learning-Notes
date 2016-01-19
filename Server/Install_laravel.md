## [Laravel](https://laravel.com/docs/5.2#server-requirements)

### Installation
* Run `composer global require "laravel/installer"`
* Add `export PATH=$PATH:~/.composer/vendor/bin` to `.bashrc`
    * Add to `.bash_profile` if you are on Mac

### Enable `mod rewrite`

* Run `a2enmod rewrite`
* `service apache2 restart`
