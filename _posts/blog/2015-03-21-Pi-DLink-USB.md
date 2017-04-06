---
layout: post
title: "Using an old D-Link WiFi dongle on Raspberry Pi"
categories: blog
excerpt: "Old hardware lives on RPi"
tags: ["Pi", "Linux", "USB", "firmware"]
image:
  feature:
---

I have a D-Link [DWL-G132][DWL] usb dongle lying around from the PC days.  Like my web cam I would like to put it to good use so the [Raspberry Pi][model-b] is a great choice. So first thing first is to plug it instead of the Edimax dongle and boot up the with the Raspbian image!

It didn't take long to find out the system doesn't have connectivity and a run of lsusb gives the following output: 

{% highlight bash %}
Bus 001 Device 004: ID 2001:3a03 D-Link Corp. DWL-G132 (no firmware) [Atheros AR5523]
{% endhighlight %}

So it appears that the device is detected but no driver can be identified. Needless to say iwconfig confirms the same. 

A bit of [researching][ar5523] suggests that there's a driver for the AR5523 chipset not shipped with Raspian. Let's get started.

### Pointing to alternate source

To be safe I want to temporarily switch out where Pi gets its packages and switch it back later. 

{% highlight bash %}
sudo vi /etc/apt/sources.list
# add the following
deb http://http.debian.net/debian/ wheezy-backports main contrib non-free
# comment out the original repo
# deb http://mirrordirector.raspbian.org/raspbian/ wheezy main contrib non-free rpi
{% endhighlight %}

then 

{% highlight bash %}
sudo apt-get update
#it will say it wants to upgrade the package)
sudo apt-get install firmware-atheros 
{% endhighlight %}

Don't forget to restore the original repo to be on the safe side.

After this my Pi model B+ is well connected with the 2.4GHz dongle.

{% highlight bash %}
lsusb
Bus 001 Device 004: ID 2001:3a03 D-Link Corp. DWL-G132 [Atheros AR5523]
{% endhighlight %}

[DWL]: http://www.dlink.com/uk/en/support/product/dwl-g132-108mbps-wireless-usb-adapter
[ar5523]: https://wiki.debian.org/ar5523
[model-b]: http://www.raspberrypi.org/products/model-b/
