---
title: Nextcloud with freenas iocage
date: 2018-07-11T00:00:00+00:00
layout: post
permalink: /nextcloud-freenas-iocage/
crosspost_to_medium: true
categories:
  - freenas
tags:
  - freenas
  - iocage
  - nextcloud
---

I was running nextcloud 10 which was deployed as a freenas plugin. Now I wanted to upgrade nextcloud to the latest version but i couldn't even get to version 11 because my php version was unsupported. I'll write here the steps that got me to version 13 of nextcloud. We'll set up and empty jail and do a manual installation of nextcloud.

## Iocage
Iocage is the new jail manager backend which will replace the old warden backend in freenas. To use iocage you'll need to use the new freenas gui or use the command-line. I started with the gui but soon realized that all features is not yet supported from the gui so to proceed we'll use the command-line.

Useful iocage commands:
```
# List jails setup with iocage
iocage list

# Start a jail
iocage start <jail_name>

# Start a console inside jail
iocage console <jail_name>
```


## The plan
Since you can't upgrade nextcloud between multiple major versions we'll have to go from 10 -> 11, 11 -> 12, 12 -> 13 one step at a time.

## Steps
First ssh into to your freenas box so we can create our new jail and start it.

```
iocage create --name nextcloud --release 11.1-RELEASE vnet=on dhcp=on bpf=yes
iocage start nextcloud
```

This will create a new jail named `nextcloud` with a network interface with dhcp.

Now we need to console in to our jail. Then we're going to install the nextcloud package which will give us the necessary php packages. I tried going with php7 but nextcloud 11 didn't support it.

```
pkg update
pkg install nextcloud-php56-13.0.4
```

Now we will install apache 2.4 and mysql server
```
pkg install apache24
pkg install mysql56-server-5.6.40
```

Now we need to update `/etc/rc.conf` so we can start apache and mysql. Add this to the end.

```
apache24_enable="yes"
mysql_enable="YES"
```

Next we're going to remove the nextcloud 13 version that pkg installed for us.

```
rm -rf /usr/local/www/nextcloud/
```

Now download version 11. Old versions can be found here https://nextcloud.com/changelog/. Unpack it and rename the folder.

```
curl https://download.nextcloud.com/server/releases/nextcloud-11.0.8.tar.bz2 -o nextcloud-11.0.8.tar.bz2
tar jxf nextcloud-11.0.8.tar.bz2
mv nextcloud nextcloud11
```

Lets make some apache configurations. First we'll enable php

```
# Install php module
pkg install mod_php56-5.6.36_1

# Add configuration
vi /usr/local/etc/apache24/Includes/php.conf
```

Add this
```
<FilesMatch "\.php$">
    SetHandler application/x-httpd-php
</FilesMatch>
<FilesMatch "\.phps$">
    SetHandler application/x-httpd-php-source
</FilesMatch>
```

Now we'll add configuration for nextcloud
```
vi /usr/local/etc/apache24/Includes/php.conf
```

Add this. Notice that we point to our nextcloud 11 installation

```
Alias /nextcloud /usr/local/www/nextcloud11
AcceptPathInfo On
<Directory /usr/local/www/nextcloud11>
    AllowOverride All
    Require all granted
</Directory>
```

It's a good idea to create a own dataset for your `config` and `data` directory. We will add them to our jail and restart

```
iocage fstab -a nextcloud "/path/to/dataset/with/nextcloud/config  /nextcloud_config  nullfs  rw  0  0"
iocage fstab -a nextcloud "/path/to/dataset/with/nextcloud/data  /nextcloud_data  nullfs  rw  0  0"
iocage restart nextcloud
```

Now copy your config and data folders contents to your newly created datasets. Then we'll symlink the directories in our nextcloud 11 installation directory

```
# On your freenas host sync old config and data directories
rsync -avx /path/to/old/nextcloud/config/ /path/to/new/nextcloud/config/
rsync -avx /path/to/old/nextcloud/data/ /path/to/new/nextcloud/data/

# In your jail remove initial config and data directories and symlink our config and data directories.
cd nextcloud11
rm -rf config
rm -rf data
ln -s /nextcloud_config config
ln -s /nextcloud_data data
```

Lets get a backup from our old nextcloud installation

```
# On old installation, then copy to folder accessible from new jail
mysqldump --single-transaction nextcloud > nextcloud-sqlbkp_`date +"%Y%m%d"`.bak

# On new jail
mysql -e "CREATE DATABASE nextcloud"

# Change to the user and password in your config. Probably ncuser and ncpassword
mysql -e "CREATE USER 'ncuser'@'localhost' IDENTIFIED BY 'ncpass';"
mysql -e "GRANT ALL PRIVILEGES ON nextcloud.* TO 'ncuser'@'localhost'"
mysql -e "FLUSH PRIVILEGES;"

# Then import the database
mysql nextcloud < nextcloud-sqlbkp.bak
```

## Finally
Now we should be able to access our nextcloud installation([http://yourip/nextcloud](http://yourip/nextcloud)) and upgrade to version 11. When this is done we can download version 12. Don't forget to update your apache configuration to the new version and add the symlinks. Then do the same for version 13.