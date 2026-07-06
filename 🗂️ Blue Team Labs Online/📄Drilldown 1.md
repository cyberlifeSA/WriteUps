- --
- Tags: #splunk #btlo #drilldown_lab
- --

Resumen
IOC
Timeline
MITRE
Detección
Recomendaciones

*Pregunta 1) WayneCorpInc no utiliza Amazon Web Service para alojamiento en la nube, así que cuando un cazador de amenazas descubrió conexiones salientes a instancias EC2, inmediatamente comenzó a profundizar en esta actividad para poder proporcionar todo el contexto posible al Equipo de Respuesta a Incidentes. Usando los registros de Sysmon, ¿cuántos nombres de host de destino se encuentran? (Formato: # Destination EC2s)*
![Pasted image 20260605083352](../Fotos/Pasted%20image%2020260605083352.png)
3

*Pregunta 2) Introduzca los nombres de host (excluyendo '.compute-1.amazon.aws.com') en orden de recuento de eventos, del más alto al más bajo (Formato: Hostname1, Hostname2, ...)*
![Pasted image 20260605083600](../Fotos/Pasted%20image%2020260605083600.png)
ec2-23-22-63-114, ec2-184-72-234-184, ec2-52-70-175-181

*Pregunta 3) Mira el campo 'interesante' de Imagen para ver qué archivos están iniciando estas conexiones. ¿Cuál es el valor de Imagen con el recuento más bajo? (Formato: Valor de imagen)*
![Pasted image 20260605085958](../Fotos/Pasted%20image%2020260605085958.png)
Respuesta: C:\inetpub\wwwroot\joomla\3791.exe

*Pregunta 4) ¿Cuál es el nombre de host y la dirección IP interna del sistema que inició esta conexión? (Formato: Nombre de anfitrión, X.X.X.X)*
![Pasted image 20260605090559](../Fotos/Pasted%20image%2020260605090559.png)
we1149srv.waynecorpinc.local, 192.168.250.70

*Pregunta 5) ¿A qué hora fue este evento de conexión? Usar TimeCreatedSystemTimeTime (Formato: AAAAAAA-MM-DDTHH:MM:SS)*
![Pasted image 20260605090942](../Fotos/Pasted%20image%2020260605090942.png)
2016-08-10 21:56:19

*Pregunta 6) ¿Cuál es el nombre de host de destino y la dirección IP de la instancia EC2 de AWS? (Formato: Nombre de anfitrión, X.X.X.X)*
![Pasted image 20260605092736](../Fotos/Pasted%20image%2020260605092736.png)
ec2-23-22-63-114.compute-1.amazonaws.com, 23.22.63.114

*Pregunta 7) Utiliza los logs de Sysmon para encontrar el hash SHA256 del ejecutable que establece esta conexión. ¿Cuál es el valor hash? (Formato: SHA256 Hash)*
![Pasted image 20260605094154](../Fotos/Pasted%20image%2020260605094154.png)
EC78C938D8453739CA2A370B9C275971EC46CAF6E479DE2B2D04E97CC47FA45D

*Pregunta 8) Busca este hash en internet para saber más sobre su reputación. En la pestaña de Comportamiento mira los resultados de Microsoft Sysinternals. ¿Qué dos direcciones IPv4 aparecen que empiezan por 23.216? (Formato: X.X.X.X, X.X.X.X)*
![Pasted image 20260605101513](../Fotos/Pasted%20image%2020260605101513.png)
23.216.147.64, 23.216.147.76

*Pregunta 9) Usando estas dos IPs recopiladas, comprueba si hay actividad de ellas en Splunk, ¡lo cual puede que no lo haya! ¿Cuál es el número de eventos por IP donde la dirección se menciona EN CUALQUIER PARTE de un registro? (Formato: IP1EventoConteo, IP2ConteoEvento)*
![Pasted image 20260605104656](../Fotos/Pasted%20image%2020260605104656.png)
0, 0

*Pregunta 10) ¿A qué hora se subió este archivo al servidor web? (Usa el valor de 'timestamp') (Formato: YYY-MM-DDTHH:MM:SS)*
![Pasted image 20260605112244](../Fotos/Pasted%20image%2020260605112244.png)
2016-08-10 21:52:47

*Pregunta 11) ¿Qué user-agent se usó para subir el archivo? (Formato: Agente de usuario completo)*
![Pasted image 20260605145850](../Fotos/Pasted%20image%2020260605145850.png)
Mozilla/5.0 (Windows NT 6.1; WOW64; Trident/7.0; rv:11.0) like Gecko

*Pregunta 12) ¿Qué URI recibió una solicitud POST del atacante para subir el archivo? (Formato: /camino/a/algo)*
![Pasted image 20260605150905](../Fotos/Pasted%20image%2020260605150905.png)
![Pasted image 20260605151716](../Fotos/Pasted%20image%2020260605151716.png)
/joomla/administrator/index.php

*Pregunta 13) ¿Cuál es la IP de origen responsable de la actividad inicial de acceso? (Formato: X.X.X.X)*
![Pasted image 20260605154733](../Fotos/Pasted%20image%2020260605154733.png)
![438](../Fotos/Pasted%20image%2020260605155954.png)
40.80.148.42

*Pregunta 14) Necesitamos entender si alguna de nuestras defensas de red ha detectado esta actividad, o si estamos completamente ciegos. Utiliza uno de los indicadores recuperados para buscar en los registros si algo ha señalado este archivo como malicioso. Proporciona cualquier marca de tiempo recuperada de un registro relevante para mostrar evidencia de algún tipo de alerta o notificación (Formato: AAAAA-MM-DD HH:MM:SS)*
![Pasted image 20260605161523](../Fotos/Pasted%20image%2020260605161523.png)
2016-10-98 15:52:45