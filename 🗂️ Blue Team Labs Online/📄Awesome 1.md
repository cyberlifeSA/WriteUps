- --
- Tags: #splunk #btlo #awesome_lab
- --

En SecureNetDynamics, un desarrollador (dev_harry) dejó sin saberlo sus claves de acceso AWS en el código fuente de la web de la empresa. Un atacante detectó rápidamente este error y utilizó las claves para vulnerar la infraestructura de AWS de SecureNet. El atacante realizó una enumeración detallada del entorno, extrajo claves sensibles de S3, implantó mecanismos de persistencia y, finalmente, lanzó un ataque de ransomware, cifrando archivos vitales. Ahora, SecureNetDynamics está en modo crisis y debe responder.

![Pasted image 20260616135902](../Fotos/Pasted%20image%2020260616135902.png)

*1) ¿Cuándo se vio la primera actividad exitosa del usuario AWS comprometido según los registros? (Formato: YYYY-MM-DD HH:MM:SS UTC)*
![Pasted image 20260616144132](../Fotos/Pasted%20image%2020260616144132.png)
![Pasted image 20260616145041](../Fotos/Pasted%20image%2020260616145041.png)
Respuesta: 2024-08-20 06:00:10 UTC

*2) ¿Cuál era la dirección IP de origen asociada a las actividades? (Formato: dirección IP)*
![Pasted image 20260616145429](../Fotos/Pasted%20image%2020260616145429.png)
Respuesta: 3.144.171.70

*3) El atacante realizó una enumeración adicional y encontró una imagen AMI a la que el usuario comprometido tenía acceso. Esta AMI se utilizó para crear instancias EC2. Encuentra el AMI-ID. (Formato: AMI-ID)*
![Pasted image 20260616152740](../Fotos/Pasted%20image%2020260616152740.png)
![Pasted image 20260616152722](../Fotos/Pasted%20image%2020260616152722.png)
Respuesta: ami-08be8aa61081045c0 

*4) El atacante cambió la regla de entrada de un grupo de seguridad, permitiendo que todos se conecten a través de cualquier puerto. Encuentra este ID de Grupo de Seguridad. (Formato: ID de Grupo de Seguridad)*
![Pasted image 20260616154550](../Fotos/Pasted%20image%2020260616154550.png)
![Pasted image 20260616154540](../Fotos/Pasted%20image%2020260616154540.png)
Respuesta: sg-085becd1c5d4f7621 

*5) El atacante, tras una enumeración adicional, encontró las credenciales de inicio de sesión de la consola de un usuario. ¿Cuál es el nombre del usuario? (Formato: Nombre de usuario)*
![Pasted image 20260616154952](../Fotos/Pasted%20image%2020260616154952.png)
![Pasted image 20260616155104](../Fotos/Pasted%20image%2020260616155104.png)
Respuesta: dev-neo

*6) ¿De qué IP se originó este inicio de sesión? (Formato: dirección IP)*
![Pasted image 20260616155657](../Fotos/Pasted%20image%2020260616155657.png)
Respuesta: 103.99.35.120

*7) ¿Qué política se aplicó para escalar el acceso a este usuario comprometido? (Formato: Política ARN)*
![Pasted image 20260616162030](../Fotos/Pasted%20image%2020260616162030.png)
![Pasted image 20260616160024](../Fotos/Pasted%20image%2020260616160024.png)
 Respuesta: arn:aws:iam::aws:policy/AdministratorAccess 

*8) El atacante descargó un archivo crítico de S3 Bucket mediante el cual se puede acceder a una instancia importante. ¿Cuál es el nombre del Archivo? (Formato: Archivoname.extension)*
![Pasted image 20260616162011](../Fotos/Pasted%20image%2020260616162011.png)
![Pasted image 20260616160508](../Fotos/Pasted%20image%2020260616160508.png)
Respuesta: devlopement-keys.pem 

*9) Para un acceso más sigiloso en el entorno, los atacantes crearon un usuario y un grupo con acceso administrativo. Busca el nombre de usuario y el nombre del grupo. (Formato: Nombre de usuario, Nombre del grupo)*
![Pasted image 20260616161952](../Fotos/Pasted%20image%2020260616161952.png)
![Pasted image 20260616161025](../Fotos/Pasted%20image%2020260616161025.png)
![Pasted image 20260616161835](../Fotos/Pasted%20image%2020260616161835.png)
![Pasted image 20260616161815](../Fotos/Pasted%20image%2020260616161815.png)
Respuesta:  dev_harry_bg, dev_group

*10) El atacante accedió a una instancia y se descargó un script de ransomware en ella. Usando el Instance.zip dado como artefacto, identifica la IP y el puerto desde donde se ha descargado el archivo. (Formato: IP:Port/File.extension)*
![Pasted image 20260616164603](../Fotos/Pasted%20image%2020260616164603.png)
Respuesta: 3.144.171.70:8080/a.py

*11) El ransomware solo se dirigió a archivos de extensión famosos. Encuentra la extensión que se añadió a ellos después del cifrado. (Formato: Extensión de cifrado)*
`grep -r -E "\.(doc|pdf|jpg|png|xls|ppt|txt)" .`

|Parte|Significado|Función|
|---|---|---|
|`grep`|**Global Regular Expression Print**|Busca patrones de texto en archivos|
|`-r`|**recursive**|Busca en **todos los subdirectorios** recursivamente|
|`-E`|**Extended Regular Expression**|Usa **expresiones regulares extendidas** (ERE) sin escaping extra|
|`"\.(doc\|pdf\|jpg\|png\|xls\|ppt\|txt)"`|**Patrón de búsqueda**|Busca textos que terminen en estas extensiones|
|`.`|**Directorio actual**|Busca desde el **directorio donde estás**|

![Pasted image 20260616165517](../Fotos/Pasted%20image%2020260616165517.png)
Respuesta: .vdkx