# REDHAT-Training-week1
week 1 lesson and lab for Red Hat Linux exam




Absolutely. I remember where we left off: you had worked through navigation, redirection (`>` vs. `>>`), `diff`, and the Week 1 final-lab directory structure. I couldn’t recover the exact original Week 1 message verbatim, but I can reconstruct the lesson from the material and exercises we used.

# RHCSA Week 1 — Linux Command-Line Foundations

**Goal:** Build enough command-line confidence that I can navigate a RHEL system, create and manipulate files/directories, inspect files, redirect output, and verify my work without relying on the GUI.

This is deliberately hands-on because RHCSA is a performance-based exam.

## Lesson 1 — Know Where I Am

Start as root or from your RHEL lab VM.

```bash
whoami
hostname
pwd
```

What these tell me:

* `whoami` — which account I am using
* `hostname` — which Linux server I am working on
* `pwd` — my current directory

Now:

```bash
ls
ls -l
ls -la
ls -lh
ls -lah
```

The important one to become comfortable with is:

```bash
ls -lah
```

It shows hidden files, permissions, ownership, sizes in human-readable format, and modification dates.

---

# Lesson 2 — Navigate the Linux Filesystem

Practice:

```bash
cd /
pwd
ls

cd /etc
pwd

cd /var/log
pwd
ls

cd /home
pwd

cd ~
pwd
```

Now learn these shortcuts:

```bash
cd ..
cd -
cd ~
cd /
```

They mean:

| Command | Meaning                      |
| ------- | ---------------------------- |
| `cd ..` | Go up one directory          |
| `cd -`  | Return to previous directory |
| `cd ~`  | Go to my home directory      |
| `cd /`  | Go to filesystem root        |

You previously correctly identified that:

```bash
cd ..
```

takes you **up one level**.

---

# Lesson 3 — Create Directories and Files

Let's build a practice environment.

```bash
cd /root
mkdir rhcsa-labs
cd rhcsa-labs

mkdir week1
cd week1

pwd
```

Create some directories:

```bash
mkdir documents
mkdir backups
mkdir logs
```

Check them:

```bash
ls -l
```

Now create files:

```bash
touch server1.txt
touch server2.txt
touch notes.txt
```

Check:

```bash
ls -l
```

The basic file-management commands we practiced were `touch`, `cp`, `mv`, `rm`, `mkdir`, and `rmdir`.

---

# Lesson 4 — Copy, Move, Rename and Delete

### Copy

```bash
cp server1.txt documents/
```

Verify:

```bash
ls documents/
```

### Rename

```bash
mv server2.txt linux-server.txt
```

Check:

```bash
ls
```

### Move

```bash
mv linux-server.txt documents/
```

### Delete

```bash
rm notes.txt
```

Verify:

```bash
ls
```

An important distinction:

```bash
cp
```

**copies** something.

```bash
mv
```

can either **move** something or **rename** it.

---

# Lesson 5 — Put Information into Files

This was an especially important part of our previous lesson.

Run:

```bash
echo "Red Hat Linux Training" > training.txt
```

Then:

```bash
cat training.txt
```

Now:

```bash
echo "RHCSA Week 1" > training.txt
```

And:

```bash
cat training.txt
```

Notice what happened.

The original line disappeared because:

```bash
>
```

**overwrites the destination file.**

Now try:

```bash
echo "Red Hat Enterprise Linux" >> training.txt
```

Then:

```bash
cat training.txt
```

You previously explained this correctly:

> `>` directs information to a destination file, while `>>` adds text without overwriting the existing contents.

That's exactly the distinction I want you to remember for RHCSA.

---

# Lesson 6 — Read Files

Practice:

```bash
cat training.txt
less training.txt
head training.txt
tail training.txt
```

A particularly useful command for logs is:

```bash
tail -10 /var/log/messages
```

On systems using the systemd journal, we'll later spend significant time with:

```bash
journalctl
```

For now, I want you comfortable with `cat`, `less`, `head`, and `tail`.

---

# Lesson 7 — Find Things

Create some practice files:

```bash
cd /root/rhcsa-labs/week1

touch report.txt
touch server.log
touch application.conf
```

Now:

```bash
find /root/rhcsa-labs -name "report.txt"
```

Try:

```bash
find /root/rhcsa-labs -name "*.txt"
```

And:

```bash
find /root/rhcsa-labs -name "*.conf"
```

This introduces wildcard matching.

For example:

```bash
*.txt
```

means:

> anything ending in `.txt`

---

# Lesson 8 — Disk Space

Two commands I want you to recognize immediately:

```bash
df -h
```

and

```bash
du -sh /var/log
```

Think of them differently:

**`df -h`**

> How much space is available on my filesystems?

**`du -sh`**

> How much space is this particular directory using?

`df -h` was also one of the answers in your previous knowledge-check material.

---

# Week 1 Hands-On Lab

Now we put the commands together.

Don't copy everything at once. Type the commands individually and look at what Linux does.

### Step 1 — Build the environment

```bash
cd /root

mkdir -p rhcsa-labs/week1/final-lab

cd rhcsa-labs/week1/final-lab
```

Check:

```bash
pwd
```

You should be here:

```text
/root/rhcsa-labs/week1/final-lab
```

### Step 2 — Create directories

```bash
mkdir production
mkdir backups
mkdir logs
```

Check:

```bash
ls -l
```

### Step 3 — Create a configuration file

```bash
cd production
```

Run:

```bash
echo "Application: InventoryApp" > inventory.conf
echo "Environment: Production" >> inventory.conf
echo "Server: $(hostname)" >> inventory.conf
```

Read it:

```bash
cat inventory.conf
```

You previously reached this exact result:

```text
Application: InventoryApp
Environment: Production
Server: rhel9lab
```

Your hostname may now be different depending on which RHEL VM you're using.

### Step 4 — Back it up

```bash
cp inventory.conf ../backups/inventory.conf.backup
```

Check:

```bash
ls -lh ../backups/
```

### Step 5 — Compare production and backup

Move up one directory:

```bash
cd ..
```

Then:

```bash
diff production/inventory.conf backups/inventory.conf.backup
```

You previously noticed something very important:

**There was no output.**

That's good.

With `diff`, no output means the two files are identical.

---

# Week 1 Challenge — Do This Without Looking Above

Starting from:

```bash
/root
```

I want you to create:

```text
rhcsa-labs/
└── week1/
    └── challenge/
        ├── production/
        │   └── server.conf
        ├── backups/
        │   └── server.conf.backup
        └── logs/
```

The contents of `server.conf` should be:

```text
Server: rhel9lab
Environment: Production
Application: WebApp
Status: Active
```

Then copy it into `backups` as:

```text
server.conf.backup
```

Finally compare them using:

```bash
diff
```

If you can accomplish that **without copying my commands**, you're starting to think like a Linux administrator rather than simply following instructions.

## Week 1 Knowledge Check

Before moving to Week 2, I want you to be able to answer these from memory:

1. What command tells me my current directory?
2. What does `cd ..` do?
3. What's the difference between `ls` and `ls -la`?
4. What's the difference between `cp` and `mv`?
5. What's the difference between `>` and `>>`?
6. How would I display a text file?
7. How would I display the last 10 lines of a file?
8. How would I search for `server.conf`?
9. What does this tell me?

```bash
df -h
```

10. If I run:

```bash
diff file1 file2
```

and receive **no output**, what does that mean?

These closely match the command-line and file-management knowledge checks in the Linux/RHCSA practice material we previously used.

### Where we actually left off

You had already answered several of these correctly. In particular, you understood `cd ..`, `>` versus `>>`, `ls`/`ls -la`, and that **no output from `diff` means no differences were found**.

So I wouldn't restart you from zero. I'd use this as a quick refresher, have you complete the **Week 1 Challenge without looking at the solution**, and then I'd move you into **Week 2: users, groups, ownership and Linux permissions (`chmod`, `chown`, `chgrp`)**—which gets us deeper into RHCSA-style administration.
