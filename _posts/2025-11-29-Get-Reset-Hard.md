---
title: "GlacierCTF – GetResetHard"
date: 2025-11-29 18:00:00 +0530
categories: [Writeups, GlacierCTF]
tags: [git, forensics, misc, glacierctf]
image:
  path: /assets/img/headers/glacierCTF.png
  alt: GlacierCTF Banner
---

# Description

1. Kevin joined our company.
2. Kevin took a s**t on the carpet.
3. Kevin `git reset --hard` the entire repo.
4. Kevin force pushed.
5. Kevin left the company.

Now it's your turn to fix the mess. You get the compressed disk with the repository.

> **Note:** The repo is completely safe.

**Downloadable file:** `gitresethard.tar.gz`

---

## Process

1. Download the `gitresethard.tar.gz`.

2. Extract the compressed file using the command below.
    ```bash
    tar -xzvf gitresethard.tar.gz
    ```

3. After decompressing the file, change the directory to `gitresethard/repo`.

4. Type in the following command to reveal any hidden/dangling commits:
    ```bash
    git fsck --lost-found
    ```
    ![Dangling Commit Found](/assets/img/getresethard-photos/40d37ebf-657a-4e39-901f-fb347f466bd4.png)

5. A hidden/dangling commit is present. Let's see the contents using the `git show` command.
    ```bash
    git show <dangling commit_id>
    ```
    ![Git Show Output](/assets/img/getresethard-photos/e28271b2-4d9e-49d3-b4f8-4d1126199cb9.png)

6. The dangling commit shows a connection using `openssl` and also includes a password which is being encrypted. I chose to use `echo` to pipe it directly.
    ![Decryption Process](/assets/img/getresethard-photos/8e8e57cf-843e-4aaf-bfa6-67c15cc161f5.png)

7. After running the command, we got the flag!

> **Flag:** `gctf{0113_wh0_g1t_r3s3t3d_th3_c4t_4789}`


---

### Summary

| Step | Action |
| :--- | :--- |
| **1** | Extract the repo archive |
| **2** | Run `git fsck` to detect orphaned objects |
| **3** | Identify dangling commit `6a81c76e…` |
| **4** | View its contents with `git show` |
| **5** | Find OpenSSL encryption clues |
| **6** | Recreate decryption using `echo` → `openssl` |
| **7** | Recover the flag |
