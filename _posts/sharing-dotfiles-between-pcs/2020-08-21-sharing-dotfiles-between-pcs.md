---
layout: post
title: Sharing dotfiles between computers
description: Hot I manage to share my dotfiles between my computers environment.
date: 2020-08-21 10:42 +0200
modified: 2020-08-21 10:42:00 +02:00
tag:
  - dotfiles
  - icloud
  - macos
---

Some years ago while I was digging inside Github's gigantic repositories base, I found Kenneth's dotfiles and
at the time he did something really interesting. He put his dotfiles inside the iCloud folder, so the folder
would automatically be updated in every MacOS device connected to that iCloud account.

At the time I was using two macbooks(personal/work), so I was eager to try that and see how it does. So here's
what I did to manage that setup.

##### Link dotfiles folder to iCloud
First we have to create a symbolic link between the dotfiles folder and the iCloud folder.
```bash
ln -S ~/dotfiles '~/Library/Mobile Documents/com~apple~CloudDocs'
```

Now the dotfiles will begin to upload and be shared with every computer using the same iCloud account. But now
how to keep every config always updated? Every alias setted? It's simple, we just have to add some more symbolic
links and then we'll be all set.

##### Create the config files symbolic links
```bash
ln -S ~/dotfiles/my.conf ~/path/to/conf/.my.conf
# do this for every config you use(zsh, bash, envs, vim, git...)
```

After these steps you shall have your configs properly setted, and after changing some config in one of the
computers it will automatically update the other ones.

Hope this can be helpfull and maybe give some insights in how we can automate small things to improve our productivity
or just for the fun of it, you can also find my dotfiles [here](https://github.com/gugsrs/dotfiles).


