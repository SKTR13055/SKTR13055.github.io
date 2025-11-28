---
layout: page
title: Know Your Bases
permalink: /references/know-your-bases/
---

| **Encoding** | **Sample / Text** | **Where is it used?** | **How to spot it?** |
| :--- | :--- | :--- | :--- |
| **Base 2 (Binary)** | `01000111 01110010` | Machine code, logic gates. | Only uses 2 symbols: **0s and 1s**. |
| **Base 8 (Octal)** | `116 151 143 145` | Linux file permissions (e.g., 755). | Digits **0-7**. Often has leading zeros. |
| **Base 10 (Decimal)** | `089 111 117 032` | Everyday math, IP addresses. | Digits **0-9**. Looks like normal numbers. |
| **Base 16 (Hex)** | `41 77 65 73 6f` | Memory dumps, Color codes. | Digits **0-9** and letters **A-F**. |
| **Base 32** | `I5XW6ZBAO5XXE...` | Google Authenticator, DNS tunneling. | **A-Z** and **2-7**. Uppercase. Ends with `=`. |
| **Base 36** | `4q9w5s` | URL shortening, compacting IDs. | **0-9** and **a-z**. Case-insensitive. No symbols. |
| **Base 45** | `K19X CSUEWQE...` | QR codes for COVID Certs. | Alphanumeric + symbols: `$ % * + - . / :`. |
| **Base 58** | `1A1zP1eP5QGefi...` | Bitcoin addresses. | Like Base64 but **NO** `0 O I l + /`. No padding (`=`). |
| **Base 62** | `ObsJmP173N2X...` | URL Shorteners (bit.ly). | **A-Z**, **a-z**, **0-9**. No symbols, no padding. |
| **Base 64** | `V2VsbCBkb25lIS...` | Email, Web traffic, Basic Auth. | **A-Z**, **a-z**, **0-9**, `+`, `/`. Ends with `=` or `==`. |
| **Base 85** | `<~:2+3L+EqaE...~>` | Adobe PDF, PostScript. | Starts with `<~`. Uses many symbols. |
| **Base 92** | `@D_<sB5GVmj...` | Internal data storage. | Wide characters but **no whitespace**. |
| **Rot13** | `Uryyb Jbeyq` | CTFs, Spoilers. | Text looks readable but "jumbled". Shifted by 13 letters. |
| **Base 65536** | `𖡅驣ꍬ𐙥啴...` | Obfuscation, Unicode mapping. | Wild mix of Unicode glyphs (Asian chars, emojis). |

> **Pro Tip:** If you see `1`, `l`, `I`, `0`, and `O` mixed together, it is **Base 64**. If those confusing characters are missing, it is likely **Base 58**.
