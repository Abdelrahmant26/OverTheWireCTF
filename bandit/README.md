Lvl 1 :-
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
Lvl 2 :-
Spaces inside a file name is very annoying, To make the shell interpret its name correctly, put it inside quotation marks
```bash
cat "spaces in this filename"
```
U can also use a backslash to escape the spaces in the file name
```bash
cat spaces\ in\ this\ filename 
```
----------------------------------
Lvl 3 :-
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
Lvl 4 :-
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
Lvl 5 :-
here we have to search all over a directory contains lots of sub-directories and file , It's a matter of some filters in find command
```bash
bandit5@bandit:~/inhere$ find . -size 1033c -not -executable
./maybehere07/.file2
```
----------------------------
Lvl 6 :-
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
Lvl 7 :-
here we just use grep command to search for occurrence of "millionth word"
```bash
bandit7@bandit:~$ cat data.txt | grep millionth
millionth	dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```
---------------------------------------------
Lvl 8 :-
our challenge here is a bit tricky , I used a simple for loop to iterate over each probable password all over the file to make grep show the count of each one and of course the corect one will have only 1 occurrence
```bash
bandit8@bandit:~$ for i in $(cat data.txt | sort -u); do echo -n $i" " ; grep -c $i data.txt ;done
0KCctkqCfY7BIOWqolXsHDaboXVTKZ49 10
1SKCEfQ151hWOx9JkeIAmOQdXiC813h1 10
3hHLofjM7m3sdyiKJF5QsMqvEIfFh5b1 10
3hW8tLnDV8acjhTQi44CKXEzHsJb3sqz 10
3nUXvAjKo7yu6fYykYu7nGGKDMuNMWZf 10
42qjuz5hdLlItNwdJYsDRpkbbvoEYiWK 10
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM 1
5g2sV4OokwqDv29Pfo6C7twjKcOk4WQV 10
5YlL2xxyEUqV6tF0P6NoHt8LOY2EGEcO 10
6lMDNhQjlOoCOZ5F8ULK2g0uT0rCdnoQ 10
...
```
------------------------------------------------
Lvl 9 :-
This challenge is quite simple just use strings command and pass output to grep
```bash
bandit9@bandit:~$ strings data.txt | grep ===
\a!;========== the
========== passwordf
========== isc
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```
----------------------
Lvl 10 :-
Also quite simple, just a one command for decoding
```bash
bandit10@bandit:~$ base64 -d data.txt 
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```
Lvl 11 :-
For this challenge, we need some command to just translate characters in order and according to a specific rule, tr command (which stands for translate) got us covered, according to Wikipedia, ROT13 flips A to M with N to Z and vice versa, A 13 opposite to 13, we need to reverse this translation
```bash
bandit11@bandit:~$ cat data.txt | tr "a-mn-zA-MN-Z" "n-za-mN-ZA-M"
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```
------------------------------
Lvl 12 :-
For this challenge, we have to reverse a hexdump of a compressed file then decompress it
reversing the hexdump can be done using xxd command
```bash
bandit12@bandit:/tmp/midgeek$ xxd -r data.txt > out
```
here I redirected the output to a file called out, now Let's find out what type of compression this file use
```bash
bandit12@bandit:/tmp/midgeek$ file out
out: gzip compressed data, was "data2.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 577
```
It's a gzip format , so we have to rename it to have a proper suffix, then decompress it
```bash 
bandit12@bandit:/tmp/midgeek$ mv out out.gz && gzip -d out.gz
```
Again, check the output file's compression type
```bash
bandit12@bandit:/tmp/midgeek$ file out
out: bzip2 compressed data, block size = 900k
```
Now It's another type, a bzip, so we decompress also
```bash
bandit12@bandit:/tmp/midgeek$ bunzip2 -d out
bunzip2: Can't guess original name for out -- using out.out
```
And decompression goes on ...
```bash
bandit12@bandit:/tmp/midgeek$ file out.out 
out.out: gzip compressed data, was "data4.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 20480
bandit12@bandit:/tmp/midgeek$ mv out.out out.out.gz
bandit12@bandit:/tmp/midgeek$ gzip -d out.out.gz 
bandit12@bandit:/tmp/midgeek$ file out.out 
out.out: POSIX tar archive (GNU)
bandit12@bandit:/tmp/midgeek$ tar -xf out.out
bandit12@bandit:/tmp/midgeek$ ls
data5.bin  data.txt  out.out
bandit12@bandit:/tmp/midgeek$ file data5.bin 
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/midgeek$ tar -xf data5.bin
bandit12@bandit:/tmp/midgeek$ ls
data5.bin  data6.bin  data.txt  out.out
bandit12@bandit:/tmp/midgeek$ file data6.bin 
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/midgeek$ bzip2 -d data6.bin
bzip2: Can\'t guess original name for data6.bin -- using data6.bin.out
bandit12@bandit:/tmp/midgeek$ file data6.bin.out 
data6.bin.out: POSIX tar archive (GNU)
bandit12@bandit:/tmp/midgeek$ tar -xf data6.bin.out
bandit12@bandit:/tmp/midgeek$ ls
data5.bin  data6.bin.out  data8.bin  data.txt  out.out
file data8.bin
data8.bin: gzip compressed data, was "data9.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 49
bandit12@bandit:/tmp/midgeek$ ls
data5.bin  data6.bin.out  data8.bin  data.txt  out.out
bandit12@bandit:/tmp/midgeek$ mv data8.bin data8.bin.gz
bandit12@bandit:/tmp/midgeek$ gzip -d data8.bin.gz 
bandit12@bandit:/tmp/midgeek$ ls
data5.bin  data6.bin.out  data8.bin  data.txt  out.out
bandit12@bandit:/tmp/midgeek$ file data8.bin 
data8.bin: ASCII text
bandit12@bandit:/tmp/midgeek$ cat data8.bin 
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn //finally!
```
--------------------------
Lvl 13 :-
This challenge is quite simple , just use the ssh key in the directory to log in as the next challenge user
```bash
bandit13@bandit:~$ ssh bandit14@localhost -p 2220 -i sshkey.private //and go through ssh stuff
```
So much easy to get the password 
```bash
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```
----------------------------------------
Lvl 14 :-
In this challenge, we need to establish a raw tcp connection to port 30000, we will use netcat
```bash
bandit14@bandit:~$ nc localhost 30000
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS //I entered this , It wawaiting for my input
Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```
Lvl 15 :-
Quite easy, u just have to add --ssl flag to ncat 
```bash
bandit15@bandit:~$ ncat --ssl localhost 30001
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```
-----------
Lvl 16 :-
It's a matter of port scanning, and trying to connect to each one of them, first we will use nmap to scan for the open ports
```bash
bandit16@bandit:~$ nmap 127.0.0.1 -p31000-32000
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-30 10:52 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00016s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.11 seconds
```
Then, we will try to connect to each port individually, using ncat ... the correct port was 31790

```bash
bandit16@bandit:~$ ncat --ssl 127.0.0.1 31790
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
... //rest of the ssh key
``` 
-------------------
Lvl 17 :-
Simply using diff command with the 2 password files as arguments, It showed the line with difference between them
```bash
bandit17@bandit:~$ diff passwords.old passwords.new
42c42
< bSrACvJvvBSxEM2SGsV5sn09vc3xgqyp //here it shows that .new has this line
---
> x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO //here it shows that .old has this line
```
-----------
Lvl 18 :-
Sadly, We cannot login to this level, We will be disconnected every time we try to connect, What a bad luck!
But!!
We can use scp which is used to securely copy files between devices over ssh, we just need to grab that readme file to the local machine 
```bash
$ scp -P 2220 bandit18@bandit.labs.overthewire.org:/home/bandit18/readme . && cat readme
```
------------
Lvl 19 :-
Now, It's a matter of user's permissions, The binary present in the home directory seems to run commands with effective uid for bandit20 
```bash
bandit19@bandit:~$ ./bandit20-do 
Run a command as another user.
  Example: ./bandit20-do id
bandit19@bandit:~$ ./bandit20-do id
uid=11019(bandit19) gid=11019(bandit19) euid=11020(bandit20) groups=11019(bandit19)
```
Which means we can read the file in /etc/bandit_pass as if we were the user bandit20
```bash
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```
--------------
Lvl20 :-
Here we have to to make 2 things at once, open a Listening connection at a specific port and stream a text (the password for the current level) to this connection
first we have to open a netcat server to listen on a port and pipe the password to it
```bash
bandit20@bandit:~$ echo -n "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -lvp 30023 &
[1] 4062049
Listening on 0.0.0.0 30023
```
Now we have a background process of a netcat server listening for a connection to pipe some text to it, SO we have to use that binary using the same port number in the netcat server
```bash
bandit20@bandit:~$ ./suconnect 30023
Connection received on localhost 41856
Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password
bandit20@bandit:~$ EeoULMCra2q0dSkYj561DX7s1CpBuOBt
```
------------------
Lvl 21 :-
This challenge is a matter of some tracing, Reading the files /etc/cron.d and follow paths of executables
```bash
bandit21@bandit:~$ cd /etc/cron.d
bandit21@bandit:/etc/cron.d$ ls
cronjob_bandit22  cronjob_bandit23  cronjob_bandit24  e2scrub_all  otw-tmp-dir  sysstat
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh 
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit:/etc/cron.d$ cat /tmp//t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```
---------------------
Lvl 23 :-
Here, and by following along with the cron job
```bash
bandit22@bandit:~$ cat /etc/cron.d/cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
```
And AFter inspecting that sh script 
```bash
bandit22@bandit:~$ cat /usr/bin/cronjob_bandit23.sh 
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```
We will see that it copies its password value which we need to view but do not have the permission to another non-protected temporary file, All we need is to act as if we were that user as the path of the file is dependent on the username, so I will execute all the commands in this script manually
```bash
bandit22@bandit:~$ myname=bandit23
bandit22@bandit:~$ mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
bandit22@bandit:~$ cat /tmp/$mytarget
0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
```
-------------------
lvl 23 :-
This challenge is a little bit tricky, we can utilise the cron job that executes every script in the /var/spool/bandit24/foo, so we will make a simple script that reads the password from /etc/bandit_pass/bandit24 and send the data to an existing listening socket with netcat
```bash
bandit23@bandit:/tmp/midgeek$ mkdir /tmp/bandd14
bandit23@bandit:/tmp/bandd14$ vim sj.sh
```
```bash
#!/bin/bash
cat /etc/bandit_pass/bandit24 | nc 127.0.0.1 32011
```
Then we have to start the netcat server and copy the script to the desired directory
```bash
bandit23@bandit:/tmp/bandd14$ nc -lvp 32011 > ott &
[1] 1953737
bandit23@bandit:/tmp/bandd14$ Listening on 0.0.0.0 32011
bandit23@bandit:/tmp/bandd14$ cp sj.sh /var/spool/bandit24/foo/script.sh && chmod +x /var/spool/bandit24/foo/script.sh
bandit23@bandit:/tmp/bandd14$ Connection received on localhost 46472 //after a while, now we have the output file
bandit23@bandit:/tmp/bandd14$ cat ott
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```
--------------------------
