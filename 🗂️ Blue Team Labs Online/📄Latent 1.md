- --
- Tags: #wireshark #volatility #btlo #latent_lab
- --
 ¿Cuál es la IP de origen del atacante? (Formato: dirección IP)
 ![Pasted image 20260627142729](../Fotos/Pasted%20image%2020260627142729.png)
10.0.2.4

¿Qué acción realizó el atacante para descubrir diferentes servicios que se ejecutan en la víctima? (Formato: Acción)
Opening the PCAP in Wireshark reveals four IP addresses across two subnets:
- `10.0.2.4` — attacker (internal)
- `10.0.2.15` — victim
- `23.58.93.34` — external
- `216.58.200.163` — external
Port Scan

¿Qué servicio se está explotando en la fase inicial? (Formato: Protocolo)
![Pasted image 20260627143432](../Fotos/Pasted%20image%2020260627143432.png)
smb

¿A qué parte está accediendo el atacante? (Formato: Compartir Red)
![Pasted image 20260627143627](../Fotos/Pasted%20image%2020260627143627.png)
/documents

¿Cuál es el nombre de la carga maliciosa que se está subiendo al share? (Formato: Nombre de la carga)
![Pasted image 20260627144642](../Fotos/Pasted%20image%2020260627144642.png)
shell.aspx

Al inspeccionar el archivo **shell.aspx**, se observa un cargador ASP.NET que contiene un extenso bloque de datos codificado en Base64. Esto sugiere que incorpora un binario o una DLL compilada, la cual probablemente es cargada y ejecutada directamente en memoria a través de la web shell.
![Pasted image 20260627160431](../Fotos/Pasted%20image%2020260627160431.png)

¿A qué hora se solicitó que el archivo de carga útil recibiera una shell inversa? (Formato: XXX DD, YYYY HH:MM:SS EDT)
![Pasted image 20260627160928](../Fotos/Pasted%20image%2020260627160928.png)
Sep 10, 2024 01:49:39 EDT

¿En qué puerto está escuchando el atacante la conexión de shell inversa? (Formato: XXXX)
![Pasted image 20260627163400](../Fotos/Pasted%20image%2020260627163400.png)
4443

¿Cuál es el nombre del archivo ejecutable sospechoso que se ejecuta en el sistema? (Formato: Nombre del programa)
![Pasted image 20260627162828](../Fotos/Pasted%20image%2020260627162828.png)
![Pasted image 20260627162806](../Fotos/Pasted%20image%2020260627162806.png)
updatenow.exe

¿Cuál es el PID del proceso anterior? (Formato: XXX)
![Pasted image 20260627162019](../Fotos/Pasted%20image%2020260627162019.png)
![Pasted image 20260627162056](../Fotos/Pasted%20image%2020260627162056.png)
900

¿Cuál es la dirección virtual del objeto archivo en la memoria? (Formato: 0xXXXXXXXXXXXX)
![Pasted image 20260627164109](../Fotos/Pasted%20image%2020260627164109.png)
0xce0657ddb1c0

El atacante ha movido el archivo ejecutable a una ubicación específica tras subirlo a la máquina de la víctima. ¿Cuál es la ubicación del archivo? (Formato: Camino completo)
![Pasted image 20260627162828](../Fotos/Pasted%20image%2020260627162828.png)
![Pasted image 20260627171819](../Fotos/Pasted%20image%2020260627171819.png)
`"C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup\updatenow.exe`

¿Qué aplicación gestionaba la conexión de shell inversa de la víctima? (Formato: Nombre de la solicitud)
![Pasted image 20260627162828](../Fotos/Pasted%20image%2020260627162828.png)
![Pasted image 20260627162806](../Fotos/Pasted%20image%2020260627162806.png)
w3wp.exe

¿Cuál es el valor hash MD5 del archivo ejecutable malicioso? (Formato: MD5)
![Pasted image 20260627173045](../Fotos/Pasted%20image%2020260627173045.png)
D797600296DDBED4497725579D814B7E

¿Qué empaquetador se utiliza para ocultar el código del malware?
![Pasted image 20260627173740](../Fotos/Pasted%20image%2020260627173740.png)
UPX

¿Cuál es el valor de entropía de la aplicación? (Formato: X.XXX)
![Pasted image 20260627173713](../Fotos/Pasted%20image%2020260627173713.png)
7932

De todas las bibliotecas que utiliza la aplicación, ¿cuál se utiliza para gestionar la funcionalidad de los sockets de Windows? (Formato: XXXXXXX.dll)
![Pasted image 20260627173840](../Fotos/Pasted%20image%2020260627173840.png)
WSOCK32.dll

¿A qué familia pertenece este malware? (Formato: Apellido)
![Pasted image 20260627174446](../Fotos/Pasted%20image%2020260627174446.png)
AgentTesla
