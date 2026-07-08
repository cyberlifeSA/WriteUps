- --
- Tags: #splunk #btlo #splunk_it_lab
- --

P1) ¿Alguno de los empleados te informó sobre un correo de phishing reciente que recibieron llamado "Factura" durante la investigación? ¿Puedes localizar la dirección IP desde la que se descargó el archivo? (Formato: X.X.X.X:Port)

![Pasted image 20260601132900](../Fotos/Pasted%20image%2020260601132900.png)

**Respuesta:** 139.59.21.147:8080

P2) ¿Cuál es el archivo que se descargó después de abrir el documento malicioso? Por favor, proporciona la ruta completa donde se descargó y guardó el archivo (Formato: C:\path\to\file.ext)

![Pasted image 20260601133346](../Fotos/Pasted%20image%2020260601133346.png)

**Respuesta:** C:\Windows\Temp\svchost.exe

P3) ¿Cuál es la URL desde la que se estaban descargando archivos adicionales? (Formato: http://algo:algo/archivo.ext)

**Respuesta:** `http://24.199.117.142:1337/svchost.exe`

P4) ¿Qué usuario de dominio parecía estar comprometido? (Formato: Nombre de usuario)

**Respuesta:** ricksanchez

P5) ¿Podría comprobar si se detectaron acciones persistentes? Por favor, nombra el programa utilizado (Formato: filename.ext)

![Pasted image 20260601134334](../Fotos/Pasted%20image%2020260601134334.png)

**Respuesta:** schtasks.exe

P6) ¿Cuál es el nombre de la tarea empleada para mantener la persistencia? (Formato: Nombre de la tarea)

**Respuesta:** Microsoft Teams Updater

P7) ¿Qué famoso guion, comúnmente usado por atacantes, se eliminó como archivo adicional para facilitar el reconocimiento interno y la enumeración? (Formato: nombre de archivo.ext)

![Pasted image 20260601135848](../Fotos/Pasted%20image%2020260601135848.png)

![Pasted image 20260601135808](../Fotos/Pasted%20image%2020260601135808.png)

**Respuesta:** PowerView.ps1

P8) ¿Qué archivo adicional desplegó el atacante para extraer las credenciales? (Formato: nombre de archivo.ext

**Respuesta:** Invoke-Mimikatz.ps1

P9) ¿Qué técnica de volcado de credenciales, similar a un método conocido que se usa a menudo en entornos de controladores de dominio, empleó el atacante? (Formato: xxxxxx)

**Respuesta:** DCSync
