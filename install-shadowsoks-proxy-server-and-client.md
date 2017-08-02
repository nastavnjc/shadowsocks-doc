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

*This is a Linode Community guide. If you're an expert on something we need a guide on, you too can [get paid to write for us](/docs/contribute).*
<hr>

[ShadowSocks](https://shadowsocks.org/) is a secure socks5 proxy, designed to protect your Internet traffic. It encrypts the traffic between you and the servers, so nobody can spy on you. The main difference compare to VPN is that Shadowsocks is not global, which means not all your traffic will go through the servers. If you want to use an Instant Messenger or a uTorrent, you will have to configure those programs settings to use the applicable Socks 5 proxy and port.

![Install ShadowSocks on Ubuntu 16.04](/install-shadowsocks-on-ubuntu-16-04.png "Install ShadowSocks on Ubuntu 16.04")

Let’s say if you find yourself in a situation where OpenVPN traffic is blocked or throttled, ShadowSocks is a good alternative to a VPN that can be installed in OpenWRT routers to tunnel the entire network traffic. That software tunnels encrypts your Internet browsing.
