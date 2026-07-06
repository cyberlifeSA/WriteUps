- --
- Tags: #wireshark #volatility #btlo  #sam_lab
- --
P1) ¿Cuál es la IP del atacante y cuál es el puerto en el que tienen una shell inversa? (Formato: IP, puerto)
![Pasted image 20260626142838](../Fotos/Pasted%20image%2020260626142838.png)
![Pasted image 20260626142824](../Fotos/Pasted%20image%2020260626142824.png)
![Pasted image 20260626143108](../Fotos/Pasted%20image%2020260626143108.png)
![Pasted image 20260626143705](../Fotos/Pasted%20image%2020260626143705.png)
172.16.0.5, 80

P2) ¿Cuál es el nombre del archivo malicioso que dio acceso remoto al atacante? (Formato: nombre de archivo.extensión)
![Pasted image 20260626143405](../Fotos/Pasted%20image%2020260626143405.png)
sample_template.hta

P3) ¿Cuál es el proceso que ha llamado la carga útil al ejecutarse? (Formato: nombre del proceso.extensión)
![Pasted image 20260626144309](../Fotos/Pasted%20image%2020260626144309.png)
![Pasted image 20260626144716](../Fotos/Pasted%20image%2020260626144716.png)
powershell.exe

P4) Sabiendo el nombre de la carga útil y el nombre del proceso, si la carga útil fue generada por msfvenom, ¿cuál sería la opción de formato que habría usado el atacante? (Formato: tipo de carga útil msfvenom)
![Pasted image 20260626150757](../Fotos/Pasted%20image%2020260626150757.png)
htb-psh

P5) ¿Qué propiedad en el script ejecutado indica que el proceso se ejecuta sin usar el shell del sistema operativo? (Formato: Nombre de la propiedad)
![Pasted image 20260626145809](../Fotos/Pasted%20image%2020260626145809.png)
![Pasted image 20260626145944](../Fotos/Pasted%20image%2020260626145944.png)
UseShellExecute
 
P6) ¿Cuál es el flujo de compresión que se ha utilizado en la carga útil? (Formato: Nombre de la emisión)
![Pasted image 20260626151455](../Fotos/Pasted%20image%2020260626151455.png)
GzipStream

P7) A partir del archivo SAM y SYSTEM que ha sido exfiltrado, ¿cuántos hashes de usuario habrían sido identificados? (Formato: número de hashes de usuario obtenidos)
![Pasted image 20260626151707](../Fotos/Pasted%20image%2020260626151707.png)
6

P8) ¿Cuál es la contraseña del SAM? (Formato: Cadena de contraseña)
![Pasted image 20260626153627](../Fotos/Pasted%20image%2020260626153627.png)
StandardUser

P9) ¿Cuál es la contraseña del Administrador? (Formato: Cadena de contraseña)
![Pasted image 20260626153659](../Fotos/Pasted%20image%2020260626153659.png)
Passw0rd!

P10) ¿Cuál es el puerto que usó el atacante para iniciar sesión en el sistema tras descifrar las contraseñas? (Formato: Número de puerto)
![Pasted image 20260626155043](../Fotos/Pasted%20image%2020260626155043.png)
22

P11) ¿Hay otros scripts que el atacante haya ejecutado en el sistema? Encuentra el nombre del script (Formato: scriptname.extension)
![Pasted image 20260626144309](../Fotos/Pasted%20image%2020260626144309.png)
jaws-enum.ps1