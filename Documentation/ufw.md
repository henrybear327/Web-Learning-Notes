# [ufw](https://www.digitalocean.com/community/tutorials/how-to-setup-a-firewall-with-ufw-on-an-ubuntu-and-debian-cloud-server)

# Installation

* `sudo apt-get install ufw`
* `sudo ufw status`
* If your VPS is configured for IPv6, ensure that UFW is configured to support IPv6 so that will configure both your IPv4 and IPv6 firewall rules. To do this, open the UFW configuration with this command: `sudo vi /etc/default/ufw`. Then make sure "IPV6" is set to "yes", like so:`IPV6=yes` Save and quit. Then restart your firewall with the following commands: `sudo ufw disable`
`sudo ufw enable`

# Setup

* `sudo ufw default deny incoming`
* `sudo ufw default allow outgoing`
* `sudo ufw allow ssh` (22/tcp)
* `sudo ufw allow www` (80/tcp)
* `sudo ufw allow ntp` (123/tcp)
* `sudo ufw allow sftp`
* `sudo ufw allow 8834/tcp` (nessus)
* if needed
  * `sudo ufw allow 443/tcp` (SSL/TLS)
