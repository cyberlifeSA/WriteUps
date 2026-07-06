- --
- Tags: #splunk #btlo #stormweavers_vision_lab
- --

*Identifica la dirección IP pública desde la que el atacante lanzó su ataque contra la aplicación web.*
(aqeui el campo **CsUriQuery** es muy util aunque no lo usamos porque muestra ejecuciones de comandos en uri)
![Pasted image 20260619130202](../Fotos/Pasted%20image%2020260619130202.png)
Respuesta: 13.250.112.17

*¿Cuál es el nombre del Azure App Service que fue inicialmente comprometido por el atacante?*
![Pasted image 20260619135608](../Fotos/Pasted%20image%2020260619135608.png)
![Pasted image 20260619135211](../Fotos/Pasted%20image%2020260619135211.png)
Respuesta: snowpine-festival

*El atacante explotó una vulnerabilidad en la aplicación web para ejecutar comandos del sistema. ¿Cuál es el PRIMER comando que ejecutó el atacante para confirmar que tenía capacidades de ejecución de comandos?*
![Pasted image 20260619140130](../Fotos/Pasted%20image%2020260619140130.png)
Respuesta: ls

*¿Qué ID de la técnica MITRE ATT&CK describe mejor el método de acceso inicial utilizado por el atacante en la aplicación web?*
![Pasted image 20260619140235](../Fotos/Pasted%20image%2020260619140235.png)
Respuesta: T1190

*El atacante confirmó que la aplicación web estaba funcionando en Azure comprobando las variables de entorno. ¿Cuál es el nombre de la variable de entorno Azure que indica que la Identidad Gestionada está disponible?*
![Pasted image 20260619140856](../Fotos/Pasted%20image%2020260619140856.png)
Respuesta: IDENTITY_ENDPOINT

*El atacante extrajo DOS tokens de acceso diferentes del Azure Instance Metadata Service (IMDS) para acceder a distintos recursos de Azure. ¿Cuáles son las DOS diferentes audiencias de recursos (alcance) para estos tokens?*
https://vault.azure.net, https://management.azure.com

*El atacante volcó las credenciales de una cuenta de usuario iniciada en la VM. ¿Cuándo se accedió/extrajo por última vez las credenciales de acceso?*
![Pasted image 20260619142341](../Fotos/Pasted%20image%2020260619142341.png)
2025-11-18 02:42:04

*Se utilizaba un comando para establecer persistencia, haciendo que la máquina se conectara de nuevo al atacante. Proporciona el número de puerto en el que se establecería esta conexión.*
![Pasted image 20260619144921](../Fotos/Pasted%20image%2020260619144921.png)
![Pasted image 20260619144935](../Fotos/Pasted%20image%2020260619144935.png)
4443

*¿Cuándo inició sesión el atacante en el Portal de Azure usando la reutilización de credenciales?*
![Pasted image 20260619150702](../Fotos/Pasted%20image%2020260619150702.png)
![Pasted image 20260619150626](../Fotos/Pasted%20image%2020260619150626.png)
2025-11-18 08:24:53

*¿Qué quien credenciales de cuenta de usuario se extrajeron de la máquina virtual de Windows, que se usó para acceder a Azure?*
![Pasted image 20260619151939](../Fotos/Pasted%20image%2020260619151939.png)
bryn@corphusky.onmicrosoft.com

*Tras comprometer las credenciales del usuario, el atacante se autenticaba en Azure usando la CLI y realizaba la enumeración. ¿Qué recurso (servicio) de Azure accedió con éxito el atacante?*
![Pasted image 20260619153618](../Fotos/Pasted%20image%2020260619153618.png)
![Pasted image 20260619153604](../Fotos/Pasted%20image%2020260619153604.png)
azure key vault

*¿Qué secreto sensible se encontró en Azure que permitió el acceso entre nubes?*
![Pasted image 20260619155826](../Fotos/Pasted%20image%2020260619155826.png)
MarshKeysBckup

*El atacante configuró una forma de leer el correo del usuario desde su cuenta de correo. Identifica la dirección de correo electrónico que utilizó el atacante para recibir los correos originales.*
![Pasted image 20260619202609](../Fotos/Pasted%20image%2020260619202609.png)
![Pasted image 20260619202519](../Fotos/Pasted%20image%2020260619202519.png)
e110t.ecorp@proton.me

*Tras acceder a AWS, el atacante aumentó sus privilegios. ¿Qué acción específica de la API IAM se utilizó para esto?*
![Pasted image 20260619203710](../Fotos/Pasted%20image%2020260619203710.png)
![Pasted image 20260619203432](../Fotos/Pasted%20image%2020260619203432.png)
CreateAccessKey

*¿A qué dos cuentas de usuario cambió el atacante como parte de la escalada de privilegios?*
![Pasted image 20260619204122](../Fotos/Pasted%20image%2020260619204122.png)
![Pasted image 20260619204024](../Fotos/Pasted%20image%2020260619204024.png)
![Pasted image 20260619204112](../Fotos/Pasted%20image%2020260619204112.png)
![Pasted image 20260619204400](../Fotos/Pasted%20image%2020260619204400.png)
EiraSnow, bryn

*Usando la primera cuenta escalada, el atacante accedió al almacenamiento en AWS. ¿Cuántos archivos descargó el atacante con éxito (a través de la línea de crédito)?*
![Pasted image 20260619204725](../Fotos/Pasted%20image%2020260619204725.png)
![Pasted image 20260619204700](../Fotos/Pasted%20image%2020260619204700.png)
4

*El atacante restableció la credencial principal de un servicio AWS. ¿Cuál es el nombre del servicio y cuándo ocurrió esto (en UTC)?*
![Pasted image 20260619210224](../Fotos/Pasted%20image%2020260619210224.png)
RDP, 2025-11-18 09:47:39