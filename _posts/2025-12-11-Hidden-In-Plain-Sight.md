---
title: "PicoCTF – Hidden In Plain Sight"
date: 2025-12-11 12:00:00 +0530
categories: [PicoCTF,Forensics]
tags: [forensics,exiftool,crytography,metadata,steghide,steganography]
image:
  path: /assets/img/headers/picoctf-banner.jpg
  alt: PicoCTF Banner
---

Challenge Author: Yahaya Meddy

Category: Forensics

Description
---

You’re given a seemingly ordinary JPG image. Something is tucked away out of sight inside the file. Your task is to discover the hidden payload and extract the flag.Download the jpg image [here](https://challenge-files.picoctf.net/c_amiable_citadel/6955bb058ab90c0306b3590016580809f527ff60d495fcee215a54f841bb9c32/img.jpg).

Process
---

1. Download the image from the Pico website.
2. The image appears to be a normal matrix-style image and does not visibly contain any information.
3. Next, I used ExifTool to examine the image metadata, which is often crucial in forensics CTFs.

    ```bash
    # Extract metadata from the image
    exiftool img.jpg
    ```

    ![Screenshot 2025-12-10 at 8.07.54 PM.png](/assets/img/Hidden-In-Plain-Sight-Photos/Screenshot_2025-12-10_at_8.07.54_PM.png)

4. From the metadata, the most odd one looks the comment part which appeared to be encoded in an unknown format, lets look what type it is encoded using the “CyberChef”.

    ![Screenshot 2025-12-10 at 8.08.07 PM.png](/assets/img/Hidden-In-Plain-Sight-Photos/Screenshot_2025-12-10_at_8.08.07_PM.png)

5. In the “CyberChef” website using the magic tool I found out that it was encoded using “base64” { You can also look at my [cheat sheet](https://sktr13055.github.io/references/know-your-bases/) to know various types of encoding}  additionally the output was revealed as.

    ```bash
    #Decoded Information
    steghide:cEF6endvcmQ=
    ```

6. This reveals additional clue which is the password for steghide (A tool for steganography) with the password which is again encoded in “base64”.

    ![Screenshot 2025-12-10 at 8.08.17 PM.png](/assets/img/Hidden-In-Plain-Sight-Photos/Screenshot_2025-12-10_at_8.08.17_PM.png)

7. Decoding with the base64 revealed the password which is -> {pAzzword} used for steghide tool.
8. Using the Steghide tool from the CLI to extract the hidden file.

    ```bash
    #Using the steghide tool to extract the flag
    steghide --extract -sf img.jpg 
    Enter Passphrase: pAzzword
    ```

    ![Screenshot 2025-12-10 at 8.07.30 PM.png](/assets/img/Hidden-In-Plain-Sight-Photos/Screenshot_2025-12-10_at_8.07.30_PM.png)

9. The output shows that the file “flag.txt” has been extracted.
 
    ```bash
    #Retrieveing the contents of the flag
    cat flag.txt
    ```

    ![Screenshot 2025-12-10 at 8.07.38 PM.png](/assets/img/Hidden-In-Plain-Sight-Photos/Screenshot_2025-12-10_at_8.07.38_PM.png)

10. Finally I got the flag for the hidden challenge.

```
picoCTF{h1dd3n_1n_1m4g3_f051f2e8}
```

Conclusion
--- 

This challenge demonstrates how important metadata analysis is in digital forensics. At first glance, the JPG image appeared completely ordinary, but a deeper inspection using exiftool revealed an unusual comment field. Decoding this metadata uncovered a hidden clue that ultimately led to the correct steghide passphrase.

By combining multiple forensic techniques—metadata extraction, encoding identification, base64 decoding, and steganographic analysis—the hidden payload was successfully extracted. This challenge reinforces a key lesson in CTF forensics: never trust what you see on the surface; valuable information is often hidden in plain sight.

--- 
Thank you for taking the time to read this write-up. I hope it clearly demonstrated the thought process, tools, and techniques used to solve this forensics challenge. Challenges like these highlight the importance of metadata analysis and steganography in CTFs.

Happy hacking, and see you in the next challenge 🚀🔍
