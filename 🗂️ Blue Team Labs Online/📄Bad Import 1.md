- --
- Tags: #splunk #btlo #bad_import_lab
- --

**Escenario**
Meridian Labs es una empresa de ingeniería con 240 personas y una aplicación orientada al cliente. El viernes por la mañana, tres de sus máquinas con Windows empezaron a hablar con un servidor cualquiera en internet. Ingeniería dice que no han cambiado nada esta semana.

*¿Qué estación de trabajo contacta primero con el servidor de comando y control del atacante? Proporciona la estación de trabajo, la IP de destino y el puerto*
![Pasted image 20260530163531](../Fotos/Pasted%20image%2020260530163531.png)
DEV-WKS-01, 142.11.206.73, 8000

*¿Desde qué directorio de trabajo se ejecutó la primera carga útil del atacante en la estación de trabajo?*
![Pasted image 20260530172740](../Fotos/Pasted%20image%2020260530172740.png)
![Pasted image 20260530172645](../Fotos/Pasted%20image%2020260530172645.png)
C:\proyects\analytics-portal\node_modules\plain-crypto-js

*El atacante colocó un binario de Windows renombrado en una ubicación no estándar. ¿Cuál es el archivo real? Según la descripción*
![Pasted image 20260531162758](../Fotos/Pasted%20image%2020260531162758.png)
Windows PowerShell

*¿Cuál es la ruta completa del archivo de script que el atacante dejó caer en la estación de trabajo como primera etapa de ejecución?*
![Pasted image 20260530211812](../Fotos/Pasted%20image%2020260530211812.png)
C:\Users\jcarter\AppData\Local\Temp\6202033.ps1

*¿Se estableció un mecanismo de persistencia por parte del Atacante? Proporciona la ruta del registro escrita y el contenido de valor.*
![Pasted image 20260530221823](../Fotos/Pasted%20image%2020260530221823.png)HKU\S-1-5-21-1004336348-1177238915-682003330-1001\Software\Microsoft\Windows\CurrentVersion\Run\WindowsTerminalUpdate, C:\ProgramData\.run.bat

*El atacante recogió varios archivos que contenían credenciales de usuario y secretos de la estación de trabajo. Proporciona el nombre del archivo (en orden alfabético) (el primero es el archivo Dot).*
Evidencia .npmrc
![Pasted image 20260531175119](../Fotos/Pasted%20image%2020260531175119.png)
Evidencia ConsoleHost_history.txt
![Pasted image 20260531175446](../Fotos/Pasted%20image%2020260531175446.png)
Evidencia id_rsa
![Pasted image 20260531175750](../Fotos/Pasted%20image%2020260531175750.png)
Evidencia credentials
![Pasted image 20260531175820](../Fotos/Pasted%20image%2020260531175820.png)
Evidencia Login Data
![Pasted image 20260531175942](../Fotos/Pasted%20image%2020260531175942.png)
![Pasted image 20260531181133](../Fotos/Pasted%20image%2020260531181133.png)
.npmrc, ConsoleHost_history.txt, credentials, id_rsa, Login Data

*El atacante se movió lateralmente a la siguiente máquina. ¿Qué es la máquina y qué cuenta se utilizó?*
![Pasted image 20260530225953](../Fotos/Pasted%20image%2020260530225953.png)
BUILD-01, svc_jenkinsdeploy

*¿En qué fecha y hora el atacante inició sesión en la siguiente máquina?*
![Pasted image 20260530230201](../Fotos/Pasted%20image%2020260530230201.png)
2026-04-02, 19:35:48

*En el host recientemente comprometido, el atacante plantó un archivo y modificó un script de compilación existente en el espacio de trabajo de Jenkins. Para disminuir la sospecha de que falsificaron una marca de tiempo de creación en uno de ellos, ¿qué valor de marca de tiempo falso se aplicó?*
![Pasted image 20260531124751](../Fotos/Pasted%20image%2020260531124751.png)
2026-01-14 11:00:00

*Las modificaciones del atacante al servidor comprometido permitieron que su código plantado llegara a un tercer host como parte de un despliegue programado. Identifica el nombre de host de este siguiente host y el protocolo (puerto TCP) utilizado para transferir archivos*
![Pasted image 20260531131712](../Fotos/Pasted%20image%2020260531131712.png)
SRV-APP-01, 445

*Apareció un nuevo archivo en la carpeta de plugins de la aplicación del portal del cliente. ¿Proporcionar su camino completo?*
![Pasted image 20260531133639](../Fotos/Pasted%20image%2020260531133639.png)
C:\inetpub\customer-portal\plugins\telemetry.js

*Un proceso que se ejecutaba en el host recientemente comprometido hizo la primera conexión saliente a la infraestructura del atacante poco después de que aterrizara el despliegue. ¿Proporcionar la ruta completa de imagen del proceso y la cuenta de usuario con la que se ejecutó?*
![Pasted image 20260531140856](../Fotos/Pasted%20image%2020260531140856.png)
C:\Program Files\nodejs\node.exe, svc_portal

*El atacante escaló privilegios en el servidor de producción. Proporciona el camino completo del binario usado para la escalada y la tubería nombrada asociada al abuso.*
![Pasted image 20260531144900](../Fotos/Pasted%20image%2020260531144900.png)
![Pasted image 20260531152614](../Fotos/Pasted%20image%2020260531152614.png)
C:\ProgramData\svc_helper.exe, spoolss

*El atacante lograba persistencia en el servidor de producción. Proporciona el nombre del usado/registrado para eso.*
![Pasted image 20260531153641](../Fotos/Pasted%20image%2020260531153641.png)
WinTelemetrySvc


