---
title: "PicoCTF – Flag-In-Flame"
date: 2025-12-12 12:00:00 +0530
categories: [PicoCTF,Forensics]
tags: [forensics,base64,steganography,cyberchef,cryptography]
image:
  path: /assets/img/headers/picoctf-banner.jpg
  alt: PicoCTF Banner
---


Challenge_Author: Prince Niyonshuti N.

Category: Forensics

Description
---

The SOC team discovered a suspiciously large log file after a recent breach. When they opened it, they found an enormous block of encoded text instead of typical logs. Could there be something hidden within? Your mission is to inspect the resulting file and reveal the real purpose of it. The team is relying on your skills to uncover any concealed information within this unusual log.Download the encoded data here: [Logs Data](https://challenge-files.picoctf.net/c_amiable_citadel/3c1d1fea48e203c9c5d64c32d94aa1f091a6b72b4cebd35a761b09d4f9c0f0d2/logs.txt). Be prepared—the file is large, and examining it thoroughly is crucial.


Process
---

1. The encoded log file was downloaded from the PicoCTF website
2. The contents of the file were initially inspected using the `cat` command, revealing a very large block of encoded data rather than readable logs.

    ![Screenshot 2025-12-10 at 8.57.35 PM.png](/assets/img/Flag-in-Flame-Photos/Screenshot_2025-12-10_at_8.57.35_PM.png)

3. Based on the structure and length of the data, it appeared to be Base64-encoded, as suggested in the challenge description.
4. The entire file was decoded using the Base64 utility, and the output was redirected to a new file.

    ```bash
    #Decoding the log file using base64 CLI tool
    base64 -d logs.txt > output.txt
    ```

    ![Screenshot 2025-12-10 at 8.57.56 PM.png](/assets/img/Flag-in-Flame-Photos/6f72c160-51bf-4794-bc3b-69b2ca259641.png)

5. After decoding, the `file` command was used to identify the type of the resulting file.

    ```bash
    #Displaying the format of the file
    file *
    ```

    ![Screenshot 2025-12-10 at 8.58.14 PM.png](/assets/img/Flag-in-Flame-Photos/Screenshot_2025-12-10_at_8.58.14_PM.png)

6. The output indicated that the decoded data was a PNG image file. The file was then renamed accordingly.

    ```bash
    #Renaming the file 
    mv output.txt image.png
    ```

    ![Screenshot 2025-12-10 at 9.02.05 PM.png](/assets/img/Flag-in-Flame-Photos/Screenshot_2025-12-10_at_9.02.05_PM.png)

    ![Screenshot 2025-12-10 at 9.02.08 PM.png](/assets/img/Flag-in-Flame-Photos/Screenshot_2025-12-10_at_9.02.08_PM.png)

7. The image was opened for inspection using the `xdg-open` command.

    ```bash
    #Opening the image
    xdg-open image.png
    ```

    ![Screenshot 2025-12-10 at 8.33.26 PM.png](/assets/img/Flag-in-Flame-Photos/Screenshot_2025-12-10_at_8.33.26_PM.png)

8. Upon viewing the image, I observed that it contained another long encoded string embedded within the image itself.
9. The encoded text was extracted from the image using optical text recognition (OCR) assistance.

    ![Screenshot 2025-12-10 at 8.39.53 PM.png](/assets/img/Flag-in-Flame-Photos/Screenshot_2025-12-10_at_8.39.53_PM.png)

10. The extracted text was then analyzed and decoded using CyberChef, which revealed the final flag.

    ```
    #Flag
    picoCTF{forensics_analysis_is_amazing_5daa4a2f}
    ```


Conclusion
---

This challenge demonstrated how seemingly ordinary files can conceal entirely different data types through encoding. By carefully inspecting the contents of the log file, identifying the Base64 encoding, and validating the decoded output using file analysis tools, the true nature of the data was revealed. The discovery of an embedded image containing another layer of encoded information reinforced the importance of thorough, step-by-step analysis in digital forensics.

This challenge emphasized key forensic principles such as verifying file formats, recognizing common encodings, and handling multi-layer data obfuscation—skills that are essential when investigating real-world security incidents.

--- 
Thank you for taking the time to read this write-up. I hope it clearly outlined the methodology and tools used to solve this forensics challenge. Stay curious, keep practicing, and happy hacking 🚀🔍
