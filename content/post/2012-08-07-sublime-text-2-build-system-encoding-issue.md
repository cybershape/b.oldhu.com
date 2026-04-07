---
date: 2012-08-07T00:00:00Z
draft: false
status: publish
tags: []
title: Sublime Text 2 Build System Encoding Issue
type: post
url: /2012/08/07/sublime-text-2-build-system-encoding-issue/
---

Sublime Text 2 allows you to write your own custom build system. The docs can be found [here](http://sublimetext.info/docs/en/reference/build_systems.html).

But when you have unicode characters in your cmd, you will get error, for example:

Marked.sublime-build:

    {
      "cmd": ["open", "-a", "Marked", "$file"],
      "selector": "text.html.markdown"
    }

Now if you have unicode in your file name, you will get such an error in your console(View -> Show Console) during building:

    UnicodeDecodeError: 'ascii' codec can't decode byte 0xe6 in position 0: ordinal not in range(128)

The stack traces into:

    ~/Library/Application Support/Sublime Text 2/Packages/Default/exec.py

line 130

    129        if not self.quiet:            
    130            print "Running " + " ".join(cmd)
    131            sublime.status_message("Building")

It's the print line causes the error. Patch this line will make everything smooth:

    130            print "Running " + cmd.__str__()

