---
title: "TryHackMe - Sakura"
date: 2026-09-13 10:00:00 +0530
categories: [TryHackMe]   
tags: [OSINT,Information_Gathering, Passive_Reconnaissance,DarkWeb,TOR,PGP,Wigle]
image:
  path: /assets/img/headers/Sakura-Header.png
---

# Try Hack Me - Sakura Room

![image.png](/assets/img/Try-Hack-Me-Sakura-Room//image.png)

## Task 1 (Introduction)

# **Background**

---

This room is designed to test a wide variety of different OSINT techniques. With a bit of research, most beginner OSINT practitioners should be able to complete these challenges. This room will take you through a sample OSINT investigation in which you will be asked to identify a number of identifiers and other pieces of information in order to help catch a cybercriminal. Each section will include some pretext to help guide you in the right direction, as well as one or more questions that need to be answered in order to continue on with the investigation. Although all of the flags are staged, this room was created using working knowledge from having led and assisted in OSINT investigations both in the public and private sector.

NOTE: All answers can be obtained via passive OSINT techniques, DO NOT attempt any active techniques such as reaching out to account owners, password resets, etc to solve these challenges.

## Task 2 (Tip-OFF)

Background The OSINT Dojo recently found themselves the victim of a cyber attack. It seems that there is no major damage, and there does not appear to be any other significant indicators of compromise on any of our systems. However during forensic analysis our admins found an image left behind by the cybercriminals. Perhaps it contains some clues that could allow us to determine who the attackers were?We've copied the image left by the attacker, you can view it in your browser [here(opens in new tab)](https://raw.githubusercontent.com/OsintDojo/public/3f178408909bc1aae7ea2f51126984a8813b0901/sakurapwnedletter.svg).

### Instructions

---

Images can contain a treasure trove of information, both on the surface as well as embedded within the file itself. You might find information such as when a photo was created, what software was used, author and copyright information, as well as other metadata significant to an investigation. In order to answer the following question, you will need to thoroughly analyze the image found by the OSINT Dojo administrators in order to obtain basic information on the attacker.

```
Question: What username does the attacker go by?
```

### Task 2 Solution

Begin by opening up the image from the provided link 

After Opening the link to the image I simply observed the image using the “Developer Tools”

![Screenshot 2026-08-31 at 7.53.28 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//Screenshot_2026-08-31_at_7.53.28_PM.png)

From there I observed in the Developer tools that the picture contained a directory which revealed the Attacker’s name which is “SakuraSnowAngelAiko”

(Normally you would download the image extract its binary, or other tools but sometimes you have to keep it simple and check the top layer before going in something deep)

```
What username does the attacker go by?

SakuraSnowAngelAiko
```

## Task 3 (Reconnaissance)

### Background

---

It appears that our attacker made a fatal mistake in their operational security. They seem to have reused their username across other social media platforms as well. This should make it far easier for us to gather additional information on them by locating their other social media accounts.

### Instructions

---

Most digital platforms have some sort of username field. Many people become attached to their usernames, and may therefore use it across a number of platforms, making it easy to find other accounts owned by the same person when the username is unique enough. This can be especially helpful on platforms such as on job hunting sites where a user is more likely to provide real information about themselves, such as their full name or location information.

A quick search on a reputable search engine can help find matching usernames on other platforms, and there are also a large number of specialty tools that exist for that very same purpose. Keep in mind, that sometimes a platform will not show up in either the search engine results or in the specialized username searches due to false negatives. In some cases you need to manually check the site yourself to be 100% positive if the account exists or not. In order to answer the following questions, use the attacker's username found in Task 2 to expand the OSINT investigation onto other platforms in order to gather additional identifying information on the attacker. Be wary of any false positives!

```
What is the full email address used by the attacker?

What is the attacker's full real name?
```

### Task 3 Solution

Now in order to find the attackers email address I looked up her username{obtained from previous task} on google, and later found her Github on Google

![Screenshot 2026-08-29 at 7.09.16 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//Screenshot_2026-08-29_at_7.09.16_PM.png)

Now the Github contained more information, lets take a look at the PGP keys it might contains some metadata so let’s download it.

![Screenshot 2026-08-26 at 9.20.41 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//Screenshot_2026-08-26_at_9.20.41_PM.png)

After downloading the file I used the gpg tool to show the keys and it revealed the email.

Next is finding the attacker’s full real name, when searched on google at first there were many writeup of other people for the same room so i still kept on searching by removing the “@protonmail.com” and just searched for the name where I was able to find twitter

![Screenshot 2026-08-26 at 9.46.14 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//Screenshot_2026-08-26_at_9.46.14_PM.png)

From the top post itself it says “Hi there! I’m Aiko Abe” hence that is her full name

```
What is the full email address used by the attacker?

SakuraSnowAngel83@protonmail.com

What is the attacker's full real name?

Aiko Abe
```

## Task 4 (Unveil)

t seems the cybercriminal is aware that we are on to them. As we were investigating into their Github account we observed indicators that the account owner had already begun editing and deleting information in order to throw us off their trail. It is likely that they were removing this information because it contained some sort of data that would add to our investigation. Perhaps there is a way to retrieve the original information that they provided?

### Instructions

---

On some platforms, the edited or removed content may be unrecoverable unless the page was cached or archived on another platform. However, other platforms may possess built-in functionality to view the history of edits, deletions, or insertions. When available this audit history allows investigators to locate information that was once included, possibly by mistake or oversight, and then removed by the user. Such content is often quite valuable in the course of an investigation. In order to answer the below questions, you will need to perform a deeper dive into the attacker's Github account for any additional information that may have been altered or removed. You will then utilize this information to trace some of the attacker's cryptocurrency transactions.

```
1. What cryptocurrency does the attacker own a cryptocurrency wallet for?
2. What is the attacker's cryptocurrency wallet address?
3. What mining pool did the attacker receive payments from on January 23, 2021 UTC?
4. What other cryptocurrency did the attacker exchange with using their cryptocurrency wallet?
```
### Task 4 Solution

The first question asks for what cryptocurrency does the attacker own? now in order to know the cryptocurrency we need to head back to the github because there I’ve observed something related to cryptocurrency

![Screenshot 2026-08-29 at 7.09.16 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//Screenshot_2026-08-29_at_7.09.16_PM%201.png)

After exploring the github repositories I found a repository which is named as “ETH” and opening that I found information. 

![Screenshot 2026-08-29 at 7.10.38 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//Screenshot_2026-08-29_at_7.10.38_PM.png)

Now the “instructions” revealed that some platforms have archived or cached the removed or edited content. Well I played around with github a lot of times so there is “history” button which is quite visible, clicked on it and checked some history

![Screenshot 2026-08-29 at 7.14.35 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//Screenshot_2026-08-29_at_7.14.35_PM.png)

Well this part took me a lot of time since I was not experienced much with “cryptocurrencies” { I do know what is crypto currency is but didn’t know detailed information about it} so I had to visit a lot of websites in order to decode this string.

After surfing the websites I found websites which accepted this string which is “Etherscan”

![Screenshot 2026-08-29 at 7.22.41 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//Screenshot_2026-08-29_at_7.22.41_PM.png)

From the results I confirmed that the address is valid and the currency used is “Ethereum”

Now to answer the next questions I have to do a little more digging, the next question was asked “what mining pool did the attacker receive payments from on January 23,2021” now this was pretty easy to do which just use the “filters” to the date which is mentioned there.

![Screenshot 2026-08-29 at 7.26.56 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//Screenshot_2026-08-29_at_7.26.56_PM.png)

After filtering to the date, I found the mining pool named as “Ethermine” hence that the answer to the question, next question asks what is the “other cryptocurrency” used, I simply clicked on the “Token Transfers” and looked for more info.

![Screenshot 2026-08-29 at 7.31.04 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//Screenshot_2026-08-29_at_7.31.04_PM.png)

Now looking at the “Token” column  I observed that “Tether USD” was used I the answer will be “Tether”

```
1. What cryptocurrency does the attacker own a cryptocurrency wallet for?
-> Ethereum

2. What is the attacker's cryptocurrency wallet address?
-> 0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef

3. What mining pool did the attacker receive payments from on January 23, 2021 UTC?
-> Ethermine

4. What other cryptocurrency did the attacker exchange with using their cryptocurrency wallet?
-> Tether
```

## Task 5 (Taunt)

Just as we thought, the cybercriminal is fully aware that we are gathering information about them after their attack. They were even so brazen as to message the OSINT Dojo on Twitter and taunt us for our efforts. The Twitter account which they used appears to use a different username than what we were previously tracking, maybe there is some additional information we can locate to get an idea of where they are heading to next?

We've taken a screenshot of the message sent to us by the attacker, you can view it in your browser [here(opens in new tab)](https://raw.githubusercontent.com/OsintDojo/public/main/taunt.png).

### Instructions

---

Although many users share their username across different platforms, it isn't uncommon for users to also have alternative accounts that they keep entirely separate, such as for investigations, trolling, or just as a way to separate their personal and public lives. These alternative accounts might contain information not seen in their other accounts, and should also be investigated thoroughly. In order to answer the following questions, you will need to view the screenshot of the message sent by the attacker to the OSINT Dojo on Twitter and use it to locate additional information on the attacker's Twitter account. You will then need to follow the leads from the Twitter account to the Dark Web and other platforms in order to discover additional information.

```
Note: Now for this “Task” I had to use a writeup in order to proceed to solve the Room, I’ll let you know why did I take help from another writeup later in this task.
```

```
What is the attacker's current Twitter handle?

What is the BSSID for the attacker's Home WiFi?
```

### Task 5 Solution

For the first question it was pretty confusing in the beginning but I solved it as we know from twitter that her previous “Twitter” handle was “AikoAbe3” and if we look at the twitter section now I saw the new name.

![Screenshot 2026-08-29 at 8.03.00 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//Screenshot_2026-08-29_at_8.03.00_PM.png)

This the new Twitter handle of the attacker which is “SakuraLoverAiko” this solves the first question, now for the second question.

Note: The second part involves using TOR{The Onion Route} Web Browser to surf inside dark web.

Note2: The reason for using another person’s writeup for this part is because the challenge involves surfing inside the dark web website called “Deep Paste” which is mentioned indirectly in the twitter post and I’ve tried to access the website, unfortunately the website was removed and I tried searching for any alternatives but failed to do hence I had no other choice to look at other people’s writeup and use the clues which was found init.

![Screenshot 2026-08-30 at 12.09.10 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//2f7ebddc-8cf6-426b-a08e-fe87d058f034.png)

Now from this details which from another writeup the “Home Wifi” is named as “DK1F-G” now in order to find this we have to use another website which is called “Wigle” which is a wireless network mapping, inside the website go to the “Advanced Filter” and then give the SSID(Before searching you need to login or create an account) 

![Screenshot 2026-08-30 at 12.29.41 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//Screenshot_2026-08-30_at_12.29.41_PM.png)

After giving the SSID the location will be revealed along with that you can see the “Net ID” which is the BSSID, this solves the task 5 and lets move on to the next task.

```
What is the attacker's current Twitter handle?

->SakuraLoverAiko

What is the BSSID for the attacker's Home WiFi?

-> 84:af:ec:34:fc:f8
```

## Task 6 (HomeBound)

### Background

---

Based on their tweets, it appears our cybercriminal is indeed heading home as they claimed. Their Twitter account seems to have plenty of photos which should allow us to piece together their route back home. If we follow the trail of breadcrumbs they left behind, we should be able to track their movements from one location to the next back all the way to their final destination. Once we can identify their final stops, we can identify which law enforcement organization we should forward our findings to.

### Instructions

---

In OSINT, there is oftentimes no "smoking gun" that points to a clear and definitive answer. Instead, an OSINT analyst must learn to synthesize multiple pieces of intelligence in order to make a conclusion of what is likely, unlikely, or possible. By leveraging all available data, an analyst can make more informed decisions and perhaps even minimize the size of data gaps. In order to answer the following questions, use the information collected from the attacker's Twitter account, as well as information obtained from previous parts of the investigation to track the attacker back to the place they call home.

```
What airport is closest to the location the attacker shared a photo from prior to getting on their flight?

What airport did the attacker have their last layover in?

What lake can be seen in the map shared by the attacker as they were on their final flight home?

What city does the attacker likely consider "home"?
```

### Task 6 Solution

Now in order to begin for this task we need to look at her twitter or X post to further investigate this task

![DC Monument.jpeg](/assets/img/Try-Hack-Me-Sakura-Room//DC_Monument.jpeg)

Now in the post it was written “Checking out some last minute cherry blossoms before heading home!” 

Upon inspection I didn’t find something about it but after keen observation I saw that in the picture there is the “Washington Monument” 

![Screenshot 2026-08-30 at 8.02.34 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//eb4833d9-3400-4870-b815-d61688c72210.png)

searching in the google about the nearest airport from the Washington Monument I found this airport “Ronald Reagan Washington National Airport” although the text required in the Challenge was small so I checked the “hint” button.

It mentioned that you need to use the Airport Code in order to submit the answer which the answer was “DCA”.

Now onto the second question which was the last layover which can be observed in her second last post.

![Screenshot 2026-08-30 at 7.40.16 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//Screenshot_2026-08-30_at_7.40.16_PM.png)

Well  the post which contained the “layover” photo the only clue which was visible is “First Class Lounge Sakura Lounge” by “JAL” well I once again use the power of the google to look for airports which contained this lounge.

There Were two airports which were Haneda Airport and one other airport, the Haneda Airport code is “HND” which answered the question.

Next is what is the name of the  lake can be seen from the post,  I actually opened “Google Maps” and located manually and found the location of the lake.

![Screenshot 2026-08-30 at 8.44.23 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//Screenshot_2026-08-30_at_8.44.23_PM.png)

From the “Google Maps” the lake is named as “Lake Inawashiro” which answered the second last question.

Now for the final question the city’s name which the attacker considered it as home.

This is pretty straight forward to the question because the answer is revealed in the clue from the task 5

![Screenshot 2026-08-30 at 12.09.10 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//a1c55676-ff5e-4e4e-b565-6f21179ef56e.png)

you can see the City’s Free Wifi Name which is “Hirosaki” which I saw at later but I actually solved it from the given “Coordinates” when used in the “Wigle” website and located it manually with the help of google maps.

![Screenshot 2026-08-30 at 8.45.26 PM.png](/assets/img/Try-Hack-Me-Sakura-Room//Screenshot_2026-08-30_at_8.45.26_PM.png)

The “Red Pin” from the image shows the Attacker’s House so zooming out from it revealed the Attacker’s City which is “Hirosaki”

```
What airport is closest to the location the attacker shared a photo from prior to getting on their flight?

-> DCA

What airport did the attacker have their last layover in?

-> HND

What lake can be seen in the map shared by the attacker as they were on their final flight home?

-> Lake Inawashiro

What city does the attacker likely consider "home"?

-> Hirosaki
```

---



## Final Thoughts

This room was probably one of the more interesting OSINT rooms I've completed because the investigation didn't rely on a single technique.

It started with something as simple as inspecting an image and finding a username, then gradually moved into:

- Username enumeration
- GitHub investigation
- PGP metadata
- Cryptocurrency investigation
- Blockchain transaction analysis
- Social media investigation
- Dark-web research
- Wi-Fi/BSSID investigation
- Wireless network mapping
- Geolocation
- Travel-route analysis

What I personally found useful was that I didn't immediately know how to solve every part. The cryptocurrency section, for example, took me some time because I wasn't familiar with blockchain investigation. The Deep Paste portion of Task 5 was another limitation because the original site was no longer accessible.

For me, that's also the point of doing these rooms. The goal isn't to already know every tool or technique before starting. It's to learn how to follow a clue, research what you don't understand, verify the information, and then use that finding to move on to the next stage of the investigation.

Overall, this was a good room on how different pieces of seemingly unrelated information can be connected together during an OSINT investigation.
