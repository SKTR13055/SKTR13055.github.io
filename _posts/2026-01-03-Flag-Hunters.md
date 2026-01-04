---
title: "PicoCTF – Flag Hunters"
date: 2026-01-03 20:20:00 +0530
categories: [PicoCTF,Reverse Engineering]
tags: [Reverse_Engineering,python,Control_flow_Hijacking,Python_Reverse_Engineering,Interpreter_Exploitation,User_Input_Vulnerability]
image:
  path: /assets/img/headers/picoctf-banner.jpg
  alt: PicoCTF Banner
---

# Flag Hunters

Challenge_Author: syreal

Category: Reverse Engineering

Description
---

Lyrics jump from verses to the refrain kind of like a subroutine call. There's a hidden refrain this program doesn't print by default. Can you get it to print it? There might be something in it for you.The program's source code can be downloaded [here](https://challenge-files.picoctf.net/c_verbal_sleep/ababdc2ebe3635578b92685dfa86f09962d7d64ff3b0fda327e8bbe93fc08764/lyric-reader.py). 

Process
---

1. Before Starting the Instance, I downloaded the provided Python file.
2. After downloading the file, I examined the contents. The file was a non-executable Python script, so I made it executable using the following command:

    ```bash
    chmod +x lyric-reader.py
    ```

    ![Screenshot 2026-01-03 at 7.26.41 PM.png](/assets/img/Flag-Hunters-Photos/Screenshot_2026-01-03_at_7.26.41_PM.png)

3. Now Executing the python file reveals the working of the program.

    ![Screenshot 2026-01-03 at 7.27.24 PM.png](/assets/img/Flag-Hunters-Photos/Screenshot_2026-01-03_at_7.27.24_PM.png)

4. Running the program initially revealed that it expected a `flag.txt` file. Since this file was missing, the program printed an error message. To continue testing locally, I created a **placeholder `flag.txt`** file so that the program would execute normally.
5. Now lets run the program in order to understand the working.

    ![Screenshot 2026-01-03 at 7.29.31 PM.png](/assets/img/Flag-Hunters-Photos/Screenshot_2026-01-03_at_7.29.31_PM.png)

6. Upon execution, the program prints a song line by line. At a certain point—during the **“Crowd”** section—it prompts the user for input. To observe how the program behaves, I initially entered a harmless string like: woo
7. The song continued and ended normally, with no hidden output displayed. This confirmed that **user input directly affects execution flow**, rather than being a simple text prompt.

    ![Screenshot 2026-01-03 at 7.31.10 PM.png](/assets/img/Flag-Hunters-Photos/Screenshot_2026-01-03_at_7.31.10_PM.png)

  Examining the source code revealed several important behaviors:

8. At the beginning of the code, there is a **hidden intro section** that reads from `flag.txt`. This section is *not printed during normal execution*.

  

    ![Screenshot 2026-01-03 at 7.31.27 PM.png](/assets/img/Flag-Hunters-Photos/Screenshot_2026-01-03_at_7.31.27_PM.png)

9. The program uses a custom instruction interpreter that:

    - Splits lyric lines using `;`
    - Iterates through them using an instruction pointer.
    - Supports control-flow commands such as `RETURN <number>`
The most critical discovery was this logic:

    ```python
    elif re.match(r"RETURN [0-9]+", line):
    ```

  If a `RETURN` instruction is encountered, the program **jumps to a specific instruction index**, similar to a function return or a jump instruction in assembly.

  This means:

  If user input contains a valid RETURN instruction, it can redirect execution
10. Re running the program, in order to test my solution.

   ![Screenshot 2026-01-03 at 7.33.06 PM.png](/assets/img/Flag-Hunters-Photos/Screenshot_2026-01-03_at_7.33.06_PM.png)

 

   <aside>

   Note: The above picture is not the original flag but just the mimic of the original flag

   </aside>

11. After Running the program, the program revealed the flag ( Not the original flag, just a place holder).
12. After starting the challenge instance on PicoCTF, I entered the following input when prompted.

    ![Screenshot 2026-01-03 at 7.35.32 PM.png](/assets/img/Flag-Hunters-Photos/Screenshot_2026-01-03_at_7.35.32_PM.png)

13. This successfully redirected execution to the hidden intro, revealing the flag

    ```
     picoCTF{70637h3r_f0r3v3r_c373964d}
    ```

Conclusion
---

This challenge demonstrates a classic reverse-engineering and exploitation concept:

- User input is treated as **executable instructions**
- Control over the instruction pointer enables unintended behavior
- A seemingly harmless input prompt becomes an execution vector

 

Key Takeaway
---

If user input controls program execution flow, the program is vulnerable.

This mirrors real-world exploitation techniques such as:

- Instruction pointer hijacking
- Return-oriented programming
- Interpreter abuse

Thank you to **syreal** and the **PicoCTF team** for creating such a clever and educational challenge. This problem beautifully bridges high-level Python logic with low-level reverse-engineering concepts, making it both fun and insightful see you in the next writeup.
