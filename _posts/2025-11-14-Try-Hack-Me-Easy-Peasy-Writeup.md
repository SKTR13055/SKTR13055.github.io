---
title: "TryHackMe - Easy-Peasy"
date: 2025-11-14 10:00:00 +0530
categories: [TryHackMe]   
tags: [boot2root, linux, privilege escalation, web]
image:
  path: assets/img/headers/TryHackMe-Easy-Peasy.png
---
# Try Hack Me - Easy Peasy Writeup

![Screenshot 2025-11-10 at 8.38.35 PM.png](/assets/img/easy-peasy/Screenshot_2025-11-10_at_8.38.35_PM.png)

# Summary

**Web Enumeration • Hash Cracking • Steganography • Privilege Escalation**

This write-up documents my process for completing the **Easy Peasy** room on TryHackMe.

My goal was to improve my enumeration skills, learn new decoding techniques, and practice privilege escalation involving cronjobs.

---

# Aim

---

Practice using tools such as Nmap and GoBuster to locate a hidden directory to get initial access to a vulnerable machine. Then escalate your privileges through a vulnerable cronjob.

# Objective

---

**Learning objective**

- To further learn about the Nmap tool.
- Enumerate Hidden Directories using the Gobuster tool.
- Escalate Privileges using a vulnerable cronjob.

# Procedure

---

***Note:** Sometimes if you notice an IP address change don’t worry that was due to the time limit of the VM was over.*

## Task 1: Enumeration through Nmap

Start up the machine and answer the following questions.

<aside>

### 1. How Many ports are open?
</aside>

1. In order to find out how many ports were you can use the nmap tool{ Since nmap was taking to much time I’ve used Rustscan in order to enumerate ports much faster}

    ![Rustscan output report](/assets/img/easy-peasy/Screenshot_2025-11-10_at_8.49.35_PM.png)


*The command for nmap is to display all the ports*
```bash
“nmap -p- <ip address>”
```

  ![Nmap output report](/assets/img/easy-peasy/Screenshot_2025-11-10_at_8.51.49_PM.png)


(Mistake 1:- Before entering the correct output I was using this command “nmap -sT <ipaddress>, which gave me only 1 open port which was wrong )

<aside>

✅ The answer to the first question is  “3” Open Ports”

</aside>

---

<aside>

### 2. What is the version of nginx?
</aside>

1. In order to determine the version of the nginx we can use the following command, this is will display the version of the nginx.
   
   ```bash
   nmap -p80 -sV 10.49.151.176
   ```

    ![Screenshot 2025-11-10 at 8.54.07 PM.png](/assets/img/easy-peasy/Screenshot_2025-11-10_at_8.54.07_PM.png)

<aside>

✅ The answer to the second question is “1.16.1”

</aside>

---

<aside>

### 3. What is running on the highest port?
</aside>

1. From the 1st task output I observe that the highest port number is “65524” now to know what service is running we can use this command.
   ```bash
   “sudo nmap -sV 10.49.151.176 -p65524”
   ```

      ![ Output of service of the highest port.](/assets/img/easy-peasy/Screenshot_2025-11-10_at_8.58.18_PM.png)

 
2. When I first entered the answer as “http” I got the answer as wrong since I thought that was the service, but later on the answer was “Apache”.

<aside>

✅ The Answer to the third question is “Apache”

</aside>

### This ends first task of this room

- I came to know later on all of the information in the first task can be done using a simple command which was
  ```bash
  sudo nmap -sCV -p80,6498,65524 10.49.151.176
  ```
   This command would basically solve the last 2 questions easily instead of giving a simple command (noted)
--- 

## Task 2: Compromising the Machine.

Now you've enumerated the machine, answer questions and compromise it!

<aside>

### 1. Using GoBuster, find flag 1?
</aside>

1. Let’s use the GoBuster on the first port which is the port 80 using the command.
  ```bash
    “gobuster dir  -u 10.49.177.158  -w /usr/share/dirb/wordslists/common.txt”
  ```

   ![Output of GoBuster on port 80](/assets/img/easy-peasy/Screenshot_2025-11-11_at_8.05.09_PM.png)

2. From the output there is a hidden directory was discovered which is the “/whatever” directory, lets go to that directory and find it out.

     ![Hidden directory ](/assets/img/easy-peasy/Screenshot_2025-11-11_at_8.06.50_PM.png)

     ![Screenshot 2025-11-11 at 8.07.03 PM.png](/assets/img/easy-peasy/Screenshot_2025-11-11_at_8.07.03_PM.png)

3. Looking in to the source code of this page there is a “hidden piece of information” which is encoded in base64.
4. Lets Decode it using the “Cyber Chef” website.

     ![Decoded Information from the Cyber Chef Website](/assets/img/easy-peasy/Screenshot_2025-11-11_at_8.07.16_PM.png)

5. After the message has been decoded I found the first flag which is “flag{f1rs7_fl4g}”

<aside>

✅ The answer to the first question is “flag{f1rs7_fl4g}”

</aside>

---

<aside>

### 2.Further enumerate the machine, what is flag 2?

</aside>

1. Since we enumerated on port 80, now lets use gobuster to enumerate on port 65524.
   ```bash
   “gobuster dir -u  10.49.177.158:65524  -w /usr/share/dirb/wordslists/common.txt”
   ```
2. After the output was displayed I found that there is a another directory which is present which is the /robots.txt ( Sorry For not capturing the Screen shot of the output)
3. I went inside to that directory and found this information.( This page basicallys tell what crawlers can find the directories and display it on the search engine results.)
4. I looked closely in the User-Agent section which was again an information and it looked some kind of hash.

      ![Output for the /robots.txt](/assets/img/easy-peasy/Screenshot_2025-11-11_at_8.12.24_PM.png)

5. In order to verify that it was a hash I quickly opened up a “hash identifier” website and pasted this information and the result was just as I expected it was an “**MD5 Hash”.**
6. I used tools such as hash cat to crack the hash but I failed and tried to find other websites which can crack the hashes luckily I found a website which was able to.
7. I inserted the hash code to decode it and after some time to process I got the output which was a flag. ([https://md5hashing.net/hash](https://md5hashing.net/hash))

      ![Decoded Hash](/assets/img/easy-peasy/Screenshot_2025-11-11_at_8.50.06_PM.png)

<aside>

✅ The answer to the second question is “flag{1m_s3c0nd_fl4g}”

</aside>

---

<aside>

### 3. Crack the hash with easypeasy.txt, What is the flag 3?
</aside>

*Note: This part took me a lot of time, because this challenge was somehow misleading.*

1. At first I’ve used various tools(hashcat, johntheripper) to crack the hash along with the word lists which was given(easypeasy.txt) but I failed to retrieve it, after spending lots of hours I was not able to retrieve it.
2. I once again looked at the “index.html” page (Which is the default page) and apparently there was a flag which was hiding in the plain sight.

    ![Hidden 3rd flag](/assets/img/easy-peasy/Screenshot_2025-11-13_at_7.26.57_PM.png)

3. I gave this flag as the answer and it was the correct answer, there might some context behind this challenge but I find this question to be somewhat misleading.

<aside>

✅ The answer to the third question is flag{9fdafbd64c47471a8f54cd3fc64cd312}

</aside>

---

<aside>

### 4. What is the hidden directory?

</aside>

1. The hidden directory was also a tricky part because I wasn’t sure where to look at I kept using GoBuster to find out the hidden directory which I failed again.
2. Remembering the same thing from the previous questions, I asked “what if the flag is somewhere in the main page like the previous question”.

    ![Source code of the index.html](/assets/img/easy-peasy/Screenshot_2025-11-13_at_8.07.39_PM.png)

3. Looking at the source code of the index.html once again, I observed that there is indeed again a hidden information but this time the information is semi-complete.
4. Since half of the message says that “it is encoded with ba” that surely means it is encoded with one of the “base encoding method” and according to my cheat sheet(which I created for other CTF purposes) it is encoded in “base62”.

    ![Cyber Chef Flag 4 answer](/assets/img/easy-peasy/Screenshot_2025-11-13_at_8.07.54_PM.png)

5. After inserting the encoded hash in the “cyber chef” website and giving the recipe as “base62” we got the output as a “hidden directory”.

<aside>

✅ The answer to the fourth question is the “/n0th1ng3ls3m4tt3r”

</aside>

---

<aside>

### 5. Using the wordlist that provided to you in this task crack the hash what is the password?

</aside>

1. Now here it is asking you to crack the hash using the “easypeasy.txt” word list which was identical to the third question ( Yes we have to actually use the hashcat to crack the hash this time.)
2. Now it is asking to crack the hash, you may be asking where is the hash? that’s where you have to find luckily I got the idea on where to find it.
3. From the previous question the answer was a hidden directory (/n0th1ng3ls3m4tt3r).
4. I Immediately went to this hidden directory and found out it exists.

     ![Hidden Directory page](/assets/img/easy-peasy/Screenshot_2025-11-13_at_8.17.02_PM.png)

5. At first the image seems to be showing a matrix style background but it didn’t contain some kind of hash displayed in front so lets look at the source code of this page.

     ![Source Code](/assets/img/easy-peasy/Screenshot_2025-11-13_at_8.17.16_PM.png)

6. After looking in the source code there is again a hidden message encoded in some manner that I’ve never seen.
7. I used “cyber chef” in order to determine the encoded message.

     ![Analyzing the Encoded message](/assets/img/easy-peasy/Screenshot_2025-11-13_at_8.25.38_PM.png)

8. I analyzed and tried other hashing methods in a sequence order (which are displayed in the output of cyberchef) using the johntheripper tool.
9. Except one hashing method others were a complete failure, I used this command in the john ripper tool
```bash
“john - -wordlist=easypeasy.txt - -format==gost hash.txt”
```
  ![Screenshot 2025-11-13 at 8.27.35 PM.png](/assets/img/easy-peasy/Screenshot_2025-11-13_at_8.27.35_PM.png)

10. Finally the decoded hash message is the “mypasswordforthatjob” and submit it as the answer.

<aside>

✅The answer to the fifth question is “mypasswordforthatjob”

</aside>

---

<aside>

### 6. What is the password to login to the machine via SSH?
</aside>

1. One more thing I noticed in the previous question is that the source code contain an image which is the “binarycodepixabay.jpg” after clicking on it, it lead me to the actual source of the image, more like a downloaded image which was stored in the server.
2. After opening the image I could not see the source code but just download the image, after downloading the image and looking at the “meta data” I was still not able to found any clue.
3. Then I tested a technique which was “steganography”{message hidden in the image, audio etc} using the tool “steghide”.

 4. I used the tool steghide to reveal information from the image.  
```bash
steghide extract -sf image.jpeg { `-sf` (or `--stegofile`)
```
   the flag specifying the "stego file" (the cover file that contains the hidden data). and the passphrase was “mypasswordforthatjob”.
    ![Screenshot 2025-11-13 at 8.53.45 PM.png](/assets/img/easy-peasy/Screenshot_2025-11-13_at_8.53.45_PM.png)

5. A “secrettext.txt” was extracted and lets display the contents of it.
6. Type in the following command to display the content.
  ```bash
  “cat secrettext.txt”
  ```
   ![Screenshot 2025-11-13 at 8.54.07 PM.png](/assets/img/easy-peasy/Screenshot_2025-11-13_at_8.54.07_PM.png)

7. We found the username aswell as password but somehow it is in binary lets again use the cyberchef to decode the message.

    ![Screenshot 2025-11-13 at 8.55.09 PM.png](/assets/img/easy-peasy/Screenshot_2025-11-13_at_8.55.09_PM.png)

8. The password was finally revealed and it is “iconvertedmypasswordtobinary” and submit it as the answer.

<aside>

✅The answer to the sixth question is the “iconvertedmypasswordtobinary”

</aside>

---

<aside>

### 7. What is the user flag?
</aside>

1. This part was the easiest one just login in to the ssh using the ssh command. 
  ```bash
  “ssh -p6498 -l boring 10.49.177.158
  ```
the password is “iconvertedmypasswordtobinary”

   ![Logging into the “boring” using ssh on port 6498](/assets/img/easy-peasy/Screenshot_2025-11-13_at_8.57.38_PM.png)

   ![Screenshot 2025-11-13 at 8.57.58 PM.png](/assets/img/easy-peasy/Screenshot_2025-11-13_at_8.57.58_PM.png)

2. I displayed the list of files using “ls” command  and found “user.txt” file, using the cat command to displayed the contents of the file, which is somehow a kind of flag which is rotated and I immediately went to cyber chef  and use the ROT13 decoding method (aka Caesar Cipher Method)

    ![Screenshot 2025-11-13 at 8.58.50 PM.png](/assets/img/easy-peasy/Screenshot_2025-11-13_at_8.58.50_PM.png)

3. Turns out I was right it was encoded in the Caesar Cipher method and got the flag and submit it as the answer.

<aside>

✅The answer for the seventh question is “flag{n0wits33msn0rm4l}”

</aside>

---

<aside>

### 8. What is the root flag? (Final flag)

</aside>

*Note: This part was a bit hard for me because this part involves cron and privilege escalation so I took some help from other people and learned from it.*

1. After retrieving the display the contents from the user.txt the next is to get the flag from the root.
2. Now I was pretty oblivious about the crontab so I had to go there through some help, the cron tab is basically located in this location /etc directory, use the command to display the contents of the cron tab “cat crontab” or you can just simply type in from the home location like this “cat /etc/crontab”. 
    
      ![Screenshot 2025-11-13 at 9.22.14 PM.png](/assets/img/easy-peasy/Screenshot_2025-11-13_at_9.22.14_PM.png)
    

3. As far as I know the cron tab is basically the number of times the command will be executed during the uptime of the operating system(it can be  set seconds,minutes,hours,weeks,months,year)
4. Now I Observed that there is a hidden file called “.mysecretcronjob.sh” and where it is located it is in cd /var/www, lets go into that directory using the “cd” command.
5. After going to that directory, I used “nano” file editor to see the contents of the file.

      ![Screenshot 2025-11-13 at 9.23.12 PM.png](/assets/img/easy-peasy/Screenshot_2025-11-13_at_9.23.12_PM.png)

6. From the contents I saw that it will start as a bash shell and then it will run as “root” user.
7. Now this where you have to apply a privilege escalation technique( *note: I’ve heard about the privilege escalation but I actually never performed it, so this would be first learning of privilege escalation, so in this step I took help from the internet)*
8. Now from the research which I’ve done is that they are performing “[reverse shell](https://www.revshells.com/)”{you can also check out the website for this.
9. From the website I gave an IP & port number{This port number is where it will connect to us} and they payload was given and pasted in the file {You can see the payload in the picture below} and then we are changing the permissions of the file by giving the S-bit {That’s another story for another time}.

     ![Screenshot 2025-11-13 at 9.23.01 PM.png](/assets/img/easy-peasy/Screenshot_2025-11-13_at_9.23.01_PM.png)

10. Now typing in the command nc -lvnp 6499” will start the “reverse shell”.

    ![Screenshot 2025-11-13 at 9.23.52 PM.png](/assets/img/easy-peasy/Screenshot_2025-11-13_at_9.23.52_PM.png)

11. From the picture, there was a successful connection and we can perform some the bash commands.
12. If you type in the command “whoami” you will get the answer as the “root”(which is basically what we want) and lets change the directory to the “root” and display the list of the files/hidden files.
13. After displaying the list of the files/hidden files, I observed that there is a hidden file called “.root.txt” in which it contains the flag and using the “cat” command to display it.

    ![Screenshot 2025-11-13 at 9.21.14 PM.png](/assets/img/easy-peasy/Screenshot_2025-11-13_at_9.21.14_PM.png)

14. Finally I got the final flag which is then submitted as the answer and I’ve successfully completed this room.

<aside>

The answer for the eighth question is “flag{63a9f0eabb98050796b649e85481845}”

</aside>

   ![Screenshot 2025-11-13 at 9.24.13 PM.png](/assets/img/easy-peasy/Screenshot_2025-11-13_at_9.24.13_PM.png)

# Results

---

- Successfully able to enumerate ports using nmap and rustscan.
- Enumerated the list of hidden directories using Gobuster.
- Able to decode hashes using CyberChef,John the Ripper.

# 🏁 Final Results

| Flag | Value |
| --- | --- |
| **Flag 1** | flag{f1rs7_fl4g} |
| **Flag 2** | flag{1m_s3c0nd_fl4g} |
| **Flag 3** | flag{9fdafbd64c47471a8f54cd3fc64cd312} |
| **User Flag** | flag{n0wits33msn0rm4l} |
| **Root Flag** | flag{63a9f0eabb98050796b649e85481845} |

--- 

# Learnings

 ✔️ **Enumeration**

Rustscan + Nmap combinations, and how to quickly spot unusual services.

 ✔️ **Web directory discovery**

Gobuster approaches for different ports.

 ✔️** Encoding & decoding**

Base64, Base62, Caesar shift, binary decoding.

 ✔️ **Hash cracking**

Using John’s different formats, identifying GOST hashes.

 ✔️ **Steganography**

Extracting embedded data from images using Steghide.

 ✔️ **Privilege escalation**

Identifying vulnerable cronjobs and exploiting them with a reverse shell.

Overall, this challenge helped me strengthen my approach to **CTF problem-solving**, especially combining multiple techniques toward a final exploit.
