-- ---
- Tags: #ejptmachine #lfi #rce #elastix
--- -
### ¿Qué habilidades puedes practicar con Beep?

1. **Reconocimiento de servicios:**
    - Comienza con un **escaneo de puertos** con **Nmap** para identificar qué servicios están expuestos.
    - La máquina presenta un servicio web vulnerable, por lo que **enumerar directorios** en la aplicación web utilizando herramientas como **Gobuster** o **Dirbuster** será fundamental.
2. **Explotación de vulnerabilidades web:**
    - La máquina tiene una vulnerabilidad de **inyección SQL (SQLi)**, por lo que necesitarás investigar el sitio web y probar inyecciones SQL para ganar acceso a la máquina.
    - Además, podrás explorar cómo automatizar este proceso con herramientas como **sqlmap** y cómo hacerlo manualmente para profundizar en la explotación.
3. **Escalada de privilegios en Linux:**
    - Una vez que consigas acceso inicial al sistema, deberás realizar una **escalada de privilegios** en **Linux**. Esto puede incluir la búsqueda de archivos con configuraciones incorrectas o vulnerabilidades en las que puedas aprovecharte para ganar acceso root.

---

`nmap -p- --min-rate 5000 -n 10.10.10.7`
- comando está diseñado para descubrir todos los puertos abiertos.

![Pasted image 20241206182133](../../Fotos/Pasted%20image%2020241206182133.png)

![Pasted image 20241206182029](../../Fotos/Pasted%20image%2020241206182029.png)

![Pasted image 20241206182144](../../Fotos/Pasted%20image%2020241206182144.png)

![Pasted image 20241206182306](../../Fotos/Pasted%20image%2020241206182306.png)

![Pasted image 20241206182538](../../Fotos/Pasted%20image%2020241206182538.png)

![Pasted image 20241206182806](../../Fotos/Pasted%20image%2020241206182806.png)

![Pasted image 20241206183617](../../Fotos/Pasted%20image%2020241206183617.png)

![Pasted image 20241206183727](../../Fotos/Pasted%20image%2020241206183727.png)

----

`nmap -sV -O -F --version-light 10.10.10.7`
- comando prioriza información sobre servicios y sistemas operativos.

![Pasted image 20241206191224](../../Fotos/Pasted%20image%2020241206191224.png)

---
---

`whatweb http://10.10.10.7`
![Pasted image 20241206194450](../../Fotos/Pasted%20image%2020241206194450.png)

Esto indica que esta realizando un redirect de http a https

![Pasted image 20241206194609](../../Fotos/Pasted%20image%2020241206194609.png)

![Pasted image 20241206201052](../../Fotos/Pasted%20image%2020241206201052.png)

### Forma 1  Para Local File Inclusion TERMINAR

Elastix es un software de servidor de comunicaciones unificadas, o sea, que reune en un solo programa fax, correo electrónico, mensajería instantanea y muchas otras opciones para comunicarte.

![Pasted image 20241206210838](../../Fotos/Pasted%20image%2020241206210838.png)

- El script esta en pearl pero como es un simple local file inclusion sacaremos lo que nos importa

`searchsploit -x php/webapps/37637.pl`

![Pasted image 20241206210957](../../Fotos/Pasted%20image%2020241206210957.png)

- con ../../../ se hace un directory pass traversal paraa puntar a otra ruta, en si esto nos muestra el contenido en esa direccion

![Pasted image 20241206211128](../../Fotos/Pasted%20image%2020241206211128.png)

Ctrl+u

![Pasted image 20241206211139](../../Fotos/Pasted%20image%2020241206211139.png)

- Es un archivo de configuración 
- Vemos usuarios y contraseñas jEhdIekWmdjE

En este punto ya podemos intentar leer todo tipo de cosas.
- Por ejemplo con proc/net/tcp podemos ver los puertos internos que esten abiertos

![Pasted image 20241206211339](../../Fotos/Pasted%20image%2020241206211339.png)

`https://10.10.10.7/vtigercrm`
admin
jEhdIekWmdjE

![Pasted image 20241206211704](../../Fotos/Pasted%20image%2020241206211704.png)


settings
settings
comunication templates
company details
edit
te permite cambiar la foto de la empresa pero admite solo archivos jpg 

Creamos un archivo

![Pasted image 20241206212233](../../Fotos/Pasted%20image%2020241206212233.png)

![Pasted image 20241206212305](../../Fotos/Pasted%20image%2020241206212305.png)

![Pasted image 20241206212356](../../Fotos/Pasted%20image%2020241206212356.png)

Nos ponemos en escucha con netcat y nos ponemos en escucha por el puerto 443
damos en safe y ya tenemos la reverse shell

*Escalada de privilegios para la forma 1*

`sudo -l`

![Pasted image 20241229185522](../../Fotos/Pasted%20image%2020241229185522.png)

![Pasted image 20241229185508](../../Fotos/Pasted%20image%2020241229185508.png)

- Estos son los únicos 2 binarios que podemos utilizar para la escalada de privilegios desde dentro del sistema vulnerado
(root) NOPASSWD: /usr/bin/nmap
(root) NOPASSWD: /usr/bin/yum

------

### Forma 2
Intentamos por el puerto 10000 y directamente somos root tras cargar una reverse shell en donde nos permite cargar comandos.
root
jEhdIekWmdjE

![Pasted image 20241206214107](../../Fotos/Pasted%20image%2020241206214107.png)

schedule commands

![Pasted image 20241206215114](../../Fotos/Pasted%20image%2020241206215114.png)

![Pasted image 20241206215732](../../Fotos/Pasted%20image%2020241206215732.png)

![Pasted image 20241206220810](../../Fotos/Pasted%20image%2020241206220810.png)

`Maquina Vulnerada`

root flag: 17f95cdd06d56254da783a091c29d6d3
user flag: 81258bf3dcc9e16f0a3cdc2a76fe8780

----

### Forma 3

Volver a revisar y hacer

----

### Posible forma 4

Debido a que existe una vulnerabilidad PHP LFI en Beep, si podemos obtener un webshell en el equipo y luego incluirlo, podemos ejecutarlo de esa manera. Intente enviarle un correo electrónico a Asterisk con un webshell.

----

### Preguntas

Para ver la version de TLS de la web 
curl -vvI https://10.10.10.7
- Muestra informacion detallada de la version de ssl y tls 

What is the common name for the set of 2014 CVEs where this is a POC exploit: () { :; };sleep 10?
¿Cuál es el nombre común para el conjunto de 2014 CVE donde este es un exploit POC: () { :; };sleep 10?
shellshock

What is the full path to the asterisk user's mail folder on Beep?
/var/spool/mail/asterisk (este no era)
/var/mail/asterisk (este era)

There are many other ways to root Beep. These questions after the root flag are hints to help identify them. What is the 2012 CVE ID for pre-authentication remote code execution vulnerablity in FreePBX / Elastix?
- CVE-2012-4869

![Pasted image 20241206230852](../../Fotos/Pasted%20image%2020241206230852.png)