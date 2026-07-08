- --
- Tags: #splunk #btlo #pikaboo_lab
- --

P1) ¿Qué dirección de correo electrónico añadió Will a su página web? (Accesible en el puerto 80) (Formato: Dirección de correo electrónico)

![Pasted image 20260610152205](../Fotos/Pasted%20image%2020260610152205.png)

**Respuesta:** `will.befired@tot.com`

P2) El atacante apuntó a la nube. Consulta la web de Will—¿se filtró algo o se dejó algo atrás? Proporciona la clave de acceso. (Formato: Clave de acceso)

![Pasted image 20260610152913](../Fotos/Pasted%20image%2020260610152913.png)

![Pasted image 20260610153420](../Fotos/Pasted%20image%2020260610153420.png)

![Pasted image 20260610153445](../Fotos/Pasted%20image%2020260610153445.png)

![Pasted image 20260610153756](../Fotos/Pasted%20image%2020260610153756.png)

**Respuesta:** AKIA3EBEV4OQLWYJIHGN

P3) Usando Splunk, encuentra al usuario al que pertenece esta clave de acceso. (Formato: Nombre de usuario)

![Pasted image 20260610154548](../Fotos/Pasted%20image%2020260610154548.png)

![Pasted image 20260610154529](../Fotos/Pasted%20image%2020260610154529.png)

**Respuesta:** thirdPartyVendor 

P4) Michelle usó Internet Explorer para descargar un archivo adjunto del correo electrónico de Will. Identifica el archivo que llevó al acceso inicial. (Formato: Archivoname.extension)

![Pasted image 20260610163202](../Fotos/Pasted%20image%2020260610163202.png)

![Pasted image 20260610164355](../Fotos/Pasted%20image%2020260610164355.png)

![Pasted image 20260610164848](../Fotos/Pasted%20image%2020260610164848.png)

![Pasted image 20260610164346](../Fotos/Pasted%20image%2020260610164346.png)

**Respuesta:** feedback.exe

P5) ¿Qué host fue comprometido primero? (Formato: Presentador)

![Pasted image 20260610164736](../Fotos/Pasted%20image%2020260610164736.png)

**Respuesta:** MACHINE-2

P6) Se produjo un ataque ASREPRoast. ¿Cuál era la cuenta objetivo? (Formato: nombre de usuario)


![Pasted image 20260610170403](../Fotos/Pasted%20image%2020260610170403.png)

![Pasted image 20260610170342](../Fotos/Pasted%20image%2020260610170342.png)

PowerShell se ejecutaba desde .`cmd.exe`

![Pasted image 20260610171154](../Fotos/Pasted%20image%2020260610171154.png)

![Pasted image 20260610172628](../Fotos/Pasted%20image%2020260610172628.png)

![Pasted image 20260610172549](../Fotos/Pasted%20image%2020260610172549.png)

**Respuesta:** pika.boo

P7) En el primer sistema comprometido, se ejecutaba una orden ofuscada. Proporciona la parte desofuscada del primer comando encontrado. (Si son varios, usar el primero, según el tiempo, con ofuscación) (Formato: String_ desofuscados

![Pasted image 20260610174326](../Fotos/Pasted%20image%2020260610174326.png)

![Pasted image 20260610174343](../Fotos/Pasted%20image%2020260610174343.png)

![Pasted image 20260610174539](../Fotos/Pasted%20image%2020260610174539.png)

**Respuesta:** findstr /S /R /N "^password" C:\Users\*.*

P8) El atacante, para acceder a otros sistemas internos con credenciales válidas, creó una instancia en la nube. Encuentra su dirección IP privada. (Formato: Dirección IP privada)


![Pasted image 20260610175330](../Fotos/Pasted%20image%2020260610175330.png)

![Pasted image 20260610175733](../Fotos/Pasted%20image%2020260610175733.png)

![Pasted image 20260610175632](../Fotos/Pasted%20image%2020260610175632.png)

**Respuesta:** 10.0.1.126 

P9) ¿En qué host inició sesión el atacante usando la cuenta recientemente comprometida? (Formato: Presentador)

![Pasted image 20260610180219](../Fotos/Pasted%20image%2020260610180219.png)

**Respuesta:** Machine-1

P10) El atacante volvió a ejecutar el comando ofuscado previamente encontrado y luego se trasladó a un sistema Linux. Busca el nombre de usuario, la IP y el protocolo usados para el acceso. (Formato: Protocolo, usuario, IP)

![Pasted image 20260610183005](../Fotos/Pasted%20image%2020260610183005.png)

![Pasted image 20260610183037](../Fotos/Pasted%20image%2020260610183037.png)

**Respuesta:** ssh, john, 10.0.1.133

P11) Se descargó un archivo desde el sistema conectado. ¿Cuál era el nombre del archivo? (Formato: Nombre de archivo.Extensión)

![Pasted image 20260610184040](../Fotos/Pasted%20image%2020260610184040.png)

**Respuesta:** l0lDump.txt

P12) Se ejecutó otra gran orden oscurecida. ¿Qué actividad se realizó? (Referencia de MITRE Tactic) (Formato: nombre de la actividad (según la táctica MITRE))

![Pasted image 20260610185406](../Fotos/Pasted%20image%2020260610185406.png)

![Pasted image 20260610185352](../Fotos/Pasted%20image%2020260610185352.png)

![Pasted image 20260610185334](../Fotos/Pasted%20image%2020260610185334.png)

**Respuesta:** Exfiltration

P13) ¿Qué servicio web se utilizó para esta actividad? Además, proporciona la clave API encontrada. (Formato: Nombre del servicio web, API)

![Pasted image 20260610190759](../Fotos/Pasted%20image%2020260610190759.png)

**Respuesta:** pastebin.com, vfkaoc1w08ELOSUoYf4npiz2KaY0Irsp

P14) ¿Cómo desapareció el entorno de la nube? (Identifica la API utilizada) (Formato: API)

![Pasted image 20260610192510](../Fotos/Pasted%20image%2020260610192510.png)

**Respuesta:** TerminatedInstances

P15) ¿Cuántas instancias desaparecieron? (Formato: Entero)

![Pasted image 20260610192350](../Fotos/Pasted%20image%2020260610192350.png)

**Respuesta:** 6

P16) Un usuario de la Máquina-1 reportó haber visto un mensaje en una caja azul antes de que desaparecieran las instancias. ¿Cuál era el mensaje? (El mensaje estará entre comillas) (Formato: Mensaje)

![Pasted image 20260610192950](../Fotos/Pasted%20image%2020260610192950.png)

![Pasted image 20260610192909](../Fotos/Pasted%20image%2020260610192909.png)

**Respuesta:** "Kaboom!! Happy Halloween"