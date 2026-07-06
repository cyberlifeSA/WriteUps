- ---
- Tags: #ejptmachine #joomla  #webshell #sql 
---

![Pasted image 20250125204204](../../Fotos/Pasted%20image%2020250125204204.png)
![Pasted image 20250125204238](../../Fotos/Pasted%20image%2020250125204238.png)

![Pasted image 20250125211202](../../Fotos/Pasted%20image%2020250125211202.png)

![Pasted image 20250125211314](../../Fotos/Pasted%20image%2020250125211314.png)

![Pasted image 20250125212252](../../Fotos/Pasted%20image%2020250125212252.png)
- admin:c2FubHVpczEyMzQ1
- Descubrimos ruta /un_caramelo

/un_caramelo
![Pasted image 20250125212428](../../Fotos/Pasted%20image%2020250125212428.png)
![Pasted image 20250125212448](../../Fotos/Pasted%20image%2020250125212448.png)

![Pasted image 20250125212633](../../Fotos/Pasted%20image%2020250125212633.png)
- sanluis12345

/adminsitrator
![Pasted image 20250125213616](../../Fotos/Pasted%20image%2020250125213616.png)


System/ site templates/cassiopeia
Incluimos una RCE en el index/php
![Pasted image 20250125214523](../../Fotos/Pasted%20image%2020250125214523.png)
![Pasted image 20250125214613](../../Fotos/Pasted%20image%2020250125214613.png)
- Damos Save 

![Pasted image 20250125214854](../../Fotos/Pasted%20image%2020250125214854.png)

`nc -nvlp 444`

![Pasted image 20250125215248](../../Fotos/Pasted%20image%2020250125215248.png)

![Pasted image 20250125215259](../../Fotos/Pasted%20image%2020250125215259.png)

![Pasted image 20250125220209](../../Fotos/Pasted%20image%2020250125220209.png) 
- joomla_db:luisillo123456 
![Pasted image 20250125220323](../../Fotos/Pasted%20image%2020250125220323.png)

-----
- Solo buscando
![Pasted image 20250125220543](../../Fotos/Pasted%20image%2020250125220543.png)
![Pasted image 20250125220558](../../Fotos/Pasted%20image%2020250125220558.png)![Pasted image 20250125220608](../../Fotos/Pasted%20image%2020250125220608.png)

---

Por lo que vemos tenemos que pasar primero a usuario luisillo
`find / -user luisillo 2>/dev/null`
![Pasted image 20250125220747](../../Fotos/Pasted%20image%2020250125220747.png)

`cat /var/backups/hidden/otro_caramelo.txt`
![Pasted image 20250125220900](../../Fotos/Pasted%20image%2020250125220900.png)
- luisillo:luisillosuperpassword

![Pasted image 20250125221001](../../Fotos/Pasted%20image%2020250125221001.png)

![Pasted image 20250125221033](../../Fotos/Pasted%20image%2020250125221033.png)

----
### FORMA 1 con OPENSSL generando nosotros la clave y modificándola en el passwd en root
**Forma mas facil sin tanta complicacion**

![Pasted image 20250125221640](../../Fotos/Pasted%20image%2020250125221640.png)
![Pasted image 20250125221649](../../Fotos/Pasted%20image%2020250125221649.png)
![Pasted image 20250125221704](../../Fotos/Pasted%20image%2020250125221704.png)
![Pasted image 20250125221722](../../Fotos/Pasted%20image%2020250125221722.png)
![Pasted image 20250125221738](../../Fotos/Pasted%20image%2020250125221738.png)

---
### Forma 2 con GTFOBINS

![Pasted image 20250125221838](../../Fotos/Pasted%20image%2020250125221838.png)
- Personalmente no nos va pero es posible en teoria escalar privilegios de esta manera