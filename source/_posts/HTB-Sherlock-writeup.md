---
title: HTB Sherlock writeups (very easy)
date: 2025-05-07 20:38:30
thumbnail: '/images/sherlock/sherlock-banner.png'
tags:
    - "HTB"
    - "Sherlock"
    - "DFIR"
    - "CTI"
category:
    - "HTB"
---

## Intro

This is more like notes or resources i found useful while blazing through the very easy challenges. I thought of just compiling these here because a whole writeup felt excessive (for this difficulty level).

So u can have a look and may even find something useful here!

## DFIR

### Brutus

The chall was straightforward and mostly reading through the contents, With keenness and a few minutes u can answer the questions easily.

This was a useful one [utmp parser](https://gist.github.com/4n6ist/99241df331bb06f393be935f82f036a5) , it helped with parsing and understanding the given utmp file.

The most I can say for this chall is:

- Use a good GUI text editor for reading the file
- [MITRE ATT&CK matrix](https://attack.mitre.org/matrices/enterprise/)
- Parse & understand utmp file


### SmartyPants

To start off, I needed the [Eric Zimmerman's tools](https://ericzimmerman.github.io/#!index.md) (.net6 for linux/mac) for the `EvtxECmd` tool, this helps to parse all the .evtx files and convert them to .csv file.

This also told me a few stuff about what event IDs indicate, and which ones we need to lookout for, one such example is

> event ID 4624: This event generates when a logon session is created 

This one helped in finding answer for 1st task.

I'm not pro with event log analysis stuff, so the given hints were super useful in filtering and going through logs faster.

Tools which helped out:

- EvtxECmd
- Event log explorer
- Timeline Explorer

### UFO-1

Mostly the MITRE stuff again to analyse, this stuff helped

- [MITRE CTI](https://attack.mitre.org/campaigns/) (Groups, Software, Campaigns)
- [MITRE navigator layers](https://mitre-attack.github.io/attack-navigator/)

### CrownJewel-1 

meow

## Threat Intelligence

### Dream Job-1

This challenge was mostly going through the [MITRE ATT&CK](https://attack.mitre.org/) , something new to me and was fun to solve this.

Stuff required for this chall:

- [MITRE CTI](https://attack.mitre.org/campaigns/)
- [MITRE navigator layers](https://mitre-attack.github.io/attack-navigator/)
- [Virustotal](https://www.virustotal.com/gui/home/upload) (Details, Relations, Behavior)

