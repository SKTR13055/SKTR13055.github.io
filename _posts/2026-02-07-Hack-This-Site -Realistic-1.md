---
title: "HackThisSite – Realistic 1"
date: 2026-12-32 13:20:00 +0530
categories: [HackThisSite,Cryptography]
tags: [Burpsuite,WebExploitation,Burpsuite_Repeater,Insecure_Design]
image:
  path: /assets/img/headers/HackThisSite.png
  alt: HackThisSite Banner
---

# Hack This Site - Realistic 1

## **Uncle Arnold's Local Band Review**

Your friend is being cheated out of hundreds of dollars. Help him make things even again!

Challenge_Description
---

From: HeavyMetalRyan

Message: Hey man, I need a big favour from you. Remember that website I showed you once before?

[Uncle Arnold's Band Review Page](https://www.hackthissite.org/missions/realistic/1/)? Well, a long time ago I made a $500 bet with a friend that my band would be at the top of the list by the end of the year. Well, as you already know, two of my band members have died in a horrendous car accident... but this a****le still insists that the bet is on!

I know you're good with computers and stuff, so I was wondering, is there any way for you to hack this website and make my band on the top of the list? My band is **Raging Inferno**. Thanks a lot, man!

Process
---

1. After opening the provided link, I scrolled through the list of bands and confirmed that **Raging Inferno** was ranked last with only a few votes.

    ![Screenshot 2026-02-07 at 7.00.01 PM.png](/assets/img/Hack-This-Site-Realistic-1-Photos/Screenshot_2026-02-07_at_7.00.01_PM.png)

2. Since normal voting only increments the count by a small amount, I decided to intercept the voting request to analyze how votes were being submitted.

    ![Screenshot 2026-02-07 at 7.30.44 PM.png](/assets/img/Hack-This-Site-Realistic-1-Photos/Screenshot_2026-02-07_at_7.30.44_PM.png)

3. I launched **Burp Suite** and enabled interception. When submitting a vote for Raging Inferno, Burp captured the HTTP request containing the voting parameter.

    ![Screenshot 2026-02-07 at 7.36.01 PM.png](/assets/img/Hack-This-Site-Realistic-1-Photos/Screenshot_2026-02-07_at_7.36.01_PM.png)

4. I sent this request to **Burp Repeater** to manually modify it.

    ![Screenshot 2026-02-07 at 7.40.48 PM.png](/assets/img/Hack-This-Site-Realistic-1-Photos/Screenshot_2026-02-07_at_7.40.48_PM.png)

5. Inside Repeater, I changed the value of the `vote` parameter from its default value to a much larger number (for example, `vote=100`) and forwarded the request.
6. After sending the modified request, I used Burp’s **Render** feature to view the updated page. The vote count for Raging Inferno increased significantly, placing the band at the top of the list.

    ![Screenshot 2026-02-07 at 7.43.01 PM.png](/assets/img/Hack-This-Site-Realistic-1-Photos/Screenshot_2026-02-07_at_7.43.01_PM.png)

7. Upon refreshing the challenge page, the system indicated that the challenge had already been completed (since it was previously solved). On a first successful attempt, the platform displays a confirmation message instead of a flag. 

Conclusion
---

This challenge highlights how improper server-side validation and flawed application logic can lead to serious security issues. By modifying the `vote` parameter in the intercepted request, it was possible to artificially inflate the vote count and manipulate the rankings. This demonstrates a classic case of parameter tampering resulting from broken business logic, where the application trusted client input without verification. 

The vulnerability maps to **OWASP A04:2021 – Insecure Design**, emphasizing the importance of implementing proper validation and enforcing logical constraints on the server side.

Takeaway
---

Applications should never rely on client-side controls alone. All critical operations must be validated on the server, including enforcing fixed vote increments, rate limiting, and user restrictions. This challenge reinforces the importance of secure application design and highlights how small logic flaws can be abused to achieve unintended outcomes. From a security perspective, it serves as a reminder that business logic vulnerabilities can be just as impactful as traditional injection-based attacks.

Thank you to Hack This Site for providing realistic challenges that help build practical security skills. This exercise offered valuable hands-on experience in identifying business logic flaws and understanding how improper input validation can be exploited in real-world applications.
