Lab 1
=====

Part A
------

### Task 2

Logging into my system via my personal laptop, which is running the Mac OS X operating system, was actually more eventful than expected.

Since I know that OS X ships with a preinstalled version of OpenSSH, I went straight to my terminal and fired the command:

*Note:* `➜  ~` is the `$PROMPT` from my terminal theme, which is part of the popular **oh-my-zsh** (https://github.com/robbyrussell/oh-my-zsh) Z Shell configuration pack. The first character is a stylistic unicode arrow, while the second character is my CWD. In this case, `~`, the home directory.

```bash
➜  ~  uname -a
Darwin Theodors-MacBook-Air.local 14.0.0 Darwin Kernel Version 14.0.0: Fri Sep 19 00:26:44 PDT 2014; root:xnu-2782.1.97~2/RELEASE_X86_64 x86_64
➜  ~  ssh vararut@meno.lsbu.ac.uk
ssh: connect to host meno.lsbu.ac.uk port 22: Operation timed out
➜  ~
```

Well, now that's not a promising start. First idea: are ports blocked?

```bash
➜  ~  ssh $arcturus
Welcome to Ubuntu 12.04.4 LTS (GNU/Linux 3.2.0-60-virtual i686)

* Documentation:  https://help.ubuntu.com/
Last login: Wed Dec 10 03:02:44 2014 from 136.148.10.8
➜  ~
```

Looks like they are not, as SSHing through port 22 on one of my own GNU/Linux boxes works unhitched.

*Note:* my password was not requested for the login as I have set this machine up with my public RSA key.

```bash
➜  ~  uname -a
Linux blog.vararu.org 3.2.0-60-virtual #91-Ubuntu SMP Wed Feb 19 04:35:08 UTC 2014 i686 i686 i386 GNU/Linux
➜  ~  ssh vararut@meno.lsbu.ac.uk
Warning : Unauthorised Access Prohibited

vararut@meno.lsbu.ac.uk\'s password:
```

Interesting! My GNU/Linux box can get through just fine, and proceeds to the SSH password prompt. Investigating further, and running the ssh command in verbose mode via `ssh -v`, reveals that my OS X machine is choking before it can even form a connection:

```bash
➜  ~  ssh -v vararut@meno.lsbu.ac.uk
OpenSSH_6.2p2, OSSLShim 0.9.8r 8 Dec 2011
debug1: Reading configuration data /etc/ssh_config
debug1: /etc/ssh_config line 103: Applying options for *
debug1: Connecting to meno.lsbu.ac.uk [136.148.76.159] port 22.
debug1: connect to address 136.148.76.159 port 22: Operation timed out
ssh: connect to host meno.lsbu.ac.uk port 22: Operation timed out
➜  ~
```

I've tried other avenues of investigation to figure out what the root cause is, but after coming up empty-handed I decided to skip to the exercise itself, since my GNU/Linux box can connect just fine:

```bash
➜  ~  ssh vararut@meno.lsbu.ac.uk
Warning : Unauthorised Access Prohibited

vararut@meno.lsbu.ac.uk\'s password:
Last login: Wed Dec 10 03:07:37 2014 from 188.226.201.122

### SNIPPED ###

Disk quotas for user vararut (uid 171224):
Filesystem  blocks   quota   limit   grace   files   quota   limit   grace
/dev/sdb1     136   50000   60000              18    2000    2500
[vararut@meno ~]$ uname -a
Linux meno.lsbu.ac.uk 2.6.18-53.el5 #1 SMP Wed Oct 10 16:34:19 EDT 2007 x86_64 x86_64 x86_64 GNU/Linux
[vararut@meno ~]$
```

We're in! Logging out is simple:

```bash
[vararut@meno ~]$ exit
logout
Connection to meno.lsbu.ac.uk closed.
➜  ~
```

> Then, compare this list and its descriptions to the general connection categories and methods of logging on and logging off. These general categories are: - a. Local Area Network (LAN) Connection - b. Internet Connection - c. Stand-Alone Connection

Turns out, my eventual login method is actually rather convoluted. It involves using an Internet Connection to first connect to my Amsterdam-based GNU/Linux blog server, followed by another SSH tunnel back here in London to where I assume the LSBU Meno servers are located. All in all, thanks to the magic of broadband, the end result is actually superbly usable and stable, despite the unnecessary roundtrip.

> If your method of connection is completely different from anything shown in lecture notes, try to give a descriptive name to your method.

I shall dub my method as such: "Mac OS X is a miserable pile of faux POSIX compliance so I just used Ubuntu".

Onward to the next task.

---

### Task 3

Pretty straightforward, let's go!

```bash
[vararut@meno ~]$ ls
[vararut@meno ~]$ pwd
/users/std/stud/v/va/vararut
[vararut@meno ~]$ xy
-bash: xy: command not found
[vararut@meno ~]$ cd ..
[vararut@meno va]$ pwd
/users/std/stud/v/va
[vararut@meno va]$ cd
[vararut@meno ~]$ pwd
/users/std/stud/v/va/vararut
[vararut@meno ~]$ cd /usr/local
[vararut@meno local]$ ls
bin    etc    include  lib64	man	       sbin   src
cams2  games  lib      libexec	mysql-3.23.32  share  tomcat
[vararut@meno local]$ cd
[vararut@meno ~]$
```

Explanations as follows:

-	`ls` lists the contents of our home folder, which is empty.
-	`pwd` prints our working directory, which is our `Meno` home folder: `/users/std/stud/v/va/vararut`
-	`xy` is not a valid command, so the shell rejects it and prints out an error message.
-	`cd ..` takes us up a folder in the directory hierarchy.
-	`pwd` prints that we're not in the shared `va` folder, which contains what I assume are the home folders of other students whose surnames start with `va`.
-	`cd` with no command arguments will take us back to our home folder.
-	`pwd` confirms that we're back home.
-	`cd /usr/local` takes us to the `/usr/local` folder.
-	`ls` shows us the contents of `/usr/local`, which includes some binary executables, games, and various other software and documentation.
-	`cd` home sweet home again!

---

### Task 4

Let's create the two files!

```bash
[vararut@meno ~]$ cat > 1st
Lorem ipsum

[vararut@meno ~]$ cat 1st
Lorem ipsum
[vararut@meno ~]$ cat > 2nd
Dolor sit

[vararut@meno ~]$ cat 2nd
Dolor sit
[vararut@meno ~]$
```

And now, let's create the directory structures! Figure 1:

```bash
[vararut@meno ~]$ mkdir old temp new
[vararut@meno ~]$ mkdir temp/junk
[vararut@meno ~]$ mv 1st old/
[vararut@meno ~]$ mv 2nd new/
[vararut@meno ~]$ tree
.
|-- new
|   `-- 2nd
|-- old
|   `-- 1st
`-- temp
    `-- junk

4 directories, 2 files
[vararut@meno ~]$
```

Resetting the directory structure as before:

```bash
[vararut@meno ~]$ mv old/1st .
[vararut@meno ~]$ mv new/2nd .
[vararut@meno ~]$ rm -rf old temp new
[vararut@meno ~]$ tree
.
|-- 1st
`-- 2nd

0 directories, 2 files
[vararut@meno ~]$
```

Constructing Figure 2:

```bash
[vararut@meno ~]$ mkdir -p major minor aug/7th
[vararut@meno ~]$ mv 1st major/
[vararut@meno ~]$ mv 2nd aug/7th/
[vararut@meno ~]$ tree
.
|-- aug
|   `-- 7th
|       `-- 2nd
|-- major
|   `-- 1st
`-- minor

4 directories, 2 files
[vararut@meno ~]$
```

Reset, and then proceed to constructing Figure 3:

```bash
[vararut@meno ~]$ mkdir -p big small medium/tall
[vararut@meno ~]$ cp 1st big/1stcopy
[vararut@meno ~]$ mv 1st small/
[vararut@meno ~]$ mv 2nd medium/tall/
[vararut@meno ~]$ tree
.
|-- big
|   `-- 1stcopy
|-- medium
|   `-- tall
|       `-- 2nd
`-- small
    `-- 1st

4 directories, 3 files
[vararut@meno ~]$
```

And finally, Figure 4:

```bash
[vararut@meno ~]$ mkdir -p time/now space/then arch/nice
[vararut@meno ~]$ cp 1st time/now/1stcopy
[vararut@meno ~]$ mv 1st space/then/
[vararut@meno ~]$ mv 2nd arch/nice/
[vararut@meno ~]$ tree
.
|-- arch
|   `-- nice
|       `-- 2nd
|-- space
|   `-- then
|       `-- 1st
`-- time
    `-- now
        `-- 1stcopy

6 directories, 3 files
[vararut@meno ~]$
```

---

### Task 5

Here is the table with my own personalised descriptions of all of the methods:

| Command | Description                                                                       |
|---------|-----------------------------------------------------------------------------------|
| `cat`   | Con**cat**enate files or input streams. Often used to show the contents of files. |
| `more`  | Page and search through long files and outputs with ease. "Show me **more**".     |
| `cp`    | **C**o**p**y files and directories.                                               |
| `mv`    | Rename (**m**o**v**e) files and directories.                                      |
| `rm`    | **R**e**m**ove files and directories.                                             |
| `ls`    | **L**i**s**t and sort files and directories.                                      |
| `mkdir` | **M**a**k**e a **dir**ectory.                                                     |
| `pwd`   | Show the **p**ath to the current **w**orking **d**irectory.                       |
| `cd`    | Change the **c**urrent **d**irectory.                                             |
| `rmdir` | **R**e**m**ove a **dir**ectory. Must be empty.                                    |

---

###Task 6

Since I was already familiarised with these commands, as well as some of their particularities (such as the `-p` option for `mkdir` to recursively create subdirectories that occur in the specified path), I don't think I would change anything, but I am always open to learning new tricks!

---

### Task 7

Off we go!

```bash
[vararut@meno ~]$ exit
logout
Connection to meno.lsbu.ac.uk closed.
➜  ~
```

---
