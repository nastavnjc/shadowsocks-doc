---
author:
  name: Linode Community
  email: docs@linode.com
description: 'Shadowsocks is a secure socks5 proxy, designed to protect your Internet traffic. It encrypts the traffic between you and the servers, so nobody can spy on you. The main difference compare to VPN is that Shadowsocks is not global, which means not all your traffic will go through the servers.'
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
 - '[Shadowsocks](https://shadowsocks.org/)'
---

*This is a Linode Community guide. If you're an expert on something we need a guide on, you too can [get paid to write for us](https://www.linode.com/docs/contribute).*
<hr>

[Shadowsocks](https://shadowsocks.org/) is a secure socks5 proxy, designed to protect your internet traffic. It encrypts the traffic between you and the servers, so nobody can spy on you. The main difference compare to VPN is that Shadowsocks is not global, which means not all your traffic will go through the server. If you want to use an Instant Messenger or a uTorrent, you will have to configure those programs settings to use the applicable Socks 5 proxy and port.

![Install Shadowsocks on Ubuntu 16.04](https://github.com/nastavnjc/shadowsocks-doc/blob/master/install-shadowsock-on-ubuntu-16-04.png "Install Shadowsocks on Ubuntu 16.04")

Let’s say you find yourself in a situation where OpenVPN traffic is blocked or throttled, Shadowsocks is a good alternative to tunnel the entire network traffic.

{: .note}
>
> This guide is written for a non-root user. Commands that require elevated privileges are prefixed with `sudo`. If you’re not familiar with the `sudo` command, see the [Users and Groups](/docs/tools-reference/linux-users-and-groups) guide.

# Server configuration
		
## 1. Before You Begin

1.  Familiarize yourself with our [Getting Started](/docs/getting-started) guide and complete the steps for setting your Linode's hostname and timezone.

2.  This guide will use `sudo` wherever possible. Complete the sections of our [Securing Your Server](/docs/security/securing-your-server) to create a standard user account, harden SSH access and remove unnecessary network services. Do **not** follow the Configure a Firewall section yet--this guide includes firewall rules specifically for a Shadowsocks server.

3.  Update your system:

        sudo apt-get update && sudo apt-get upgrade

## 2. Open Corresponding Firewall Ports

In this case we're using Shadowsocks's default port `8000`, but this could be any port you specify later in the configuration file.

    sudo ufw allow 8000/tcp
    sudo ufw enable

## 3. Installing Pip

Pip is an easy to install package management system which is used to install and manage software found in the Python Package Index and it give us a convenient way to install Shadowsocks. Make sure that `pip` it's installed on your system, if not then use below command to install it.

    sudo apt-get install python-pip
    sudo apt-get install python-m2crypto
    
This will installs the Python PIP and Python-m2crypto packages along with other dependencies. The **m2crypto** package is used to encrypt the tunnel traffic.

## 4. Installing Shadowsocks

Once the dependent packages are installed, issue the following `pip` command in your command line terminal to install shadowsocks.

    sudo pip install shadowsocks

This will installs the latest available package.

## 5. Configuring Shadowsocks

Before we start Shahdowsocks on your Linode, let’s create a new file and put the following configuration contents in it that contains your hostname or server ip address , server port number, local port number, a password used to encrypt transfer, connection timeout and encryption method like “aes-256-cfb”, “bf-cfb”, “des-cfb” or “rc4”, etc. The default encryption method used is not secure so we will be using `aes-256-cfb` which is recommended.

1.  Run the below command to open a new file using your command line editor and put the following configuration parameters in it.

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
> Be sure to replace `your_server_ip` with the ip address of your own Ubuntu Server. Usually, Shadowsocks listen on port `8000` but you can change with your own port. If so, remenber to modify the previous firewall rule accordingly.
`local-port` is referring to a listening port on your client device (Windows PC, Apple PC, etc.), you can leave it as it is. Be sure to replace **p4ssw0rD** with your own strong password. 

2.  Save and close the configuration file and move to the next step to start your Shahdosocks server on your Linode.

## 6. Starting Shadowsocks

1.  Once you have your configuration in place, use below commands to start, stop or restart your Shadowsocks server as shown:

        sudo ssserver -c /etc/shadowsocks.json -d start
    
        sudo ssserver -c /etc/shadowsocks.json -d stop
    
        sudo ssserver -c /etc/shadowsocks.json -d restart
    
2.  Check from its log file if the server has been started successfully, any error will be reported here:

        tail /var/log/shadowsocks.log
    
3.  Also check if the server is listening on port `8000` using below command:

        netstat -tlnp
    
## 7. Starting at system boot (optional)

1.  Run the below command to open the `/etc/rc.local` file using your command line editor:

        sudo vim /etc/rc.local

2.  Add the following line to auto start Shadosocks service at boot:

        /usr/bin/python /usr/local/bin/ssserver -c /etc/shadowsocks.json -d start
        
3.  It's a good idea to restart your server to see if everything is working:

        sudo shutdown -r now

4.  Once restarted, verify the log file again:
  
        tail /var/log/shadowsocks.log
		
# Client configuration

## Linux operating system


## Windows operating system

## Configuring Firefox to Use Shadowsocks

Open your firefox web browser on the system where you have installed the Shadowsocks client.
Open the menu from the the top right corner of your firefox web browser, select the `Advanced` option and click on the `Settings` button under the `Network` menu bar.

![Firefox proxy setting](https://github.com/nastavnjc/shadowsocks-doc/blob/master/firefox-set-proxy.png "Firefox proxy setting")

Next, under the connection settings, choose `Manual Proxy Configuration` and enter corresponding values as shown below.

![Firefox proxy setting](https://github.com/nastavnjc/shadowsocks-doc/blob/master/firefox-manual-proxy.png "Firefox proxy setting")

Now press the `OK` key to start surfing the web using your Shadowsocks proxy server.

# Conclusion

Once you have successfully setup your Shadowsocks client side installation and integrated it with your Shadowsocks server, all the traffic will be passed through your proxy server and you will have an easy access to all protected websites in your region that will help you surf the internet privately and securely.

# More information

You may wish to consult the following resources for additional information on this topic. While these are provided in the hope that they will be useful, please note that we cannot vouch for the accuracy or timeliness of externally hosted materials.

+ [Shadowsocks main site](https://shadowsocks.org/)
+ [Shadowsocks on GitHub](https://github.com/shadowsocks)
