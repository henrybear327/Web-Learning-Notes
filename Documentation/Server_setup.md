# Ubuntu Server setup

## Login

### [User accounts setup and securing them](https://www.youtube.com/watch?v=EuIYabZS3ow)

1. Not-root user with superuser privileges
    * login using `ssh root@[server IP]`
    * add new user using `adduser [username]`
      * remove user from user group `sudo gpasswd -d [username] [group]`
      * look up user group `groups [username]`
    * *(optional)* give [username] sudo privileges using `gpasswd -a [username] sudo`
        * Or just use `su root` to switch to `root` and do superuser jobs
    * Give [username] privileges to upload to `/var/www`
    ```
    sudo adduser <username> www-data
    sudo chown -R www-data:www-data /var/www
    sudo chmod -R g+rwX /var/www
    ```
2. Disable remote root login
    * go to `vim /etc/ssh/sshd_config`, and search for `/PermitRootLogin`. Change `PermitRootLogin` to `no` and save it.
    * run `service ssh restart`
    * Logout of `root`
3. Public key authentication
    * Login to `[username]`
    * Generate SSH key on **local machine** using `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`
    * `cat .ssh/id_rsa.pub` or `more .ssh/id_rsa.pub`
    * Run `mkdir .ssh` on [username], and restrict its permission by running `chmod 700 .ssh`.
    * Add local machine's public key to [username]'s `vim .ssh/authorized_keys`
4. Disable password authentication
    * Switch to `root` user
    * run `vim /etc/ssh/sshd_config`
    * search for `PasswordAuth`
    * change `#PasswordAuthentication yes` to `PasswordAuthentication no`
    * run `service ssh restart`

### [Words about Linux permission](http://superuser.com/questions/295591/what-is-the-meaning-of-chmod-666)

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

## Connection protection service

1. Cloudflare
    * Manually setup A to point to the IP address(@), and CNAME www(@)
    * Run throught all security service setup
2. [Let's encrypt](https://letsencrypt.readthedocs.org/en/latest/using.html#installation)
    * If using Cloudflare, please read [this article](https://bepsvpt.wordpress.com)
    * Otherwise, clone [this repo](https://github.com/letsencrypt/letsencrypt) and do
    ```
    git clone https://github.com/letsencrypt/letsencrypt

    cd letsencrypt

    ./letsencrypt-auto
    ```

## [Notes about DNS](https://support.dnsimple.com/articles/differences-between-a-cname-alias-url/)

```
Here’s the main differences:

The A record maps a name to one or more IP addresses, when the IP are known and stable.

The CNAME record maps a name to another name. It should only be used when there are no other records on that name.

The ALIAS record maps a name to another name, but in turns it can coexist with other records on that name.

The URL record redirects the name to the target name using the HTTP 301 status code.


Some important rules to keep in mind:

The A, CNAME, ALIAS records causes a name to resolve to an IP. Vice-versa, the URL record redirects the name to a destination. The URL record is simple and effective way to apply a redirect for a name to another name, for example to redirect www.example.com to example.com. The A name must resolve to an IP, the CNAME and ALIAS record must point to a name.
```

# Apache

## Installation

* `sudo apt-get update && sudo apt-get install apache2`
* Use `ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'` to check server IP address

##  Important config
  * apache configuration directory listing off
    * The best way to do this is disable it with webserver apache2. In my Ubuntu 14.X - open `/etc/apache2/apache2.conf`, change from
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
      * run
        ```
        # vim /etc/httpd/conf/httpd.conf (RHEL/CentOS/Fedora)
        # vim /etc/apache/apache2.conf (Debian/Ubuntu)
        ```
      * Add
        ```
        ServerSignature Off
        ServerTokens ProductOnly
        ```
      * restart apache by `sudo service apache2 restart`

## Possible issues

* [Could not reliably determine the servers fully qualified domain name](http://askubuntu.com/questions/256013/could-not-reliably-determine-the-servers-fully-qualified-domain-name)
  * add `ServerName localhost` to `apache2.conf` in `/etc/apache2`then restart apache by `sudo service apache2 restart`

## Notes on [Local testing](http://askubuntu.com/questions/92069/how-to-add-custom-directory-e-g-phpmyadmin)
  * Simply grant current user the root permission to edit `/var/www/ folder`

# PHP

## Installation

* `sudo apt-get install php5 libapache2-mod-php5 php5-mcrypt`
* `sudo php5enmod mcrypt`
* Go to `sudo vim /etc/apache2/mods-enabled/dir.conf`, and move `index.php` to the front of `index.html`. Then, `sudo service apache2 restart`

## Possible problem

* [Headers and client library minor version mismatch](http://stackoverflow.com/questions/10759334/headers-and-client-library-minor-version-mismatch)
  * install `sudo apt-get install php5-mysqlnd` and restart apache2

## Error log

* `/var/log/apache2/error.log` or `/var/log/httpd/error_log`

# [Composer](https://getcomposer.org/doc/00-intro.md)

You can start with this [good video tutorial](https://www.youtube.com/watch?v=FKUAAZSJiGY).

### Installation

* Php must be installed first.
* Run these commands to globally install composer on Linux/Unix/OSX
```
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```
