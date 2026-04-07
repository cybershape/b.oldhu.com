---
date: 2013-10-28T00:00:00Z
draft: false
status: publish
tags: []
title: Suunto to Nike Plus Uploader
type: post
url: /2013/10/28/suunto-to-nike-plus-uploader/
---

I used to run with nike plus system and got all my runs saved there. I also use [SmashRun](http://smashrun.com/) to analyze my runs which syncs to Nike Plus. Then I bought this great watch: Suunto Quest Running Pack. It uploads only to it's own website Movescount.com.

So I decided to write a tool to convert my Suunto moves into Nike Plus runs. I run it everytime I sync with Movescount.com via the Moveslink application. It will automatically collect all new moves and upload them all into my Nike Plus account. Then I can see them on my iPhone's Nike Running app or SmashRun website.

If you need it, you can find it here:

[Suunto2Nike Releases](https://www.evernote.com/pub/hhhtj/Public)

Also, I personally found Ambit 2 some uncomfortable so I move to Garmin 620 platform. But I keep all my data at sporttracks.mobi now.

Updated 2014/1/1:

A new v0.0.4 version released with the following new features, check it out at the above release link!

 * Support Quest and Ambit 2
 * Support Windows and Mac
 * Support GPS track conversion

Updated 2014/5/29:

Nike has closed the legacy login URL(which I used in my application). Now they accept OAuth request only. It's a good move towards better security. But they currently only open the OAuth API [to the partners](https://developer.nike.com/support.html#faqsection_5-faqitem). So this application now stops working and cannot be fixed until Nike opens the API to individual developers.

Updated 2014/8/27:

I've updated the app to support the latest Nike login API and new .sml file format. Please download the latest version (v0.2.1 now) from the above release link.