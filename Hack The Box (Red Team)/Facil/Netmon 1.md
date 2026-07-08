-- --
Tags: #ejptmachine 
-- --

*Esta maquina se puede resolver mediante el protocolo FTP o mediante Searchsploit y bajar un script de manera local o la red, veremos las dos formas*

FTP
https://snowscan.io/htb-writeup-netmon/#
Searchsploit + psexec.py
https://hipotermia.pw/htb/netmon
https://0xrick.github.io/hack-the-box/netmon/
### Forma 1 FTP

`nmap -p- --min-rate 5000 -sCV -n 10.10.10.152 -oN portScan`

![Pasted image 20250112145916](../../Fotos/Pasted%20image%2020250112145916.png)

![Pasted image 20250112145958](../../Fotos/Pasted%20image%2020250112145958.png)

![Pasted image 20250112150328](../../Fotos/Pasted%20image%2020250112150328.png)

- Credenciales por defecto no funcionan prtgadmin:prtgadmin 

![Pasted image 20250112150601](../../Fotos/Pasted%20image%2020250112150601.png)

Nos movemos cada vez mas dentro del directorio Users hasta llegar al user.txt y descargarlo

![Pasted image 20250112150723](../../Fotos/Pasted%20image%2020250112150723.png)

![Pasted image 20250112150738](../../Fotos/Pasted%20image%2020250112150738.png)

User Flag: 84d35483e8d2577f967d818809f88b65

`/progradata/paessler/PRTG Network Monitor`
Activamos modo binario para no tener problemas con la descarga

![Pasted image 20250112151726](../../Fotos/Pasted%20image%2020250112151726.png)

![Pasted image 20250112152759](../../Fotos/Pasted%20image%2020250112152759.png)

prtgadmin
- Vemos que la clave no se decoficia con base64 revisaremos mas archivos

![Pasted image 20250112153454](../../Fotos/Pasted%20image%2020250112153454.png)

![Pasted image 20250112153628](../../Fotos/Pasted%20image%2020250112153628.png)

PrTg@dmin2018
- Recordemos que es un OLD backup entonces la contrasena como esperabamos no funcionaba en el panel de logeo del puerto 80, probamos cambiando la fecha por 2019 en adelante y permite el logeo
- prtgadmin:PrTg@dmin2019

![Pasted image 20250112154120](../../Fotos/Pasted%20image%2020250112154120.png)

- De partida ya vemos la versión instalada pero eso lo tocaremos para descargar script con searchsploit y psexec en la forma 2.

![Pasted image 20250112155900](../../Fotos/Pasted%20image%2020250112155900.png)

I’ll follow the post, and go to Setup > Account Settings > Notifications:

![Pasted image 20250112160129](../../Fotos/Pasted%20image%2020250112160129.png)

![Pasted image 20250112160147](../../Fotos/Pasted%20image%2020250112160147.png)

Bajamos

![Pasted image 20250112160247](../../Fotos/Pasted%20image%2020250112160247.png)

Elegimos el de abajo

![Pasted image 20250112160312](../../Fotos/Pasted%20image%2020250112160312.png)

----

- *Paso no necesario de colocar usuario y clave en los parámetros de la notificación*

---
![Pasted image 20250112160419](../../Fotos/Pasted%20image%2020250112160419.png)

- Parametros: test.txt;net user anon p3nT3st! /add;net localgroup administrators anon /add
Guardamos

Hacemos click en notificación en el recuadro a la derecha y luego a la campana para hacer una notificación de prueba

![Pasted image 20250112160556](../../Fotos/Pasted%20image%2020250112160556.png)

Resultado

![Pasted image 20250112160626](../../Fotos/Pasted%20image%2020250112160626.png)


----

https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#powershell
https://swisskyrepo.github.io/InternalAllTheThings/cheatsheets/shell-reverse-cheatsheet/#openssl

![Pasted image 20250112161726](../../Fotos/Pasted%20image%2020250112161726.png)

- Usaremos el del medio

Lo unico que cambiaremos sera el unicio que solo le mandaremos shell; y la ip con el puerto

![Pasted image 20250112162110](../../Fotos/Pasted%20image%2020250112162110.png)

- Esta segunda linea shell; .... es la que realmente necesitamos para vulnerar la maquina y generar la reverse shell

![Pasted image 20250112162141](../../Fotos/Pasted%20image%2020250112162141.png)

![Pasted image 20250112162155](../../Fotos/Pasted%20image%2020250112162155.png)

`Maquina Vulnerada`

user.txt

![Pasted image 20250112162436](../../Fotos/Pasted%20image%2020250112162436.png)

root.txt

![Pasted image 20250112162325](../../Fotos/Pasted%20image%2020250112162325.png)

Root Flag: 7db8898f76e38ccef018607cfa2bd359

-----
### Forma 2 Searchsploit y psexec.py de impacket

**Searchsploit + psexec.py**, una vez obtenido las credenciales del panel de prtg system administrator podemos ver la versión que esta corriendo lo cual es útil para ver si existe algún exploits disponible.

`searchsploit prtg network monitor 18.2.38`

![Pasted image 20250112163741](../../Fotos/Pasted%20image%2020250112163741.png)

![Pasted image 20250112171705](../../Fotos/Pasted%20image%2020250112171705.png)

User credentials : `pentest:P3nT3st!`

`python3 /usr/local/bin/psexec.py anon@10.10.10.152`

![Pasted image 20250112172502](../../Fotos/Pasted%20image%2020250112172502.png)

`Maquina Vulnerada`

