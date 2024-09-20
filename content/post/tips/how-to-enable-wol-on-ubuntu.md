+++
title = 'How to Enable Wol on Ubuntu'
date = 2024-09-20T19:23:28+08:00
draft = false

+++

# Prepared in BIOS

Firstly, you need to enable WOL in your bios settings, commonly called _Wake by Lan_ or _Wake by PCIE device_. 

Then, save and restart the computer and log in to the Ubuntu OS.

# Configure your network

You need the sudo permissions to edit some of the configuration files.

Use your commonly used editor(like vim or nano) to edit the `/etc/netplan/02-wol.yaml`, or you may need to create it.

```yaml
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    <interface-name>:
      match:
        macaddress: "xx:xx:xx:xx:xx:xx"
      wakeonlan: true
```

You need to change the `interface-name` to your name of an interface, and change the `xx:xx:xx:xx:xx:xx` to your MAC address, which can be found after using  `ifconfig`.

![image-20240920204331514](https://raw.githubusercontent.com/bryanjia222/img/main/image-20240920204331514.png)

The `enp6s0` on the top left corner is my interface name, and the `d8:43:ae:ca:xx:xx` is my MAC address. Replace them with yours.

Then reboot your computer and use the command `sudo ethtool enp6s0 | grep Wake` to check the configuration above.

If you get a message like this:

```shell
	Supports Wake-on: pumbg
	Wake-on: g
```

the configuration is successful.

> The Wake-on: g means you can use a magic frame to wake your computer up. If the flag is d, the computer won't be woken up by Lan.
