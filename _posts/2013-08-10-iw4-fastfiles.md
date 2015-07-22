---
layout: post
title: "IW4 Fastfiles"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Recently I got bored and decided to do some poking around in IW4MP.exe. I just kind of searched around and worked on dissembling some of the larger functions in the game. I got to the main zone loading code and decided I should just go ahead and jump right into decoding the zone format. This took a little time to get it figured out but I finally discovered that the format is already almost completely decoded! The internal data that is compressed with zlib is acually just a stream of asset headers with the data behind pointers streamed after it! For example: the rawfile asset has a header of this

<pre>
<code class="cpp">typedef struct
{
     const char* name;
     int sizeCompressed;
     int sizeUnCompressed;
     char * compressedData;
} rawfile_t;</code>
</pre>

looks like this in a file
![rawfilehex]({{ ASSET_PATH }}/images/rawfilehex.png)
Each asset is streamed in such a way.
Now the header for these data blocks is fairly simple. It looks something like this (NOTE: this is just my personal notes that I took while figuring stuff out so if it doesn't make sense then its ok):

<pre>
<code class="cpp">int dataSize; // filesize - sizeof(XHeader_t) or filesize - 40
int XZoneMemorySizes[9]; // I don't really know all of these yet
// some rough estimates are as follows
// XZoneMemorySizes[2] = dataSize * 0.4;
// XZoneMemorySizes[5] = dataSize * 1.3;
int scriptStringCount;
int padding1; // always 0xFFFFFFFF
int assetCount;
int padding2; // always 0xFFFFFFFF
int padding3[scriptStringCount]; // so thats 0xFFFFFFFF repeated scriptStringCount times
// Script strings follow. Simply scriptStringCount null terminated strings.
foreach(asset)
{
     int assetType;
     int padding4; // always 0xFFFFFFFF
}
foreach(asset)
{
     char* assetData; // described above
}</code>
</pre>

The final buffer that you come up with is then loaded into memory and zlib compressed and a header is added to the beginning of the file. The unsigned fastfile header is "IWffu100" followed by the version number (276 for IW4 fastfiles), and then there is 8 bytes of what seems to be random data (I can't see the code using it anywhere but it might be a date or something?).

So thats the basics of the IW4 fastfile format. It is similar to the IW3 one which is fairly well documented but had some large details missing. I hope you found this interesting or at least informative. I will leave you with a teaser of something that *might* happen......

![zonebuilder]({{ ASSET_PATH }}/images/zonebuilder2.png)
