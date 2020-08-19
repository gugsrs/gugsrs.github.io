---
layout: post
title: Tmux Spotify Plugin
date: 2020-08-19 01:00 +0700
modified: 2020-08-19 16:49:47 +07:00
description: How I made a tmux plugin to show spotify's track information.
tag:
  - tmux
  - plugin
  - spotify
  - script
image: /assets/tmux-spotify-mac.png
---

It's been about 5 years since I've started to use Tmux and I really recommend it to everyone, it's been a great addition to my set of tools, and it does an amazing job.
I'll not focus this post on tmux itself, but on how I managed to create a plugin to show my spotify track info in it.

A while back I was searching a way to tweak my tmux status bar for it to show the spotify info, and since I used Mac I discover that the easiest way to do that would be using osascript(AppleScript).
So I endup with something like this:

```bash
#!/usr/bin/env osascript

tell application "Spotify"
  if it is running then
    if player state is playing then
      set track_name to name of current track
      set artist_name to artist of current track

      artist_name & " - " & track_name
    end if
  end if
end tell
```

As use can see it's a simple Apple Script code that tells the Spotify Application what it wants and prints it at the end.

## Creating the plugin
With the track info, now I could go on to the next step, that was create a plugin to set it in tmux statusbar automatically.

Taking a look at some projects like [tmux-simple-git-status](https://github.com/kristijanhusak/tmux-simple-git-status), I was able to identify how to achieve what I wanted. Since it was very similar I ended using some parts of the `tmux-simple-git-status` code.

I'll breakup the plugin in three main parts:

**1. Retrieve tmux status bar value**

```bash
tmux show-option -gqv "status-right"
```

**2. Replace substring with spotify info**
```bash
${string/$placeholder/$spotify_info}
```
`string`: tmux status-right value.<br>
`placelholder`: value setted in `.tmux.conf` to be setted with the script automatically.<br>
`spotify_info`: spotify track info retrieved with osascript.<br>


**3. Set tmux new status bar value**

```bash
tmux set-option -gq "status-right" "$value"
```
`value`: string generated in step 2 to be setted as new value for the status bar.<br>


## Final Result
<figure>
	<img src="{{ page.image }}" alt="Terminal screenshot">
	<figcaption>Fig 1. Spotify info in tmux status bar.</figcaption>
</figure>

With this three steps I was able to succeed in creating the plugin, and now in a very easy way it is possible to show the user's spotify info on tmux, unfortnatelly it's only for MacOS by now.

If you are interested in this project you can find it here: [`gugsrs/tmux-spotify-mac`](https://github.com/gugsrs/tmux-spotify-mac)
