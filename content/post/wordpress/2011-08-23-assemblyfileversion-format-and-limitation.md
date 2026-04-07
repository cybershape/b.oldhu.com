---
date: 2011-08-23T00:00:00Z
meta:
  _wpas_done_ms: "1"
  jabber_published: "1314094802"
draft: false
status: publish
tags: []
title: AssemblyFileVersion Format and Limitation
type: post
url: /2011/08/23/assemblyfileversion-format-and-limitation/
---

It has to be the format of x.x.x.x and each x is a 16-bit number, therefore the max AssemblyFileVersion is 65535.65535.65535.65535

It's a limitation of Windows, not .NET:

[http://msdn.microsoft.com/en-gb/library/aa381058.aspx](http://msdn.microsoft.com/en-gb/library/aa381058.aspx)

FILEVERSION: Binary version number for the file. The version consists of two 32-bit integers, defined by four 16-bit integers.
