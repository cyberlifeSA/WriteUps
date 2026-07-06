- --
- Tags: #splunk #siem 
- - -
**TRYHACKME**

*Pregunta 1*
¿Cuál es el contenido de flag.txt en la carpeta de inicio del usuario?
THM{faa9bac345709b6620a6200b484c7594}

*Pregunta 2*
¿Cuál es el nombre del usuario de la puerta de atrás que se creó en el servidor tras la explotación?
Para ver qué usuario se añadió después de que se ejecutara el exploit, podemos comprobar el archivo . En esto, solo tenemos que buscar las entradas de nuevo usuario. Destaca uno en particular aquí, creado el 4 de julio de 2024.`/var/log/auth.log`
Filtro: `index=* "new user"`
![Pasted image 20260115114737](../Fotos/Pasted%20image%2020260115114737.png)
Respuesta: eviluser

*Pregunta 3*
¿Cómo se llama el paquete de aspecto malicioso instalado en el servidor?
Sabemos que debe haber algún tipo de paquete instalado, así que podemos buscar así. Considera que esta vez estamos buscando en Linux. Como buscamos un paquete, podemos ser muy específicos con la fuente:
Filtro: `source="/var/log/dpkg.log" "installed"`
![Pasted image 20260115115517](../Fotos/Pasted%20image%2020260115115517.png)
Respuesta: datacollector

*Pregunta 4*
¿Cuál es el nombre del plugin instalado en el servidor tras una explotación exitosa?
Esta vez buscamos un plugin en teamcity, así que especifiquemos la fuente para teamcity, que es `/opt/teamcity/*`. Estamos buscando un plugin que haya sido subido, así que este debería ser nuestro término de búsqueda.
Filtro: `source="/opt/teamcity/*" "plugin" "uploaded"` o `index=* source="/opt/teamcity/*" "plugin" "uploaded"`
![Pasted image 20260115122624](../Fotos/Pasted%20image%2020260115122624.png)
Respuesta: AyzzbuXY.zip
