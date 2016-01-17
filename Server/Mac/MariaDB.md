# MariaDB

## [Installation](https://mariadb.com/kb/en/mariadb/building-mariadb-on-mac-os-x-using-homebrew/)

We will be using Homebrew for simplicity.

* run `brew update`
* run `brew install mariadb`
* run `unset TMPDIR && mysql_install_db`
* Check the output that was on the screen and run the command similar to this: `/usr/local/Cellar/mariadb/5.2.6/bin/mysql_secure_installation`

## Usage

Use `mysql.server start` to start and `mysql.server stop` MariaDB

## Login

Use `mysql -uroot` to login

# Sequel Pro

* Install it for easy database managament!
