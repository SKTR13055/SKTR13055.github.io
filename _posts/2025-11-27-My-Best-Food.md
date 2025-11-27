---
title: "GlacierCTF – My Best Food"
date: 2025-11-27 18:00:00 +0530
categories: [GlacierCTF, Writeups]
tags: [osint, geolocation, overpass-api, osm, trilateration, python, ctf]
image:
  path: /assets/img/headers/glacierCTF.png
  alt: GlacierCTF
---

# Glacier CTF My Best Food Writeup

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

### **1. Incorrect Hypothesis #1 — OSINT Rabbit Hole**

I first tried to solve the challenge using OSINT:

- Found the challenge author’s website
- Looked at their social media (GitLab, PixelFed)
- Saw food pictures and assumed they were hints
- Found a picture of Votivekirche
- Assumed the restaurant must be somewhere nearby

This led me to:

- Searching restaurants near Votivekirche
- Going through dozens of Google reviews manually
- Trying to match timestamps and patterns

None of this was relevant to the actual challenge.

---

### **2. Incorrect Hypothesis #2 — Photo Location Deduction**

I doubled down on the church photo:

- Tried to find the **exact place** where the photo was taken
- Looked for the nearest restaurants
- Checked their Google reviews for several hours

This approach was also incorrect because:

- The photos were personal posts
- They were not connected to the challenge
- The correct solution was entirely inside the Rust file

---

## What I Learned From My Mistakes

This challenge taught me several important lessons about solving OSINT + geolocation problems more effectively.

### 1. Don’t rely on unrelated visual clues
I initially focused on:
- The challenge author’s website
- His food collage
- A photo of Votivkirche
- Nearby restaurants and their reviews

None of these were actual clues. The intended solution was not based on images or social media, so this approach wasted time.

### 2. Properly analyze the provided code
The real hints were inside `main.rs`:
- Bar, ATM, and taxi categories
- Distances for each
- A commented hint about centroid calculations
- A missing Overpass API function

The challenge was solvable entirely through the code, not external OSINT.

### 3. Overpass API data varies by year
Solvers got slightly different coordinates because:
- Some Overpass API instances used older 2022 OpenStreetMap data
- Others used newer 2023–2024 snapshots

This taught me that geolocation challenges may not produce pixel-perfect identical results depending on API data age.

### 4. Mathematical trilateration is more accurate than manual guessing
My early attempts relied on:
- Visually checking the map
- Proximity-based guessing
- Searching random restaurant reviews

Only proper trilateration with real distance calculations produced the correct coordinates.

### 5. Reproducibility matters
Using Ale’s Python script (shared after the CTF ended) provided:
- Correct centroid calculation
- Proper trilateration
- Coordinates that matched the intended solution

This reinforced the importance of using precise, repeatable methods.

### 6. Failed hypotheses still provide lessons
Even though my two initial hypotheses were incorrect, they helped me understand:
- The pitfalls of overthinking OSINT challenges
- Why staying within the problem scope is important
- The value of trusting mathematical hints over subjective clues

--- 
# **Acknowledgments**

A special thanks to **ale**, who shared a clean Python trilateration script with me **after the CTF had ended**.

Her script helped me understand the mathematical approach better and verify where my earlier calculations were slightly off. She did not assist during the competition — only afterward, fully respecting CTF rules.
