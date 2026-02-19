---
title: "HackThisSite – Realistic 2"
date: 2026-02-19 13:20:00 +0530
categories: [HackThisSite,Realistic]
tags: [WebExploitation,Hidden_Directory,Data_Leakage,SQL_Injection,Injection]
image:
  path: /assets/img/headers/Hack-This-Site-Realistic-2-Banner.png
  alt: HackThisSite-Realistic-2-Banner
---
# Hackthissite Realistic-2

Disclaimer
---
The content shown in this challenge references extremist “white power” ideology, which I strongly condemn. This write-up is created strictly for educational and ethical cybersecurity learning purposes, and I do not support or endorse any form of racism, hate, or discrimination.

Theory
---

**What is SQL Injection?**

SQL Injection is a security vulnerability that happens when a website does not properly check the data a user enters.

If user input is directly sent to the database without proper validation, an attacker can insert special SQL commands instead of normal text.

This allows them to manipulate the database in ways the developer never intended.

**How Does a Normal Login Work?**

When you enter a username and password, the website checks them using a database query like this:

```sql
SELECT*FROM usersWHERE username='john'AND password='1234';
```

This query asks the database:

> “Is there a user with this username and password?”

If the answer is yes, you are logged in.

If not, login fails.

 **Where the Problem Happens?**

If the website does not filter what users type, someone can enter SQL code instead of a real username.

For example, typing:

```
' OR 1=1 --
```

changes the query into something like:

```sql
SELECT*FROM users WHERE username=''OR 1=1-- ' AND password = '';
```

---

 Why Does This Work?

Let’s break it down simply:

- `'` closes the original username field.
- `OR 1=1` adds a condition that is **always true**.
- `-` tells the database to ignore the rest of the line.

Since `1=1` is always true, the database returns a valid result.

The website thinks the login is correct — even though no real password was used.

That’s how authentication gets bypassed.

---

🎯 **What Can Attackers Do With SQL Injection?**

If a website is vulnerable, attackers might:

- Log in without credentials
- View sensitive data (like emails or passwords)
- Modify database content
- Delete records
- Take control of admin accounts

The damage depends on how vulnerable the system is.

---

🛡 **How Can Developers Prevent It?**

To stop SQL Injection, developers should:

- Use **prepared statements (parameterized queries)**
- Validate and sanitize all user input
- Avoid showing detailed database error messages
- Use proper access controls in the database

When input is handled safely, injected SQL code will not work.

---

📝 Simple Way to Remember

> SQL Injection happens when a website trusts user input too much.
> 

If input is not properly handled, attackers can turn text fields into database commands.

Now lets move on to the Challenge in order to understand about SQL Injection in more clear manner.

Challenge_Description
---

From: DestroyFascism

Message: I have been informed that you have quite admirable hacking skills. Well, this racist hate group is using

[their website](https://www.hackthissite.org/missions/realistic/2)

to organize a mass gathering of ignorant racist bastards. We cannot allow such bigoted aggression to happen. If you can gain access to their administrator page and post messages to their main page, we would be eternally grateful.

Process
---

1.  I visited the main website and observed there was no visible login or admin panel.

    ![Screenshot 2026-02-14 at 8.34.01 PM.png](/assets/img/Hack-This-Site-Realistic-2-Photos/Screenshot_2026-02-14_at_8.34.01_PM.png)

2. After the website is loaded, the content is all about the “White Power”.
3. Since the website does not contain any login fields or anything similar, I decided to view the “Page source” of the website to find any potential “data leakage”.

    ![Screenshot 2026-02-14 at 9.10.19 PM.png](/assets/img/Hack-This-Site-Realistic-2-Photos/Screenshot_2026-02-14_at_9.10.19_PM.png)

4. Looking at the source code an “href” tag leads to another hidden directory which is the “update.php” 

    ![Screenshot 2026-02-14 at 9.10.39 PM.png](/assets/img/Hack-This-Site-Realistic-2-Photos/Screenshot_2026-02-14_at_9.10.39_PM.png)

5. Visiting the page gave me the login form which says “enter your username and password, white brother!” now lets try giving the normal input and see the response.

    ![Screenshot 2026-02-14 at 9.10.45 PM.png](/assets/img/Hack-This-Site-Realistic-2-Photos/75fbbb5d-0c2d-4e14-a746-d00f7a3e9ca0.png)

6. The response was pretty obvious but just wanted to confirm because you never know, Initially I gave the wrong syntax of the SQL payload which gave me the response (as seen below picture).

    ![Screenshot 2026-02-14 at 9.11.21 PM.png](/assets/img/Hack-This-Site-Realistic-2-Photos/0f8bd70b-8e9e-41a8-b02b-4f81d435b384.png)

7. From the output it returned an “SQL Error” which indicates that the presence of SQL injection is there.

    ![Screenshot 2026-02-14 at 9.17.52 PM.png](/assets/img/Hack-This-Site-Realistic-2-Photos/Screenshot_2026-02-14_at_9.17.52_PM.png)

8. This time I gave proper “SQL Payload” in order to gain access and observe the response.

    ![Screenshot 2026-02-14 at 9.09.29 PM.png](/assets/img/Hack-This-Site-Realistic-2-Photos/Screenshot_2026-02-14_at_9.09.29_PM.png)

9. After giving the payload it redirected me to this page, later I came to know that the challenge was solved. 


Conclusion
---

This challenge demonstrates how improper input validation and lack of parameterized queries can lead to authentication bypass through SQL injection. By analyzing the page source, identifying hidden directories, and testing input fields with structured payloads, we were able to confirm and exploit the vulnerability.

The exercise reinforces the importance of reconnaissance, controlled testing, and understanding backend query logic when performing web application security assessments.

Key Takeaways
---
- Always inspect page source for hidden endpoints.
- Error messages are valuable indicators of vulnerabilities.
- SQL Injection often becomes visible through improper error handling.
- Authentication bypass can occur when user input is directly embedded in SQL queries.
- Developers must use prepared statements and proper input sanitization to prevent SQLi attacks.
- According to the **OWASP Top 10 (2025)**, SQL Injection falls under **A05: Injection**, not Insecure Design.

---

Thank you for taking the time to read this write-up. I truly appreciate your interest and support. I hope this walkthrough helped you better understand how SQL injection vulnerabilities can be identified and exploited in a controlled learning environment. Keep learning, keep practicing, and most importantly — use your skills ethically! 🚀
