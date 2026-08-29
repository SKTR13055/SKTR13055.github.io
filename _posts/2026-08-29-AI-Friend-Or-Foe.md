---
layout: post
title: "M.Tech Dissertation: AI: Friend or Foe? An Experimental Evaluation of LLM-Assisted Malware
Classification Under Adversarial Conditions"
date: 2026-08-29
tech: ["Cybersecurity", "Research", "AI"]
categories: [Projects]
featured: true
---

# Research

# About the Project

## AI Friend or Foe?

### An Experimental Evaluation  LLM-Based Malware Classification Under Adversarial Payload Transformations

The rapid development of Generative Artificial Intelligence has changed the cybersecurity landscape. Modern AI systems can assist security professionals with code analysis and threat investigation, while the same technology can also be misused to assist in generating or modifying malicious code.

This raised an important question for this research:

> **Can Large Language Models be relied upon to assist with malware classification when the visibility of the underlying payload is progressively reduced?**
> 

This dissertation investigates that question by benchmarking the behaviour of multiple Large Language Models against malware-like payloads under different levels of concealment.

---

## 🔬 Research Foundation

The project was inspired by the research of **Owen Slubowski**, particularly his work on using AI to identify AI-generated malware.

His research demonstrated that LLMs could assist in identifying malicious behaviour when analysing readable source code.

This dissertation builds upon that research by investigating what happens when malware-like payloads are progressively transformed to make their underlying behaviour less visible.

The research does **not** attempt to develop a new malware detection model. Instead, it evaluates the behaviour of existing LLMs when used as **assistive tools for malware classification**.

The first four payload categories used in this research were based on Slubowski's study, while four additional malware-like categories were prepared in a controlled academic environment. The final dataset consisted of **eight payload categories**: keylogger, HTTP covert channel, port scanner, reverse shell, ransomware, DLL injection, brute-force tool, and logic bomb.

---

## 🧪 Experimental Approach

The experiment evaluated **five Large Language Model platforms**:

- ChatGPT
- Google Gemini
- Perplexity
- DeepSeek
- BlackBox AI

The payloads were examined through a progressive transformation pipeline:

```
Plain Payload
      ↓
Identifier Renaming
      ↓
Base92 Encoding
      ↓
AES Encryption
```

Each stage progressively reduced the visibility of the original payload.

The experiment used **three prompt strategies**. These were based on the prompt structure used in previous research rather than being developed entirely from scratch for this dissertation.

The three approaches were:

1. **Prompt 1 – Basic:** Directly requested the function of the program and a malicious/not-malicious classification.
2. **Prompt 2 – Security Analyst Context:** Asked the model to analyse the payload from the perspective of a security analyst.
3. **Prompt 3 – MITRE ATT&CK and Cyber Kill Chain:** Added established cybersecurity frameworks as analytical context.

The purpose was to observe whether changing the amount of security-related context influenced LLM classification behaviour.

---

## 📊 Plain Payload Testing

The plain payload stage established the baseline behaviour of each LLM.

A total of **16 payload instances** were tested with each prompt.

### Performance by Prompt 1

Prompt 1: Basic request: Please describe the function of the attached program. In the
first line, please indicate if this program is “Malicious” or “The file is not malicious”.

| LLM \ Payload | Keylogger (Py) | Keylogger (C++) | HTTP Covert (Py) | Port Scanner (Py) | Reverse Shell (Py) | Ransomware (Py) | Ransomware (Java) | Ransomware (C++) | DLL Injection (Py) | Brute Force (Py) | Brute Force (Java) | Brute Force (C++) | Brute Force (Shell) | Logic Bomb (Py) | Logic Bomb (Java) | Logic Bomb (C++) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ChatGPT-5.3 | Malicious | Malicious | Not Malicious/Can be used Malicious | The file is not malicious | Malicious | Malicious | Malicious | Malicious | Malicious | **Non-Malicious, yet still indicated the file could be used maliciously** | **Non-Malicious, yet still indicated the file could be used maliciously** | **Non-Malicious, yet still indicated the file could be used maliciously** | **Non-Malicious, yet still indicated the file could be used maliciously** | **Non-Malicious, yet still indicated the file could be used maliciously** | **Non-Malicious, yet still indicated the file could be used maliciously** | **Non-Malicious, yet still indicated the file could be used maliciously** |
| Google Gemini 3.1 PRO | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicioius (Classified as Keylogger) | Not Malicious | Malicious |
| Perplexity | Malicious | Malicious | Not Malicious | Not Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Not Malicioius | Not Malicious | Not Malicious | Not Malicious | Malicious (Logic Bomb) | Malicious (Wiper) | Malicious (Logic Bomb) |
| Deep Seek -V3 | Malicious | Malicious | Not Malicious/Can be used malicious | Not Malicious/ Can be used malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Not Malicious/ Can be used as Malicious | Not Malicious/ can be used as malicious | Not malicious/Can be used maliciously | Not Malicious/ Can be used maliciously | Malicious | Malicious | Malicious (Wiper) |
| Black Box 1.0 | Malicious | Malicious | Not Malicious/Could be used Maliciously | Not Malicious | Malicious (RAT) | Not Malicious | Malicious(Destructive) | Malicious | Malicious | Not Malcious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious (100% Safe) |

| LLM | Malicious Classifications |
| --- | --- |
| ChatGPT | **7/16** |
| Gemini | **14/16** |
| Perplexity | **9/16** |
| DeepSeek | **10/16** |
| BlackBox | **5/16** |

ChatGPT showed a relatively balanced approach and was more likely to recognize dual-use situations. Gemini produced the most aggressive malicious classification pattern, while BlackBox produced the lowest malicious classification rate under this prompt.

### Performance by Prompt 2

Prompt 2: Request with medium context: You are a security analyst at my company
tasked with securing our systems. Please analyze the attached file and report on its
functions, then please decide if this program is “Malicious” or “The file is not malicious”.

| LLM \ Payload | Keylogger (Py) | Keylogger (C++) | HTTP Covert (Py) | Port Scanner (Py) | Reverse Shell (Py) | Ransomware (Py) | Ransomware (Java) | Ransomware (C++) | DLL Injection (Py) | Brute Force (Py) | Brute Force (Java) | Brute Force (C++) | Brute Force (Shell) | Logic Bomb (Py) | Logic Bomb (Java) | Logic Bomb (C++) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ChatGPT-5.3 | Malicious | Malicious | Not Malicious | Not Malicious | Malicious | Malicious | Malicious | Potentially Malicious | Malicious | Not Malicious/Can be used Maliciously | Not Malicious/Can be used Maliciously | Not Malicious/Can be used Maliciously | Not Malicious/Can be used Maliciously | Potentially Malicious | Potentially Malicious | Potentially Malicious |
| Google Gemini 3(Flash/3.1 PRO) | Malicious | Malicious | Malicious | Malicious | Malicious | malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious |
| Perplexity | Malicious | Malicious | Not Malicious | Not Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Malicious | Malicious | Malicious |
| Deep Seek -V3 | Malicious | Malicious | Malicious | Malicious | Malicious | Not Malicious | Malicious | Malicious | Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Malicious | Malicious | Malicious |
| Black Box 1.0 | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious |

| LLM | Malicious Classifications |
| --- | --- |
| ChatGPT | **7/16** |
| Gemini | **16/16** |
| Perplexity | **10/16** |
| DeepSeek | **11/16** |
| BlackBox | **16/16** |

Adding security-analyst context increased malicious classifications for several models. Gemini and BlackBox classified all 16 samples as malicious under this prompt.

### Performance By Prompt 3

Prompt 3:  A highly specific prompt: You are a security analyst at my company
tasked with securing our systems. Using the MITRE Cyber Kill Chain and Att&ck
framework as a guide to help you classify, please analyze the attached file and report on its functions; then, please decide if this program is "Malicious." If it is, please provide a brief statement as to why, and if not, simply state “The File is not malicious”.

| LLM \ Payload | Keylogger (Py) | Keylogger (C++) | HTTP Covert (Py) | Port Scanner (Py) | Reverse Shell (Py) | Ransomware (Py) | Ransomware (Java) | Ransomware (C++) | DLL Injection (Py) | Brute Force (Py) | Brute Force (Java) | Brute Force (C++) | Brute Force (Shell) | Logic Bomb (Py) | Logic Bomb (Java) | Logic Bomb (C++) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ChatGPT-5.3 | Malicious | malicious | Malicious | Not Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Potentially Malicious (Dual use for all Bruteforce) | Potentially Malicious | Potentially Malicious | Potentially Malicious | potentially malicious or destructive dual-use utility | potentially malicious or destructive dual-use utility | potentially malicious or destructive dual-use utility |
| Google Gemini 3(Flash/3.1 PRO) | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious |
| Perplexity | Malicious | Malicious | Malicious | Not Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Malicious | Malicious | Malicious |
| Deep Seek -V3 | Malicious | Malicious | Not Malicious | Not Malicious | Malicious | Not Malicious | Malicious | Malicious | Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Malicious | Malicious | Malicious |
| Black Box 1.0 | Malicious | Malicious | Malicious | Not Malicious | Malicious | Not Malicious | Malicious | Malicious | Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Malicious | Malicious | Malicious |

| LLM | Malicious Classifications |
| --- | --- |
| ChatGPT | **8/16** |
| Gemini | **16/16** |
| Perplexity | **11/16** |
| DeepSeek | **9/16** |
| BlackBox | **10/16** |

Prompt 3 produced a more security-focused classification pattern. Gemini again classified all 16 samples as malicious, while the other models continued to show different levels of caution and selectivity.

These results demonstrated that **the wording and context of the prompt could influence the classification produced by an LLM**, particularly when dealing with dual-use or less directly malicious payloads.

---

## 🔀 Identifier-Renaming Obfuscation

After the plain-payload testing, four payloads that had consistently been detected were selected for identifier-renaming obfuscation.

Meaningful variable and function names were replaced with less descriptive identifiers while keeping the underlying functionality unchanged.

The result was particularly interesting:

| Payload | Language | ChatGPT | Gemini | Perplexity | DeepSeek | Blackbox |
| --- | --- | --- | --- | --- | --- | --- |
| Keylogger | Python | Malicious | Malicious | Malicious | Malicious | Malicious (**Minecraft info stealer/logger** |
| Reverse Shell | Python | Malicious | Malicious | Malicious | Malicious | Malicious |
| Ransomware | Java | Malicious | Malicious | Malicious | Malicious | Malicious |
| DLL Injection | Python | Malicious | Malicious | Malicious | Malicious | **Malicious active post-exploitation malware** |

| LLM | Malicious Classifications |
| --- | --- |
| ChatGPT | **4/4** |
| Gemini | **4/4** |
| Perplexity | **4/4** |
| DeepSeek | **4/4** |
| BlackBox | **4/4** |

**Obfuscation Results Summary:**

- 4/4 obfuscated payloads: LLMs deobfuscated and correctly identified as malicious
- Unexpected finding: DeepSeek Ransomware(Py) FLIPPED from "Not malicious" → "Malicious" after obfuscation ( Prompt 3 + Python_ransomware_Obfuscated)
- Key insight: LLMs analyze BEHAVIOR + structure, not just readable names
- Obfuscation makes "demo code" look more suspicious to some models

**Updated research claim:**
"While LLMs demonstrate impressive deobfuscation capabilities on identifier-renamed malware, model-specific behaviors reveal complex interactions between code readability, obfuscation, and threat perception."

---

## 🔐 Base92 Encoding

The next stage applied **Base92 encoding** to selected obfuscated payloads.

Only four payload samples were tested at this stage.

| LLM | Malicious Classifications |
| --- | --- |
| ChatGPT | **0/4** |
| Gemini | **4/4** |
| Perplexity | **2/4** |
| DeepSeek | **4/4** |
| BlackBox | **4/4** |

The results showed a much stronger difference between the models.

ChatGPT classified none of the four Base92 samples as malicious, while Gemini, DeepSeek, and BlackBox classified all four as malicious. Perplexity produced an intermediate result.

This indicated that Base92 encoding reduced the semantic visibility of the payload and produced substantially greater variation between the models.

---

## 🔒 AES Encryption

AES encryption represented the highest concealment stage of the main experiment.

The original payload logic was no longer directly visible to the LLMs. The models therefore had substantially less direct information available for determining the underlying behaviour.

The results showed the greatest variation across the models.

### AES –  Performance By Prompt 1

Please describe the function of the attached program. In the
first line, please indicate if this program is “Malicious” or “The file is not malicious”.

| LLMs/Payload | Keylogger (Py) (File_1) | Keylogger (C++) (File_1) | HTTP Covert (Py) (File_2) | Port Scanner (Py) (File_3) | Reverse Shell (Py) (File_4) | Ransomware (Py) (File_5) | Ransomware (Java) (File_5) | Ransomware (C++) (File_5) | DLL Injection (Py) (File_6) | Brute Force  (File_7) 1 | Brute Force  (File_7) File 2 | Brute Force  (File_7) 3 | Brute Force (File_7)4 | Logic Bomb (File_8) 1 | Logic Bomb  (File_8) 2 | Logic Bomb (File_8) 3 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ChatGPT | Not Malicious/Could be malicious | Not Malicious/Could be malicious | **The file is not malicious**, though its concealed contents remain undetermined due to encryption/obfuscation. | **The file is not malicious**, though it contains opaque encoded data whose concealed contents cannot be fully verified without additional context or decoding information. | Same as previous | Not Malicious | Not Malicious | Not Malicious | Malware(Due to High Entropy) | Malicious | Not Malicious | Not Malicious | Malicious (Large, high Entropy) | Malicious | Malicious | Not Malicious |
| Gemini Pro | Not Malicous | Not malicious | Not malicious | Not malicious (it does not pose a direct malicious threat.) | Not Malicious | Not Malicious | Not malicious | Not Malicious | Not Malicious | Not Malicious  | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious |
| Perplexity | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious |
| Deep Seek | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not malicious | Not Malicious | Not Malicious |
| Blackbox | Not Malicious(Bitcoin Private Key) | Not Malicious(Bitcoin Private Key) | Not Malicious ( Bitcoin Private Key) | Not Malicious(Bitcoin Private key) | Not Malicious(Bitcoing Private Key) | Not malicious (same as last time) | Not Malicious | **It IS malicious**. This is a deliberate **steganographic malware dropper {Not even close }** | Not Malicious | Not malicious | Not Malicious | Not Malicious | Not malicious | Not Malicious | Not Malicious | Not Malicious |

| LLM | Malicious Classifications |
| --- | --- |
| ChatGPT | **5/16** |
| Gemini | **0/16** |
| Perplexity | **16/16** |
| DeepSeek | **0/16** |
| BlackBox | **0/16** |

Perplexity classified every encrypted sample as malicious, while Gemini, DeepSeek, and BlackBox classified all samples as non-malicious under this prompt. ChatGPT produced a mixed response.

### AES – Performance By Prompt 2

You are a security analyst at my company
tasked with securing our systems. Please analyze the attached file and report on its functions, then please decide if this program is “Malicious” or “The file is not malicious”.

| LLMs/Payload | Keylogger (Py) (File_1) | Keylogger (C++) (File_1) | HTTP Covert (Py) (File_2) | Port Scanner (Py) (File_3) | Reverse Shell (Py) (File_4) | Ransomware (Py) (File_5) | Ransomware (Java) (File_5) | Ransomware (C++) (File_5) | DLL Injection (Py) (File_6) | Brute Force (Py) (File_7) | Brute Force (Java) (File_7) | Brute Force (C++) (File_7) | Brute Force (Shell) (File_7) | Logic Bomb (Py) (File_8) | Logic Bomb (Java) (File_8) | Logic Bomb (C++) (File_8) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ChatGPT | Malicious | Malicious | Malicious | Not Malicious | Not malicious (based strictly observable evidence) | Potentially Malicious until fully analyzed | Suspicious/ Potentially malicious | Malicious | Malicious | Malicious | Potentially Malicious | Potentially Malicious / Suspicious | Suspicious / Potentially Malicious | Suspicious / Potentially Malicious | Malicious | Malicious |
| Gemini Pro | Malicious(Needed SHA-256 Hash) | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious |
| Perplexity | Not Malicious (Needed SHA-256 Hash) | Not Malicious | Not Malicious | Not Malicious | Not Malicious (Current Public intelligence) | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | File not Malicious |
| Deep Seek | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious |
| Blackbox | Malicious | **Likely Malicious (but requires professional analysis)** | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious  | Malicious | Malicious | Malicious | Malicious |

| LLM | Malicious Classifications |
| --- | --- |
| ChatGPT | **8/16** |
| Gemini | **16/16** |
| Perplexity | **0/16** |
| DeepSeek | **16/16** |
| BlackBox | **15/16** |

The addition of security-analyst context substantially changed the behaviour of several models. Gemini and DeepSeek moved to complete malicious classification, while Perplexity moved to complete non-malicious classification.

### AES – Performance By Prompt 3

You are a security analyst at my company
tasked with securing our systems. Using the MITRE Cyber Kill Chain and Att&ck
framework as a guide to help you classify, please analyze the attached file and report on its functions; then, please decide if this program is "Malicious." If it is, please provide a brief statement as to why, and if not, simply state “The File is not malicious”.

| LLMs/Payload | Keylogger (Py) (File_1) | Keylogger (C++) (File_1) | HTTP Covert (Py) (File_2) | Port Scanner (Py) (File_3) | Reverse Shell (Py) (File_4) | Ransomware (Py) (File_5) | Ransomware (Java) (File_5) | Ransomware (C++) (File_5) | DLL Injection (Py) (File_6) | Brute Force (Py) (File_7) | Brute Force (Java) (File_7) | Brute Force (C++) (File_7) | Brute Force (Shell) (File_7) | Logic Bomb (Py) (File_8) | Logic Bomb (Java) (File_8) | Logic Bomb (C++) (File_8) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ChatGPT | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious |
| Gemini Pro | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious |
| Perplexity | **The File is not malicious** (as presented). {**no definitive MITRE Kill Chain / ATT&CK classification can be made**.} | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious | Not Malicious |
| Deep Seek | Likely Malicious | Likely Malicious | Likely Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious |
| Blackbox | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious | Malicious |

| LLM | Malicious Classifications |
| --- | --- |
| ChatGPT | **16/16** |
| Gemini | **16/16** |
| Perplexity | **0/16** |
| DeepSeek | **13/16** |
| BlackBox | **16/16** |

Prompt 3 produced the strongest malicious classification pattern for most of the models. ChatGPT, Gemini, and BlackBox classified all 16 encrypted samples as malicious, while DeepSeek classified 13/16 as malicious and Perplexity classified none as malicious.

Importantly, these classifications should **not** be interpreted as successful decryption or direct analysis of the original malware logic. Because the payloads were encrypted, some classifications appeared to rely on characteristics such as unusual structure, randomness, limited readable content, security context, or other indirect indicators.

---

## 🧪 Benign AES Control

To determine whether encrypted content could also produce malicious classifications when it was actually harmless, a **Benign AES Control** was introduced.

A harmless English story was encrypted using AES without first being transformed through the malware-payload obfuscation pipeline.

The same encrypted benign sample was then evaluated using Prompt 3.

| LLM | Result |
| --- | --- |
| ChatGPT | **False Positive** |
| Gemini | **Highly Suspicious / Presumed Malicious** |
| Perplexity | **False Positive** |
| DeepSeek | **Not Malicious** |
| BlackBox | **False Positive** |

Four of the five models therefore produced a malicious or suspicious interpretation of the benign encrypted content, while DeepSeek correctly classified it as non-malicious.

This experiment was important because it demonstrated that **encrypted content itself can become a source of false-positive behaviour**. However, because only one benign sample was tested, the result should be considered a preliminary observation rather than a statistically general conclusion.

---

## 🧠 What Did the Experiment Show?

The overall results revealed several important patterns.

### 1. Readable payloads were easier to classify

When the source code was visible, the models had more information available for analysing the actual behaviour.

### 2. Identifier renaming had limited impact

All five models detected the four selected obfuscated samples, resulting in **4/4 malicious classifications for every model**.

### 3. Encoding created greater variation

Base92 encoding produced much greater disagreement between models, with results ranging from **0/4 to 4/4** malicious classifications.

### 4. Encryption produced the greatest variation

AES encryption produced dramatically different results depending on both the LLM and the prompt.

For example, under AES Prompt 1, Perplexity classified **16/16** samples as malicious while Gemini, DeepSeek, and BlackBox classified **0/16** as malicious. Under Prompt 3, ChatGPT, Gemini, and BlackBox moved to **16/16**, while Perplexity remained at **0/16**.

### 5. Prompt context influenced classification

Moving from a basic prompt to security-oriented prompts changed classification behaviour for several models. The effect was not uniform across every LLM.

### 6. Encrypted benign content could produce false positives

The Benign AES Control demonstrated that an encrypted harmless sample could still be interpreted as malicious or suspicious by several models.

---

## 🤖 Comparative LLM Behaviour

The models did not behave identically throughout the experiment.

| LLM | Behaviour Observed |
| --- | --- |
| **ChatGPT** | Balanced and context-sensitive; demonstrated awareness of dual-use cases |
| **Gemini** | Highly aggressive in malicious classification, especially under security-oriented prompts |
| **Perplexity** | More dependent on threat-intelligence or hash-based reasoning in some encrypted cases |
| **DeepSeek** | Contextual and cautious; sometimes treated unverifiable encoded content conservatively |
| **BlackBox AI** | Inconsistent; detected malicious intent but sometimes misidentified malware types or transformation characteristics |

These observations are based on the comparative behaviour documented in the dissertation.

---

## 🧩 AI: Friend or Foe?

The title **"AI Friend or Foe?"** represents the central question behind the research.

AI itself is neither inherently a friend nor a foe.

It can be used by defenders to assist with analysis, while adversaries can also attempt to use generative AI to create or modify malicious code.

The research therefore focuses on a more important question:

> **How reliable is AI when we depend on it for cybersecurity analysis?**
> 

The experimental results suggest that LLMs can provide useful assistance, particularly when the underlying code is visible and interpretable. However, their responses can vary substantially when payload visibility decreases.

This means an LLM classification should not automatically be treated as definitive malware analysis.

---

## ⚙️ Automation Prototype

The project also includes an automation prototype exploring how this type of benchmarking could eventually be automated.

The proposed workflow is:

```
Payload
   ↓
Prompt Selection
   ↓
LLM Submission
   ↓
Response Collection
   ↓
Classification
   ↓
Result Storage
   ↓
Analysis
```

The prototype was **developed and explored as a proof of concept**, but it was not used to generate the final experimental results.

The final testing was conducted manually because practical constraints included **API token costs, rate limits, platform restrictions, model availability, and safety controls**.

The prototype therefore represents a possible starting point for future researchers interested in creating a fully automated LLM benchmarking platform.

---

## 🎯 Research Contribution

The primary contribution of this dissertation is the **experimental benchmarking and analysis** of LLM-assisted malware classification under progressively reduced payload visibility.

The project builds upon existing research rather than claiming to have invented the underlying prompt methodology or evaluation approach.

The research extends the experimental setting by:

- Evaluating five LLM platforms.
- Testing eight malware-like payload categories.
- Representing payloads across multiple programming languages.
- Testing plain payloads under three prompt conditions.
- Applying identifier-renaming obfuscation.
- Applying Base92 encoding.
- Applying AES encryption.
- Introducing a benign AES control.
- Comparing classification behaviour across transformation stages.
- Documenting differences in model behaviour.

The purpose is to provide a controlled experimental foundation that can be reproduced, challenged, and extended by future researchers.

Link to the Repo : https://github.com/SKTR13055/AI-Friend-or-Foe

---

## ⚠️ Responsible Research

All malware-like samples included in this research were intended strictly for controlled academic and cybersecurity research purposes.

The purpose of this project is to evaluate AI-assisted malware classification and **not to develop, distribute, or deploy operational malware or to attack real-world systems**.

Any reproduction or experimentation based on this research should be performed only in an **isolated, controlled, and properly authorized research environment**.

Researchers should never execute or deploy the experimental payloads against systems, networks, devices, accounts, or services without explicit authorization.

**The author/owner of this repository does not encourage or authorize the use of these materials for malicious or unlawful activities. The author/owner is not responsible for any misuse of the materials, including unauthorized execution, modification, deployment, distribution, or use of the payloads by third parties. Users are solely responsible for ensuring that their use of the repository and its contents is lawful, ethical, and properly authorized.**

By accessing or using the materials in this repository, users acknowledge that they assume responsibility for their own actions and for maintaining an appropriate controlled environment when conducting security research.

**Use responsibly. Research ethically. Test only where you have permission.**

---

## 🚀 Future Research

The results also identify several opportunities for future research.

Future researchers could investigate:

- Larger and more diverse malware datasets.
- Additional LLM platforms and newer model versions.
- Automated API-based experimentation.
- Dynamic malware analysis.
- Sandbox integration.
- Static analysis alongside LLM analysis.
- YARA-based comparison.
- Entropy-based analysis.
- Behavioural analysis.
- Larger benign control datasets.
- More advanced transformation techniques.
- Automated statistical analysis and visualization.

In particular, repeating the experiments with newer LLM versions could determine whether improvements in model reasoning and security controls lead to more reliable classification under obfuscated, encoded, and encrypted conditions.

---

## 🙏 Acknowledgement

This research was inspired by the work of **Owen Slubowski,** whose research into AI-assisted malware analysis provided an important foundation for this dissertation.

The objective of this project is to build upon existing research and provide a foundation that future researchers can reproduce, evaluate, improve, and extend.

---

## 🤝 Contribution to Future Researchers

This repository is intended to provide a starting point for future academic and cybersecurity research.

Future researchers and developers are encouraged to **contribute to and improve the automation prototype(main.py)**. The existing prototype can be extended with better API integration, automated response collection, improved result processing, support for additional LLM platforms, and more efficient experimental workflows.

Such improvements could make the testing process easier, more repeatable, and more efficient for future projects investigating LLM-assisted malware analysis.

Possible areas for contribution include:

- Improving and expanding the existing automation code.
- Adding support for additional LLM APIs and platforms.
- Automating response collection and classification.
- Improving result storage and analysis.
- Adding support for larger datasets.
- Improving error and rate-limit handling.
- Developing better visualization and reporting features.
- Extending the workflow for future benchmarking experiments.

**Contributions are welcome.** If you improve the code or develop additional functionality, consider contributing it back to the repository so that future researchers can build upon the work rather than starting from scratch.

All contributions should remain focused on **authorized, ethical, and academic cybersecurity research**.

Link to the Repo: https://github.com/SKTR13055/AI-Friend-or-Foe


## Final Thoughts

This project reinforced an important lesson:

> **AI should be treated as an assistive technology in cybersecurity, not as an unquestionable authority.**
> 

The experiments demonstrated that LLM responses can change depending on the **model, prompt context, payload representation, and amount of information available for analysis**.

As AI becomes increasingly integrated into cybersecurity workflows, understanding these limitations will be essential for using it responsibly and effectively.
