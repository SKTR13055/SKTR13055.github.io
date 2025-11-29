---
title: "GlacierCTF – GetResetHard"
date: 2025-11-29 18:00:00 +0530
categories: [GlacierCTF, Writeups]
tags: [ctf, git, git-forensics, misc, writeup]
image:
  path: /assets/img/headers/glacierCTF.png
  alt: GlacierCTF
---
# GlacierCTF-Get Reset Hard

Author: ecomaikgolf.com

# Description

1. Kevin joined our company.
2. Kevin took a s**t on the carpet.
3. Kevin git reseted --hard the entire repo.
4. Kevin force pushed.
5. Kevin left the company.

Now its your turn to fix the mess. You get the compressed disk with the repository.

Note: the repo is completely safe

Downloadable file: gitresethard.tar.gz

---

## Process

1. Download the gitresethard.tar.gz
2. Extract the compressed file using the command below.

```bash
tar -xzvf gitresethard.tar.gz”
```

1. After decompressing the file change the directory to “gitresethard/repo” 
2. Type in the following command to reveal any hidden/dangling commit

```bash
git fsck --lost-found
```

![Screenshot 2025-11-23 at 7.52.45 AM.png](/assets/img/getresethard-photos/40d37ebf-657a-4e39-901f-fb347f466bd4.png)

1. A hidden/dangling commit is present, lets see the contents using the “git show” command.

```bash
git show <dangling commit>
```

![Screenshot 2025-11-23 at 7.52.45 AM.png](/assets/img/getresethard-photos/e28271b2-4d9e-49d3-b4f8-4d1126199cb9.png)

1. The dangling commit shows a connection using “openssl” and also includes a password which is being encrypted so lets try to use command., we could use ssl but I choose to use “echo” command for faster process.

![Screenshot 2025-11-23 at 7.52.54 AM.png](/assets/img/getresethard-photos/8e8e57cf-843e-4aaf-bfa6-67c15cc161f5.png)

1. After using the command we got the flag.

<aside>

Flag: gctf{0113_wh0_g1t_r3s3t3d_th3_c4t_4789}

</aside>

### Summary

| Step | Action |
| --- | --- |
| **1** | Extract the repo archive |
| **2** | Run `git fsck` to detect orphaned objects |
| **3** | Identify dangling commit `6a81c76e…` |
| **4** | View its contents with `git show` |
| **5** | Find OpenSSL encryption clues |
| **6** | Recreate decryption using `echo` → `openssl` |
| **7** | Recover the flag |