---
title: Decidim QA - FAQ
category: Technique
order: 1
---

# ❓What should I do if❓ :

## I got an error message saying : `too many open files` or `too many processes`
Machines vary, but there is a limit to the number of open files or processes running. 
  
* OSX

1. List all limit on a system :

```bash
ulimit -a
```

will output : 
```bash
-t: cpu time (seconds)              unlimited
-f: file size (blocks)              unlimited
-d: data seg size (kbytes)          unlimited
-s: stack size (kbytes)             8192
-c: core file size (blocks)         0
-v: address space (kbytes)          unlimited
-l: locked-in-memory size (kbytes)  unlimited
-u: processes                       1418
-n: file descriptors                7168
```

3. Note the original number

4. Suppose we want to increase the number of processes (I suggest ajusting by close steps):

```bash
ulimit -u 1518
```
5. Repeat until it's working. If not, revert the original value.
 
