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

Lab 2
=====

Part A
------

### Task 7

> What does the abbreviation pwd stand for?

`pwd` stands for `print working directory`.

---

### Task 8

> What does the output of the pwd command represent with respect to the location of the root directory in the Linux file system hierarchy?

It represents a location string starting from the filesystem root, `/`.

---

### Task 9

> What does the abbreviation `cd` stand for?

Change directory.

---

### Task 10

> What does the result of the `cd ..` command represent with respect to the location of the previous directory location in the Linux file system hierarchy?

`cd ..` takes you up one directory in the filesystem hierarchy.

---

### Task 11

> Which directory in the Linux file system does the `cd /` command take you to?

The root.

---

### Task 12

> Which directory in the Linux file system does the `cd` command take you to?

Your home folder.

---

### Task 13

> Is it obvious what the `touch` command is designed to do?

Intuitive, but not exactly obvious.

---

### Task 14

> Type `man touch` and press the Enter key. What does the MAN page for `touch` say about the purpose of this command?

"Change file timestamps".

---

### Task 15

> What are the permissions assigned to users and groups for `file1`?

Read/Write for the owner (me), Read for my group, Read for global. `rw-r--r--`.

---

### Task 16

> What are the new `file1` permissions for users and groups?

Everyone can read, write and execute. `rwxrwxrwx`.

---

### Task 17

> Are these permissions reasonable for `file1`?

`777` is generally never reasonable, and a crutch used in cases where proper permissions are misunderstood.

---

### Task 18

> Why is it not possible for you to create the xyz directory in the `/usr/bin` directory?

```bash
[vararut@meno ~]$ cd /usr/bin
[vararut@meno bin]$ mkdir xyz
mkdir: cannot create directory `xyz': Permission denied
[vararut@meno bin]$
```

Permission denied.

---

### Task 19

> Which character in the listing displayed after typing `ls -l` indicates that xyz is a directory?

The `d` at the start.

---

### Task 20

> Why was it not possible to delete the subfiles directory?

`rmdir` will not work on non-empty directories, to prevent accidental misuse.

---

### Task 21

> How does the output of the `ls` command differ from that of the `ls -l` command?

`-l` switches to a long listing format, with more information.

---

### Task 22

> A file listing for which directory is displayed after typing `ls /` ?

The root directory.

```bash
[vararut@meno ~]$ ls /
bin   etc   lib64	misc  opt   save     srv  tom	 usr
boot  home  lost+found	mnt   proc  sbin     sys  user	 var
dev   lib   media	net   root  selinux  tmp  users
[vararut@meno ~]$
```

---

### Task 23

> Which files in your home directory are listed after typing `ls A*.txt`? What do these files have in common?

They all start with the prefix `A` and end in `.txt`.

---

### Task 24

> Explain the error messages that are displayed after typing the `ls` and `mv` commands.

`ls` will return that there is no such file or directory.

`mv` will return permission denied for each affected file.

---

### Task 25

> Which account could perform the `mv` command without getting the error message?

The root user.

---

### Task 26

> Procedurally, how does copying a file differ from moving a file?

`cp` creates a new inode while moving a file just switches a pointer. Moving/renaming is atomic, while copying takes time.

---

### Task 27

> What additional feature does the `rm -i` command offer over the rm command?

It makes the `rm` command interactive, prompting for user confirmation.

---

### Task 28

> What output is produced for the command `cat mlist` ?

```bash
[vararut@meno ~]$ ls
file1  mlist  xyz
[vararut@meno ~]$ ls > mlist
[vararut@meno ~]$ echo "This is the last line of the file" >> mlist
[vararut@meno ~]$ cat mlist
file1
mlist
xyz
This is the last line of the file
[vararut@meno ~]$
```

This is because the `ls > mlist` redirection will overwrite the file, and then the `echo >> mlist` redirection will append the last line.

---

### Task 29

> Assume the previous paragraph is stored in a file called `textfile3.rtf`. What would be the output for the following `grep` command? `grep -c "Data" textfile3.rtf`

```bash
[vararut@meno ~]$ echo "For example, the command grep "ACK" ftpSummaryLarge.rtf displays only those lines in the ftpSummaryLarge.rtf file that contain the key word ACK. Grep can also be used to determine how many lines in a given file contain a specific keyword. For example, one may wish to know the number of packets exchanged during a data transfer between two computers that meets a specific condition, such as the number of packets that contain application data. Issuing a command such as grep -c "Data" tftpSummaryLarge.rtf, would instantly reduce a potentially large file into a single value that represents the number of data packets transferred." > textfile3.rtf
[vararut@meno ~]$ grep -c "Data" textfile3.rtf
1
[vararut@meno ~]$
```

---

Lab 3
=====

Part A
------

### Task 2

```bash
[vararut@meno ~]$ mkdir -p temp personal/funstuff personal/taxes professional/societies/ieee professional/societies/acm professional/courses/general professional/courses/major/cs475 professional/courses/major/cs381/programs professional/courses/major/cs381/labs professional/courses/major/cs381/notes professional/courses/major/cs213
[vararut@meno ~]$ tree
.
|-- personal
|   |-- funstuff
|   `-- taxes
|-- professional
|   |-- courses
|   |   |-- general
|   |   `-- major
|   |       |-- cs213
|   |       |-- cs381
|   |       |   |-- labs
|   |       |   |-- notes
|   |       |   `-- programs
|   |       `-- cs475
|   `-- societies
|       |-- acm
|       `-- ieee
`-- temp

17 directories, 0 files
[vararut@meno ~]$
```

---

### Task 3

```bash
[vararut@meno ~]$ pwd
/users/std/stud/v/va/vararut
[vararut@meno ~]$
```

---

### Task 4

Path to home directory:

```bash
[vararut@meno courses]$ cd
[vararut@meno ~]$ pwd
/users/std/stud/v/va/vararut
[vararut@meno ~]$
```

Path to `acm` directory:

```bash
[vararut@meno ~]$ cd professional/societies/acm/
[vararut@meno acm]$ pwd
/users/std/stud/v/va/vararut/professional/societies/acm
[vararut@meno acm]$
```

Two relative pathnames:

```bash
[vararut@meno courses]$ pwd
/users/std/stud/v/va/vararut/professional/courses
[vararut@meno courses]$ ls ../societies/acm/
[vararut@meno courses]$ ls ~/professional/societies/acm/
[vararut@meno courses]$
```

---

### Task 5

```bash
[vararut@meno ~]$ cd /usr
[vararut@meno usr]$ ll
total 344
drwxr-xr-x   2 root root 69632 Sep 28 15:40 bin
drwxr-xr-x   2 root root  4096 Oct 10  2006 etc
drwxr-xr-x   2 root root  4096 Oct 10  2006 games
drwxr-xr-x 129 root root 12288 Aug 13  2010 include
drwxr-xr-x   5 root root  4096 Dec 10  2010 java
drwxr-xr-x   6 root root  4096 Sep 11  2007 kerberos
drwxr-xr-x  79 root root 65536 Nov  3  2011 lib
drwxr-xr-x  90 root root 69632 Nov  7  2011 lib64
drwxr-xr-x  10 root root  4096 Mar  4  2008 libexec
drwxr-xr-x  16 root root  4096 Dec 10  2010 local
drwxr-xr-x   2 root root 20480 Jul 11 12:46 sbin
drwxr-xr-x 208 root root  4096 Aug 13  2010 share
drwxr-xr-x   4 root root  4096 Mar  4  2008 src
drwx--x--x   3 tftp bin   4096 Sep 16  2008 tftpdir
lrwxrwxrwx   1 root root    10 Mar  4  2008 tmp -> ../var/tmp
drwxr-xr-x   2 root root  4096 Jun 21  2010 users
drwxr-xr-x   3 root root  4096 Mar  4  2008 X11R6
[vararut@meno usr]$ ls | wc -l
17
[vararut@meno usr]$
```

There are 17, through `ls | wc -l`. They are all directories, except for `tmp` which is a symlink.

---

### Task 6

```bash
[vararut@meno ~]$ pwd
/users/std/stud/v/va/vararut
[vararut@meno ~]$ ls /usr/bin/ | wc -l
2328
[vararut@meno ~]$
```

---

### Task 7

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

### Task 8

```bash
[vararut@meno ~]$ cat /etc/passwd | grep vararut
vararut:x:171224:219:Theodor Vararu, Student,:/users/std/stud/v/va/vararut:/bin/bash
[vararut@meno ~]$
```

| User name | User ID | Group ID | Personal Information    | Home Directory                 | Login Shell |
|-----------|---------|----------|-------------------------|--------------------------------|-------------|
| `vararut` | 171224  | 219      | Theodor Vararu, Student | `/users/std/stud/v/va/vararut` | `/bin/bash` |

---

### Task 9

```bash
[vararut@meno ~]$ cd /boot/
[vararut@meno boot]$ ll
total 7897
-rw-r--r-- 1 root root   61583 Oct 10  2007 config-2.6.18-53.el5
drwxr-xr-x 2 root root    1024 Apr  8  2011 grub
-rw------- 1 root root 2547073 Jul 11 12:46 initrd-2.6.18-53.el5.img
-rw-r--r-- 1 root root 2334422 Mar  4  2008 initrd-2.6.18-53.el5kdump.img
drwx------ 2 root root   12288 Mar  4  2008 lost+found
-rw-r--r-- 1 root root   88644 Oct 10  2007 symvers-2.6.18-53.el5.gz
-rw-r--r-- 1 root root 1150812 Oct 10  2007 System.map-2.6.18-53.el5
-rw-r--r-- 1 root root 1844028 Oct 10  2007 vmlinuz-2.6.18-53.el5
[vararut@meno boot]$
```

`vmlinuz-2.6.18-53.el5`\.

---

### Task 10

There are no `.profile` or `.login` files in the home directory for this particular user configuration. Rather, there is a `.bash_profile` file in the home directory:

```bash
[vararut@meno ~]$ cat .bash_profile | head -n 5
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
  . ~/.bashrc
[vararut@meno ~]$
```

---

### Task 11

```bash
[vararut@meno ~]$ ls -lah
total 84K
drwx--x--x    4 vararut ustud 4.0K Dec 11 01:35 .
drwxr-xr-x  186 root    root  4.0K Nov 25 20:03 ..
-rw-------    1 vararut ustud 3.5K Dec 11 00:01 .bash_history
-rw-r--r--    1 vararut ustud   24 Aug 22 12:03 .bash_logout
-rw-r--r--    1 vararut ustud  178 Aug 22 12:03 .bash_profile
-rw-r--r--    1 vararut ustud  124 Aug 22 12:03 .bashrc
-rw-r--r--    1 vararut ustud  515 Aug 22 12:03 .emacs
drwxr-xr-x    3 vararut ustud 4.0K Aug 22 12:03 .kde
-rw-------    1 vararut ustud   65 Dec 11 01:35 .lesshst
drwxrws--T+   3 root    www   4.0K Aug 22 12:03 .public_html
-rw-------    1 vararut ustud    8 Sep 25 15:13 .sh_history
-rw-------    1 vararut ustud 3.6K Sep 25 14:20 .viminfo
-rw-r--r--    1 vararut ustud  22K Sep 25 14:14 .zcompdump
-rw-r--r--    1 vararut ustud  658 Sep 25 14:21 .zshrc
-rw-r--r--    1 vararut ustud  658 Sep 25 14:18 .zshrc-old
[vararut@meno ~]$
```

---

### Task 12

```bash
[vararut@meno ~]$ ls -il
total 12
232512 drwxr-xr-x  4 vararut ustud 4096 Dec 11 01:43 personal
232515 drwxr-xr-x  4 vararut ustud 4096 Dec 11 01:43 professional
232510 drwxr-xr-x  2 vararut ustud 4096 Dec 11 01:43 temp
[vararut@meno ~]$ ls -ila /
total 218
     2 drwxr-xr-x 29 root    root  4096 Dec 11 01:05 .
[vararut@meno ~]$
```

---

### Task 13

```bash
[vararut@meno ~]$ ls -ilah /boot/vmlinuz-2.6.18-53.el5
15 -rw-r--r-- 1 root root 1.8M Oct 10  2007 /boot/vmlinuz-2.6.18-53.el5
[vararut@meno ~]$ more /boot/vmlinuz-2.6.18-53.el5

******** /boot/vmlinuz-2.6.18-53.el5: Not a text file ********

[vararut@meno ~]$
```

---

### Task 14

```bash
[vararut@meno labs]$ ls -ilah lab1
232528 -rw-r--r-- 1 vararut ustud 0 Dec 11 01:47 lab1
[vararut@meno labs]$
```

---

### Task 15

```bash
[vararut@meno ~]$ ls /usr/include/sys/t*.h
/usr/include/sys/termios.h  /usr/include/sys/timex.h
/usr/include/sys/timeb.h    /usr/include/sys/ttychars.h
/usr/include/sys/time.h     /usr/include/sys/ttydefaults.h
/usr/include/sys/times.h    /usr/include/sys/types.h
[vararut@meno ~]$
```

---

### Task 16

Off we go!

```bash
[vararut@meno ~]$ exit
logout
Connection to meno.lsbu.ac.uk closed.
➜  ~
```

---
