# [SWAP](https://www.digitalocean.com/community/tutorials/additional-recommended-steps-for-new-ubuntu-14-04-servers)

## One-liner

`sudo fallocate -l 1G /swapfile && sudo chmod 600 /swapfile && sudo mkswap /swapfile && sudo swapon /swapfile && sudo sh -c 'echo "/swapfile none swap sw 0 0" >> /etc/fstab'`

## Create a Swap File Explained

Adding "swap" to a Linux server allows the system to move the less frequently accessed information of a running program from RAM to a location on disk. Accessing data stored on disk is much slower than accessing RAM, but having swap available can often be the difference between your application staying alive and crashing. This is especially useful if you plan to host any databases on your system.

Advice about the best size for a swap space varies significantly depending on the source consulted. Generally, an amount equal to or double the amount of RAM on your system is a good starting point.

Allocate the space you want to use for your swap file using the `fallocate` utility. For example, if we need a 4 Gigabyte file, we can create a swap file located at `/swapfile` by typing: `sudo fallocate -l 1G /swapfile`.

After creating the file, we need to restrict access to the file so that other users or processes cannot see what is written there: `sudo chmod 600 /swapfile`.

We now have a file with the correct permissions. To tell our system to format the file for swap, we can type: `sudo mkswap /swapfile`

Now, tell the system it can use the swap file by typing: `sudo swapon /swapfile`

Our system is using the swap file for this session, but we need to modify a system file so that our server will do this automatically at boot. You can do this by typing: `sudo sh -c 'echo "/swapfile none swap sw 0 0" >> /etc/fstab'`. With this addition, your system should use your swap file automatically at each boot.
