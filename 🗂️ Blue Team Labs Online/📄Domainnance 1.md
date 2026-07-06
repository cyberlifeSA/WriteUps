- --
- Tags: #splunk #btlo #domainnance_lab
- --

![Pasted image 20260607114500](../Fotos/Pasted%20image%2020260607114500.png)

*P1) El atacante intentó iniciar sesión en el servidor web vulnerable. ¿Puedes encontrar las credenciales correctas que iniciaron sesión con el atacante? (Formato: usuario:contraseña)*
![Pasted image 20260607115804](../Fotos/Pasted%20image%2020260607115804.png)
admin:password

*P2) ¿Cuál es la IP de la máquina atacante al interactuar con el servidor web? (Formato: X.X.X.X)*
192.168.1.13

![Pasted image 20260607120202](../Fotos/Pasted%20image%2020260607120202.png)
*P3) El atacante podía ejecutar comandos en el WebServer. ¿Cuántas órdenes se ejecutaron? (Formato: Conteo)*
6

*P4) El atacante encontró un archivo útil en el WebServer. ¿Cuál es el nombre del archivo mencionado anteriormente? (Formato: nombre de archivo.extensión)*
![Pasted image 20260607120534](../Fotos/Pasted%20image%2020260607120534.png)
.credentials.txt

*P5) ¿Qué datos importantes hay presentes dentro del archivo? (Formato: xxxx xxxxx : xxxxxxxx)*
![Pasted image 20260607123018](../Fotos/Pasted%20image%2020260607123018.png)
Mike Tyson : Pa55w0rd

*P6) ¿Cuál es la subred interna de red que el atacante encontró y escaneó? (Formato: X.X.X.X/XX)*
![Pasted image 20260607125544](../Fotos/Pasted%20image%2020260607125544.png)
10.0.2.0/24

*P7) Con la ayuda de Splunk, ¿cuál es el nombre de dominio del entorno AD? (Formato: domain.tld)*
![Pasted image 20260607130436](../Fotos/Pasted%20image%2020260607130436.png)
![Pasted image 20260607130520](../Fotos/Pasted%20image%2020260607130520.png)
highlysecured.tech

*P8) Una cuenta de usuario, "mtyson", descargó algunos archivos en uno de los sistemas. ¿Cómo se llama el sistema? (Formato: NombreComputadora)*
![Pasted image 20260607131720](../Fotos/Pasted%20image%2020260607131720.png)
![Pasted image 20260607132437](../Fotos/Pasted%20image%2020260607132437.png)
DESKTOP-2FII9FV

*P9) ¿Cuál fue el primer archivo que se descargó en el sistema? Proporcionar el Nombre Original y el Nombre Final con extensión (Formato: NombreReal, NombreDeDado)*
![Pasted image 20260607142405](../Fotos/Pasted%20image%2020260607142405.png)
Invoke-Mimikatz.ps1, Troubleshooter.ps1

*P10) ¿Qué comando One-Liner usó el Atacante para volcar credenciales? (Formato: Mando)*
![Pasted image 20260607144723](../Fotos/Pasted%20image%2020260607144723.png)
powershell  -Command '. .\Troubleshooter.ps1 ; Invoke-Mimikatz -Command "privilege::debug" ; Invoke-Mimikatz -Command "sekurlsa::logonpasswords"'

*P11) El atacante también descargó un CompiledBinary y lo almacenó bajo un nombre legítimo. ¿Con qué nombre se guardó el archivo? (Formato: Filename.ext)*
![Pasted image 20260607145216](../Fotos/Pasted%20image%2020260607145216.png)
svch0st.exe

*P12) El atacante afirma haber realizado con éxito un movimiento lateral. ¿Qué ticket se utilizó para ejecutar el ataque de pasar el ticket? (Formato: Nombre del billete)*
![Pasted image 20260607153013](../Fotos/Pasted%20image%2020260607153013.png)
![Pasted image 20260607152954](../Fotos/Pasted%20image%2020260607152954.png)
0-60a10000-mtyson@krbtgt~HIGHLYSECURED.TECH-HIGHLYSECURED.TECH.kirbi

*Q13) It is believed that the attacker has achieved domain-wide compromise. What technique was used? (Format: Xxxxxx Xxxxxx)*
![Pasted image 20260607153205](../Fotos/Pasted%20image%2020260607153205.png)Golden Ticket

*P14) ¿Qué comando se utilizó para realizar el ataque? (Formato: Mando)*
mimikatz.exe  "kerberos::golden /user:mtyson /domain:highlysecured.tech /sid:S-1-5-21-2778836013-2025790062-2140220986-1108 /krbtgt:31d6cfe0d16ae931b73c59d7e0c089c0 /id:500 /ptt"
