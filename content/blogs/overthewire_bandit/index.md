---
title: "OverTheWire: Bandit"
date: 2025-09-20
tags: ["ctf","bandit","walkthrough","linux","overthewire"]
---

A compact, readable walkthrough for **OverTheWire: Bandit**.
Each level has: **Password**, **Goal**, **Commands you may need**, **Solution** (succinct commands to run), and a **Tip**.

---

## Quick setup
SSH host: `bandit.labs.overthewire.org`
SSH port: `2220`
Example login:
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
````

---
# Walkthrough (0 → 17)
---

## Level 0 → 1

**Password:** `bandit0`

**Goal**
Log in via SSH and retrieve the password for level 1 from the file `readme`.

**Commands you may need**
`ssh`, `ls`, `cat`, `pwd`

**Solution**

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
# password: bandit0
cat readme
```

**Tip**
Always `exit` or `Ctrl-D` to log out before SSHing to the next level.

---
## Level 1 → 2

**Password:** `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`

**Goal**
The next password is in a file named `-` in the home directory.

**Commands you may need**
`ls`, `cat`, `file`

**Solution**

```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
# use ./- to avoid treating '-' as an option
cat ./-
```

**Tip**
Prefix with `./` (or use the full path) for filenames beginning with `-`.

---

## Level 2 → 3

**Password:** `263JGJPfgU6LtdEvgfWU1XP5yac29mFx`

**Goal**
The next password is in a file called `spaces in this filename`.

**Commands you may need**
`ls`, `cat`

**Solution**

```bash
ssh bandit2@bandit.labs.overthewire.org -p 2220
cat ./"--spaces in this filename--"
```

**Tip**
Quotes handle spaces in filenames.

---

## Level 3 → 4

**Password:** `MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`

**Goal**
Find a hidden file inside the `inhere` directory that contains the password.

**Commands you may need**
`ls -a`, `file`, `cat`, `find`

**Solution**

```bash
ssh bandit3@bandit.labs.overthewire.org -p 2220
ls -a inhere
cat inhere/...Hiding-From-You
```

**Tip**
`ls -a` reveals dotfiles; `file` helps spot human-readable files.

---

## Level 4 → 5

**Password:** `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`

**Goal**
The password is in the only human-readable file inside the `inhere` directory.

**Commands you may need**
`file`, `cat`, `ls`, `find`

**Solution**

```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220
file inhere/*     # find the file that says "ASCII text"
cat inhere/<that-file>
```

**Tip**
`file` quickly separates text files from binaries and junk.

---

## Level 5 → 6

**Password:** `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`

**Goal**
Find a **human-readable** file under `inhere` that is exactly **1033 bytes** and **not executable**.

**Commands you may need**
`find`, `file`, `cat`

**Solution**

```bash
ssh bandit5@bandit.labs.overthewire.org -p 2220
# find files of size 1033 bytes, not executable
find . -type f -size 1033c ! -executable 2>/dev/null -exec file {} \; -print
# then cat the matching file
cat ./path/to/matching-file
```

**Tip**
Use `2>/dev/null` to ignore permission errors.

---

## Level 6 → 7

**Password:** `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`

**Goal**
Find the file owned by user `bandit7`, group `bandit6`, and size **33 bytes**.

**Commands you may need**
`find`, `ls`, `cat`

**Solution**

```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220
find / -type f -size 33c -user bandit7 -group bandit6 2>/dev/null -print
# once found:
cat /path/to/file
```

**Tip**
Search from `/` if home dirs don't contain it; redirect errors to keep output clean.

---

## Level 7 → 8

**Password:** `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

**Goal**
In `data.txt`, locate the password next to the word **millionth**.

**Commands you may need**
`sort`, `grep`, `awk`, `sed`, `strings`

**Solution**

```bash
ssh bandit7@bandit.labs.overthewire.org -p 2220
# find lines containing 'millionth'
grep -n "millionth" data.txt
# or sort then grep if helpful
sort data.txt | grep "millionth"
# extract the word next to it (adjust field separator if needed)
grep "millionth" data.txt | awk '{for(i=1;i<=NF;i++) if($i=="millionth") print $(i+1)}'
```

**Tip**
Data files can be messy; try `strings` if file looks binary.

---

## Level 8 → 9

**Password:** `dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`

**Goal**
In `data.txt`: find the single line that occurs only once (all others repeat).

**Commands you may need**
`sort`, `uniq`, `grep`

**Solution**

```bash
ssh bandit8@bandit.labs.overthewire.org -p 2220
sort data.txt | uniq -u
```

**Tip**
`uniq -u` prints lines that appear exactly once (input must be sorted).

---

## Level 9 → 10

**Password:** `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`

**Goal**
In `data.txt`: identify human-readable strings preceded by several `=` characters.

**Commands you may need**
`strings`, `grep`

**Solution**

```bash
ssh bandit9@bandit.labs.overthewire.org -p 2220
strings data.txt | grep -E "=+"
```

**Tip**
`strings` extracts readable sequences from binary blobs.

---

## Level 10 → 11

**Password:** `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`

**Goal**
`data.txt` contains base64-encoded data; decode it to reveal the password.

**Commands you may need**
`strings`, `base64`, `grep`

**Solution**

```bash
ssh bandit10@bandit.labs.overthewire.org -p 2220
strings data.txt | base64 -d
# or, if the data is embedded:
strings data.txt | grep -v '[[:space:]]' | base64 -d
```

**Tip**
If `base64 -d` complains, try trimming non-base64 characters first.

---

## Level 11 → 12

**Password:** `dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`

**Goal**
`data.txt` is ROT13 encoded (letters rotated by 13); decode it to get the password.

**Commands you may need**
`tr`, `strings`

**Solution**

```bash
ssh bandit11@bandit.labs.overthewire.org -p 2220
strings data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

**Tip**
ROT13 is symmetric: the same transform encodes and decodes.

---

## Level 12 → 13
**Password:** `7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`

**Goal**
The file `data.txt` is a **hexdump** of another file that has been repeatedly compressed.
You need to reverse the hexdump, then keep extracting/decompressing until you reach the password.
Work in a temporary directory so you don’t clutter the home folder.

**Commands you may need**
`xxd`, `file`, `gunzip`, `bunzip2`, `tar`, `mkdir`, `cp`, `mv`

**Solution**
```bash
ssh bandit12@bandit.labs.overthewire.org -p 2220

# Create temporary working directory
cd /tmp
WORKDIR=$(mktemp -d)
cd $WORKDIR

# Copy hexdump
cp ~/data.txt .

# Step 1: reverse hexdump
xxd -r data.txt data

# Step 2: gzip decompress
mv data data.gz && gunzip data.gz

# Step 3: bzip2 decompress
mv data data.bz2 && bunzip2 data.bz2

# Step 4: gzip decompress again
mv data data.gz && gunzip data.gz

# Step 5: extract first tar archive
mv data data.tar && tar -xf data.tar
# check extracted files
ls

# Step 6: extract second tar archive
# replace <filename> with actual extracted file name from previous step
tar -xf <extracted-file-from-step5>
ls

# Step 7: bzip2 decompress
# replace <filename> with actual .bz2 file
bunzip2 <extracted-file-from-step6>

# Step 8: extract tar if needed
# replace <filename> with actual extracted file
tar -xf <extracted-file-from-step7>
ls

# Step 9: final gzip decompress
# replace <filename> with final .gz file
mv <extracted-file-from-step7> data8.gz
gunzip data8.gz

# Step 10: read the password
cat <final-text-file>
````

**Tip**
After each step, always run `file <filename>` to know the next decompression tool to use. Eventually, you’ll reveal the password.

---

## Level 13 → 14

**Password:** `FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`

**Goal**
You are given a private SSH key (instead of the password file). Use that key to SSH as user `bandit14` and read `/etc/bandit_pass/bandit14`.

**Commands you may need**
`ssh -i`, `chmod`, `cat`

**Solution**

```bash
ssh bandit13@bandit.labs.overthewire.org -p 2220
ls
exit

scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .
chmod 600 sshkey.private
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

**Tip**
`chmod 600` on the key avoids OpenSSH refusing to use it.

---

## Level 14 → 15

**Password:** `You get from 'Level 13 → 14' the sshkey.private`

**Goal**
Submit the password for the current level to **localhost port 30000**; the server will return the next password.

**Commands you may need**
`nc` (netcat), `cat`

**Solution**

```bash
ssh bandit14@bandit.labs.overthewire.org -p 2220
# send current password to localhost:30000 and receive next password
cat /etc/bandit_pass/bandit14 | nc localhost 30000
```

**Tip**
If `nc` isn't available, `telnet localhost 30000` can work interactively.

---

## Level 15 → 16

**Password:** `8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`

**Goal**
Submit the current level password to **localhost port 30001** using **SSL/TLS**; one SSL server will return the next password.

**Commands you may need**
`openssl s_client`, `nc`, `nmap`

**Solution**

```bash
ssh bandit15@bandit.labs.overthewire.org -p 2220
# scan for listening ports in the general area if needed:
nmap -p 30000-31000 localhost
# connect with openssl and send the password
echo "CURRENT_PASSWORD" | openssl s_client -connect localhost:30001 -quiet
# or:
cat /etc/bandit_pass/bandit15 | openssl s_client -connect localhost:30001 -quiet
```

**Tip**
Use `-quiet` and try `-ign_eof` if the connection behaves oddly; read `man s_client` for the interactive commands.

---

## Level 16 → 17

**Password:** `kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx`

**Goal**
Find which port in the range **31000–32000** on localhost will return the next password when you submit the current password; test which of those ports use SSL and which do not.

**Commands you may need**
`nmap`, `openssl s_client`, `nc`

**Solution**

```bash
ssh bandit16@bandit.labs.overthewire.org -p 2220
# find open ports in range
nmap -p 31000-32000 localhost
# test SSL and non-SSL endpoints; example:
cat /etc/bandit_pass/bandit16 | nc localhost <port>
# or for SSL:
cat /etc/bandit_pass/bandit16 | openssl s_client -connect localhost:<port> -quiet
```

The password you will get is `kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx`

**Tip**
One port will give the actual password; others echo what you sent. `nmap` is handy to find open ports quickly.

---



# Notes

* Use `file`, `strings`, and `xxd` liberally — they’re lifesavers for binary/text distinctions.
* When in doubt, read `man` pages for the tools suggested.

---
