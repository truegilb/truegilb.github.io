---
layout: post
title: "Resurrecting a 7 year old Android tablet"
categories: article
excerpt: "Kindle Fire Gen 1 still workable as kids tablet"
tags: ["Android", "CM", "Cyanogen", "Kindle Fire", "Custom", "ROM", "nostalgic", "flash" ]
---

First, I still hanging on to my [Kindle Fire 1st generaiton](https://en.wikipedia.org/wiki/Kindle_Fire#Hardware) that was released in 2011(!). That also happened to be my first touch-based tablet (No, I didn't own a Palm one). Loved that little device and very soon learned about what rooting an Android device meant. It was early days for mobile and especially small-formed tablet. It bear so much potential and especially it opened up what AOSP and very soon the active scene of custom rom. 

# Custom Recovery for Android

Here's a refresh of Recovery and specifically TWRP which I've got before after installing FireFireFire from [here](https://hashofcodes.wordpress.com/kindle-fire-support/kindle-fire-1st-generation-otter/).

Some really old school android hacks with the glory days of XDA - which I occasionally miss.

Now what do I do with this TI OAMP based device? Mostof the apps not working anymore. That leads me to look further into the custom rom scene again.

# Custom ROM

I have the custom recovery, so just need a [CyanogenMod](https://en.wikipedia.org/wiki/CyanogenMod)(CM) rom to flash... Wait, but CM is no longer in operation anymore - how about lineage? Unfortunately a quick searh shows that Lineaage is not supporting [Texas Instrument's OMAP platform](https://en.wikipedia.org/wiki/Texas_Instruments_OMAP) - which could be an multimedia chipset with ARM core that has gone out of favor.

So now what? getting some [cm11 zip file](https://archive.org/details/cmarchive_nighlies) from archive - thanks to the kind souls at archive.org. What is a nightly? It's essential a CI build 

Then just flash it - but wait there are errors with a call to set_meta_recursive() which shows up in the flash log screen under [TWRP](https://twrp.me/about/).

<pre>
    Installing update...     
    set_metadata_recursive: some changes failed
</pre>

Another search on [stackexchange](https://android.stackexchange.com/questions/62982/flashing-cm-11-i-get-set-metadata-recursive-some-changes-failed) shows the TWRP that I got doesn't have that set_metadata interface so some enterprising users have used set_parm interface just like cm10. If one is interested in learning the updater-script in Android ROM, a detailed writeup is here on [XDA](https://forum.xda-developers.com/showthread.php?t=2377695).

So how about unzipping the ROM and look further?

<pre>
    mkdir otter; cd otter;
    unzip ../cm11-nightly-otter.zip
</pre>

According to that post, `META-INF/com/google/android/updater-script` was the culprit.

So let's look at the function signature:

    set_metadata("/tmp/backuptool.sh", "uid", 0, "gid", 0, "mode", 0755);
    set_perm(0, 0, 0755, "/tmp/backuptool.sh");

These two are equivalent, just need to write a script to `s/set_metadata/set_perm/` and `s/set_metadata_recursive/set_perm_recursive` to achieve a simple remapping of argument position and functoin name (there are at least a couple dozens of these).

What's next is to repackage the ROM image and flash to try it on the hardware! You need to use the jar utility despite the plain old unzip works for extraction. Do `apt-get install default-jdk` as needed or other JDK disto. The command to package is

<pre>
    cd otter; jar cf * ../cm11-nightly-fixed.zip .
    jar tf ../cm11-nightly-fixed.zip # to test
</pre>

Note that I get into the directory so that every file is not prefixed with `otter`.

# What about the Android apps?

I deliberately do not want Play store on this device (to minimize kids getting apps themselves and the whole sign in). So we'll keep to VLC, and Opera mini etc. for the basics.

There is also the F-droid aggregation site for non-google apps.
