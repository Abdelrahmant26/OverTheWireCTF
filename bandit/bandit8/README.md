our challenge here is a bit tricky , I used a simple for loop to iterate over each probable pass>
```bash
bandit8@bandit:~$ for i in $(cat data.txt | sort -u); do echo -n $i" " ; grep -c $i data.txt ;do>
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
