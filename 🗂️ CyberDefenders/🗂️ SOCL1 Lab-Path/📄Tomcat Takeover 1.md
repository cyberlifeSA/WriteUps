- --
- Tags: #wireshark #cyberdefenders #tomcattakeover_lab
- --
Dada la actividad sospechosa detectada en el servidor web, el archivo PCAP revela una serie de solicitudes en varios puertos, indicando un posible comportamiento de escaneo. ¿Puedes identificar la dirección IP de origen responsable de iniciar estas solicitudes en nuestro servidor?

![Pasted image 20260701150354](../../Fotos/Pasted%20image%2020260701150354.png)

**Respuesta:** 14.0.0.120

Basándote en la dirección IP identificada asociada al atacante, ¿puedes identificar el país de donde se originaron las actividades del atacante?

![Pasted image 20260701150730](../../Fotos/Pasted%20image%2020260701150730.png)

**Respuesta:** China

A partir del archivo PCAP, se detectaron múltiples puertos abiertos como resultado del escaneo activo del atacante. ¿Cuál de estos puertos da acceso al panel de administración del servidor web?

![Pasted image 20260701151010](../../Fotos/Pasted%20image%2020260701151010.png)

![Pasted image 20260701151037](../../Fotos/Pasted%20image%2020260701151037.png)

**Respuesta:** 8080

Tras el descubrimiento de puertos abiertos en nuestro servidor, parece que el atacante intentó enumerar y descubrir directorios y archivos en nuestro servidor web. ¿Qué herramientas puedes identificar a partir del análisis que ayudaron al atacante en este proceso de enumeración?

![Pasted image 20260701151124](../../Fotos/Pasted%20image%2020260701151124.png)

**Respuesta:** gobuster

Tras el esfuerzo de enumerar directorios en nuestro servidor web, el atacante realizó numerosas solicitudes para identificar interfaces administrativas. ¿Qué directorio específico relacionado con el panel de administración descubrió el atacante?

![Pasted image 20260701151223](../../Fotos/Pasted%20image%2020260701151223.png)

**Respuesta:** /manager

Tras acceder al panel de administración, el atacante intentó forzar las credenciales de inicio de sesión. ¿Puedes determinar el nombre de usuario y la contraseña correctos que el atacante usó con éxito para iniciar sesión?

![Pasted image 20260701151807](../../Fotos/Pasted%20image%2020260701151807.png)

![Pasted image 20260701153252](../../Fotos/Pasted%20image%2020260701153252.png)

![Pasted image 20260701153314](../../Fotos/Pasted%20image%2020260701153314.png)

**Respuesta:** admin:tomcat

Una vez dentro del panel de administración, el atacante intentó subir un archivo con la intención de establecer un shell inverso. ¿Puedes identificar el nombre de este archivo malicioso a partir de los datos capturados?

![Pasted image 20260701160718](../../Fotos/Pasted%20image%2020260701160718.png)

![Pasted image 20260701160641](../../Fotos/Pasted%20image%2020260701160641.png)

**Respuesta:** JXQOZY.war

Tras establecer con éxito un reverse shell en nuestro servidor, el atacante pretendía asegurar la persistencia en la máquina comprometida. A partir del análisis, ¿puedes determinar qué comando específico están programados para ejecutar para mantener su presencia?

![Pasted image 20260701162128](../../Fotos/Pasted%20image%2020260701162128.png)

![Pasted image 20260701162157](../../Fotos/Pasted%20image%2020260701162157.png)