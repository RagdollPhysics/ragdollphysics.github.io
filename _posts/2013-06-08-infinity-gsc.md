---
layout: post
title: "Infinity GSC... Or Whatever"
description: ""
category: 
tags: []
author: "TheApadayo"
---
{% include setup %}

So I've been meaning to write a post about this for a while.  This project that I have been working on, IW5GSC, Infinity GSC, GSC Emulator, or whatever you want to call it.  It is something that I think is a huge accomplishment for me.  While I don't want to release the source of it due to the idiocy of the community (if you actually looked then you could probably find it), I do, however, want to share how it works.  Hopefully there are some of you out there that like this sort of technical analysis.

The way most compilers work is there is a Lexer and a Parser.  The Lexer turns characters into objects called "tokens" that are converted into a stream and fed into the parser.  The Parser then interprets the pattern of these tokens and forms a "parse tree" which is then formatted into an [Abstract Syntax Tree](http://en.wikipedia.org/wiki/Abstract_syntax_tree) or AST.  This AST contains all of the information about your code.  For this project, I used [ANTLR](http://www.antlr.org/) for my parser and lexer.  This was nice because it has a C# target that I can use with InfinityScript.  Antlr turns a special ".g" file written in their own language in order to create a lexer and parser pair that generates the AST that I want it to.

The AST is then parsed by my own code to build a tree of nodes that are used by the runner.  The runner is currently in a state that could be described complete hell.  I wrote it to be a sort of one time thing that should just work.  Turns out, when you decided to add things like threading, debugging, or other fun features, you actually need to think out what the hell you are doing before you write it.  The code currently runs through a very elaborate system that evaluates a list of C# objects that were generated during the compilation process. Turns out, this is really inefficient and hard to monitor due to the way things like if statements, while statements, and other such things are stored.  The code for each of these are stored under the individual objects and doesn't return from the if statement execution until it finishes with the code under if, or the else, or the else if, and etc.  This creates issues when you try to put in debug breaks or thread endings into the mix.  I am planning a rewrite ~~that stores the compiled code as a bytecode that is interpreted to allow for debugging and other things that need to execute on an instruction by instruction basis.~~ Nope... Static recompilation to C# it is.

The stack system that I wrote works pretty well for what it turned into.  It works as a dictionary of objects of pre-defined types.  The global "stack" is just a list of these dictionaries that can be added to or taken away from.  This allowed the stepping in and out of blocks of code like if statements without having to clean up variables that were used inside the code block.  This feels like one of the few things that will make it into the rewrite.

The interface with InfinityScript took me a while too.  It uses all of the functions directly in the GameInterface class.  This gives me full control over what happens like script calls, or notifications, or other such things.  I did have to write a global dispatcher for level and entity notifies that gets passed to each script domain.

This whole project was a nice opportunity for me to work on new techniques of C# coding for me.  It also gave me a great insight on to how the GSC system works in IW4 and IW5 respectively.  I spent a great deal of time reversing them to see how they work.  I will make a note here that this project deals with fairly high level emulation.  I am emulating the results of the script rather than the method.  Most of the code is pretty sloppy and hacked together but fairly functional as far as I can tell.

Please do leave comments on what you like here.  I saw a recent spike in popularity since STvLKER posted a link to here on FourDeltaOne.  Thanks for that.  I had almost forgotten about this place.  I hope to (don't get your hopes up) keep content coming here in the future.  Do comment if you like this kind of content or even if you don't.  I really just want to get some feedback for this. If you got this far then thanks for reading!