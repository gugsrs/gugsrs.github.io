---
title: Some, easy to forget, things to remember
permalink: /notes/
layout: page
excerpt: Cheatsheet
comments: false
---

#### Accessing MacOSX Keychain from Terminal

MacOSX has a built-in command called `security`, it lets you manage your Keychain.
Kudos to [this blog post from Kramidnarg](http://kramidnarg.com/exploring-secure-notes-in-the-mac-os-x-keychain.html) that helped in my research.
Since I use Keychain's notes to store some keys I've created an alias to retrive a key and copy to clipboard. It can be easily improved to a function passing attributes...

```bash
security find-generic-password -C note -s "NameOfMyNote" -w | perl -lne 'print pack "H*", $_' | awk -F "[><]" '/string/{print $3}'| pbcopy
```
Not the most beautiful command, but it's a nice start.
