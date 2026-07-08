- --
- Tags: #ejptmachine  #curl #token #suid #nano
---

![Pasted image 20250119175537](../../Fotos/Pasted%20image%2020250119175537.png)

![Pasted image 20250119175525](../../Fotos/Pasted%20image%2020250119175525.png)

![Pasted image 20250119175915](../../Fotos/Pasted%20image%2020250119175915.png)

![Pasted image 20250119175958](../../Fotos/Pasted%20image%2020250119175958.png)

![Pasted image 20250119180151](../../Fotos/Pasted%20image%2020250119180151.png)

Obtenemos password 

![Pasted image 20250119181944](../../Fotos/Pasted%20image%2020250119181944.png)

- lapassworddebackupmaschingonadetodas

Forma de obtenerlo tambien por curl

![Pasted image 20250119182757](../../Fotos/Pasted%20image%2020250119182757.png)


`hydra -L /usr/share/wordlists/rockyou.txt -p lapassworddebackupmaschingonadetodas ssh://172.17.0.2:5000 -t 10`

![Pasted image 20250119182531](../../Fotos/Pasted%20image%2020250119182531.png)

- lovely:lapassworddebackupmaschingonadetodas

![Pasted image 20250119182845](../../Fotos/Pasted%20image%2020250119182845.png)

![Pasted image 20250119182949](../../Fotos/Pasted%20image%2020250119182949.png)

Ctrl X
Ctrl R

![Pasted image 20250119183029](../../Fotos/Pasted%20image%2020250119183029.png)

![Pasted image 20250119183154](../../Fotos/Pasted%20image%2020250119183154.png)

---

*Otra forma escalada de privilegios*

![Pasted image 20250119183358](../../Fotos/Pasted%20image%2020250119183358.png)