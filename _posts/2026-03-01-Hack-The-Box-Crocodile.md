---
title: "HackTheBox – Crocodile"
date: 2026-03-01 13:20:00 +0530
categories: [HackTheBox]
tags: [WebExploitation,Ftp_anon,Nmap,Burpsuite,ftp,Linux,OWASP_TOP_10]
image:
  path: /assets/img/headers/Hack-The-Box-Crocodile-Banner.png
  alt: Hack-The-Box-Crocodile-Banner
---

# Hack the Box - Crocodile

Machine : Linux

Process

1. After launching the machine, I first verified that it was active by pinging the target IP address to confirm connectivity
2. Next, I performed a port scan using **Nmap** to identify open ports:

    ```
    nmap <target-ip>
    ```

    ![Screenshot 2026-02-28 at 7.30.34 PM.png](/assets/img/Hack-the-Box-Crocodile-Photos/Screenshot_2026-02-28_at_7.30.34_PM.png)

3. The scan revealed two open ports:
    - **21 (FTP)**
    - **80 (HTTP)**
    To gather more detailed information about the services, I ran:

     ```
      nmap -sC -sV <target-ip>
      ```

    - `sC` runs default NSE scripts
    - `sV` detects service versions

        ![Screenshot 2026-02-28 at 7.31.02 PM.png](/assets/img/Hack-the-Box-Crocodile-Photos/Screenshot_2026-02-28_at_7.31.02_PM.png)

4. The detailed scan revealed:
    - FTP service running **vsftpd 3.0.3**
    - HTTP service running **Apache httpd 2.4.41**
    - Anonymous FTP login was enabled

    The FTP server returned code **230**, confirming that anonymous login was successful.

    This configuration aligns with **Security Misconfiguration** under the **OWASP Top 10:2025 (A02)**, as sensitive access was exposed due to improper configuration.

5. I connected to the FTP server using:

    ```
    ftp <target-ip>
    ```

    When prompted for credentials, I entered:

    ```
    Username: anonymousPassword: anonymous
    ```

    After logging in, I listed the directory contents and found two files:

    ![Screenshot 2026-02-28 at 7.32.43 PM.png](/assets/img/Hack-the-Box-Crocodile-Photos/Screenshot_2026-02-28_at_7.32.43_PM.png)

    To download the files, I used:

    ```
    get <filename>
    ```

    ![Screenshot 2026-02-28 at 7.33.22 PM.png](/assets/img/Hack-the-Box-Crocodile-Photos/Screenshot_2026-02-28_at_7.33.22_PM.png)

    ![Screenshot 2026-02-28 at 7.33.48 PM.png](/assets/img/Hack-the-Box-Crocodile-Photos/Screenshot_2026-02-28_at_7.33.48_PM.png)

6. Upon inspecting the contents of both files, I discovered usernames and corresponding passwords stored in plaintext. One of the higher-privileged-sounding usernames identified was: “admin”. Storing credentials in plaintext maps to **A04:2025 – Cryptographic Failures**, since sensitive data was not properly protected.
7. Next, I moved to the HTTP service. The website appeared to be a standard e-commerce page with no obvious vulnerabilities. Since the challenge referenced a PHP file, I performed directory brute forcing using **Gobuster**. Because I was specifically looking for PHP files, I used the `-x` switch:

    ![Screenshot 2026-02-28 at 7.44.30 PM.png](/assets/img/Hack-the-Box-Crocodile-Photos/Screenshot_2026-02-28_at_7.44.30_PM.png)

8. After enumerating the directory I found out that “/login.php”. This endpoint appeared to handle authentication.
9. I attempted to authenticate to `/login.php` using the credentials obtained from the FTP server. To analyze login behavior more closely, I intercepted the request using **Burp Suite**.

    ![Screenshot 2026-02-28 at 7.44.50 PM.png](/assets/img/Hack-the-Box-Crocodile-Photos/Screenshot_2026-02-28_at_7.44.50_PM.png)

10. During testing, I observed:
    - Failed login attempts returned HTTP **200 OK**
    - A successful login returned HTTP **302 Found**

    The 302 status code indicated a redirect, confirming successful authentication.

    After providing the correct username and password, I successfully logged in and retrieved the root flag.


Tasks
--- 
- Task 1
    
    What Nmap scanning switch employs the use of default scripts during a scan?
    
    → -sC (Script)
    
- Task 2
    
    What service version is found to be running on port 21?
    
    → vsftpd 3.0.3
    
- Task 3
    
    What FTP code is returned to us for the "Anonymous FTP login allowed" message?
    
    → 230
    
- Task 4
    
    After connecting to the FTP server using the ftp client, what username do we provide when prompted to log in anonymously?
    
    → anonymous
    
- Task 5
    
    After connecting to the FTP server anonymously, what command can we use to download the files we find on the FTP server?
    
    → get
    
- Task 6
    
    What is one of the higher-privilege sounding usernames in 'allowed.userlist' that we download from the FTP server?
    
    → admin
    
- Task 7
    
    What version of Apache HTTP Server is running on the target host?
    
    → Apache httpd 2.4.41
    
- Task 8
    
    What switch can we use with Gobuster to specify we are looking for specific filetypes?
    
    → -x
    
- Task 9
    
    Which PHP file can we identify with directory brute force that will provide the opportunity to authenticate to the web service?
    
    → login.php
    

Conclusion
---

The Crocodile machine demonstrates how seemingly minor security weaknesses can be chained together to achieve full system compromise. The combination of anonymous FTP access, exposed plaintext credentials, and weak authentication controls created a clear attack path from initial enumeration to successful login.

Although categorized as a “Very Easy” machine on Hack The Box, it effectively reinforces core penetration testing fundamentals: thorough enumeration, careful analysis of exposed services, and logical chaining of vulnerabilities. This challenge highlights how misconfigurations and poor credential management remain common and impactful security issues in real-world environments.

---

Key Takeaways
---

- Always enumerate **all open ports** — not just web services.
- Misconfigurations (e.g., anonymous FTP) can expose sensitive data.
- Credentials stored in plaintext represent a critical security failure.
- Authentication behavior (e.g., HTTP status codes) can reveal success or failure states.
- Small findings can often be chained into full compromise.

This machine reinforces the importance of methodical testing and not overlooking basic security hygiene issues.

Vulnerability Classification (OWASP Top 10:2025)
---

Mapped according to the **OWASP Top 10:2025** framework:

- **A02:2025 – Security Misconfiguration**
    
    Anonymous FTP access allowed unauthorized file retrieval.
    
- **A04:2025 – Cryptographic Failures**
    
    User credentials were stored in plaintext without hashing or encryption.
    
- **A07:2025 – Authentication Failures**
    
    Weak authentication controls enabled credential reuse and predictable login responses.
    

(Official documentation available on the OWASP Top 10 project page.)

---

Thank you for taking the time to read this write-up. I hope it clearly demonstrates both the technical steps taken and the security reasoning behind them.

If you have any feedback or suggestions for improvement, they are always welcome.
