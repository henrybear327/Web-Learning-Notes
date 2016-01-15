# Old notes

## MariaDB

### [Installation](https://downloads.mariadb.org/mariadb/repositories/#mirror=opencas&distro=Ubuntu&distro_release=trusty--ubuntu_trusty&version=10.1)

* `sudo apt-get install software-properties-common`
* `sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xcbcb082a1bb943db`
* `sudo add-apt-repository 'deb [arch=amd64,i386] http://mirrors.opencas.cn/mariadb/repo/10.1/ubuntu trusty main'`
* `sudo apt-get update`
* `sudo apt-get install mariadb-server`
* `sudo service mysql stop`
* `sudo mysql_install_db`

### [Secure MariaDB](https://mariadb.com/kb/en/mariadb/mysql_secure_installation/)

* `sudo service mysql start`
* run `mysql_secure_installation`
* rename `root` user
  * Method 1:
    * login to mysql using `mysql -u [rootusername] -p`
    * run `rename user 'root'@'localhost' to 'newAdminUser'@'localhost';`
    * check by running `select user,host,password from mysql.user;`
    * flush privileges for these changes to happen: `FLUSH PRIVILEGES;`
  * Method 2 (preferred):
    * login to mysql using `mysql -u [rootusername] -p`
    * run `use mysql; update user set user='admin' where user='root'; flush privileges;`
    * If you also want to change password, run `update user set password=PASSWORD('new password') where user='admin';` before `flush privileges;`

## phpmyadmin

### Installation

* `sudo apt-get install phpmyadmin apache2-utils`

### [Important config](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-12-04)
  * [Change access url](http://www.thetechrepo.com/main-articles/488) from `phpmyadmin` to something else
    * `vim /etc/phpmyadmin/apache.conf`
    * `Alias /[whatever the name is] /usr/share/phpmyadmin`
    * `sudo service apache2 restart`
  * change to utf8_general

### Possible problem

* [Can't open phpmyadmin](http://stackoverflow.com/questions/10111455/i-cant-access-http-localhost-phpmyadmin)
  * `vim /etc/apache2/apache2.conf`
  * Add the phpmyadmin config to the file: `Include /etc/phpmyadmin/apache.conf`
  * `sudo service apache2 restart`
