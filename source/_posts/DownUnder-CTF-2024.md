---
title: 'DownUnder CTF 2024 writeup'
thumbnail: '/images/downunder_CTF_2024/ductf.png'
date: 2024-7-8
tags:
  - 'CTF'
  - 'Forensics'
  - 'Misc'
category:
    - "CTF"
---
    
My writeup on DownUnder CTF 2024, it was a big and competitive CTF but still really fun and a lot of learning! My focus was more on the Forensics and Misc Section this time.

---

## Forensics:

### Baby's First Forensics

> They've been trying to breach our infrastructure all morning! They're trying to get more info on our covert kangaroos! We need your help, we've captured some traffic of them attacking us, can you tell us what tool they were using and its version?

> NOTE: Wrap your answer in the `DUCTF{}`, e.g. `DUCTF{nmap_7.25}`

> File: "capture.pcap"

So I need to find the tool which they are using, for this I'll start up my wireshark for analysing the pcap file.

![image](images/downunder_CTF_2024/ductf_bff1.png)

Checking the Statistics/Conversations, they are mostly TCP packets so need to narrow down the search to those.

![image](images/downunder_CTF_2024/ductf_bff2.png)

Following the TCP stream, at the start, `User-Agent:` header mentions `(Nikto/2.1.6)` that's version of something, looking into it. It's actually a vulnerability scanner tool.

So that's the answer! Wrapping it in format, we get the flag.

Flag: `DUCTF{Nikto_2.1.6}`

### SAM I AM

> The attacker managed to gain Domain Admin on our rebels Domain Controller! Looks like they managed to log on with an account using WMI and dumped some files.

> Can you reproduce how they got the Administrator's Password with the artifacts provided?

> File: "samiam.zip"

On unzipping the given file, we have `sam.bak` and `system.bak` file to work with. We need to find the Administrator password with the given artifacts.

For this, I'll use **impacket** `impacket-secretsdump` file which finds secrets from the artifacts.

![image](images/downunder_CTF_2024/ductf_sam1.png)

We got the Administrator password hash, now this can be decoded using **Hashcat** or [crackstation.net](https://crackstation.net/)

![image](images/downunder_CTF_2024/ductf_sam2.png)

And we got the password!

Flag: `DUCTF{!checkerboard1}`

### Bad Policies

> Looks like the attacker managed to access the rebels Domain Controller.

> Can you figure out how they got access after pulling these artifacts from one of our Outpost machines?

> File: "badpolicies.zip"

On opening the zip file, we are given a file directory with Group policy files in it. 

![image](images/downunder_CTF_2024/ductf_bad1.png)

Upon analyzing the files, we can find a cpassword in the `Groups.xml` file.

Upon a little google search about this, it can be decrypted using `gpp-decrypt` in kali.

![image](images/downunder_CTF_2024/ductf_bad2.png)

And that's the flag!

Flag: `DUCTF{D0n7_Us3_P4s5w0rds_1n_Gr0up_P0l1cy}`

---

## Misc:

### Wacky Recipe

> Our Cyber Chef has been creating some wacky recipes recently, though he has been rather protective of his secret ingredients. Use this Chicken Parmi recipe and decipher the missing values to discover the chef's secret ingredient!

> File: "recipe.txt"

{% folding cyan::Recipe: %}

Chicken Parmi.

Our Cyber Chef has been creating some wacky recipes recently, though he has been rather protective of his secret ingredients.
Use this Chicken Parmi recipe and decipher the missing values to discover the chef's secret ingredient!
This recipe produces the flag in flag format.

Ingredients.
?? dashes pain
?? cups effort
1 cup water
4 kg bread crumbs
26 ml hot canola oil
13 kg egg yolks
24 teaspoons all purpose spices
7 teaspoons herbs
26 kg flour
26 kg sliced chicken breasts
1 dashes salt
11 dashes pepper
7 dashes pride and joy
10 kg tomato sauce
14 g cheese
13 kg ham
2 g pasta sauce
6 dashes chilli flakes
5 kg onion
9 dashes basil
19 dashes oregano
10 dashes parsley
20 teaspoons sugar

Cooking time: 25 minutes.

Pre-heat oven to 180 degrees Celsius.

Method.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Remove bread crumbs from 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Add hot canola oil to 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Remove egg yolks from 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Remove all purpose spices from 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Add herbs to 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Add flour to 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Remove sliced chicken breasts from 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Remove salt from 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Add pepper to 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Remove pride and joy from 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Add tomato sauce to 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Remove cheese from 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Remove ham from 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Combine pain into 1st mixing bowl.
Add pasta sauce to 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Remove chilli flakes from 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Remove onion from 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Add basil to 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Combine pain into 1st mixing bowl.
Add oregano to 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Add water to 1st mixing bowl.
Combine pain into 1st mixing bowl.
Add parsley to 1st mixing bowl.
Add effort to 1st mixing bowl.
Put water into 1st mixing bowl.
Combine pain into 1st mixing bowl.
Add sugar to 1st mixing bowl.
Add effort to 1st mixing bowl.
Liquefy contents of the mixing bowl.
Pour contents of the mixing bowl into the 1st baking dish.
Refrigerate for 1 hour.

{% endfolding %}

We are given a recipe of Chicken Parmi and somehow need to decrypt this and obtain the flag.

We can see that there's missing value of "pain" and "effort".
Upon searching for a while, I found that this is actually a programming language called **Chef**.

Reading through how this language works, [source](https://www.dangermouse.net/esoteric/chef.html) it's a stack-based language where programs look like cooking recipes.

There's 4 things to note

1) Put means push on top of the stack

2) Add means "+" operation with top of stack element

3) Remove means "-" operation with top of stack element

4) Combine means "*" operation with top of stack element

With this knowledge, we can assume some value to assign to pain and effort and run the program in [IDE](https://esolangpark.vercel.app/ide/chef)

We can try some ascii values indexes, first assuming 10 and 11

![image](images/downunder_CTF_2024/ductf_chef1.png)

mmm, we get something which are symbols and numbers, so maybe try some characters indexes.

Let's try 75 and 80 which are 'K' and 'P'

![image](images/downunder_CTF_2024/ductf_chef2.png)

Uh now we are out of alphanumeric values...

Let's try 30 and 35

![image](images/downunder_CTF_2024/ductf_chef3.png)

Oh, seeing alphabets is progress! i played around a little bit to tried a few more numbers

Using 27 and 21 got us the flag

![image](images/downunder_CTF_2024/ductf_chef4.png)

Flag: `DUCTF{2tsp_Vegemite}`
