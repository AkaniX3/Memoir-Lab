---
title: CTF@CIT 2025 writeup
date: 2025-05-05 09:18:32
thumbnail: '/images/CIT-CTF-2025/citctf_banner.webp'
tags:
    - "CTF"
    - "Forensics"
category:
    - "CTF"
---

Forensics writeup for CTF@CIT 2025 ! This time i was able to clear the whole category, let's gooo 🔥

---

## Forensics:

### Brainrot Quiz!

> Bombardiro Crocodillo or....? You find out...

> File: "brainrot.pcap"

A pcap file is given so let's fire up wireshark for analysing it.

![image](images/CIT-CTF-2025/brainrot-1.png)

While browsing through the packets, I found some sus base64 string.

Dropping it into cyberchef-

![image](images/CIT-CTF-2025/brainrot-2.png)

And boom, Got the flag !

Flag: `CIT{tr4l4l3r0_tr4l4l4}`

### True CTF Love

> I got this strange email from another CTF participant not too long ago. I am just not sure what they mean by this...

> Do you love CTFs as much as they do?

> File: "The_Flag_Well_Capture_Together.eml"

{% folding cyan::Email Contents: %}

Return-Path: <ilovectfs19@waifu.club>
Delivered-To: idespisectfs14@memeware.net
Received: by mx1.mailhub.li (Postfix, from userid 65534)
	id 531FD380054; Tue, 22 Apr 2025 16:29:02 +0000 (UTC)
X-Spam-Checker-Version: SpamAssassin 3.4.2 (2018-09-13) on 4e7ad924bcc3
X-Spam-Level: 
X-Spam-Status: No, score=-1.3 required=5.0 tests=ALL_TRUSTED,BAYES_00,
	DKIM_SIGNED,DKIM_VALID,DKIM_VALID_AU,URIBL_BLOCKED,URIBL_SBL_A,
	URIBL_WS_SURBL shortcircuit=_SCTYPE_ autolearn=disabled version=3.4.2
Received: from mail.mailhub.li (unknown [10.69.4.14])
	by mx1.mailhub.li (Postfix) with ESMTPS id 98762380048
	for <idespisectfs14@memeware.net>; Tue, 22 Apr 2025 16:29:00 +0000 (UTC)
Authentication-Results: mx1.mailhub.li;
	dkim=pass (2048-bit key; unprotected) header.d=waifu.club header.i=@waifu.club header.b="e65uxTcZ";
	dkim-atps=neutral
MIME-Version: 1.0
DKIM-Signature: v=1; a=rsa-sha256; c=relaxed/simple; d=waifu.club; s=mail;
	t=1745339340; bh=HSq3Fk4UngoT3615kRTwX9TQfq9o0GNk3L5esFLg2e4=;
	h=Date:From:To:Subject:From;
	b=e65uxTcZ2s8RKde5x7GoMWDhM27qMUa2vpmCC6uPR/kCsC5Tl1lgVNCik9TBiIn7x
	 ThMSG0m17ElJR+eQ3IFACqhDjoJkCdLo+iYAwvx4Go1OOYUYRx7dn7tUisIKy2p7Ns
	 DjJMauF8H1fwIpO6kFZKUPiPescPp6mBJIWBOARUNxRSSReBJv+B8GibZJbN4c64c0
	 wOVpmrc1P3sGs/K1i8sjzcHVJyNdBBV2e71n5gJFfbo5EkM/HSmba8Vvfdg2BGkVaY
	 OriRs9vs5+XwV8v9stPhL48avJipOSz1ykfbXW3//QZYpAOGyQz8lhE2cek5YLJulB
	 yO/Pz8vtbkwjA==
	b=V293LCB3aGF0IGEgYmVhdXRpZnVsIGxpdHRsZSBwb2VtLiBJIGFsbW9zdCBzaGVkI
	 GEgdGVhciByZWFkaW5nIHRoYXQuIEhvcGVmdWxseSB5b3UgbGVhcm5lZCBtb3JlIGFi
	 b3V0IGVtYWlsIGhlYWRlcnMuIEJ1dCBzZXJpb3VzbHksIGl0IGdldHMgbWUgd29uZGV
	 yaW5nLi4uIGRvIHlvdSBsb3ZlIENURnMgYXMgbXVjaCBhcyB0aGV5IGRvPwoKQ0lUe2
	 lfbDB2M19jdGYkX3QwMH0=
Content-Type: text/plain; charset=UTF-8;
 format=flowed
Content-Transfer-Encoding: 8bit
Date: Tue, 22 Apr 2025 12:29:00 -0400
From: ilovectfs19@waifu.club
To: idespisectfs14@memeware.net
Subject: =?UTF-8?Q?The_Flag_We=E2=80=99ll_Capture_Together?=
User-Agent: Roundcube Webmail/1.4.15
Message-ID: <94db41ae58ad070173fc571e2008de8b@waifu.club>
X-Sender: ilovectfs19@waifu.club

My dearest one, whose heart I hold,
In your eyes, I see a world untold.
Where logic and riddles dance like stars,
But you, my love, stand behind bars.

I know you don’t share in my joy,
The flags I chase, the games I deploy.
You sigh, you frown, and often retreat,
While I dive into challenges, so sweet.

But, oh, my love, can you see?
These puzzles are not just code to me.
They’re like whispers from a distant shore,
A challenge to unlock, a path to explore.

I dream of the day when you’ll join the chase,
And feel the thrill of solving with grace.
Not because you have to, but because you’ll find,
The joy in puzzles that ignites your mind.

I won’t push, I won’t rush,
No, I’ll be gentle, soft as a hush.
But if you ever wish to see what I see,
I’ll be here, waiting, with a flag for you and me.

Together, we can solve what’s unknown,
In the world of CTFs, you won’t be alone.
Let’s take it slow, no need to fear,
For love is the answer, my darling, my dear.

So, when you're ready, just take my hand,
Let’s solve the puzzle, make our stand.
And who knows, perhaps you’ll see—
That even CTFs, too, can be love’s key.

{% endfolding %}

Here I have an email file given, going through the file, u can kinda get the idea of another base64 string.

so trying it in cyberchef-

![image](images/CIT-CTF-2025/true-1.png)

Another quick flag !

Flag: `CIT{i_l0v3_ctf$_t00}`

### We lost the flag

> Sorry everyone, we unfortunately lost the flag for this challenge.

> File: "lost.png"

Here's a png file, and on opening it doesnt display anything.. mmm maybe try checking it

![image](images/CIT-CTF-2025/lost-1.png)

Not so helpful? well, its not even a png file,,, or is it? (vsauce moment)

I checked the File header real quick, and this found this

![image](images/CIT-CTF-2025/lost-2.png)

png is not this right?

Quick google search told me that its JFIF for .jpg files

![image](images/CIT-CTF-2025/lost-3.png)

Fixing the file header now, and now we can view the image !

![image](images/CIT-CTF-2025/lost-4.png)

Got the flag ! this was an cheeky but easy one too~

Flag: `CIT{us1ng_m4g1c_1t_s33m5}`

### Bits 'n Pieces 

> Somewhere in these digital fragments lies what you've been searching for your entire lifetime, or really just this weekend ;)

> File: "Cache0000.bin"

Firstly shoutout to my team Bits & Pieces, we got a chall named for us XD

So there's a bin file, `file` `exiftool` weren't really helpful, so with a bit of searching i found it's a Windows cache file and we can parse it using a github repo [bmc-tools](https://github.com/ANSSI-FR/bmc-tools)

![image](images/CIT-CTF-2025/bits-1.png)

Parsing it we got 2992 files 💀 oh boy we got alot to go through.

Using a little bit of iq, I can see that these are pieces of the windows page, it's like puzzle pieces of a screenshot.

![image](images/CIT-CTF-2025/bits-2.png)

I couldn't really go through them easily, so i searched for an effective way and found this github repo [RdpCacheStitcher](https://github.com/BSI-Bund/RdpCacheStitcher) This made it so much more easier to go through the pieces and connect them to make some sensible content.

![image](images/CIT-CTF-2025/bits-3.png)

Gotcha, This flag was tedious and took a WHILE, but got it

(also u can see in the image i got baited by a fake flag before the real one 😭)

Flag: `CIT{c4ch3_m3_if_y0u_c4n}`

### Baller

> Find the flag.

> File: "baller.zip"

Alright so I have a zip file, and upon extracting we get 3 plain txt files (TLDR; they are useless)

So i spend doing some lil forensics on all the 3 files and go zero progress for like 20mins.

except for,,, `binwalk` yes ! binwalk showed something not in the plain txt files, BUT the .zip file

![image](images/CIT-CTF-2025/baller-1.png)

there's a .gif file hidden in the .zip which didn't show up upon extracting.

Binwalking the .zip file gave another zip file and on and on and on, infinite loop...

I need a more smarter approach. Focus on the decimal offset value of the file.

the gif file is AFTER the zip file offset ends. That makes sense why i was not able to extract it. More talks later, now let's try getting the gif file explicitly and for that we can use

```bash
dd if=baller.zip of=hidden.gif bs=1 skip=16631
```

dd command helps us extract the file when we specify the offset.

Upon opening the file

![image](images/CIT-CTF-2025/baller-2.png)

We got win win, a cute duck which brought the flag to us !

Flag: `CIT{im_balling_fr}`

---

That's all for today, thanks for reading 🍞✨