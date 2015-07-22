---
layout: post
title: "Zombies Game"
description: ""
category: 
tags: []
---
{% include JB/setup %}

So a couple of weeks ago I started working on a game in C#. This game is going to be really shitty &amp; awesome at the same time.  I am the only one working on it currently, and it is likely that it will stay that way for the time being.  It is currently using 2(3) external libraries. They are [OpenTK](http://www.opentk.com/) for OpenGL graphics, [Jitter](www.jitter-physics.com) for Physics, and [LibSquish](http://code.google.com/p/libsquish/) for texture compression (which doesn't work yet).  I have no real basis for what the game will be about other than that I want it to be a zombies type game.  I feel like some of the ideas I have gotten so far draw heavily from the Call of Duty style zombies.  At this point, art is nonexistent  game mechanics is nonexistent  framework is almost done, and there are large bugs in the animation system that could hinder the project.  I wrote a whole filysystem/asset/resource system that loads from either plain text or binary based on availability.  This filesystem loads files as needed or when they are "cached".  I also had this idea for a "CacheList" instead of the standard level file.  This allows me to run one of these lists to load all the assets needed for a level from many different asset files (or ZPAKS as I am calling them).  These ZPACKS are .dat files that contain binary data with a header that describes the asset type, name, and offset in the file.  This is alright for now and seems sufficient until I need to start compressing stuff/encrypting stuff (might use a hash ID of the loaded classes to make sure stuff is legit for online play if I get that far).

Screenshot:

![screenshot]({{ ASSET_PATH }}/images/snip.png)


Hopefully I can get some more game mechanics such as player physics implemented as well as level loading via CacheLists in the next few weeks.

-TheApadayo