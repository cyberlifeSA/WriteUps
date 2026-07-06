- --
- Tags: #splunk #btlo #middle_mayhem_lab
- --

*Accede al sitio web en el navegador, preséntalo en el marcador e identifica el framework y la versión de JavaScript utilizada.*
![Pasted image 20260601152125](../Fotos/Pasted%20image%2020260601152125.png)
Next.js, 15.0.0

*Usando Splunk, encuentra la dirección IP del atacante*
![Pasted image 20260601154800](../Fotos/Pasted%20image%2020260601154800.png)218.92.0.204

*Analiza los registros SIEM para determinar cuántos URI únicos fue accedido por el atacante*
![Pasted image 20260601155454](../Fotos/Pasted%20image%2020260601155454.png)
9930

*Explora el lugar e identifica dos ubicaciones específicas que puedan revelar estructuras internas o posibles accesos que no están destinados al público. Proporciona las dos URLs relativas.*
![Pasted image 20260601160802](../Fotos/Pasted%20image%2020260601160802.png)
/admin, /admin/file-upload

*Basándonos en el Framework y la Versión, ¿qué CVE reciente podría usarse para eludir la autorización?*
CVE-2025-29927 (Next.js) - Authorization Bypass

*Encuentra el encabezado HTTP relevante en los registros SIEM que indique la explotación de CVE. Proporciona el nombre de la cabecera.*
CVE quick Investigation
![Pasted image 20260601161752](../Fotos/Pasted%20image%2020260601161752.png)
![Pasted image 20260601162617](../Fotos/Pasted%20image%2020260601162617.png)
`x-middleware-subrequest`

*¿Qué URI interesante accedió el atacante tras explotar el CVE?*
/api/upload

*El atacante intentó subir un shell inverso. Averigua la IP y el puerto al que se conectaría el destino una vez establecida la conexión.*
![Pasted image 20260601164408](../Fotos/Pasted%20image%2020260601164408.png)
113.89.232.157, 31337

*Tras comprometer el servidor WebApp, el atacante intentó moverse lateralmente. Identifica la técnica utilizada, tal como se registra en los registros SIEM.*
![Pasted image 20260601170613](../Fotos/Pasted%20image%2020260601170613.png)
![Pasted image 20260601170754](../Fotos/Pasted%20image%2020260601170754.png)
SSH Brute Force

*Identify the user account that achieved successful lateral movement to another server.*
first form
![Pasted image 20260601170613](../Fotos/Pasted%20image%2020260601170613.png)

second form
![Pasted image 20260601170911](../Fotos/Pasted%20image%2020260601170911.png)
![Pasted image 20260601171113](../Fotos/Pasted%20image%2020260601171113.png)
dbserv
