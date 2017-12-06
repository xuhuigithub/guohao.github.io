---
title: Linux获取进程运行的完整路径
date: 2017-10-23 11:30:13
tags: linux
---

<p>通过ps及top命令查看进程信息时，只能查到相对路径，查不到的进程的详细信息，如绝对路径等。
<p>这时，可以通过以下的方法来查看进程的详细信息：

<p>Linux在启动一个进程时，系统会在/proc下创建一个以PID命名的文件夹，在该文件夹下会有我们的进程的信息，其中包括一个名为exe的文件即记录了绝对路径，通过ll或ls –l命令即可查看。

```
$ ls -l /proc/PID
```

示例输出如下
> 总用量 0
dr-xr-xr-x    2 work work 0 10月 23 11:25 attr
-r--------    1 work work 0 10月 23 11:25 auxv
-r--r--r--    1 work work 0 10月 23 11:25 cgroup
--w-------    1 work work 0 10月 23 11:25 clear_refs
-r--r--r--    1 work work 0 10月 23 08:48 cmdline
-rw-r--r--    1 work work 0 10月 23 11:25 coredump_filter
-r--r--r--    1 work work 0 10月 23 11:25 cpuset
lrwxrwxrwx    1 work work 0 10月 23 09:03 cwd -> /home/work/guohao/dev/sample
-r--------    1 work work 0 10月 23 11:25 environ
lrwxrwxrwx    1 work work 0 10月 23 08:48 exe -> /home/work/.jumbo/opt/sun-java8/bin/java
dr-x------    2 work work 0 10月 21 18:22 fd
dr-x------    2 work work 0 10月 23 11:25 fdinfo
-r--------    1 work work 0 10月 23 11:25 io
-rw-------    1 work work 0 10月 23 11:25 limits
-rw-r--r--    1 work work 0 10月 23 11:25 loginuid
-r--r--r--    1 work work 0 10月 23 11:25 maps
-rw-------    1 work work 0 10月 23 11:25 mem
-r--r--r--    1 work work 0 10月 23 11:25 mountinfo
-r--r--r--    1 work work 0 10月 23 11:25 mounts
-r--------    1 work work 0 10月 23 11:25 mountstats
dr-xr-xr-x    4 work work 0 10月 23 11:25 net
dr-x--x--x    2 work work 0 10月 23 09:16 ns
-r--r--r--    1 work work 0 10月 23 11:25 numa_maps
-rw-r--r--    1 work work 0 10月 23 11:25 oom_adj
-r--r--r--    1 work work 0 10月 23 11:25 oom_score
-rw-r--r--    1 work work 0 10月 23 11:25 oom_score_adj
-r--r--r--    1 work work 0 10月 23 11:25 pagemap
-r--r--r--    1 work work 0 10月 23 11:25 personality
lrwxrwxrwx    1 work work 0 10月 23 11:25 root -> /
-rw-r--r--    1 work work 0 10月 23 11:25 sched
-r--r--r--    1 work work 0 10月 23 11:25 sessionid
-r--r--r--    1 work work 0 10月 23 11:25 smaps
-r--r--r--    1 work work 0 10月 23 11:25 stack
-r--r--r--    1 work work 0 10月 23 08:48 stat
-r--r--r--    1 work work 0 10月 23 11:32 statm
-r--r--r--    1 work work 0 10月 23 08:48 status
-r--r--r--    1 work work 0 10月 23 11:25 syscall
dr-xr-xr-x  149 work work 0 10月 13 13:28 task
-r--r--r--    1 work work 0 10月 23 11:25 wchan

<p>其中的cwd就是命令执行的目录,exe是程序的绝对路径。
