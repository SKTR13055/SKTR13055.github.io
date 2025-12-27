---
title: "PicoCTF – Hash Crack"
date: 2025-12-23 20:20:00 +0530
categories: [PicoCTF,Cryptograhy]
tags: [Cryptography, hashcat, MD5, decoder]
image:
  path: /assets/img/headers/picoctf-banner.jpg
  alt: PicoCTF Banner
---

Hash Crack

Challenge Author: Nana Ama Atombo-Sackey

Category: Cryptography

Description
--- 

A company stored a secret message on a server which got breached due to the admin using weakly hashed passwords. Can you gain access to the secret stored within the server?

Additional details will be available after launching your challenge instance.

Process
---

1. Launch the instance from the Pico CTF website.
2. After launching the challenge instance from the PicoCTF platform,I connected to the server using the provided “nc(netcat)” command.

    ![Screenshot 2025-12-23 at 8.35.34 PM.png](/assets/img/Hash-Crack-Photos/Screenshot_2025-12-23_at_8.35.34_PM.png)

3. Upon connecting it to the server it displayed welcome message along with that it gives a hash which is 

    ```
    482c811da5d5b4bc6d497ffa98491e38
    ```

4. After copying the message I used online hash identifier tool to determine the hash type.

    ![Screenshot 2025-12-23 at 8.36.10 PM.png](/assets/img/Hash-Crack-Photos/Screenshot_2025-12-23_at_8.36.10_PM.png)

5. Since the hash has been identified as MD5 so by using an MD5 decoder (alternatively, tools like Hashcat could also be used), I cracked the hash and obtained the value.

    ![Screenshot 2025-12-23 at 8.37.04 PM.png](/assets/img/Hash-Crack-Photos/Screenshot_2025-12-23_at_8.37.04_PM.png)

6. Entering this password into the server prompt granted access to the next stage.

    ![Screenshot 2025-12-23 at 8.37.27 PM.png](/assets/img/Hash-Crack-Photos/Screenshot_2025-12-23_at_8.37.27_PM.png)

7. After giving the correct response, it gave another hash which was 

    ```
    b7a785fc1ea228b9061041b7cec4bd3c52ab3ce3
    ```

8. Repeating the previous steps, reveals the new decoded message.

    ![Screenshot 2025-12-23 at 8.42.28 PM.png](/assets/img/Hash-Crack-Photos/Screenshot_2025-12-23_at_8.42.28_PM.png)

9. After pasting the hash value to the “hash Identifier” reveals the hash type as “SHA1”, now decoding this value reveals the actual password.

    ![Screenshot 2025-12-23 at 8.39.07 PM.png](/assets/img/Hash-Crack-Photos/Screenshot_2025-12-23_at_8.39.07_PM.png)

10. After decoding the hash value I got the result as “letmein” and giving the password to the new request from the instance.

    ![Screenshot 2025-12-23 at 8.39.32 PM.png](/assets/img/Hash-Crack-Photos/Screenshot_2025-12-23_at_8.39.32_PM.png)

11. I am almost there, now the final hash is presented which is 

    ```
    916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745
    ```

    ![Screenshot 2025-12-23 at 8.40.48 PM.png](/assets/img/Hash-Crack-Photos/Screenshot_2025-12-23_at_8.40.48_PM.png)

12. The last hash was possibly identified as “SHA256” , now decoding this value gives the decoded value of the hash.

    ![Screenshot 2025-12-23 at 8.40.57 PM.png](/assets/img/Hash-Crack-Photos/Screenshot_2025-12-23_at_8.40.57_PM.png)

13. Giving this value to the instance hopefully gives the flag.

    ![Screenshot 2025-12-23 at 8.41.19 PM.png](/assets/img/Hash-Crack-Photos/Screenshot_2025-12-23_at_8.41.19_PM.png)

14. Finally the flag has been revealed

    ```bash
    picoCTF{UseStr0nG_h@sheEs_&PaSswDs!_93e052d7}
    ```

Conclusion
--- 

This challenge highlights the critical importance of using **strong hashing algorithms combined with salting and secure password policies**. Weak or unsalted hashes such as MD5 and SHA1 are highly vulnerable to dictionary and rainbow table attacks, making them unsuitable for password storage.

Through systematic hash identification and decoding, access to the protected resource was easily obtained—demonstrating how attackers can exploit poor cryptographic practices.


Thank You
--- 

Thank you for reviewing this write-up. I appreciate the opportunity to work through this challenge and strengthen my understanding of cryptographic vulnerabilities.

Special thanks to PicoCTF and Nana Ama Atombo-Sackey for creating an engaging and educational challenge.

See you in the next write-up!
