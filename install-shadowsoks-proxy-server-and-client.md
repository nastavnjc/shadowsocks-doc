---
author:
  name: Linode Community
  email: docs@linode.com
description: 'ShadowSocks is a secure socks5 proxy, designed to protect your Internet traffic. It encrypts the traffic between you and the servers, so nobody can spy on you. The main difference compare to VPN is that Shadowsocks is not global, which means not all your traffic will go through the servers.'
keywords: 'shadowsocks, Ubuntu, proxy'
license: '[CC BY-ND 4.0](https://creativecommons.org/licenses/by-nd/4.0)'
published: 'Wednesday, August 2th, 2017'
modified: 'Wednesday, August 2th, 2017'
modified_by:
  name: Linode
title: 'Installing and configuring a ShawdowSocks Proxy Server and Client on Ubuntu 16.04.2 LTS (Xenial Xerus)'
contributor:
  name: Roberto Rossi
external_resources:
 - '[ShadowSocks](https://shadowsocks.org/)'
---

*This is a Linode Community guide. If you're an expert on something we need a guide on, you too can [get paid to write for us](https://www.linode.com/docs/contribute).*
<hr>

[ShadowSocks](https://shadowsocks.org/) is a secure socks5 proxy, designed to protect your Internet traffic. It encrypts the traffic between you and the servers, so nobody can spy on you. The main difference compare to VPN is that Shadowsocks is not global, which means not all your traffic will go through the server. If you want to use an Instant Messenger or a uTorrent, you will have to configure those programs settings to use the applicable Socks 5 proxy and port.

![Install ShadowSocks on Ubuntu 16.04](https://github.com/nastavnjc/shadowsocks-doc/blob/master/install-shadowsock-on-ubuntu-16-04.png "Install ShadowSocks on Ubuntu 16.04")

Let’s say you find yourself in a situation where OpenVPN traffic is blocked or throttled, ShadowSocks is a good alternative to tunnel the entire network traffic.

{: .note}
>
> This guide is written for a non-root user. Commands that require elevated privileges are prefixed with `sudo`. If you’re not familiar with the `sudo` command, see the [Users and Groups](/docs/tools-reference/linux-users-and-groups) guide.

## Before You Begin

1.  Complete the [Getting Started](/docs/getting-started) guide.

2.  Follow the [Securing Your Server](/docs/security/securing-your-server/) guide to create a standard user account, harden SSH access and remove unnecessary network services; this guide will use `sudo` wherever possible. Do **not** follow the *Configuring a Firewall* section--this guide has instructions specifically for a ShadowSocks production server.

3.  Log in to your Linode via SSH and check for updates using `apt-get` package manager.

        sudo apt-get update && sudo apt-get upgrade

## Open Corresponding Firewall Ports

In this case we're using ShadowSocks's default port 8000, but this could be any port you specify later in the configuration file.

    sudo ufw allow ssh
    sudo ufw allow 8000/tcp
    sudo ufw enable

## Installing Pip on Ubuntu 16.04

There are multiple ways to install it on your Linux system, but the most simple way is to install it using 'pip' command. Pip is an easy to install package management system which is used to install and manage software packages found in the Python Package Index and it give us the convenient to way to install Shadowsocks. Before you can use, make sure that its installed on your system, if not then use below command to install it on your Ubuntu server.

    sudo apt-get install python-pip
    sudo apt-get install python-m2crypto
    
This will installs the Python PIP and Python-m2crypt packages along with its other dependencies, the m2crypt package makes the encryption more faster. Once you asked for confirmation, press **Y** key to continue the installing the required package along with other dependencies.

## Installing Shadowsocks on Ubuntu 16.04

Once the dependent packages are installed, issue the following ‘pip’ command in your command line terminal to install shadowsocks.

    sudo pip install shadowsocks

This will installs the latest available package.

## Configuring Shadowsocks on Ubuntu 16.04

Before we start shahdowsocks on our server, let’s create a new file and put the following configuration contents in it that contains your hostname or server IP (IPv4/IPv6) , server port number, local port number, a password used to encrypt transfer, connection timeout and and encryption method like “aes-256-cfb”, “bf-cfb”, “des-cfb” or “rc4”, etc. the default encryption method used is table, which is not secure so we will be using 'aes-256-cfb' which is recommended.

So, run the below command to open a new file using your command line editor and put the following configuration parameters in it.

    sudo vim /etc/shadowsocks.json
    
{: .file }
/etc/shadowsocks.json
:   ~~~ conf
{
    "server":"your_server_ip",
    "server_port":8000,
    "local_port":1080,
    "password":"p4ssw0rD",
    "timeout":600,
    "method":"aes-256-cfb"
}
    ~~~

{: .note}
>
> Be sure to replace **your_server_ip** with the ip address of your Ubuntu Server. Usually, ShadowSocks listen on port  **8000**  but you can change with your own port. If so, remenber to add a firewall rule accordingly.
**local_port** is referring to a listening port on your device (PC, Phone, etc.), youcan leave it as it is.
Be sure to replace **p4ssw0rD** with your own strong password. 

Save and close the configuration file using **wq!** and move to the next step to start your Shahdosocks server on your Ubuntu 16.04 server.
