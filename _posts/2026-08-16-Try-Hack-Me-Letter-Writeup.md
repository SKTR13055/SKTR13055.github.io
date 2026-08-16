---
title: "TryHackMe - Letter"
date: 2026-08-16 10:00:00 +0530
categories: [TryHackMe]   
tags: [OSINT, linux, Information_Gathering, Reconnaissance  ]
image:
  path: /assets/img/headers/TryHackMe-Letter.png
---
# TryHackMe - Letter

Can you help us find out more about this letter?

It's just another Monday morning on your mail delivery route when an unusual letter catches your eye. The envelope is battered, riddled with holes as if it's been through a storm. The address is barely legible, and your coworkers at the post office wave it off as a lost cause.

But something about it nags at you.

You carefully open the damp envelope. Inside, you find a faded newspaper clipping and a short handwritten note. The clipping is torn and water-damaged, with key sections missing. The note is personal, clearly not meant for your eyes, but the fragments you can read hint at a story buried in time.

**Objective:**  Use the clues provided in the zip file to uncover the full name and age of the person mentioned in the note.

Flag format: THM{Name_Surname_age}, only the first letter of the name and surname should be capitalised

Example: THM{Pierre-Henry_Lagaffe_23}

## 1. Extracting and Inspecting the Files

First, I downloaded the ZIP file provided by the challenge and extracted it using `unzip`:

```bash
unzip "FileName"
```

After extracting the contents, I used the `file` command to determine what type of files were included:

```bash
file *
```

This revealed the files that were provided for the investigation, including the note and the image of the letter

![Screenshot 2026-08-13 at 1.39.45 PM.png](/assets/img/TryHackMe-Letter-Photos/Screenshot_2026-08-13_at_1.39.45_PM.png)

## 2. Reading the Note

The first file I investigated was `Note.txt`.

I used `cat` to display its contents:

```
cat Note.txt
```

![Screenshot 2026-08-13 at 1.40.32 PM.png](/assets/img/TryHackMe-Letter-Photos/Screenshot_2026-08-13_at_1.40.32_PM.png)

The note was written in **French**, so I translated it into English to understand the clues.

The translated message contained two particularly important clues:

![Screenshot 2026-08-13 at 1.41.28 PM.png](/assets/img/TryHackMe-Letter-Photos/Screenshot_2026-08-13_at_1.41.28_PM.png)

- The person's **great-grandfather was too young to have a driver's license**.
- He was the **youngest member of the team**.

These clues suggested that I would need to identify a historical event involving a team and then determine which team member was the youngest.

The driver's-license clue also gives us an approximate age range. Since the minimum driving age is 18, the person was likely **under 18 years old** at the time of the event.

## 3. Investigating the Letter

Next, I opened the image of the letter:

```
xdg-open "Letter.png"
```

The letter itself did not provide much obvious information. However, I noticed two potentially useful details.

![letter.png](/assets/img/TryHackMe-Letter-Photos/letter.png)

First, the envelope appeared to be associated with **Lettre Verte**, the French postal service.

Second, there was a strange series of markings at the bottom-right of the envelope. This looked like a postal barcode or routing code.

Since this could potentially contain information about the destination of the letter, I decided to investigate French postal barcodes.

## 4. Decoding the French Postal Barcode

I searched for information about French postal barcode formats and found a French postal barcode decoder.

The barcode could be analyzed using the following resource:

`https://www.dcode.fr/french-postal-barcode`

![Screenshot 2026-08-13 at 1.54.11 PM.png](/assets/img/TryHackMe-Letter-Photos/Screenshot_2026-08-13_at_1.54.11_PM.png)

The automatic decoder did not initially give me the result I expected, so I analyzed the digits manually and considered the ordering of the barcode.

The decoded sequence was:

```
06792
```

After reversing the relevant ordering, the postal code became:

```
29760
```

The postal code **29760** corresponds to **Penmarch**, a commune in Finistère, France.

This gave me the first major lead:

> **The story is connected to Penmarch, France.**
> 

---

2nd Challenge

1. For the second challenge this was the most hard part for me ,  I Inspected the second picture which was the first half page of a French newspaper.

## 5. Investigating the Newspaper

The second major clue was the damaged newspaper clipping.

![Newspaper_clipping.png](/assets/img/TryHackMe-Letter-Photos/Newspaper_clipping.png)

The image appeared to be part of an old **French newspaper**, but a significant portion of the page was missing or damaged.

I examined the visible headline and other text and searched for the newspaper using the keywords and titles that were still readable.

After some searching, I was able to locate the original newspaper material.

![Screenshot 2026-08-13 at 2.31.06 PM.png](/assets/img/TryHackMe-Letter-Photos/Screenshot_2026-08-13_at_2.31.06_PM.png)

One of the most important headlines referred to a:

> **Catastrophe in Finistère**
> 

Since the postal code had already led us to **Penmarch**, I searched for historical catastrophes in Penmarch/Finistère matching the newspaper's date and description.

This led me to information about the **23 May 1925 catastrophe in Penmarch**.

A useful archive containing information about the event was:

`https://kbcpenmarch.franceserv.com/la-catastrophe-du-23-mai-1925-selon-la-presse-locale.html`

The archive provided additional information about the event and the people involved.

# 6. Identifying the Person

At this point, the clues from the note became much more useful.

The note told us that:

1. The person was **too young to have a driver's license**.
2. He was the **youngest member of the team**.

The historical information about the 23 May 1925 catastrophe listed the members of the relevant team.

![Screenshot 2026-08-13 at 2.51.24 PM.png](/assets/img/TryHackMe-Letter-Photos/Screenshot_2026-08-13_at_2.51.24_PM.png)

Among them was:

> **Gourlaouen Yves Marie**
> 

He was identified as the **youngest member of the team** and was only **15 years old** at the time.

This matches the clues from the note:

- He was under 18, so he would not have been old enough to legally obtain a driver's license.
- He was the youngest member of the team.
- The letter also contains the clue **“Edward.G”**, which helped connect the person mentioned in the note to the **Gourlaouen** family name.

The evidence therefore points to:

**Yves-Marie Gourlaouen — 15 years old**

---

# 7. Final Flag

The required flag format is:

```
THM{Name_Surname_age}
```

Therefore, the final flag is:

```
THM{Yves-Marie_Gourlaouen_15}
```

## Flag

**`THM{Yves-Marie_Gourlaouen_15}`**
