---
title: Break the Syntax 2025 writeup
date: 2025-05-11 19:26:02
thumbnail: '/images/BtS-CTF-2025/btsctf-banner.png'
tags:
    - "CTF"
    - "Forensics"
Category:
    - "CTF"
---

This is my writeup for Break the Syntax 2025 ! I got almost all the forensics challs, except for the last one (cmon i was so close 😭)

It was overall very stego oriented and guessy challs, but I had fun exploring new type of stuff.

## Forensics

### Monkey see

> 🙈

> File: "monkey-see.pcapng"

![image](images/BtS-CTF-2025/see1.png)

So there's some USB packets given, upon a lil research on this, u can find some helpful stuff like [usb HID](https://wiki.osdev.org/USB_Human_Interface_Devices) and also this super good writeup [kaizen 2018 writeup](https://abawazeeer.medium.com/kaizen-ctf-2018-reverse-engineer-usb-keystrok-from-pcap-file-2412351679f4) 

I figured out that the leftover capture data is what we need to look into, extract the information bits and transform it into readable format.

```bash
tshark -r monkey-see.pcapng -Tfields -Eseparator=, -e usb.capdata -Y 'usb.transfer_type == 0x01 && usb.dst == "host" && !(usb.capdata == 00:00:00:00:00:00:00:00)' | sed 's/://g' > usb.capdata
```

with some headscratching and tries, this is the one liner I used to extract the usb capture data which looks like this

![image](images/BtS-CTF-2025/see2.png)

now, as I mentioned the 2 links above, i made a small script for this

{% folding cyan::Script: %}
```py
KEY_CODES = {
    0x04: ['a', 'A'], 0x05: ['b', 'B'], 0x06: ['c', 'C'], 0x07: ['d', 'D'],
    0x08: ['e', 'E'], 0x09: ['f', 'F'], 0x0A: ['g', 'G'], 0x0B: ['h', 'H'],
    0x0C: ['i', 'I'], 0x0D: ['j', 'J'], 0x0E: ['k', 'K'], 0x0F: ['l', 'L'],
    0x10: ['m', 'M'], 0x11: ['n', 'N'], 0x12: ['o', 'O'], 0x13: ['p', 'P'],
    0x14: ['q', 'Q'], 0x15: ['r', 'R'], 0x16: ['s', 'S'], 0x17: ['t', 'T'],
    0x18: ['u', 'U'], 0x19: ['v', 'V'], 0x1A: ['w', 'W'], 0x1B: ['x', 'X'],
    0x1C: ['y', 'Y'], 0x1D: ['z', 'Z'], 0x1E: ['1', '!'], 0x1F: ['2', '@'],
    0x20: ['3', '#'], 0x21: ['4', '$'], 0x22: ['5', '%'], 0x23: ['6', '^'],
    0x24: ['7', '&'], 0x25: ['8', '*'], 0x26: ['9', '('], 0x27: ['0', ')'],
    0x28: ['\n', '\n'], 0x29: ['[ESC]', '[ESC]'], 0x2A: ['[BACKSPACE]', '[BACKSPACE]'],
    0x2B: ['\t', '\t'], 0x2C: [' ', ' '], 0x2D: ['-', '_'], 0x2E: ['=', '+'],
    0x2F: ['[', '{'], 0x30: [']', '}'], 0x31: ['\\', '|'], 0x32: ['#', '~'],
    0x33: [';', ':'], 0x34: ["'", '"'], 0x35: ['`', '~'], 0x36: [',', '<'],
    0x37: ['.', '>'], 0x38: ['/', '?'], 0x39: ['[CAPSLOCK]', '[CAPSLOCK]']
}

def decode_usb_file(filepath):
    result = []
    with open(filepath, 'r') as f:
        for line in f:
            data = line.strip()
            if len(data) < 8:
                continue

            modifier = int(data[0:2], 16)
            shift = int(data[2:4], 16) == 0x02
            keycode = int(data[6:8], 16)

            if keycode == 0:
                continue

            char_pair = KEY_CODES.get(keycode)
            if char_pair:
                result.append(char_pair[1] if shift else char_pair[0])
            else:
                result.append(f'[UNKNOWN:{hex(keycode)}]')

    return ''.join(result)

output = decode_usb_file(usb.capdata)
print("Decoded Output:\n", output)
```
{% endfolding %}

running this script we got a HUGE plain text wall, and in that the flag was present !

![image](images/BtS-CTF-2025/see3.png)

format it a lil and voila~

`Flag: BtSCTF{m0nk3y_tYpE!!1!!oneone!}`


### Chiroptera Timida

> Shall pass this childish brainrot and get the flag!

> File: "Bat_Song.wav"

This is a quick one using audacity:

- Open file in audacity

- View in Spectrogram

- Zoom in and see the flag

![image](images/BtS-CTF-2025/chiro1.png)

`Flag: BtSCTF{I_am_batman_I_can_hear_it}`


### monkey paint?

> It seems like our monkey has got some new special abilities 🐵

> File: "monkey-paint.pcapng"

This is another USB data file, so I ran the same logic and got some different results... or more like nothing lmao

well, this the the difference was the capture data

![image](images/BtS-CTF-2025/paint1.png)

Took me a while to figure out, but again [usb HID](https://wiki.osdev.org/USB_Human_Interface_Devices) helped in figuring out that it's usb mouse capture data

A lil bit of google search and i came across this [USB mouse pcap visualizer](https://github.com/WangYihang/USB-Mouse-Pcap-Visualizer) i mean 💀 the chall is joeover now

- git clone the repo

- setup the project

- run the cmd

```bash
poetry run python usb-mouse-pcap-visualizer.py -i monkey-paint.pcapng -o data.csv
```

- open run the csv in their website [mouse pcap visualizer](https://usb-mouse-pcap-visualizer.vercel.app/)

with a couple iterations to speed up the process I got these

![image](images/BtS-CTF-2025/paint2.png)

concatenate and u get the flag

`Flag: BtSCTF{yeah_it_does!11!}`

### copypasta

> I was moving one of the most relatable copypastas to me to a pendrive, but I think something went wrong during copying and pasting (hehe) and I can't open the file. To make matters worse, I forgot the password, but it should be one of those in a wordlist. Can you help me recover my favourite copypasta?

> Files: "copypasta.pdf" "wordlist.txt"

So we have a pdf file this time, firstly i cracked the password using `pdfcrack` and using the given wordlist

```bash
pdfcrack -w wordlist.txt copypasta.pdf
```

and i got the password `pumpkin`

I tried a lot of forensic methods but i couldn't figure out a way to get much info or fix it.

Cutting short from using `pdf-parser` and other many stuff, i found this website [pdf repair](https://www.freepdfconvert.com/repair-pdf) and it did the magic

![image](images/BtS-CTF-2025/copypasta1.png)

`Flag: BtSCTF{we_have_to_censor_that_one_and_another_one_and_finally_that_one}`

### Sus data

> We caught the suspect, but his pendrive contained only this data. What could it be?

> File: "Sus_data"

This time i got a data file and found no info even after using file info cmds & tools.

I opened it in `hexeditor` and obviously found that the file format is wrong.

This was kinda similar chall too the [picoctf corrupt one](https://github.com/Dvd848/CTFs/blob/master/2019_picoCTF/c0rrupt.md)

so I fixed the file headers, u can see the before and after

![image](images/BtS-CTF-2025/sus1.png)

well, the file still wasn't fixed, i made use of `pngcheck` to understand more about it

```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ pngcheck -v Sus_data                  
zlib warning:  different version (expected 1.2.13, using 1.3.1)

File: Sus_data (321007 bytes)
  chunk IHDR at offset 0x0000c, length 13
    500 x 500 image, 24-bit RGB, non-interlaced
  chunk sRGB at offset 0x00025, length 1
    rendering intent = perceptual
  chunk gAMA at offset 0x00032, length 4: 0.45455
  chunk pHYs at offset 0x00042, length 9: 3779x3779 pixels/meter (96 dpi)
  chunk IDAT at offset 0x00057, length 65445
    zlib: deflated, 32K window, fast compression
    zlib: inflate error = -3 (data error)
ERRORS DETECTED in Sus_data
```

I knew it's a CRC error, but I just couldn't figure out a way to fix it... ggs ⚰️

I won't leave it a cliffhanger ofc- 

After the ctf ended, **Tamer (@yousef15865)** on discord posted the solve on how to solve the chall, and I got the privilege to add his answer here!

The issue was that the IEND chunk's CRC ended up inside the IDAT chunk by mistake. This made the IDAT chunk 4 bytes longer than it should’ve been. Once we removed the misplaced IEND CRC from the IDAT chunk, the image should load correctly.

The script to fix it

```py
def remove_hex_pattern(file_path, hex_pattern):
    pattern_bytes = bytes.fromhex(hex_pattern)

    with open(file_path, 'rb') as f:
        content = f.read()

    modified_content = content.replace(pattern_bytes, b'')

    with open(file_path, 'wb') as f:
        f.write(modified_content)

    print(f"Pattern {hex_pattern} removed from {file_path}")

remove_hex_pattern('Sus_data', 'AE426082')  
```

and voila, u can open the image!

and again a huge thanks to **Tamer (@yousef15865)** for helping in completing this one.

![image](images/BtS-CTF-2025/sus2.png)

`Flag: BtSCTF{Hecker_Picasso_3175624}`

thanks for reading 🍞✨
