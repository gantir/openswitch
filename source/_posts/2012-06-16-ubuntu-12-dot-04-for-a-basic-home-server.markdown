---
layout: post
title: "Ubuntu 12.04 for a basic home server"
date: 2012-06-16 20:28
comments: true
categories: linux ubuntu server how-to
---

I'm writing this article mainly as a resource for myself so the next time I set up an Ubuntu server I'll have something of a checklist to go by.  All this information has been gleaned from various resources on the Internet, I'm not even sure where most of the knowledge came from.

Software: Ubuntu Server (headless) 12.04 32-bit <br />
Hardware: HP Workstation xw4400 - Core2 Duo - 2GB Memory - Two 1TB HDD in RAID1

<!-- more -->

## Initial Installation

After [downloading Ubuntu Server](http://www.ubuntu.com/download) install the OS as per normal.  In my case I needed to create a RAID1 array and install the OS to it.  If you're looking for a tutorial on how to do that, [here's a good one](https://help.ubuntu.com/community/Installation/SoftwareRAID).  When it comes time for selecting additional packages to install, just install OpenSSH and Samba.

{% img http://farm8.staticflickr.com/7227/7383417674_1119cdb3e3.jpg" %}

### Configure static IP and DNS

By default, Ubuntu will install with an IP address assigned via DHCP.  Obviously this is less than ideal for a server.  Once logged in to the system we'll need to edit /etc/network/interfaces. 

{% codeblock %}
sudo nano /etc/network/interfaces
{% endcodeblock %}
	
In a stock install you'll see these lines:

{% codeblock /etc/network/interfaces %}
auto eth0
iface eth0 inet dhcp
{% endcodeblock %}
	
Change them to look like this (substitute your actual address scheme).  Note the addition of dns-nameservers, in my experience this has always been required when configuring the server for a static IP address:

{% codeblock /etc/network/interfaces %}
auto eth0
iface eth0 inet static
	address 192.168.1.200
	netmask 255.255.255.0
	network 192.168.1.0
	broadcast 192.168.1.255
	gateway 192.168.1.1
	dns-nameservers 192.168.1.1
{% endcodeblock %}

Some older tutorials on setting up DNS on Ubuntu Server will tell you to edit /etc/resolv.conf directly.  This will not work, and while there are a handful of methods of configuring DNS the above method I just laid out is the one I prefer.

Now we need to restart the networking service.  First bring it down:

{% codeblock %}
sudo ifdown eth0
{% endcodeblock %}
	
Then bring it up:

{% codeblock %}	
sudo ifup eth0
{% endcodeblock %}	
	
## Configure SSH

It's a small thing, but I always change the port SSH uses on my server.  Granted, it's a small step of security but every little bit helps.

{% codeblock %}
sudo nano /etc/ssh/sshd_config
{% endcodeblock %}

Where you see this line:

{% codeblock /etc/ssh/sshd_config %}
# What ports, IPs and protocols we listen for
Port 22
{% endcodeblock %}
	
Change the port number to whatever you want below 65000, making sure you're not choosing a port that's already in use.

{% codeblock /etc/ssh/sshd_config %}
# What ports, IPs and protocols we listen for
Port 12345
{% endcodeblock %}
	
Now, if you're so inclined you can do the rest of the setup via SSH.

## Install Webmin

There are several applications available for server management, and I like [Webmin](http://www.webmin.com/) the best.  Here's how to install it.

Edit the sources.list file:

{% codeblock %}
sudo nano /etc/apt/sources.list
{% endcodeblock %}

Add these lines to the bottom of the page:

{% codeblock /etc/apt/sources&#46;list %}
deb http://download.webmin.com/download/repository sarge contrib
deb http://webmin.mirror.somersettechsolutions.co.uk/repository sarge contrib
{% endcodeblock %}

Import the GPG key and add it:

{% codeblock %}
wget http://www.webmin.com/jcameron-key.asc
sudo apt-key add jcameron-key.asc
{% endcodeblock %}

Update the source list:

{% codeblock %}
sudo apt-get update
{% endcodeblock %}

Install Webmin:

{% codeblock %}
sudo apt-get install webmin
{% endcodeblock %}

Webmin will install itself and can be accessed at https://192.168.1.200:10000.  You will see a login page, enter the username and password that you signed on to your server with.  Right off the bat you'll find that your username and password don't work.  It's a common problem, here's how to fix it:

{% codeblock %}
cd /usr/share/webmin
sudo perl changepass.pl /etc/webmin root newpass
{% endcodeblock %}

Substitute "newpass" for whatever password you want.  Now you can go back to the Webmin login page and your credentials will work.  The default username is "root", enter the password you chose.

At this point you'll be able to do virtually all your server management via the web interface of Webmin.  I find Webmin to be ideal for managing Samba shares, software updates and everything else I need to control in a simple home server environment.