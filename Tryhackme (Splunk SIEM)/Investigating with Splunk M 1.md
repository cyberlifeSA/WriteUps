- --
- Tags: #splunk #siem 
- - -
**TRYHACKME**

*Pregunta 1*
¿Cuántos eventos se recopilaron e ingerieron en el **índice principal**?
Filtro: `index=*`

![Pasted image 20260115160820](../Fotos/Pasted%20image%2020260115160820.png)

Respuesta: 12256

*Pregunta 2*
En uno de los huéspedes infectados, el adversario logró crear un usuario de backdoor. ¿Cuál es el nuevo nombre de usuario?
Filtro: `EventID=4720`

![Pasted image 20260115161515](../Fotos/Pasted%20image%2020260115161515.png)

Respuesta: A1berto

*Pregunta 3*
En el mismo host, también se actualizó una clave de registro respecto al nuevo usuario de backdoor. ¿Cuál es el camino completo de esa clave del registro?
Filtro: `EventID=13 "A1berto"`

![Pasted image 20260115163638](../Fotos/Pasted%20image%2020260115163638.png)

Respuesta: HKLM\SAM\SAM\Domains\Account\Users\Names\A1berto

![Pasted image 20260323154521](../Fotos/Pasted%20image%2020260323154521.png)

*Pregunta 4*
Examina los registros e identifica al usuario al que el adversario intentaba suplantar.

![Pasted image 20260115163813](../Fotos/Pasted%20image%2020260115163813.png)

Respuesta: Alberto

*Pregunta 5*
¿Cuál es el comando que se usa para añadir un usuario de puerta trasera desde un ordenador remoto?
Filtro: `EventID=4688 "net user"`

![Pasted image 20260115164203](../Fotos/Pasted%20image%2020260115164203.png)

Instruccion remota

![Pasted image 20260115164150](../Fotos/Pasted%20image%2020260115164150.png)

Respuesta: C:\windows\System32\Wbem\WMIC.exe" /node:WORKSTATION6 process call create "net user /add A1berto paw0rd1

*Pregunta 6*
¿Cuántas veces se observó el intento de inicio de sesión del usuario de la puerta trasera durante la investigación?
Filtro nuevamente: `EventID=4624 "Alberto"`

![Pasted image 20260115171529](../Fotos/Pasted%20image%2020260115171529.png)

Respuesta: 0

*Pregunta 7*
¿Cuál es el nombre del host infectado en el que se ejecutaron comandos sospechosos de Powershell?
Filtro nuevamente: `EventID=4688 "net user"`

![Pasted image 20260115170308](../Fotos/Pasted%20image%2020260115170308.png)

![Pasted image 20260115170258](../Fotos/Pasted%20image%2020260115170258.png)

Respuesta: James.browne 

*Pregunta 8*
El registro de PowerShell está habilitado en este dispositivo. ¿Cuántos eventos se registraron para la ejecución maliciosa de PowerShell?
El ID del evento para el inicio de sesión en PowerShell es 4103 4104
Filtro: `EventID=4103` 

![Pasted image 20260115170603](../Fotos/Pasted%20image%2020260115170603.png)

![Pasted image 20260115171006](../Fotos/Pasted%20image%2020260115171006.png)

Sabemos que son 79 por los resultados y sabemos que es malicioso porque son todos estos eventos provinientes del u suario James.Browne que se encuentra infectado.
Respuesta: 79

*Pregunta 9*
Un script Powershell codificado desde el host infectado iniciaba una solicitud web. ¿Cuál es la URL completa?
Filtro: `EventID=4103 James.browne`

![Pasted image 20260115172701](../Fotos/Pasted%20image%2020260115172701.png)

decode

![Pasted image 20260115172724](../Fotos/Pasted%20image%2020260115172724.png)

decode

![Pasted image 20260115172739](../Fotos/Pasted%20image%2020260115172739.png)

Luego agregamos el sub directorio

![Pasted image 20260115172949](../Fotos/Pasted%20image%2020260115172949.png)

Respuesta: hxxp[://]10[.]10[.]10[.]5/news[.]php  
THM **requiere formato defanged** (con corchetes [.]) para **evitar clics accidentales** en C2 URLs.


