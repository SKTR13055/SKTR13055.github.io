---
title: "PicoCTF – EVEN RSA CAN BE BROKEN???"
date: 2026-01-30 13:20:00 +0530
categories: [PicoCTF,Cryptography]
tags: [Cryptography,python,RSA,RSA_Explanation,decoder]
image:
  path: /assets/img/headers/picoctf-banner.jpg
  alt: PicoCTF Banner
---

Challenge_Author: Michael Crotty

Category: Cryptography

Description
---

This service provides you an encrypted flag. Can you decrypt it with just N & e?
Additional details will be available after launching your challenge instance.

Theory
---

- RSA (Rivest-Shamir-Adleman) is a foundational **Asymmetric Encryption** or **Public Key Cryptography** algorithm. Its security is based on the mathematical difficulty of factoring the product of two large prime numbers.
- This method utilizes a pair of mathematically linked keys: a **Public Key**, denoted as **(n,e)** used for encryption, and a **Private Key**, denoted as **(n,d)** used for decryption.
- The value **n** known as the **modulus**, is shared in both keys and is formed by multiplying two secret, large prime numbers (**p** and **q**). While the modulus and the public exponent (**e**) are made public, the private exponent (**d**) is calculated using the original primes and must be kept strictly confidential to ensure the security of the encrypted data.
- In 2026, RSA remains a global standard for securing digital communications and verifying electronic signatures.

**Notations used in RSA**
---

- p,q = two large, distinct prime numbers
- n = p x q
- φ(n)=(p−1)(q−1)= Euler’s totient function
- e = public exponent
- d = private exponent


**RSA Generation Process**
---

### 1. Selection of Prime Numbers

Two large prime numbers are chosen:

```

p = A large prime number

q = A large Prime number

```

These primes must be kept **secret** and are randomly generated to ensure security.

---

### 2. Computation of the Modulus

The modulus n is computed as:


```
n = p x q
```

The modulus nnn determines the key size (e.g., 2048-bit, 3072-bit) and is shared in both the public and private keys.

---

### 3. Euler’s Totient Function

Compute:


```
φ(n)=(p−1)(q−1)
```

This value represents the number of integers less than n that are coprime to n and is used to derive the private key.

---

### 4. Selection of the Public Exponent

Choose an integer e such that:

```

1<e<φ(n)


gcd(e,φ(n))=1

```

This ensures that e is co prime with φ(n)

Common choices include:

```

e = 65537

```

due to its efficiency and strong security properties.

---

### 5. Computation of the Private Exponent

The private exponent ddd is computed as the **modular multiplicative inverse** of e modulo 

```

d≡e^−1(modφ(n))

```

This means:

```
e⋅d≡1 (mod φ(n))
```

---

## Formation of RSA Keys

- **Public Key**:

```
Public Key=(n,e)
```

- **Private Key**:

```
Private Key=(n,d)
```

The private key implicitly depends on the secret values p and q. Disclosure of these primes allows an attacker to compute d, breaking the system.

---

## Encryption and Decryption (Formal Representation)

 **Encryption** of a plaintext message m (where 0 ≤ m <n ):

```
c=m^e(modn)
```

- **Decryption** of ciphertext c:

```
m=c^d(modn)
```

---

 

Process
---

1. Launch the instance from the PicoCTF website.
2. After launching the instance from the website using the provided command to connect to the PicoCTF server.

    ```bash
    $ nc verbal-sleep.picoctf.net <port_number>
    ```

3. After connecting it to the server, the following information will be provided.

    ![Screenshot 2026-01-08 at 7.09.45 PM.png](/assets/img/EVEN_RSA_CAN_BE_BROKEN_Photos/Screenshot_2026-01-08_at_7.09.45_PM.png)

    ```bash
    N: 17102525859120144002416542651391278337140077040833275221843824772555934230636354679425796336559609274359684076986516005760992508978915197682762402821578754
    e: 65537
    cyphertext(c): 11592086696783413111647889172801291397383557852015043536130425991638078723238054446356735964903847678064403422154004203233372810065628290565664257767342215
    ```

4. The above information includes, the value of N, e, cipher text now in decode this information which can be done in 2 ways, so let me show the both ways.


1st Way
---

1. Go to the website “[https://www.dcode.fr/rsa-cipher](https://www.dcode.fr/rsa-cipher)”.
2. In the website insert the values which were given in the terminal.
3. After giving all the values, the flag will be revealed on the “left side”.

    ![Screenshot 2026-01-08 at 7.18.30 PM.png](/assets/img/EVEN_RSA_CAN_BE_BROKEN_Photos/Screenshot_2026-01-08_at_7.18.30_PM.png)

4. Final Flag
```
picoCTF{tw0_1$_pr!m341c6ed35}
```

2nd Way
---

1. From the given code, The value of N is of 1024 bits, which is vulnerable then from the value of N we factorize it using the “[factordb](https://factordb.com)” website and the values of p and q are as follows.

    ```
    p: 2

    q:8551262929560072001208271325695639168570038520416637610921912386277967115318177339712898168279804637179842038493258002880496254489457598841381201410789377
    ```

2. Now after getting the values of p and q we use the “Euler Totient” to get the value of phi(N) which is required to get the value of “d”.

   ```
    Getting the Value of  φ(n)

    ---

    φ(n)=(p−1)(q−1) → Euler’s Totient

    φ(n) = (2-1)(8551262929560072001208271325695639168570038520416637610921912386277967115318177339712898168279804637179842038493258002880496254489457598841381201410789377 -1)

    φ(n) = 1 x 8551262929560072001208271325695639168570038520416637610921912386277967115318177339712898168279804637179842038493258002880496254489457598841381201410789376

    φ(n) = 855126292956…  → Value of φ(n)
   ```

3. Now the known values are “e” and “φ(n)” which can be used to calculate the value of “d” in the following formula

    ```
    d=e^−1 mod φ(N)
    ```

4. Now calculating in order to get the value of “d” which can be used to in the decryption formula. 

5. To find a modular inverse, we use the **Extended Euclidean Algorithm**, which finds integers x,y, such that:

   ```

    Extended Euclidean Algorithm (Theoritcally)

    ---

    ex+φ(N)y = gcd(e,φ(N))

    Since RSA requires:

    gcd(e,φ(N))=1

    the value of x  mod  φ(N) is your **`d`**.

   ```

6. Since doing this “theoretically” is quite difficult I created program to calculate the value of d.

    ```python
    from sympy import mod_inverse

    e = 65537
    phi = 8551262929560072001208271325695639168570038520416637610921912386277967115318177339712898168279804637179842038493258002880496254489457598841381201410789376

    d = mod_inverse(e, phi)
    print(d)
    
    ```

  - This give the value of d which is

    ```
    d = 5617843187844953170308463622230283376298685
    ```

7. Now Applying the Decryption formula which is

    ```

    m=c^d(modn)

   ```

    - We can decrypt the message by using this code

    ```python
    from Crypto.Util.number import long_to_bytes

    c = 11592086696783413111647889172801291397383557852015043536130425991638078723238054446356735964903847678064403422154004203233372810065628290565664257767342215
    N = 17102525859120144002416542651391278337140077040833275221843824772555934230636354679425796336559609274359684076986516005760992508978915197682762402821578754
    d = 5617843187844953170308463622230283376298685

    # Decrypt
    m = pow(c, d, N)

    # Convert integer to bytes safely
    print(long_to_bytes(m).decode())

    ```

8. Finally the flag the has been revealed from the code.

    ```
    picoCTF{tw0_1$_pr!m341c6ed35}
    ```

Conclusion
---

This challenge shows how RSA can fail completely when insecure parameters are used. Although the modulus appears large and the public exponent is standard, the system is broken because it uses a **1024-bit key** and an invalid prime factor (**p = 2**). Either issue alone weakens RSA significantly; together, they make decryption trivial. This reinforces that RSA’s security depends not just on the algorithm, but on correct and modern key generation practices.

Takeaway
---

- **1024-bit RSA is obsolete** and should never be used in modern systems.
- **Even one weak prime (like p = 2) destroys RSA security entirely.**
- Cryptography is only as strong as its implementation and parameter choices.

Thank you for taking the time to read this write-up. I hope this walkthrough was helpful in understanding both how RSA works internally and how poor parameter choices can completely break its security. Happy hacking and learning!
