-- -
- Tags: #ejptmachine #curl #nibbleblog #rce #fileuploadvulnerability
--- -
### ¿Qué habilidades puedes practicar con Nibbles?

1. **Reconocimiento y escaneo de puertos:**
    - Al igual que en la mayoría de las máquinas de Hack The Box, el primer paso es realizar un **escaneo de puertos** para identificar los servicios y puertos abiertos. Utilizando herramientas como **Nmap** podrás obtener una visión clara de los servicios disponibles, lo que te permitirá planificar tu ataque.
2. **Explotación de vulnerabilidades web:**
    - En **Nibbles**, uno de los principales vectores de ataque es una vulnerabilidad en una aplicación web. Deberás realizar un **reconocimiento web** exhaustivo para identificar posibles **vulnerabilidades** como **inyección SQL (SQLi)**, **cross-site scripting (XSS)**, o **autenticación débil**.
    - Este tipo de vulnerabilidad te permitirá obtener acceso al sistema o **obtener credenciales** que te ayuden en etapas posteriores del proceso.
3. **Escalada de privilegios en Linux:**
    - Una vez que consigas acceso inicial al sistema, tendrás que realizar una **escalada de privilegios** para obtener privilegios más altos en el sistema **Linux**. Buscarás **permisos incorrectos** en archivos importantes o configuraciones erróneas en servicios que puedan permitirte elevar tus privilegios.
    - También es común que se necesite **abuso de configuraciones de Sudo** o el aprovechamiento de **vulnerabilidades locales** para lograr la escalada.

---

`nmap -p- --open -sCV -n 10.10.10.75 -oN portscan`
![Pasted image 20241127223822](../../Fotos/Pasted%20image%2020241127223822.png)

![Pasted image 20241127223902](../../Fotos/Pasted%20image%2020241127223902.png)
![Pasted image 20241127225516](../../Fotos/Pasted%20image%2020241127225516.png)
`curl -vsk http://10.10.10.75/`
![Pasted image 20241127225529](../../Fotos/Pasted%20image%2020241127225529.png)
- /nibbleblog/

Con gobuster descubrimos también /README allí encontraremos la versión de nibbleblog
![Pasted image 20241128135334](../../Fotos/Pasted%20image%2020241128135334.png)
![Pasted image 20241128135203](../../Fotos/Pasted%20image%2020241128135203.png)

`dirb http://10.10.10.75/nibbleblog/`
![Pasted image 20241127225859](../../Fotos/Pasted%20image%2020241127225859.png)
- admin
- admin.php

Finalmente desubrimos que la contrasena esta por defecto como
admin
nibbles
![Pasted image 20241128134651](../../Fotos/Pasted%20image%2020241128134651.png)

![Pasted image 20241128135504](../../Fotos/Pasted%20image%2020241128135504.png)

![Pasted image 20241128135648](../../Fotos/Pasted%20image%2020241128135648.png)

![Pasted image 20241128135906](../../Fotos/Pasted%20image%2020241128135906.png)

![Pasted image 20241128135913](../../Fotos/Pasted%20image%2020241128135913.png)

![Pasted image 20241128140933](../../Fotos/Pasted%20image%2020241128140933.png)
user flag: 8e269bd3baff49efe49caf6107115701

`Maquina Vulnerada`


*Escalada de privilegios*

`locate php-reverse-shell`
- En kali linux por defecto tenemos esta reverse shell que emplearemos
![Pasted image 20241128142245](../../Fotos/Pasted%20image%2020241128142245.png)

![Pasted image 20241128142330](../../Fotos/Pasted%20image%2020241128142330.png)

Modificamos solo la ip , el puerto es a eleccion
![Pasted image 20241128142426](../../Fotos/Pasted%20image%2020241128142426.png)

![Pasted image 20241128142453](../../Fotos/Pasted%20image%2020241128142453.png)

![Pasted image 20241128142708](../../Fotos/Pasted%20image%2020241128142708.png)

Subimos un archivo se piensa que es una imagen pero es un archivo en este caso.

![Pasted image 20241128142802](../../Fotos/Pasted%20image%2020241128142802.png)
save changes

![Pasted image 20241128143217](../../Fotos/Pasted%20image%2020241128143217.png)

Entramos a esta ruta para la ejecución
![Pasted image 20241128143245](../../Fotos/Pasted%20image%2020241128143245.png)

![Pasted image 20241128143313](../../Fotos/Pasted%20image%2020241128143313.png)

Ahora si podemos hacer unzip
![Pasted image 20241128143510](../../Fotos/Pasted%20image%2020241128143510.png)

![Pasted image 20241128143619](../../Fotos/Pasted%20image%2020241128143619.png)
cat monitor.sh
![Pasted image 20241128143632](../../Fotos/Pasted%20image%2020241128143632.png)

`sudo -l`
![Pasted image 20241128143720](../../Fotos/Pasted%20image%2020241128143720.png)
- Con sudo -l vemos que el usuario nibbler puede ejecutar el archivo monitor.sh ademas tenemos permisos para modificar este script.

Lo que haremos será entonces modificar todo el script hacer que se ejecute el comando su para que pasemos a usuario root en la maquina victima.
![Pasted image 20241128143932](../../Fotos/Pasted%20image%2020241128143932.png)

`sudo -u root /home/nibbler/personal/stuff/monitor.sh`
![Pasted image 20241128144634](../../Fotos/Pasted%20image%2020241128144634.png)

![Pasted image 20241128144952](../../Fotos/Pasted%20image%2020241128144952.png)

---

La flag se pudo haber obtenido también asi
![Pasted image 20241128144752](../../Fotos/Pasted%20image%2020241128144752.png)

root flag: e674f42256cf8b4cc10a02cb1094df22

CVE-2015-6967