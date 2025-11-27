---
title: "GlacierCTF – My Best Food"
date: 2025-10-27 18:00:00 +0530
categories: [GlacierCTF, Writeups]
tags: [osint, geolocation, overpass-api, osm, trilateration, python, ctf]
image:
  path: /assets/img/headers/glacierCTF.png
  alt: GlacierCTF
---

# My Best Food CTF Writeup

# Description

We all have that one teammate who promises to deliver challenges for the CTF, but always just barely makes it in time, leaving no room for testing. Usually, it works out in the end...

But not this time. They’re so late that the CTF is already live, and all we have is a half-finished challenge and a lot of frustration!

The last message we got from the author said:

**"Sry for being late this year. Next year for sure. I’ll hide in my favourite food place until this blows over. uwu"**

The original goal of the challenge was to locate their favorite restaurant. Even with the incomplete code, this was still possible — but only if you interpreted it correctly.

> Note:
> 
> 
> This write-up is mostly a correction of my earlier mistakes.
> 
> I first explain the *correct solution*, and then the *wrong approaches I initially took*.
> 

---

# **Correct Process**

### **1. Extracting the Challenge**

After downloading `best_food.tar.gz`, extract it using:

```
tar -xvzf best_food.tar.gz

```

Inside the folder you will find:

- `flag.txt` (fake flag)
- `main.rs`
- `sha256sum`

Only *main.rs* is relevant for solving the challenge.

---

### **2. Analyzing `main.rs`**

Opening `main.rs` with:

```
cat main.rs

```

reveals:

- The code is intentionally incomplete (as stated in the challenge)
- There are **commented-out coordinate blocks**
- Each block contains: **(latitude, longitude, distance)**
- The categories are: **bar**, **atm**, and **taxi**

The distances provided are:

```
bar_dist  = 0.6412652119499
atm_dist  = 0.2822972577454
taxi_dist = 0.9572772921063

```

These represent **real-world distances** from the secret restaurant.

---

### **3. Understanding the Hint**

The intended method:

1. Use the **Overpass API** to download all:
    - Bars
    - ATMs
    - Taxi stands
    
    **in the correct city (Graz, Austria)**.
    
2. Compute the **centroid** of each point cloud.
3. Use the three distances above.
4. Perform **geographical trilateration** to compute the intersection point.
5. The intersection = **the restaurant’s coordinates**.
6. Search the restaurant’s Google reviews → retrieve the flag.
    
    ![ale_image.webp](/assets/img/My-Best-Food-photos/ale_image.webp)
   

**Note:** For this step, I executed the trilateration using **Ale’s Python code**, which she shared *after the challenge*. Her script helped automate the distance-based intersection calculation.

---

### **4. Trilateration**

Using a Python implementation of haversine distance + least-squares optimization:

- Compute 3 centroids for:
    - bar
    - atm
    - taxi
- Use trilateration to compute the intersection.

This yields approximate coordinates around:

```
47.08601061, 15.42746599

```

Plotting this location in Google Maps leads directly to the restaurant.

![Screenshot 2025-11-24 at 7.05.21 PM.png](/assets/img/My-Best-Food-photos/Screenshot_2025-11-24_at_7.05.21_PM.png)

---

### **5. Final Step — Finding the Flag**

Open the restaurant in Google Maps, scroll to the **reviews**, and search carefully.

You eventually find a Base64-encoded string:

```
Z2N0ZntnMDBkIGYwMGQsIGh1bTAwcm43dXMgZjAwZCByM2YxM30=

```

Decoding it gives the real flag:

```
gctf{g00d f00d, hum0r0u5 f00d r3v13w5}

```

---

## **My (Incorrect) Process**

### **1. OSINT Rabbit Hole**

I initially focused on OSINT:

- Found the challenge author’s website
- Looked up their social media (GitLab, PixelFed)
- Found food pictures
- Saw a photo of a church (Votivekirche)
- Assumed the restaurant was nearby

This led me to:

- Searching restaurants near Votivekirche
- Reading dozens of reviews by hand
- Trying to correlate timestamps

None of this was relevant.

---

### **2. Incorrect Hypothesis**

I tried:

- Pinpointing the *exact location* where the church photo was taken
- Checking nearby restaurants
- Reading their reviews for hours

This was all incorrect because:

- The photos were personal posts
- They were not related to the challenge
- The real solution was fully contained inside the Rust file

---

# **Acknowledgments**

A special thanks to **ale**, who shared a clean Python trilateration script with me **after the CTF had ended**.

Her script helped me understand the mathematical approach better and verify where my earlier calculations were slightly off. She did not assist during the competition — only afterward, fully respecting CTF rules.
