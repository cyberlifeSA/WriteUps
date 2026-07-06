- --
- Tags: #wireshark #cyberdefenders #jetbrains_lab
- --
Identificar la dirección IP del atacante ayuda a rastrear la fuente y a detener nuevos ataques. ¿Cuál es la dirección IP del atacante?
![Pasted image 20260630154033](../../Fotos/Pasted%20image%2020260630154033.png)
23.158.56.196

Para identificar posibles explotaciones de vulnerabilidades, ¿qué versión de nuestro servicio de servidor web está ejecutando?
![Pasted image 20260630155248](../../Fotos/Pasted%20image%2020260630155248.png)

![Pasted image 20260630155208](../../Fotos/Pasted%20image%2020260630155208.png)
2023.11.3

Después de identificar la versión de nuestro servicio de servidor web, ¿qué número CVE corresponde a la vulnerabilidad que explotó el atacante?
![Pasted image 20260630155551](../../Fotos/Pasted%20image%2020260630155551.png)
En particular, Interpreta y decodifica los comandos que recibe mediante parámetros HTTP para posteriormente ejecutarlos en el servidor. Esta capacidad brinda al atacante un punto de apoyo inicial, permitiéndole ejecutar comandos arbitrarios, ampliar el compromiso del sistema y conservar el control sobre el entorno afectado.

Tras analizar la información actualizada publicada en la página de avisos de seguridad de JetBrains, se confirma que la vulnerabilidad presente en la versión afectada de TeamCity corresponde a **CVE-2024-27198**.

![Pasted image 20260630160252](../../Fotos/Pasted%20image%2020260630160252.png)
CVE-2024-27198

El atacante aprovechó la vulnerabilidad para crear una cuenta de usuario. ¿Qué credenciales creó?
![Pasted image 20260630161606](../../Fotos/Pasted%20image%2020260630161606.png)
![Pasted image 20260630161552](../../Fotos/Pasted%20image%2020260630161552.png)
c91oyemw:CL5vzdwLuK

El atacante subió un webshell para asegurar su acceso al sistema. ¿Cuál es el nombre del archivo que subió el atacante?
![Pasted image 20260630162222](../../Fotos/Pasted%20image%2020260630162222.png)
NSt8bHTg.zip

¿Cuándo ejecutó el atacante su primer comando a través del web shell?
![Pasted image 20260630162552](../../Fotos/Pasted%20image%2020260630162552.png)

![Pasted image 20260630164120](../../Fotos/Pasted%20image%2020260630164120.png)
2024-06-30 08:03

El atacante manipuló un archivo de texto que contenía las credenciales del usuario administrador del servidor web. ¿Qué nuevo nombre de usuario y contraseña escribió el atacante en el archivo?
![Pasted image 20260630165340](../../Fotos/Pasted%20image%2020260630165340.png)
a1l4m:youarecompromised

¿Cuál es el ID de la Técnica MITRE para la acción del atacante en la pregunta anterior (Q7) al manipular el archivo de texto?
De acuerdo con el contexto de la actividad realizada por el atacante sobre los archivos, la subtécnica de **MITRE ATT&CK** correspondiente a la técnica de **Manipulación de Datos** es **T1565.001 – Data Manipulation: Stored Data**
T1565.001

El atacante intentó escapar del contenedor pero no lo consiguió. ¿Qué orden usó para eso?
![Pasted image 20260630171111](../../Fotos/Pasted%20image%2020260630171111.png)

![Pasted image 20260630171044](../../Fotos/Pasted%20image%2020260630171044.png)

![Pasted image 20260630171534](../../Fotos/Pasted%20image%2020260630171534.png)
docker run --rm -it -v /:/host ubuntu chroot /host