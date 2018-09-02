# Ubuntu Server setup notes

* Tested OS: `Ubuntu 16.04 x64`
* Special thanks to [BePsvPT](https://github.com/BePsvPT) for scrutinizing my notes and making suggestions.

# FYI

This is all the work that I will do for a new Ubuntu server setup.

If you find anything wrong or strange, feel free to send me pull requests or submit issues. I will try my best to merge and discuss with you.

# Let's start!

## [User accounts setup and securing them](https://www.youtube.com/watch?v=EuIYabZS3ow)

0. Login for the first time, using `ssh root@[Your server IP]`
1. Setup a not-root user (with superuser privileges)
    * add new user using `adduser [username]`
        * use `su root` to switch norma user to `root` and do superuser jobs
    * *(optional)* give [username] sudo privileges using `gpasswd -a [username] sudo`
        * remove user from user group `sudo gpasswd -d [username] [group]`
        * look up user group `groups [username]`
2. Disable remote root login
    * go to `vim /etc/ssh/sshd_config`, and search for `PermitRootLogin`. Change `PermitRootLogin` to `no` and save it.
    * run `service ssh restart`
    * Setup up a password for root by running `passwd`
    * Logout of `root`
3. Public key authentication
    * Login to `[username]@[Your server IP]`
    * Generate SSH key on **local machine** using `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`, and `cat ~/.ssh/id_rsa.pub` or `more ~/.ssh/id_rsa.pub`
    * Run `mkdir .ssh` on **your server**, and restrict its permission by running `chmod 700 .ssh`.
    * Add local machine's public key to **your server**'s `vim .ssh/authorized_keys`
4. Disable password authentication
    * Switch to `root` user by `su root`
    * run `vim /etc/ssh/sshd_config`
    * search for `PasswordAuth`
    * change `#PasswordAuthentication yes` to `PasswordAuthentication no`
    * run `service ssh restart`
    * Logout of `[username]`

** NOTICE: ** From now on, you should be executing commands using the account that has root privileges.

## System update

* `apt update; apt upgrade;`
  * If the kernel is updated, run `poweroff --reboot`

## Disable annoying locale warning

### [Solution 1](http://askubuntu.com/questions/599808/cannot-set-lc-ctype-to-default-locale-no-such-file-or-directory)

* `sudo locale-gen "en_US.UTF-8"`
* `sudo dpkg-reconfigure locales`
* `vim /etc/default/locale`
* add
```
LC_CTYPE="en_US.UTF-8"
LC_ALL="en_US.UTF-8"
LANG="en_US.UTF-8"
```
* Logout and login again

### Solution 2

* `apt-cache search "^language-pack-[a-z][a-z]$"`, and `sudo apt install` the language you want
* [https://stackoverflow.com/questions/36394101/pip-install-locale-error-unsupported-locale-setting?lq=1]
   * `export LC_ALL=C`

## [ZSH](https://github.com/robbyrussell/oh-my-zsh/wiki/Installing-ZSH)

* `sudo apt-get install zsh`
* `sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`
* `sudo chsh -s /bin/zsh [username]`

## Apache

### [Installation](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04)

* `apt update && apt install apache2`
    * Use `ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'` to check server IP address
* Give [username] privileges to upload to `/var/www` by running
```
sudo adduser [username] www-data
sudo chown -R www-data:www-data /var/www
sudo chmod -R g+rwX /var/www
```

###  Important configuration
  * apache configuration directory listing off
    * The best way to do this is disable it with webserver apache2. Run `vim /etc/apache2/apache2.conf`, change from
    ```
    <Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
    ```
    to
    ```
    <Directory /var/www/>
        Options FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
    ```
    then restart apache by `sudo service apache2 restart`
    * [Hide Apache Version and OS Identity from Errors](http://www.tecmint.com/apache-security-tips/)
      * run `vim /etc/apache2/apache2.conf (Debian/Ubuntu)`
        * Sidenotes: `vim /etc/httpd/conf/httpd.conf (RHEL/CentOS/Fedora)`
      * Add

        ```
        ServerSignature Off
        ServerTokens ProductOnly
        ```
      * restart apache by `sudo service apache2 restart`
    * [Disable the status info](https://ubuntuforums.org/showthread.php?t=1435840)
      * run `sudo a2dismod status` to disable mod_status on Apache.
        * `sudo a2enmod status` will re-enable it
        *
        ```
        If you look inside the /etc/apache2/mods-available/status.conf file that you posted the contents of, all it's doing is telling Apache to return server status information ("SetHandler server-status") when someone visits the URL "http://localhost/server-status" ("<Location /server-status>"). It is also only allowing this when accessed from localhost ("Allow from localhost ip6-localhost"). The line for "<IfModule mod_status.c>" is checking whether mod_status is installed on the system. If it's not installed then there's no point in having Apache try to return non-existent server status information.
        ```


### Possible issues

* [Could not reliably determine the servers fully qualified domain name](http://askubuntu.com/questions/256013/could-not-reliably-determine-the-servers-fully-qualified-domain-name)
  * add `ServerName localhost` to `apache2.conf` in `/etc/apache2`then restart apache by `sudo service apache2 restart`

### Notes on [Local testing](http://askubuntu.com/questions/92069/how-to-add-custom-directory-e-g-phpmyadmin)
  * Simply grant current user the root permission to edit `/var/www/` folder

## MariaDB

### [Installation](https://downloads.mariadb.org/mariadb/repositories/#mirror=ossplanet&distro=Ubuntu&distro_release=xenial--ubuntu_xenial&version=10.1) (version 10.1)

* Before installing...
    ```
    sudo apt-get install software-properties-common
    sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
    sudo add-apt-repository 'deb [arch=amd64,i386,ppc64el] http://ftp.ubuntu-tw.org/mirror/mariadb/repo/10.1/ubuntu xenial main'
    ```
* `sudo apt update; sudo apt install mariadb-server`
* `mysql_secure_installation`
* Use `sudo service mysql status` to check database status

### [Secure MariaDB](https://mariadb.com/kb/en/mariadb/mysql_secure_installation/)

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


### Sidenotes

* `sudo service mysql start|stop`
* [Final fix for phpMyAdmin to work](http://superuser.com/questions/957708/mysql-mariadb-error-1698-28000-access-denied-for-user-rootlocalhost)

```
sudo mysql -u root
use mysql;
update user set plugin='' where User='root';
flush privileges;
exit;
```

### [Removing MySQL](https://www.unixmen.com/how-to-install-lamp-stack-on-ubuntu-16-04/)

* `sudo systemctl stop mysql`
* `sudo apt remove --purge mysql-server mysql-client mysql-common`
* `sudo apt autoremove; sudo apt autoclean`
* `sudo rm -rf /var/lib/mysql/; sudo rm -rf /etc/mysql/`

## PHP7

### PHP Installation

* `sudo apt install php libapache2-mod-php php-mysql`
  * If you need to use `curl` in PHP, run `sudo apt-get install php-curl`
* `sudo vim /etc/apache2/mods-enabled/dir.conf` and move `index.php` to the first of the list, which should look like this
    ```
    <IfModule mod_dir.c>
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
    </IfModule>
    ```
* `sudo systemctl restart apache2`
    * Check status using `sudo systemctl status apache2`
* Create a test page
    * `sudo vim /var/www/html/info.php`
    * Add
    ```
    <?php
    phpinfo();
    ```
    Now go to `[Server IP]/info.php`, you should see your page
    * Run `sudo rm /var/www/html/info.php` after testing
* Check Apache status by `sudo systemctl status apache2`
* `vim /etc/php/7.0/apache2/php.ini`
    * `expose_php=Off`

### Error log

* `vim /var/log/apache2/error.log`

## phpmyadmin

### [Installation for php7](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-16-04?utm_source=legacy_reroute)

* `sudo apt install phpmyadmin php-mbstring php-gettext`
    * Warning: When the first prompt appears, apache2 is highlighted, but not selected. **If you do not hit Space to select Apache, the installer will not move the necessary files during installation.** Hit Space, Tab, and then Enter to select Apache.
* `sudo phpenmod mbstring;`

### [Important config](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-16-04)
  * [Change access url](http://www.thetechrepo.com/main-articles/488) from `phpmyadmin` to something else
    * `vim /etc/phpmyadmin/apache.conf`
    * `Alias /[whatever the name is] /usr/share/phpmyadmin`
    * `sudo service apache2 restart`
  * Add access password
    * `sudo vim /etc/apache2/conf-available/phpmyadmin.conf`
    * Add `AllowOverride All` directive within the `<Directory /usr/share/phpmyadmin>` section of the configuration file,
    ```
    <Directory /usr/share/phpmyadmin>
    Options FollowSymLinks
    DirectoryIndex index.php
    AllowOverride All
    ...
    ```
    * `sudo systemctl restart apache2`
    * `sudo vim /usr/share/phpmyadmin/.htaccess`
    ```
    AuthType Basic
    AuthName "Restricted Files"
    AuthUserFile /etc/phpmyadmin/.htpasswd
    Require valid-user
    ```
    Meanings are listed as followed:
    ```
    AuthType Basic: This line specifies the authentication type that we are implementing. This type will implement password authentication using a password file.
    AuthName: This sets the message for the authentication dialog box. You should keep this generic so that unauthorized users won't gain any information about what is being protected.
    AuthUserFile: This sets the location of the password file that will be used for authentication. This should be outside of the directories that are being served. We will create this file shortly.
    Require valid-user: This specifies that only authenticated users should be given access to this resource. This is what actually stops unauthorized users from entering.
    ```
    * `sudo apt-get install apache2-utils`
    * Create username and password `sudo htpasswd -c /etc/phpmyadmin/.htpasswd [username]`
  * change server connection collation to utf8_general_ci

### Possible problem

* [Can't open phpmyadmin](http://stackoverflow.com/questions/10111455/i-cant-access-http-localhost-phpmyadmin)
  * `vim /etc/apache2/apache2.conf`
  * Add the phpmyadmin config to the file: `Include /etc/phpmyadmin/apache.conf`
  * `sudo service apache2 restart`

### Some Notes

* `/etc/mysql/debian.cnf`

## [Composer](https://getcomposer.org/doc/00-intro.md)

You can start with this [good video tutorial](https://www.youtube.com/watch?v=FKUAAZSJiGY).

### Installation

* Php must be installed first.
* Run these commands to globally install composer on Linux/Unix/OSX
```
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```

## Use connection protection service

1. Cloudflare
    * Manually setup A to point to the IP address(@), and CNAME www(@)
    * Run through all security service setup
2. [Let's encrypt](https://letsencrypt.readthedocs.io/en/latest/using.html#installation)
    * If you are using Cloudflare, please read [this article](https://bepsvpt.wordpress.com/2015/12/25/introduction-to-lets-encrypt/)
    * Otherwise, do
    ```
    sudo apt install letsencrypt python-letsencrypt-apache;
    letsencrypt --apache -d [Your domain name];
    ```

# [SWAP](https://www.digitalocean.com/community/tutorials/additional-recommended-steps-for-new-ubuntu-14-04-servers)

## One-liner for 2GB swap file

`sudo fallocate -l 2G /swapfile && sudo chmod 600 /swapfile && sudo mkswap /swapfile && sudo swapon /swapfile && sudo sh -c 'echo "/swapfile none swap sw 0 0" >> /etc/fstab'`

## Create a Swap File Explained

Adding "swap" to a Linux server allows the system to move the less frequently accessed information of a running program from RAM to a location on disk. Accessing data stored on disk is much slower than accessing RAM, but having swap available can often be the difference between your application staying alive and crashing. This is especially useful if you plan to host any databases on your system.

Advice about the best size for a swap space varies significantly depending on the source consulted. Generally, an amount equal to or double the amount of RAM on your system is a good starting point.

Allocate the space you want to use for your swap file using the `fallocate` utility. For example, if we need a 4 Gigabyte file, we can create a swap file located at `/swapfile` by typing: `sudo fallocate -l 1G /swapfile`.

After creating the file, we need to restrict access to the file so that other users or processes cannot see what is written there: `sudo chmod 600 /swapfile`.

We now have a file with the correct permissions. To tell our system to format the file for swap, we can type: `sudo mkswap /swapfile`

Now, tell the system it can use the swap file by typing: `sudo swapon /swapfile`

Our system is using the swap file for this session, but we need to modify a system file so that our server will do this automatically at boot. You can do this by typing: `sudo sh -c 'echo "/swapfile none swap sw 0 0" >> /etc/fstab'`. With this addition, your system should use your swap file automatically at each boot.

# [ufw](https://www.digitalocean.com/community/tutorials/how-to-setup-a-firewall-with-ufw-on-an-ubuntu-and-debian-cloud-server)

## Check settings

* `sudo ufw app list` or `sudo ufw app info "Apache Full"` to look at look at the Apache Full profile

## Installation

* `sudo apt install ufw`
* `sudo ufw status`
* If your VPS is configured for IPv6, ensure that UFW is configured to support IPv6 so that will configure both your IPv4 and IPv6 firewall rules. To do this, open the UFW configuration with this command: `sudo vi /etc/default/ufw`. Then make sure "IPV6" is set to "yes", like so:`IPV6=yes` Save and quit. Then restart your firewall with the following commands: `sudo ufw disable`
`sudo ufw enable`

## Setup

```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw allow www
sudo ufw allow ntp
sudo ufw allow sftp
sudo ufw allow 8834/tcp
sudo ufw allow 443/tcp
```

Notes for each setting:
* `sudo ufw allow ssh` (22/tcp)
* `sudo ufw allow www` (80/tcp)
* `sudo ufw allow ntp` (123/tcp)
* `sudo ufw allow sftp`
* `sudo ufw allow 8834/tcp` (nessus)
* if needed
  * `sudo ufw allow 443/tcp` (SSL/TLS)

# ntp

* `sudo dpkg-reconfigure tzdata`
  *  select time zone
* `sudo apt install ntp; sudo service ntp start`
* Use `ntpq -p` to check the status

# htop

* `sudo apt-get install htop`

# Nessus

* Go to `https://www.tenable.com/products/nessus/select-your-operating-system#tos` and download the version you need
  * Use `filezilla` to upload the file(`apt install filezilla`)
  * Use interactive mode
* Obtain a key for Nessus [here](https://www.tenable.com/products/nessus/nessus-plugins/obtain-an-activation-code)
* Install Nessus by running `dpkg -i [Filename]`
* Start Nessus by running `/etc/init.d/nessusd start`, and visit `[server ip]:8834`

# Final step

* Run `sudo apt autoremove; sudo apt autoclean`
* Run `sudo update-grub` if necessary
* Take a snapshot of the current system state

# Notes

## [Linux file system permission](http://superuser.com/questions/295591/what-is-the-meaning-of-chmod-666)

```
permission to:  owner      group      other     
                /¯¯¯\      /¯¯¯\      /¯¯¯\
octal:            6          6          6
binary:         1 1 0      1 1 0      1 1 0
what to permit: r w x      r w x      r w x

binary         - 1: enabled, 0: disabled

what to permit - r: read, w: write, x: execute

permission to  - owner: the user that create the file/folder
                 group: the users from group that owner is member
                 other: all other users
```

## [DNS](https://support.dnsimple.com/articles/differences-between-a-cname-alias-url/)

```
Here’s the main differences:

The A record maps a name to one or more IP addresses, when the IP are known and stable.

The CNAME record maps a name to another name. It should only be used when there are no other records on that name.

The ALIAS record maps a name to another name, but in turns it can coexist with other records on that name.

The URL record redirects the name to the target name using the HTTP 301 status code.


Some important rules to keep in mind:

The A, CNAME, ALIAS records causes a name to resolve to an IP. Vice-versa, the URL record redirects the name to a destination. The URL record is simple and effective way to apply a redirect for a name to another name, for example to redirect www.example.com to example.com. The A name must resolve to an IP, the CNAME and ALIAS record must point to a name.
```
