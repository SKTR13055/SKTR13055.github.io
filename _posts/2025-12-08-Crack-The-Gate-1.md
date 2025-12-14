---
title: "PicoCTF – Crack The Gate 1"
date: 2025-12-08 12:00:00 +0530
categories: [PicoCTF,Web Exploitation]
tags: [webexploitation,burpsuite,base64,cryptography,caesarcipher]
image:
  path: /assets/img/headers/picoctf-banner.jpg
  alt: PicoCTF Banner
---

# Crack the Gate 1


Challenge Author: Yahaya Meddy

Description
--- 

We’re in the middle of an investigation. One of our persons of interest, ctf player, is believed to be hiding sensitive data inside a restricted web portal. We’ve uncovered the email address he uses to log in: [ctf-player@picoctf.org](mailto:ctf-player@picoctf.org). Unfortunately, we don’t know the password, and the usual guessing techniques haven’t worked. But something feels off... it’s almost like the developer left a secret way in. Can you figure it out?

Additional details will be available after launching your challenge instance.

Process
--- 

1. Launch the instance from the PicoCTF Website.
2. After navigating to the website a login form will be displayed.
3. Using the email provided in the description with a common password failed to authenticate.

    ![Screenshot 2025-12-08 at 6.35.53 PM.png](/assets/img/Crack-the-Gate-1-Photos/Screenshot_2025-12-08_at_6.35.53_PM.png)

    ![Screenshot 2025-12-08 at 6.36.07 PM.png](/assets/img/Crack-the-Gate-1-Photos/Screenshot_2025-12-08_at_6.36.07_PM.png)

4. Looking at source code of the login form we can see that there is an “encoded message”.

    ![Screenshot 2025-12-08 at 6.36.16 PM.png](/assets/img/Crack-the-Gate-1-Photos/Screenshot_2025-12-08_at_6.36.16_PM.png)

5. Since the encoded message consisted only of shifted alphabetical characters, it was likely a Caesar cipher.
 
   ```
    NOTE: Jack - temporary bypass: use header “X-Dev-Access: yes”
   ```
   <aside>
    This indicates a hard-coded developer backdoor in the application. The server checks for a specific HTTP request header (X-Dev-Access) and, if present, bypasses normal authentication     logic entirely.
   </aside>
 
6. By this clue it means that we need add a “Request Header” which is the X-Dev-Access in the Request section which can be done in the Burpsuite.

    ![Screenshot 2025-12-08 at 6.35.27 PM.png](/assets/img/Crack-the-Gate-1-Photos/Screenshot_2025-12-08_at_6.35.27_PM.png)

7. After Opening and Capturing the request and inserting the “X-Dev-Access” as yes and sending it I got the response as “200 OK” which gave the flag.

```

Flag: picoCTF{brut4_f0rc4_cbb8faa7}

```

Conclusion:
--- 
This challenge demonstrates the dangers of leaving developer backdoors in production code. By inspecting client-side source code and modifying HTTP headers, authentication was bypassed without brute forcing the password.
