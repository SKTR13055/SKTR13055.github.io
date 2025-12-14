---
title: "PicoCTF – Riddle Regsitry"
date: 2025-12-08 12:00:00 +0530
categories: [PicoCTF,Forensics]
tags: [forensics,exiftool,hidden_pdf,metadata,picoctf]
image:
  path: /assets/img/headers/picoctf-banner.jpg
  alt: PicoCTF Banner
---

Description
--- 

Hi, intrepid investigator! 📄🔍 You've stumbled upon a peculiar PDF filled with what seems like nothing more than garbled nonsense. But beware! Not everything is as it appears. Amidst the chaos lies a hidden treasure—an elusive flag waiting to be uncovered.Find the PDF file here

[Hidden Confidential Document](https://challenge-files.picoctf.net/c_amiable_citadel/aa8b61ad4587052b730bc07906352e33d5a803f25708e2c8c115147039541a76/confidential.pdf) -> Challange file

and uncover the flag within the metadata.

Process
---

1. Click on the link to view the pdf. { In MacOs It will download automatically, where as in linux the pdf will open on the new tab}
    
    ![Screenshot 2025-12-08 at 2.34.57 PM.png](/assets/img/Riddle_Regsitry_Photos/Screenshot_2025-12-08_at_2.34.57_PM.png)
    
    ![Screenshot 2025-12-08 at 2.35.06 PM.png](/assets/img/Riddle_Regsitry_Photos/Screenshot_2025-12-08_at_2.35.06_PM.png)
    
2. Looking at these “blocked” messages through the “Developer Tools” revealed only one clue which is “The author have done a great and good job” while the rest is just bogus.
3. Download the file using the “wget” command and not through the “print” function { as this will strip out the information which is part of the challenge}

    ```bash
    wget https://challenge-files.picoctf.net/c_amiable_citadel/aa8b61ad4587052b730bc07906352e33d5a803f25708e2c8c115147039541a76/confidential.pdf
    ```

4. Using the “Exiftool” to reveal the meta data ( As mentioned in the CTF description) 

    ```bash
    Exiftool confidential.pdf
    ```

    Output

    ```
    ExifTool Version Number         : 13.36
    File Name                       : confidential.pdf
    Directory                       : .
    File Size                       : 183 kB
    File Modification Date/Time     : 2025:11:08 00:47:36+05:30
    File Access Date/Time           : 2025:12:08 12:41:50+05:30
    File Inode Change Date/Time     : 2025:12:08 12:41:50+05:30
    File Permissions                : -rw-rw-r--
    File Type                       : PDF
    File Type Extension             : pdf
    MIME Type                       : application/pdf
    PDF Version                     : 1.7
    Linearized                      : No
    Page Count                      : 1
    Producer                        : PyPDF2
    Author                          : cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9jYTc2YmJiMn0=
    ```

1. Since the clue mentioned that the “The Author have done a great and good job” we look at the value of the Author which is basically encoded in “Base64” {If you want to know more about encoding methods you can take a look at this [Cheat Sheet](https://sktr13055.github.io/references/know-your-bases/)}
2. Either Through Command Terminal or through the internet you can decode it.

```bash
echo "cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9jYTc2YmJiMn0=" > file.txt #You can give any file name you want 

base64 -d file.txt # Decoding through Base64

picoCTF{puzzl3d_m3tadata_f0und!_ca76bbb2} #The Flag
```

_Through Base64 website_

[https://www.base64decode.org/](https://www.base64decode.org/)

![Screenshot 2025-12-08 at 2.52.36 PM.png](/assets/img/Riddle_Regsitry_Photos/Screenshot_2025-12-08_at_2.52.36_PM.png)

The Flag
--- 

```
picoCTF{puzzl3d_m3tadata_f0und!_ca76bbb2}
```
