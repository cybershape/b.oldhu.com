---
date: 2012-02-16T00:00:00Z
draft: false
status: publish
tags: ["android"]
title: How to unpack and repack Android's boot.img correctly
type: post
url: /2012/02/16/how-to-unpack-and-repack-androids-boot.img-correctly/
---

boot.img's structure is described in bootimg.h, so the safest way to unpack boot.img is using split_bootimg.pl.

During the unpack, the original split_bootimg.pl outputs the kernel boot args as "Command line", which should be included in the args of mkbootimg.

Another important parameter is the base address, which is calculated as tags_addr - 0x00000100. I modified the original split_bootimg.pl to output this info. Check [here](https://gist.github.com/tiejunhu/1832541) if you want to use the modified version.

After the split, now you have the boot.img-kernel, boot.img-ramdisk.gz, cmd line and base address. Then you can simply replace the boot.img-kernel with your zImage and repack the whole thing using the mkbootimg tool. Or if you want to modify ramdisk, you can unpack and repack it using gzip and cpio.
