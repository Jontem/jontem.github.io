---
title: Configure a wireless connection on raspbian
date: 2022-07-21T00:00:00+00:00
layout: post
permalink: /configure-a-wireless connection-on-raspbian/
categories:
  - linux
tags:
  - linux
  - raspberry
  - raspbian
  - wpa_supplicant
  - wpa_cli
  - wifi
---

In this post we'll go through how to configure a wireless connection on raspbian.

## Configure wpa_supplicant

First we need to configure the `wpa_supplicant`.

```
# /etc/wpa_supplicant/wpa_supplicant.conf

ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=SE

network={
     ssid="my ssid"
     psk="mypassword"
     scan_ssid=1
}
```

More documentation on how the `wpa_supplicant` config works can be found in `/usr/share/doc/wpa_supplicant/examples/wpa_supplicant.conf`.

For configuration changes to take effect directly either run `killall -HUP wpa_supplicant` or `wpa_cli reconfigure`

## wpa_cli

To interact with the `wpa_supplicant` you can use the `wpa_cli` interactive command line tool.

- To check status type `status`.
- To scan for networks type `scan`. The command will immediately return `OK` and after a couple of seconds print `<3>CTRL-EVENT-SCAN-RESULTS`.
- To view scan results type `scan_results`
- To see available commands type `help`

## Trouble with legacy hardware
When using an older legacy wifi adapter you may get an error from the `wpa_supplicant` saying
```
nl80211: Driver does not support authentication/association or connect commands
```
You can then try with an older legacy driver `wext`. To test run 

```
# wpa_supplicant -B -i wlan0 -D wext -c /etc/wpa_supplicant/wpa_supplicant.conf
```

If this works you should update `/etc/dhcpcd.conf` and add this
```
interface wlan0
env ifwireless=1
env wpa_supplicant_driver=wext
```
This will make `dhcpcd` which loads the `wpa_supplicant` via a hook at boot to load the `wpa_supplicant` with the legacy driver.
