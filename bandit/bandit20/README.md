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
