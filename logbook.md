Lab 1
=====

Part A
------

### Task 2

Logging into my system via my personal laptop, which is running the Mac OS X operating system, was actually more eventful than expected.

Since I know that OS X ships with a preinstalled version of OpenSSH, I went straight to my terminal and fired the command:

> *Note:* `➜  ~` is the `$PROMPT` from my terminal theme, which is part of the popular **oh-my-zsh** (https://github.com/robbyrussell/oh-my-zsh) Z Shell configuration pack. The first character is a stylistic unicode arrow, while the second character is my CWD. In this case, `~`, the home directory.

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

> *Note:* my password was not requested for the login as I have set this machine up with my public RSA key.

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

Part B
------

### Task 1

Logging into my system right away.

```bash
➜  ~  ssh vararut@meno.lsbu.ac.uk
Warning : Unauthorised Access Prohibited

vararut@meno.lsbu.ac.uk's password:
Last login: Wed Dec 10 21:23:21 2014 from 188.226.201.122
[vararut@meno ~]$
```

---

### Task 2

```bash
[vararut@meno ~]$ who
richj3   pts/9        2014-12-10 20:30 (cpc2-slam6-2-0-cust66.2-4.cable.virginm.net)
santamaa pts/10       2014-12-10 19:54 (host86-132-102-246.range86-132.btcentralplus.com)
coxr2    pts/11       2014-12-10 22:18 (5ec1bde7.skybroadband.com)
sekyewaj pts/12       2014-12-10 21:53 (90.198.211.30)
vararut  pts/15       2014-12-10 22:17 (188.226.201.122)
[vararut@meno ~]$
```

Looks like there are 5 users logged in, and the longest active user is `santamaa` who logged in at `2014-12-10 19:54`. I assume there are no users logged in via `telnet`, since trying to do so on the default port times out:

```bash
➜  ~  telnet meno.lsbu.ac.uk
Trying 136.148.76.159...
telnet: Unable to connect to remote host: Connection timed out
➜  ~
```

---

### Task 3

> Use the `date` command to display the current time. Show your session.

```bash
[vararut@meno ~]$ date
Wed Dec 10 22:50:27 GMT 2014
[vararut@meno ~]$
```

---

### Task 4

```bash
[vararut@meno ~]$ history | grep "cal"
224  cd /usr/local
294  cal 4
295  cal 52
296  cal 1752
297  cal 1952
298  cal 2004
299  cal 2013
300  history | grep "cal"
[vararut@meno ~]$
```

> Does the command work fine both leap and non-leap years? How could you tell?

I honestly didn't bother inspecting the output when I used the commands, since I already know that `cal` is rock solid, but scrolling up to look at the output for `cal 2014` does reveal that February has 29 days as expected.

1752 has 366 days and this is unsurprising, as it is divisible by 4 and as such a leap year. Google presents 10 results on the main page, as is standard, with a proeminent entry for the Wikipedia Article on this subject, http://en.wikipedia.org/wiki/Gregorian_calendar. It states that its namesake is after its inventor:

> It is named for Pope Gregory XIII, who introduced it in 1582.

---

### Task 5

> Use the `pwd` command to display the name of your home directory. Show your session.

```bash
[vararut@meno ~]$ pwd
/users/std/stud/v/va/vararut
[vararut@meno ~]$
```

---

### Task 6

```bash
[vararut@meno ~]$ uname
Linux
[vararut@meno ~]$ uname -n
meno.lsbu.ac.uk
[vararut@meno ~]$ uname -p
x86_64
[vararut@meno ~]$
```

The operating system is GNU/Linux, which consists of the GNU core utils running on top of the Linux kernel. This is evident by running `uname -o`, which will return the operating system name:

```bash
[vararut@meno ~]$ uname -o
GNU/Linux
[vararut@meno ~]$
```

Richard Stallman is always keen to remind us of the fact that Linux is just the kernel, rather than the full operating system: http://en.wikipedia.org/wiki/GNU/Linux_naming_controversy.

The nodename is `meno.lsbu.ac.uk`, and the processor type is `x86_64`.

---

### Task 7

> Which operating system does the operating system run on?

Err, this question seems phrased incorrectly. The operating system runs on 16-bit minicomputers.

> Who owns the rights of RSTS?

Apparently DEC sold their rights to Mentec at one point.

> How old is RSTS?

The first version was released in 1970, so 34 years old.

> What does RSTS stand for?

"Resource Sharing Timesharing System".

> When was RSTS operating system written?

1970.

> Where was RSTS written?

Digital Equipment Corporation headquarters in Maynard, Massachusetts, United States.

---

### Task 8

| OS           | Google.com Hits |
|--------------|-----------------|
| UNIX         | 174,000,000     |
| Linux        | 454,000,000     |
| FreeBSD      | 23,900,000      |
| NetBSD       | 818,000         |
| Solaris      | 78,100,000      |
| Windows 8    | 756,000,000     |
| Windows 7    | 786,000,000     |
| Windows XP   | 238,000,000     |
| Windows 2000 | 145,000,000     |
| Windows NT   | 53,100,000      |
| Windows 98   | 96,400,000      |
| Windows 95   | 91,900,000      |
| Windows 3.1  | 25,300,000      |
| Mac OS       | 213,000,000     |

The most popular are, ordered by hits: Windows 7, Windows 8 and Linux. Windows aggregates a total of **2,482,800,000** results.

---

### Task 9

> What is the name of world’s first time-sharing operating system?

CTSS, apparently.

---

### Task 10

| Acronym | Expansion                                                                   |
|---------|-----------------------------------------------------------------------------|
| BOS     | Either IBM's Basic Operating System or Motorola's Business Operating System |
| BOSS    | Bharat Operating System Solutions                                           |
| DOS     | Disk Operating System                                                       |
| JOSS    | JOHNNIAC Open-Shop System                                                   |
| MOS     | Mobile Operating System                                                     |
| POS     | PERQ Operating System                                                       |
| TSOS    | Time Sharing Operating System                                               |

---

### Task 11

EDSAC, 6 May 1949, the University of Cambridge Mathematical Laboratory in England.

---

### Task 12

> When did the first version of UNIX come out and what was its name?

The first version was called UNICS and it came out in 1970.

> Who were the authors of UNIX?

It was written by Ken Thompson and Dennis Ritchie (RIP).

> Where (organization) was UNIX designed and written?

AT&T Bell Labs.

> When (year) was UNIX rewritten in C?

1972.

> When did BSD UNIX come out?

1977.

> When did FreeBSD come out?

1993.

> Who said this and when: *"...the number of UNIX installations has grown to 10, with more expected..."*

Dennis Ritchie and Ken Thompson, June 1972.

> When was TCP/IP implemented in the UNIX kernel and in which UNIX flavor?

In BSD Unix, starting with version 4.1c.

> When did Solaris 9.0E come out?

2003.

> Who created the B language and when?

Ken Thompson with Dennis Ritchie, circa 1969.

> Who created the C language and when?

Dennis Ritchie, circa 1969.

---

### Task 13

Off we go!

```bash
[vararut@meno ~]$ exit
logout
Connection to meno.lsbu.ac.uk closed.
➜  ~
```

---

Part C
------

### Task 2

| Command | Short Description                                | Example Use                          |
|---------|--------------------------------------------------|--------------------------------------|
| `touch` | Create an empty file, or update an existing one. | `touch test.txt`                     |
| `cp`    | Copy files and directories.                      | `cp -rv ~/Desktop/*.jpg ~/Pictures/` |
| `mv`    | Rename/move files and directories.               | `mv .bashrc .bash_profile`           |
| `rm`    | Remove files and directories.                    | `rm -rf /`                           |
| `mkdir` | Make directories and subdirectories.             | `mkdir -p ho/ho/ho/merry/christmas`  |
| `rmdir` | Remove empty directories.                        | `rmdir -p ho/ho/ho/merry/christmas`  |
| `ls`    | List files.                                      | `ls ~/Pictures/*.jpg`                |
| `lpr`   | Print files.                                     | `lpr thesis.txt`                     |
| `cd`    | Change directory.                                | `cd /bin/`                           |
| `pwd`   | Print working directory.                         | `pwd`                                |

| Command  | Short Description                                           |
|----------|-------------------------------------------------------------|
| `open`   | Opens a file or device.                                     |
| `read`   | Read from the standard input and store in a shell variable. |
| `write`  | Write to a file descriptor.                                 |
| `close`  | Close a file descriptor.                                    |
| `pipe`   | Create a pipe inode.                                        |
| `socket` | Create a network communication endpoint.                    |
| `mkfifo` | Create a named pipe.                                        |
| `system` | Execute a shell command.                                    |
| `printf` | Format and print data.                                      |

---

### Task 3

```bash
[vararut@meno ~]$ cat /etc/motd
################################################################################
Unauthorised access is prohibited

NOTE: meno now supports https. To develope a https site create a .private_html directory and then run wwwset to set the correct permissions viz

$ cd
$ mkdir .private_html
$ wwwset

By putting content into .private_html you make now view it at https://mypages.lsbu.ac.uk/~username


Non-https  personal web pages are still available on this server using the URL
http://mypages.lsbu.ac.uk/~username

STUDENT personal web pages are only visible on-campus. If you are developing a web site and wish to view it from off campus you will need to connect to the VPN first. Details can be found at
http://www.lsbu.ac.uk/ict/services/vpn.shtml

Please note that web pages should be located in your .public_html folder and any PHP scripts you develop should be located in your .public_html/cgi-bin folder
###############################################################################

[vararut@meno ~]$
```

---

### Task 4

```bash
[vararut@meno ~]$ echo $PATH
/usr/lib64/qt-3.3/bin:/usr/kerberos/bin:/usr/local/bin:/bin:/usr/bin:/usr/lib/oracle/11.2/client64/bin:/users/std/stud/v/va/vararut/bin:.
[vararut@meno ~]$ echo $path

[vararut@meno ~]$ echo $LINES
25
[vararut@meno ~]$ echo $HOME
/users/std/stud/v/va/vararut
[vararut@meno ~]$ echo $home

[vararut@meno ~]$
```

---

### Task 5

```bash
[vararut@meno ~]$ echo $LINES
25
[vararut@meno ~]$ export LINES=15
[vararut@meno ~]$ echo $LINES
15
[vararut@meno ~]$ man ls
[vararut@meno ~]$ echo $LINES
25
[vararut@meno ~]$
```

Running `man ls` changes the value of `LINES` back to `25`. So, still `25`.

---
