- --
- Tags: #splunk #btlo #rottencloud_lab
- --

Analiza los registros de AWS CloudTrail e identifica la dirección IP del atacante. Cabe señalar que los usuarios legítimos trabajaban de forma remota desde India
![Pasted image 20260617125642](../Fotos/Pasted%20image%2020260617125642.png)
Respuesta: 172.235.129.221

El atacante realizó reconocimientos en instancias EC2. ¿Qué llamada API o EventoName específico se generó durante esta actividad de reconocimiento?
![Pasted image 20260617125729](../Fotos/Pasted%20image%2020260617125729.png)
Respuesta: DescribeInstances 

Tras recopilar información sobre las instancias EC2, el atacante intentó encontrar las contraseñas de las instancias para establecer conexiones. Identificar el secretID que contenía la contraseña de la instancia de Windows
![Pasted image 20260617125836](../Fotos/Pasted%20image%2020260617125836.png)
Respuesta: zeta9/windows/admin-password 

El atacante descubrió y dirigió buckets S3 para descargar datos sensibles. Averigua cuántos buckets únicos de S3 fueron objetivos, así como el total de archivos descargados de todos los buckets.
![Pasted image 20260617130229](../Fotos/Pasted%20image%2020260617130229.png)
![Pasted image 20260617130210](../Fotos/Pasted%20image%2020260617130210.png)
![Pasted image 20260617130152](../Fotos/Pasted%20image%2020260617130152.png)
![Pasted image 20260617130132](../Fotos/Pasted%20image%2020260617130132.png)
![Pasted image 20260617130116](../Fotos/Pasted%20image%2020260617130116.png)
![Pasted image 20260617133526](../Fotos/Pasted%20image%2020260617133526.png)
Respuesta: 3, 5

A través del análisis del historial de navegación de la instancia EC2 comprometida, el atacante encontró rastros que conducían a un portal web secreto utilizado por personas restringidas. Utilizando correlación cruzada con otras fuentes de registro, identifica la URL de este portal secreto.
![Pasted image 20260617133915](../Fotos/Pasted%20image%2020260617133915.png)
zeta9-research-portal.azurewebsites.net 

El atacante cambió a otro entorno en la nube explotando una vulnerabilidad. Proporciona el comando completo utilizado para esta actividad entre
![Pasted image 20260617134408](../Fotos/Pasted%20image%2020260617134408.png)
![Pasted image 20260617134450](../Fotos/Pasted%20image%2020260617134450.png)
![Pasted image 20260617134420](../Fotos/Pasted%20image%2020260617134420.png)
cmd=curl -H secret:4ebc6d54-f421-4321-81c4-fd9e29d28a0f 'http://169.254.130.3:8081/msi/token?api-version=2017-09-01&resource=https://management.azure.com/'

Tras acceder al entorno Azure, el atacante pudo listar y acceder a datos desde servicios de almacenamiento en la nube. Identifica el nombre del contenedor de blob de almacenamiento específico al que se ha dirigido.
![Pasted image 20260617140924](../Fotos/Pasted%20image%2020260617140924.png)
quantum-research-secrets

Determina cuántos archivos descargó con éxito el atacante del almacenamiento de blob de Azure durante el ataque.
![Pasted image 20260617142851](../Fotos/Pasted%20image%2020260617142851.png)
6

Para acceder a los sistemas de la división secreta, el atacante vandalizó el sitio web de la organización desplegando contenido malicioso. Proporciona la URL que alojaba el código web desfigurado.
![Pasted image 20260617143524](../Fotos/Pasted%20image%2020260617143524.png)
![Pasted image 20260617143544](../Fotos/Pasted%20image%2020260617143544.png)
https://pastebin.com/raw/sBEs83q3