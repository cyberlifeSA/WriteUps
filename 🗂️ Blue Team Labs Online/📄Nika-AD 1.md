- --
- Tags: #splunk #btlo #nika-ad_lab
- --

*P1) Mediante análisis de paquetes de red, determinar la dirección IP del atacante (Formato: X.X.X.X)*
![Pasted image 20260613110728](../Fotos/Pasted%20image%2020260613110728.png)
192.168.1.12

*P2) ¿Qué servicio apuntó el atacante para acceder al servidor web? (Formato: XXX)*
![Pasted image 20260613115833](../Fotos/Pasted%20image%2020260613115833.png)
![Pasted image 20260613115911](../Fotos/Pasted%20image%2020260613115911.png)
SMB

*P3) ¿Cuál es el sistema operativo y la versión del servidor web objetivo? (Formato: xxxxxxx X)*
![Pasted image 20260613125647](../Fotos/Pasted%20image%2020260613125647.png)
Windows 7

*P4) Basándonos en la información y el flujo de los paquetes, identificar el exploit popular que se está utilizando aquí (Formato: XxxxxxxXxxx)*
EternalBlue

![Pasted image 20260613130402](../Fotos/Pasted%20image%2020260613130402.png)

**P5) Sin verificar su identidad, el atacante recibió un hash del usuario del Controlador de Dominio. Usando Splunk, determina qué tipo de ataque se utilizó (Formato: XXXXXXxxxxxxx)**
![Pasted image 20260613131515](../Fotos/Pasted%20image%2020260613131515.png)
as-reproasting

*P6) ¿Qué usuario de dominio fue objetivo en el ataque? (Formato: DOMINIO\Usuario)*
![Pasted image 20260613133123](../Fotos/Pasted%20image%2020260613133123.png)
![Pasted image 20260613133202](../Fotos/Pasted%20image%2020260613133202.png)
![Pasted image 20260613133342](../Fotos/Pasted%20image%2020260613133342.png)
CYPYRATE\nrob

*P7) ¿En qué ordenador inició sesión el atacante tras comprometer al usuario? (Formato: XXXX-XXXXX)*
![Pasted image 20260613134338](../Fotos/Pasted%20image%2020260613134338.png)
![Pasted image 20260613134754](../Fotos/Pasted%20image%2020260613134754.png)
![Pasted image 20260613134707](../Fotos/Pasted%20image%2020260613134707.png)
File-Server

*P8) ¿Qué protocolo utilizó el atacante para acceder al ordenador? (Formato: Protocolo)*
![Pasted image 20260613145613](../Fotos/Pasted%20image%2020260613145613.png)
![Pasted image 20260613145544](../Fotos/Pasted%20image%2020260613145544.png)
rdp

*P9) Identificar cualquier intento de escalada de privilegios en el mismo ordenador. ¿Qué archivo o script se usó para encontrar posibles métodos de escalada? (Formato: file.ext)*
![Pasted image 20260613160706](../Fotos/Pasted%20image%2020260613160706.png)
Power.ps1

*P10) Determinar el servicio explotado para la escalada de privilegios en el mismo ordenador. ¿Cuál es el nombre del servicio que fue abusado? (Formato: NombreDeservicio)*
![Pasted image 20260613164202](../Fotos/Pasted%20image%2020260613164202.png)
AV_Troubleshoot

*P11) ¿Qué comando se utilizó para abusar del servicio? (Formato: Mando)*
![Pasted image 20260613164814](../Fotos/Pasted%20image%2020260613164814.png)
net localgroup Administrators CYPYRATE\nrob /add

*P12) ¿Cuál es el ID MITRE para la técnica utilizada para escalar privilegios a través del servicio explotado en el mismo ordenador? (Formato: TXXXX.xxx)*
![Pasted image 20260613170706](../Fotos/Pasted%20image%2020260613170706.png)
T1574.010

*P13) ¿Qué ejecutable malicioso utilizó el atacante para volcar las credenciales? (Formato: file.ext)*
![Pasted image 20260613170944](../Fotos/Pasted%20image%2020260613170944.png)
mimikatz.exe

*P14) ¿Desde qué directorio se ejecutó el ejecutable malicioso? (Formato: X:\path\to\directorio)*
C:\Users\nrob\Downloads\m\mimikatz-master\x64\

*P15) ¿A qué ordenador accedió el atacante usando un usuario recién comprometido con un método similar al observado anteriormente? (Formato: XX-XXXXX)*
![Pasted image 20260613172405](../Fotos/Pasted%20image%2020260613172405.png)
![Pasted image 20260613172806](../Fotos/Pasted%20image%2020260613172806.png)
IT-WKSTN

*P16) ¿Qué credenciales de usuario de dominio probablemente se usaron desde el volcado de credenciales para iniciar sesión en el nuevo sistema? (Formato: DOMINIO\usuario)*
CUPYRATE\ryen

*P17) ¿Qué ataque utilizó el atacante para mantener el acceso persistente a la red? (Formato: Xxxxxx Xxxxxx)*
Golden Ticket

*P18) Proporcionar el hash aes-256 utilizado para ejecutar el ataque (Formato: Hash)*
![Pasted image 20260613183537](../Fotos/Pasted%20image%2020260613183537.png)
![Pasted image 20260613183608](../Fotos/Pasted%20image%2020260613183608.png)
4c517465330767f6a9ab9bb02f6b07da6a6c7f401087b97fceb3fdbe1cb9ee2a

*P19) ¿Proporcionar el nombre del ticket (archivo) que se usó en el ataque previamente detectado? (Formato: file.ext)*
comp.kirbi

*P20) ¿Qué utilidad usó el atacante para ejecutar comandos en el Controlador de Dominio? (Formato: file.ext)*
![Pasted image 20260613184658](../Fotos/Pasted%20image%2020260613184658.png)
![Pasted image 20260613185205](../Fotos/Pasted%20image%2020260613185205.png)![Pasted image 20260613184749](../Fotos/Pasted%20image%2020260613184749.png)![Pasted image 20260613185129](../Fotos/Pasted%20image%2020260613185129.png)   
psexec.exe


