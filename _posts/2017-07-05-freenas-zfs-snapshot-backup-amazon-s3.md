---
id: 655
title: FreeNAS ZFS snapshot backup to Amazon S3
date: 2017-07-05T16:31:41+00:00
author: jonte
layout: post
guid: https://www.mourtada.se/?p=655
permalink: /freenas-zfs-snapshot-backup-amazon-s3/
categories:
  - ZFS
tags:
  - Amazon s3
  - Backup
  - FreeNAS
  - ZFS
---
I&#8217;ve been looking for a way to backup my FreeNAS ZFS snapshots to an offsite location. I didn&#8217;t find much information how to do this so i had to come up with my own solution.

In this post I&#8217;m going to show you how to save your encrypted ZFS snapshots in Amazon S3. We&#8217;re going to use a FreeBSD jail together with [GnuPG](https://www.gnupg.org/) and [s3cmd](https://github.com/s3tools/s3cmd).

## Adding a jail in FreeNAS

Go to the FreeNAS web ui and click Jails. Click add and choose name. If you click advanced here you can change the ip-adress for the jail(I wanted to use DHCP).

&nbsp;<figure id="attachment_657" style="width: 508px" class="wp-caption alignnone">

<img class="size-full wp-image-657" src="https://www.mourtada.se/wp-content/uploads/2017/07/Screen-Shot-2017-07-05-at-15.40.09.png" alt="Adding an empty jail in Freenas" width="508" height="887" srcset="https://www.mourtada.se/wp-content/uploads/2017/07/Screen-Shot-2017-07-05-at-15.40.09.png 508w, https://www.mourtada.se/wp-content/uploads/2017/07/Screen-Shot-2017-07-05-at-15.40.09-172x300.png 172w" sizes="(max-width: 508px) 100vw, 508px" /><figcaption class="wp-caption-text">Adding an empty jail in FreeNAS</figcaption></figure> 

Click ok and FreeNAS will setup a new jail for you which takes a minute or two.

From now on we will have to work in the FreeNAS shell(SSH must be enabled under services in the Web UI).

To list all the jails running on your FreeNAS host we can run:

<pre>$ jls
</pre>

Verify that the jail you created is listed.

To enter the jail run:

<pre>$ jexec your_jail_name
$ # Verify that you're in the jail
$ hostname
backup
</pre>

We&#8217;re going to need to install some packages. First we need GnuPG which we&#8217;ll use to encrypt our snapshots. Then we will need the s3cmd which is used for uploading our snapshots to Amazon s3.

<pre>$ pkg install security/gnupg
$ pkg install net/py-s3cmd</pre>

I&#8217;m going to use symmetric AES256 encryption with a passphrase file because i don&#8217;t want to store my data in the cloud unencrypted. So generate a random passphrase which you will need to store at multiple locations(not just inside the jail). Because if the passphrase is lost your backups will be worthless. The passphrase file needs to be accessible by the backupscript. I have placed my passphrase file in in the root directory.

<pre>$ echo "mypassphrase" &gt; /root/snapshot-gpg-passphrase
$ chmod 400 /root/snapshot-gpg-passphrase
</pre>

Next we&#8217;ll create a folder holding our current list of snapshots that we&#8217;re going to keep synced with S3.

<pre>$ mkdir /root/s3_sync_bucket
$ chmod 600 /root/s3_sync_bucket
</pre>

We also need to configure s3cmd so run this and answer all questions:

<pre>$ s3cmd --configure
</pre>

## The backup script

This script should be run on the FreeNAS host. What is does:

  1. Creates a snapshot of the specified dataset
  2. Sends it to the backup jail where it&#8217;s encrypted and saved to file
  3. Removes the snapshot on the FreeNAS host
  4. Removes all snapshots older than 7 days
  5. Syncs the local s3 bucket directory with S3 using s3cmd



Create the script and run it manually or with crontab

<pre>$ touch /root/backup_script.sh
$ chmod 700 /root/backup_script.sh
$ /root/backup_script.sh my-pool/my-dataset
</pre>

Edit the script to fit your needs.

## Decrypting a backup

To decrypt a backup:

<pre>$ gpg --batch --decrypt --passphrase-file /root/pass-gpg &lt; backup_file</pre>