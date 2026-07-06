- --
- Tags: #ejptmachine #burpsuite #curl #wget 
-- --
### ¿Qué habilidades puedes practicar con Bank?

1. **Reconocimiento y escaneo de puertos:**
    - Al igual que en la mayoría de las máquinas de Hack The Box, el primer paso es realizar un **escaneo de puertos** para identificar los servicios y puertos abiertos. Utilizando herramientas como **Nmap** podrás obtener una visión clara de los servicios disponibles, lo que te permitirá planificar tu ataque.
2. **Explotación de vulnerabilidades web:**
    - En **Bank**, uno de los principales vectores de ataque es una vulnerabilidad en una aplicación web. Deberás realizar un **reconocimiento web** exhaustivo para identificar posibles **vulnerabilidades** como **inyección SQL (SQLi)**, **cross-site scripting (XSS)**, o **autenticación débil**.
    - Este tipo de vulnerabilidad te permitirá obtener acceso al sistema o **obtener credenciales** que te ayuden en etapas posteriores del proceso.
3. **Escalada de privilegios en Linux:**
    - Una vez que consigas acceso inicial al sistema, tendrás que realizar una **escalada de privilegios** para obtener privilegios más altos en el sistema **Linux**. Buscarás **permisos incorrectos** en archivos importantes o configuraciones erróneas en servicios que puedan permitirte elevar tus privilegios.
    - También es común que se necesite **abuso de configuraciones de Sudo** o el aprovechamiento de **vulnerabilidades locales** para lograr la escalada.
---

Walkthrough: https://www.youtube.com/watch?v=gh8zdCQVGlk&ab_channel=ElPing%C3%BCinodeMario
https://0xdf.gitlab.io/2020/07/07/htb-bank.html

`nmap -p- --min-rate 5000 -sCV -n 10.10.10.29 -oN portScan`
![Pasted image 20241208170350](../../Fotos/Pasted%20image%2020241208170350.png)

`dig axfr bank.htb @10.10.10.29`
- Para ver host domain
![Pasted image 20241208173038](../../Fotos/Pasted%20image%2020241208173038.png)
`10.10.10.29 bank.htb chris.bank.htb ns.bank.htb www.bank.htb`
- Agregamos al /etc/hosts 
![Pasted image 20241208173128](../../Fotos/Pasted%20image%2020241208173128.png)

Visitar http://www.bank.htb, http://chris.bank.htb, http://ns.bank.htb también conduce a este sitio.
![Pasted image 20241208173334](../../Fotos/Pasted%20image%2020241208173334.png)

Sin embargo http://bank.htb/ automaticamente nos redirige a login.php 
![Pasted image 20241208173512](../../Fotos/Pasted%20image%2020241208173512.png)

A la dirección bank.htb apuntamos con gobuster para buscar directorios
`gobuster dir -u http://bank.htb -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php`
![Pasted image 20241208210826](../../Fotos/Pasted%20image%2020241208210826.png)
- Se suponía viendo walkthrough que tenia que tirar el directorio balance-transfer pero tardaba demasiado 

Cambiamos mejor a wfuzz con estos parámetros específicos
`wfuzz -c --hc 404 -t 200 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://bank.htb/FUZZ`
![Pasted image 20241208212610](../../Fotos/Pasted%20image%2020241208212610.png)
- Enumeración de directorios web Exitoso.

Haremos un filtro utilizando expresiones regulares.
![Pasted image 20241208222710](../../Fotos/Pasted%20image%2020241208222710.png)

![Pasted image 20241208223830](../../Fotos/Pasted%20image%2020241208223830.png)

`curl http://bank.htb/balance-transfer/ | awk '{print $6, $10}' | tr -d '"' | grep "align=right>2"`
![Pasted image 20241208223837](../../Fotos/Pasted%20image%2020241208223837.png)
- La idea es ver si hay algún numero diferente refiriéndonos al tamaño del archivo para ver si podría ese tener algo interesante quizás.
- Nombre de fichero: 68576f20e9732f1b2edc4df5b8533230.acc

Haremos un wget para traérnoslo a nuestra maquina.
`wget http://bank.htb/balance-transfer/68576f20e9732f1b2edc4df5b8533230.acc`
![Pasted image 20241208224116](../../Fotos/Pasted%20image%2020241208224116.png)

Obtenemos credenciales que estaban en el archivo
![Pasted image 20241208224155](../../Fotos/Pasted%20image%2020241208224155.png)

Logeamos con las credenciales hayadas
![Pasted image 20241213205156](../../Fotos/Pasted%20image%2020241213205156.png)

La web funciona con extensión php y hay una carga de archivos como vemos en la parte de support por ende intentaremos cargar un archivo malicioso para intentar obtener una reverse shell
- Código malicioso.php
![Pasted image 20241213212808](../../Fotos/Pasted%20image%2020241213212808.png)
![Pasted image 20241213212857](../../Fotos/Pasted%20image%2020241213212857.png)
Cargamos
![Pasted image 20241213212941](../../Fotos/Pasted%20image%2020241213212941.png)
No acepta la carga de archivos php.
![Pasted image 20241213213005](../../Fotos/Pasted%20image%2020241213213005.png)

Cuando esto sucede podemos intentar con Burpsuite forzar que nos acepte este formato
- Interceptamos la carga del archivo
![Pasted image 20241213221743](../../Fotos/Pasted%20image%2020241213221743.png)
- Podríamos cambiar el formato donde dice application/x-php y cambiar por otra extensión e ir probando a punta de prueba y error pero revisemos que entrega por detras la data del error
![Pasted image 20241213222049](../../Fotos/Pasted%20image%2020241213222049.png)
- Dice que si cargamos un archivo con extensión .htb se ejecutara como un .php y con esto ya lo tenemos hecho.
![Pasted image 20241213222255](../../Fotos/Pasted%20image%2020241213222255.png)
Cargamos .htb
![Pasted image 20241213222306](../../Fotos/Pasted%20image%2020241213222306.png)
Carga exitosa 
![Pasted image 20241213222326](../../Fotos/Pasted%20image%2020241213222326.png)
Ahora nos da acceso al directorio upbloads al cual antes no podíamos ingresar
![Pasted image 20241213222442](../../Fotos/Pasted%20image%2020241213222442.png)
- Podríamos ya ejecutar comandos en teoría

![Pasted image 20241213222531](../../Fotos/Pasted%20image%2020241213222531.png)

Urlencodeamos la reverse shell
![Pasted image 20241213223458](../../Fotos/Pasted%20image%2020241213223458.png)
Estabamos esperando en escucha con netcat por el puesto respectivo
![Pasted image 20241213223540](../../Fotos/Pasted%20image%2020241213223540.png)

![Pasted image 20241213223620](../../Fotos/Pasted%20image%2020241213223620.png)

`Maquina Vulnerada`

User Flag: c196bfe044a0cf906a362e21aa867536

----

*Escalada de privilegios*

![Pasted image 20241213224009](../../Fotos/Pasted%20image%2020241213224009.png)

![Pasted image 20241213224218](../../Fotos/Pasted%20image%2020241213224218.png)

`Maquina Vulnerada`

![Pasted image 20241213224249](../../Fotos/Pasted%20image%2020241213224249.png)

Root Flag: b8bf44549a3d1e41d075e069fbb73854

---

*Preguntas*

- Optional question: There is an alternative way to bypass the login form on Bank without finding creds for chris. What kind of vulnerability allows for this? Choose from SQLI, EAR, XSS, or XSRF.

La vulnerabilidad **EAR** (Error-based Authentication Bypass) ocurre cuando una aplicación web maneja de manera inapropiada los errores de base de datos durante el proceso de autenticación. Por lo general, esta vulnerabilidad ocurre cuando el sistema permite que los mensajes de error devuelvan información sobre la base de datos (como detalles del esquema o las consultas SQL), lo que puede ayudar a un atacante a modificar la consulta para omitir el inicio de sesión.

En el contexto de **Bank**, un atacante podría explotar una falla de **EAR** al provocar un error en la base de datos durante el inicio de sesión. Si la aplicación no filtra adecuadamente los mensajes de error o no los maneja de manera segura, un atacante podría usar estos errores para ajustar su entrada y evadir la autenticación.

---

- There's an alternative path from www-data to root on Bank. What is the full path of the critical Linux system file that is world-writable that shouldn't be?

/etc/passwd

![Pasted image 20241213232658](../../Fotos/Pasted%20image%2020241213232658.png)
Ponemos por ejemplo hola  confirmamos hola, nos tira un cash, reemplazamos en /etc/passwd
![Pasted image 20241213232707](../../Fotos/Pasted%20image%2020241213232707.png)
Al nosotros querer logear como root pasa por el /etc/passwd y luego por el /etc/shadow donde estan las contraseñas hasheadas pero al estar ya la contraseña hasheada en el passwd no revisa el /etc/shadow y permite el acceso con la contraseña que creamos ya que tenemos en este caso permisos de escritura en este directorio.
![Pasted image 20241213232810](../../Fotos/Pasted%20image%2020241213232810.png)