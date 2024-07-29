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
FOr this challenge, we need some command to just translate characters in order and according to a specific rule, tr command (which stands for translate) got us covered, according to Wikipedia, ROT13 flips A to M with N to Z and vice versa, A 13 opposite to 13, we need to reverse this translation
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

