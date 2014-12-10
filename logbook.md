Lab 1
=====

Part A
------

**Task 2**: Logging into my system via my personal laptop, which is running the Mac OS X operating system, was actually more eventful than expected.

Since I know that OS X ships with a preinstalled version of OpenSSH, I went straight to my terminal and fired the command:

```
➜  ~  uname -a
Darwin Theodors-MacBook-Air.local 14.0.0 Darwin Kernel Version 14.0.0: Fri Sep 19 00:26:44 PDT 2014; root:xnu-2782.1.97~2/RELEASE_X86_64 x86_64
➜  ~  ssh vararut@meno.lsbu.ac.uk
ssh: connect to host meno.lsbu.ac.uk port 22: Operation timed out
➜  ~
```

*Note:* `➜  ~` is from my terminal theme, which is the popular

Well, now that's not a promising start. First idea: are ports blocked?
