---
title: "PicoCTF – Corrupted File"
date: 2025-12-17 20:20:00 +0530
categories: [PicoCTF,Forensics]
tags: [forensics,xxd,magic_bytes,hex_values,xdg,hexedit]
image:
  path: /assets/img/headers/picoctf-banner.jpg
  alt: PicoCTF Banner
---

# Corrupted File

Challenge_Author: Yahaya Meddy

Category: Forensics

Description
---

This file seems broken... or is it? Maybe a couple of bytes could make all the difference. Can you figure out how to bring it back to life?
Download the file [here](https://challenge-files.picoctf.net/c_amiable_citadel/bdd976098377529fe779dbd31b424f69e51327b5ba68fd247dfcc074f0684141/file).

Process
---

1. Download the file from the Pico website or you can click  “here” to download the file.
2. After downloading the file, I checked its type of data using the file command.

    ```bash
      #Displaying the file format
      file *
    ```

    ![Screenshot 2025-12-17 at 7.03.26 PM.png](/assets/img/Corrupted-file-Photos/Screenshot_2025-12-17_at_7.03.26_PM.png)

3. Since the output mentioned only “data” the only thing I can do is to look at the raw bytes using the "xxd" command.

    ```bash
    #Displaying the hex values
    xxd file
    ```

    ![Screenshot 2025-12-17 at 7.04.09 PM.png](/assets/img/Corrupted-file-Photos/Screenshot_2025-12-17_at_7.04.09_PM.png)

4. Inspecting the starting bytes, I observed that it contains “JFIF” which is a part of the “JPEG” file format header.
5. Looking up on the internet, I found out that the first hex bytes were changed and which you can compare it in the [wikipedia](https://en.wikipedia.org/wiki/List_of_file_signatures).
6. Using “Hexedit” or “Hexed.it” to edit the hex values.

    ![Screenshot 2025-12-17 at 7.24.26 PM.png](/assets/img/Corrupted-file-Photos/Screenshot_2025-12-17_at_7.24.26_PM.png)

    ![Screenshot 2025-12-17 at 7.15.01 PM.png](/assets/img/Corrupted-file-Photos/Screenshot_2025-12-17_at_7.15.01_PM.png)

7. After changing the hex values, I exported the file.
8. Now inspecting the file type of the file once again to see any changes.

    ```bash
    #Checking the file type
    file *
    ```

    ![Screenshot 2025-12-17 at 7.14.25 PM.png](/assets/img/Corrupted-file-Photos/Screenshot_2025-12-17_at_7.14.25_PM.png)

9. Now the file type displays a “JPEG” file which is an image file.
10. Using the “xdg” command to display the image.

        ```bash
        xdg-open file # -> thats the file name you can rename it to file.jpg
        ```

       ![Screenshot 2025-12-17 at 7.14.46 PM.png](/assets/img/Corrupted-file-Photos/ce069b67-cf9f-42dd-a38a-cdc83d0b3a2a.png)

11. Finally the flag has been revealed.

    ```
    picoCTF{r3st0r1ng_th3_by73s_249e4e3c}
    ```


Conclusion
---

This challenge highlights the importance of understanding **file signatures and magic bytes** in digital forensics. Even when a file appears broken or unrecognizable, a quick hex inspection can reveal clues about its true format. By restoring just a few critical bytes, the file was brought back to life and the hidden information was successfully recovered.

This is a great example of how low-level file analysis plays a crucial role in forensic investigations and CTF challenges alike.


Thanks for Reading

Thank you for taking the time to read this write-up! I hope it was helpful and informative. Happy hacking, and good luck with your future PicoCTF challenges 🚀
