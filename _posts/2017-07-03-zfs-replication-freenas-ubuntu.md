---
id: 645
title: ZFS replication from FreeNAS to ubuntu
date: 2017-07-03T18:04:39+00:00
author: jonte
layout: post
guid: https://www.mourtada.se/?p=645
permalink: /zfs-replication-freenas-ubuntu/
categories:
  - ZFS
tags:
  - Backup
  - FreeBSD
  - FreeNAS
  - OpenZFS
  - Ubuntu
  - ZFS
---
Recently i&#8217;ve have set up an old computer to be used as NAS. I&#8217;m using FreeNAS which has some SMB shares and a jail running Nextcloud. The setup is running on a mirror boot device(2x USB drives) with one mirrored volume for data(2x 500GB leftover drives).

FreeNAS is built on top of FreeBSD and ZFS(OpenZFS). When looking at options for backing up the data on FreeNAS i started digging into ZFS with it&#8217;s snapshot and replication capabilities. In FreeBSD ZFS comes builtin but with linux because of licens issues you have to install ZFS manually.

Snapshots is a really nice feature of ZFS which is can be seen as point-in-time copy of a dataset. A snapshots diskpace is only the changes in the dataset that has happened which makes it disk space effective. Snapshots can easily be rollbacked, replicated to another machine or mounted at another path.

Of course things got out of hand i&#8217;ve struggled for some hours with the ZFS replication to a ubuntu host that i&#8217;ve prepared. It turns out that FreeBSD uses a new version of ZFS that makes the receiving side hang with 100% CPU usage as described here [#5999](https://github.com/zfsonlinux/zfs/issues/5999). It&#8217;s funny because the OpenZFS initiative did a change to use feature flags instead of version numbers to make compatibility less of a problem. Anyhow in 0.7 of ZFS on linux this problem seems to have been fixed. So i tried to compile the sources but got lost somewhere after all those steps required. Then i found this PPA https://launchpad.net/~zfs-native/+archive/ubuntu/daily which only has package for trusty(14.04) so installed trusty and used the precompiled packages.

## Installing ZFS on linux(0.7 RC on ubuntu trusty 14.04)

<pre>$ sudo add-apt-repository ppa:zfs-native/daily
$ sudo apt-get update
$ sudo apt-get install zfsutils-linux
# Then reboot
</pre>

Create a simple lab ZFS pool with a file vdev(4GB)

<pre>$ dd if=/dev/zero of=example.img bs=1M count=4096
$ sudo zpool create pool-test /home/user/example.img
$ sudo zpool status
</pre>

## Replicate the snapshot

Take a snapshot
  
`zfs snapshot mypool/dataset@snapshotname`

To list all snapshots
  
`zfs list -t snapshot`

`zfs send` can be used to send a snapshot to standard output. Send to file with verbose logging.
  
`zfs send -v mypool/dataset@snapshotname > snapshotfile`

Send snapshot to another host with ssh(to overwrite existing datataset use -F on the receive command)
  
`zfs send -v mypool/dataset@snapshotname | ssh myhost zfs receive -v myotherpool/newdataset`

For the above to work ssh keys needs to be set up between hosts.

Login as root and Set up SSH keys on your FreeNAS host.
  
`ssh-keygen -q -t rsa`

Copy the contents of `/root/.ssh/id_rsa.pub`

On your ubuntu host

<pre># if not already exists
$ mkdir /root/.ssh
# if not already exists
$ touch /root/authorized_keys
# paste the contents of your FreeNAS root id_rsa.pub here
# if not already exists
$ chmod -R 600 /root/.ssh
</pre>

Then do a manual login from the FreeNAS host to add the ubuntu host to known_hosts.

## Conclusion

As of know i probably will use FreeBSD as the receiving side instead or save the snapshots to file and upload to cloud storage.