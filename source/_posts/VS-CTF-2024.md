---
title: 'VSCTF 2024 writeup'
thumbnail: '/images/vs_CTF_2024/vsctf.png'
date: 2024-6-15
tags:
  - 'CTF'
  - 'Reverse'
---

Our team, 'Bits & Pieces', had a blast participating in the VSCTF event, and we placed 44th rank this time, It was a great run and we tackled some really interesting challenges! Here's my writeup on the challs i solved in the CTF.

---

## Reverse:

### intro-reversing

> Flag will be printed out straight away when you run the binary.

We are given an elf 64-bit file, and as you know it's a rev chall. Boot up that disassembler! On opening the file in binary ninja.

![image](images/vs_CTF_2024/vs1.png)

First thing we can see in the main function, its printing something and then sleeps for god knows how long XD

it's pretty straightforward, we just patch and rmeove that sleep command, then save it and run the file.

![image](images/vs_CTF_2024/vs2.png)

Flag: `vsctf{1nTr0_r3v3rR51ng!}`

---

### awa-jelly

> [JellyCTF](https://jellyc.tf/) has some amazing challenges on [Jelly Hoshiumi](https://x.com/jellyhoshiumi) (星海ジェリー), one of the few VTubers who loves CTF. Inspired by a challenge, I made one based on [AWA5.0](https://github.com/TempTempai/AWA5.0). Can you find the redacted input that matches the screenshot?

![image](images/vs_CTF_2024/vs3.png)

Looking at the image, I immediately started experimenting with the output and given txt file.

First I wasn't able to understand much on how the awa code works, so i read through the given github link. 

![image](images/vs_CTF_2024/vs4.png)

Well i found that the language uses infinite FILO stack that holds bubbles, So the intructions need to passed in reverse order. The characters `k,q,v,x,z` are obsolete which was actually useful to know. I tried to work around with this by running a test string as input to see what output we get. Running a few iterations,

```
1st: abcdefghijlmnoprstuwy0123456789_ -> rlstuwyc01f2i3m4eb56g789_oadhjnp
2nd: rlstuwyc01f2i3m4eb56g789_oadhjnp -> 4feb56gs78w90_2ouladyhjnp3rtc1im
3rd: 4feb56gs78w90_2ouladyhjnp3rtc1im -> owuladyehj6n7p935frtgc1im_4bs802
4th: owuladyehj6n7p935frtgc1im_4bs802 -> 365frtguc1dihmn_aw4bys802polej79
```

I noticed it just transposes text and is easy to just map out.

```py
position_mapping = [26, 17, 7, 27, 16, 10, 20, 28, 12, 29, 1, 14, 30, 25, 31, 0, 2, 3, 4, 5, 6, 8, 9, 11, 13, 15, 18, 19, 21, 22, 23, 24]
```

Now reversing this and applying it on output string

```py
s1 = "rlstuwyc01f2i3m4eb56g789_oadhjnp"
s2 = "abcdefghijlmnoprstuwy0123456789_"

get_indexes = {}
for i in range(0,len(s)):
    get_indexes[s1[i]] = i

position_mapping = []
for i in s2:
    position_mapping.append(get_indexes[i])

print(position_mapping)

s3 = "1o1i_awlaw_aowsay3wa0awa!iJlooHi"
for i in position_mapping:
    print(s3[i],end="")
print()

# output: J3lly_0oooosHii11i_awawawawaawa!
```

And there you have it!

Flag: `vsctf{J3lly_0oooosHii11i_awawawawaawa!}`
