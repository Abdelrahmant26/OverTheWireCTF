Because the '-' character is reserved for bash to make the executable read "Standard input",In 
order to read a file called '-' u must specify it inside an absolute or cd to its parent directory and use relative path
```bash
cat /home/bandit1/- 
```
OR 
```bash
cd /home/bandit1/ && cat ./-
```
-------------------------------
Spaces inside a file name is very annoying, To make the shell interpret its name correctly, put it inside quotation marks
```bash
cat "spaces in this filename"
```
U can also use a backslash to escape the spaces in the file name
```bash
cat spaces\ in\ this\ filename 
```
----------------------------------
In linux, a hidden file starts with a dot '.', to show a hidden file use "ls -a"
```bash
bandit3@bandit:~/inhere$ ls -a
.  ..  ...Hiding-From-You
```
here the file is called ...Hiding-from-you
```bash
 cat ...Hiding-From-You
```
--------------------------------
here we have several files to test, we have to loop over them using the find command
```bash
find . -type f -exec file {} +
./-file00: data
./-file03: data
./-file08: data
./-file02: data
./-file04: data
./-file01: data
./-file07: ASCII text
./-file06: data
./-file05: data
./-file09: data
```
we got it! The file07 is the one, simply cat it,but since the naming is not normal as it has the '-' character, use absolute/relative paths as in bandit1 
```bash
cd inhere && cat ./-file07
```
----------------------------
here we have to search all over a directory contains lots of sub-directories and file , It's a matter of some filters in find command
```bash
bandit5@bandit:~/inhere$ find . -size 1033c -not -executable
./maybehere07/.file2
```
----------------------------
Again, it's a matter of some filters in find command but also we will face a minor problem, since we search all over the filesystem we will encounter some garbage results/errors such as searching in /proc directory 
```bash
bandit6@bandit:~$ find / -user bandit7 -group bandit6
find: ‘/sys/kernel/tracing’: Permission denied
find: ‘/sys/kernel/debug’: Permission denied
find: ‘/sys/fs/pstore’: Permission denied
find: ‘/sys/fs/bpf’: Permission denied
find: ‘/snap’: Permission denied
find: ‘/run/lock/lvm’: Permission denied
find: ‘/run/systemd/inaccessible/dir’: Permission denied
find: ‘/run/systemd/propagate/systemd-udevd.service’: Permission denied
find: ‘/run/systemd/propagate/systemd-resolved.service’: Permission denied
find: ‘/run/systemd/propagate/systemd-networkd.service’: Permission denied
find: ‘/run/systemd/propagate/systemd-logind.service’: Permission denied
find: ‘/run/systemd/propagate/irqbalance.service’: Permission denied
find: ‘/run/systemd/propagate/chrony.service’: Permission denied
find: ‘/run/systemd/propagate/polkit.service’: Permission denied
find: ‘/run/systemd/propagate/ModemManager.service’: Permission denied
find: ‘/run/systemd/propagate/fwupd.service’: Permission denied
find: ‘/run/lvm’: Permission denied
find: ‘/run/cryptsetup’: Permission denied
find: ‘/run/multipath’: Permission denied
.....
```
to solve this problem, we have to use redirection to redirect any STDERR to the bit bucket "/dev/null" which makes nothing except recieving input 
```bash
bandit6@bandit:~$ find / -user bandit7 -group bandit6 2>/dev/null
/var/lib/dpkg/info/bandit7.password
```
-----------------------------------------------------------------
here we just use grep command to search for occurrence of "millionth word"
```bash
bandit7@bandit:~$ cat data.txt | grep millionth
millionth	dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```
---------------------------------------------
