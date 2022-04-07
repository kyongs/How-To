#### 1. Change permission

```Shell
$ sudo bash
# echo 0 > /proc/sys/kernel/yama/ptrace_scope
```

If you want to maintain the configuration when the server is restarted, change `sysctl.d` file.
You have to change `kernel.yama.ptrace_scope=1` to `kernel.yama.ptrace_scope=0`.

```Shell
$ cd /etc/sysctl.d
$ vi 10-ptrace.conf
```

```Shell
...
...
"10-ptrace.conf" 22L, 1292C                                                                                                                                                                    1,1           All
# The PTRACE system is used for debugging.  With it, a single user process
# can attach to any other dumpable process owned by the same user.  In the
# case of malicious software, it is possible to use PTRACE to access
# credentials that exist in memory (re-using existing SSH connections,
# extracting GPG agent information, etc).
#
# A PTRACE scope of "0" is the more permissive mode.  A scope of "1" limits
# PTRACE only to direct child processes (e.g. "gdb name-of-program" and
# "strace -f name-of-program" work, but gdb's "attach" and "strace -fp $PID"
# do not).  The PTRACE scope is ignored when a user has CAP_SYS_PTRACE, so
# "sudo strace -fp $PID" will work as before.  For more details see:
# https://wiki.ubuntu.com/SecurityTeam/Roadmap/KernelHardening#ptrace
#
# For applications launching crash handlers that need PTRACE, exceptions can
# be registered by the debugee by declaring in the segfault handler
# specifically which process will be using PTRACE on the debugee:
#   prctl(PR_SET_PTRACER, debugger_pid, 0, 0, 0);
#
# In general, PTRACE is not needed for the average running Ubuntu system.
# To that end, the default is to set the PTRACE scope to "1".  This value
# may not be appropriate for developers or servers with only admin accounts.
kernel.yama.ptrace_scope = 1 ############### Change to 0!!!! ##########
```

#### 2. Check process id of MySQL

```Shell
$ ps -ef | grep mysql
vldb     69448 67836  0 03:15 pts/2    00:00:00 /bin/sh ./bin/mysqld_safe --defaults-file=/usr/local/mysql/my.cnf
vldb     69720 69448 84 03:15 pts/2    00:07:20 /usr/local/mysql/bin/mysqld --defaults-file=/usr/local/mysql/my.cnf --basedir=/usr/local/mysql --datadir=/home/vldb/test-data --plugin-dir=/usr/local/mysql/lib/plugin --log-error=/home/vldb/log-data/mysql_error.log --pid-file=/home/vldb/test-data/mysql.pid --socket=/tmp/mysql.sock --port=3306
vldb     69803 67895 13 03:17 pts/3    00:00:59 ./bin/mysql -uroot -px xx tpcc
vldb     69809 66216  0 03:24 pts/1    00:00:00 grep --color=auto mysql

```

#### 3. Execute GDB and attach pid of MySQL (In this case, `69720`)

```Shell
root@vldb:~# gdb
GNU gdb (Ubuntu 8.1.1-0ubuntu1) 8.1.1
Copyright (C) 2018 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word".
(gdb) attach 69720
Attaching to process 69720
[New LWP 67166]
[New LWP 67167]
[New LWP 67168]
[New LWP 67169]
[New LWP 67170]
[New LWP 67171]
[New LWP 67172]
[New LWP 67173]
[New LWP 67174]
[New LWP 67175]
[New LWP 67176]
[New LWP 67177]
[New LWP 67178]
[New LWP 67179]
[New LWP 67180]
[New LWP 67181]
[New LWP 67182]
[New LWP 67183]
[New LWP 67184]
[New LWP 67189]
[New LWP 67190]
[New LWP 67191]
[New LWP 67192]
[New LWP 67193]
[New LWP 67194]
[New LWP 67195]
[New LWP 67196]
[New LWP 67197]
[New LWP 67198]
[New LWP 67202]
[New LWP 67203]
[New LWP 67204]
[New LWP 67205]
[New LWP 67206]
[New LWP 67207]
[New LWP 67208]
[New LWP 67209]
[New LWP 67210]
[New LWP 67212]
[New LWP 67276]
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
0x00007f7ecce54cb9 in __GI___poll (fds=0x55ad72d61440, nfds=2, timeout=timeout@entry=-1) at ../sysdeps/unix/sysv/linux/poll.c:29
29	../sysdeps/unix/sysv/linux/poll.c: No such file or directory.
(gdb)
```
