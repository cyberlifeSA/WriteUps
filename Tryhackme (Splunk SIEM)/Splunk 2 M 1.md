- --
- Tags: #splunk #siem 
- - -
### Serie 100
*Pregunta 1*
Amber Turing esperaba que Frothly fuera adquirida por un posible competidor que no prosperó, pero visitó su página web para encontrar la información de contacto de su equipo ejecutivo. ¿Cuál es el dominio web que visitó?
Filtro: `index="botsv2" amber`
Filtro: `index="botsv2" sourcetype="pan:traffic"`

![Pasted image 20260115183429](../Fotos/Pasted%20image%2020260115183429.png)

![Pasted image 20260115183408](../Fotos/Pasted%20image%2020260115183408.png)

Dato: `10.0.1.200`
Filtro:
```
index="botsv2" sourcetype="stream:http" "10.0.2.101" *beer*
| dedup site
| table site
```
beer es la indsutria pro eso filtramos y encontramos la web dns relacionada
Respuesta: www.berkbeer.com

*Pregunta 2*
Amber encontró la información de contacto del ejecutivo y le envió un correo electrónico. ¿Qué archivo de imagen mostraba la información de contacto del ejecutivo? Ejemplo de respuesta: /path/image.ext
Filtro: `index="botsv2" sourcetype="stream:http" "10.0.2.101" "www.berkbeer.com"`

![Pasted image 20260115184633](../Fotos/Pasted%20image%2020260115184633.png)

![Pasted image 20260115184620](../Fotos/Pasted%20image%2020260115184620.png)

Respuesta: /images/ceoberk.png

*Pregunta 3*
¿Cómo se llama el CEO? Proporciona el nombre y apellido.
Filtro: `index="botsv2" sourcetype="stream:smtp" "berkbeer.com"`

![Pasted image 20260115185121](../Fotos/Pasted%20image%2020260115185121.png)

![Pasted image 20260115185233](../Fotos/Pasted%20image%2020260115185233.png)

Respuesta: Martin Berk

*Pregunta 4*
¿Cuál es la dirección de correo electrónico del CEO?
Filtro: `index="botsv2" sourcetype="stream:smtp" "berkbeer.com"`

![Pasted image 20260115185329](../Fotos/Pasted%20image%2020260115185329.png)

Respuesta: mberk@berkbeer.com

*Pregunta 5*
Tras el primer contacto con el CEO, Amber contactó con otro empleado de este competidor. ¿Cuál es la dirección de correo electrónico de ese empleado?
Filtro: `index="botsv2" sourcetype="stream:smtp" "berkbeer.com"`

![Pasted image 20260115185553](../Fotos/Pasted%20image%2020260115185553.png)

Respuesta: hbernhard@berkbeer.com

*Pregunta 6*
¿Cuál es el nombre del archivo adjunto que Amber envió a un contacto en el competidor?
Filtro: `index="botsv2" sourcetype="stream:smtp" "berkbeer.com"`

![Pasted image 20260115190037](../Fotos/Pasted%20image%2020260115190037.png)

Respuesta: Saccharomyces_cerevisiae_patent.docx

*Pregunta 7*
¿Cuál es la dirección de correo electrónico personal de Amber?
Filtro: `index="botsv2" sourcetype="stream:smtp" "berkbeer.com"`
Click en `content_body` vemos contenido base64

![Pasted image 20260115190815](../Fotos/Pasted%20image%2020260115190815.png)

![1100](../Fotos/Pasted%20image%2020260115190858.png)

Respuesta: ambersthebest@yeastiebeastie.com
### Serie 200
*Pregunta 8*
¿Qué versión de TOR Browser instaló Amber para ocultar su navegación web? Guía de respuesta: Numérica con uno o más delimitadores.
Filtro: `index="botsv2" amber tor install`

![Pasted image 20260115194436](../Fotos/Pasted%20image%2020260115194436.png)

Respuesta: 7.0.4

*Pregunta 9*
¿Cuál es la dirección IPv4 pública del servidor que ejecuta www.brewertalk.com?
Filtro:
```
index="botsv2" source="stream:dns" "www.brewertalk.com"  
| table host_addr{}  
| dedup host_addr{}
```

![Pasted image 20260115194617](../Fotos/Pasted%20image%2020260115194617.png)

Respuesta: 52.42.208.228

*Pregunta 10*
Proporciona la dirección IP del sistema utilizado para ejecutar un escaneo de vulnerabilidades web contra www.brewertalk.com.
Filtro:
```
index="botsv2" source="stream:http" "www.brewertalk.com"  
| table dest_ip  
| dedup dest_ip
```

![Pasted image 20260115195203](../Fotos/Pasted%20image%2020260115195203.png)

Respuesta: 45.77.65.211

*Pregunta 11*
La dirección IP de Q#2 también está siendo utilizada por un software probablemente diferente para atacar una ruta de URI. ¿Qué es el camino URI? Guía de respuesta: Incluye la barra hacia adelante en tu respuesta. No incluyas la cadena de consulta ni otras partes del URI. Ejemplo de respuesta: /phpinfo.php
Filtro:
```
index="botsv2" src_ip="45.77.65.211" dest_ip="172.31.4.249"
| stats count by uri_path
```

![Pasted image 20260115195914](../Fotos/Pasted%20image%2020260115195914.png)

Respuesta: /member.php

*Pregunta 12*
¿Qué función SQL está siendo abusada en la ruta del URI según la pregunta anterior?
Analizamos el evento que vimos anteriormente para saber la funciona busada de SQL

![Pasted image 20260115200227](../Fotos/Pasted%20image%2020260115200227.png)

Respuesta: updatexml

*Pregunta 13*
¿Cuál fue el valor de la cookie que el navegador de Kevin transmitió a la URL maliciosa como parte de un ataque XSS? Guía de respuesta: Todos los dígitos. No el nombre de la galleta ni símbolos como un signo de igualdad.
Filtro:
`index="botsv2" sourcetype="stream:http" kevin "<script>"`

![Pasted image 20260115200803](../Fotos/Pasted%20image%2020260115200803.png)

Respuesta: 1502408189

*Pregunta 14*
¿Qué nombre de usuario brewertalk.com fue creado maliciosamente por un ataque de spear phishing?

![Pasted image 20260115201739](../Fotos/Pasted%20image%2020260115201739.png)

![Pasted image 20260115201802](../Fotos/Pasted%20image%2020260115201802.png)

Respuesta: KIagerfield

### Serie 300
*Pregunta 15*
La crítica presentación de PowerPoint de Mallory en su MacBook fue cifrada por ransomware el 18 de agosto. ¿Cuál es el nombre de este archivo después de que fue cifrado?
Filtro: `index="botsv2" host="MACLORY-AIR13" (*.ppt OR *.pptx)`

![Pasted image 20260116114549](../Fotos/Pasted%20image%2020260116114549.png)

Respuesta: Frothly_marketing_campaign_Q317.pptx.crypt

*Pregunta 16*
También hay un archivo de película de Juego de Tronos que fue cifrado. ¿Qué temporada y episodio es?
Filtro: `index="botsv2" host="MACLORY-AIR13" (got OR game OR thrones) *crypt*`

![Pasted image 20260116115009](../Fotos/Pasted%20image%2020260116115009.png)

Respuesta: S07E02

*Pregunta 17*
Kevin Lagerfield utilizó una memoria USB para mover malware a kutekitten, el MacBook personal de Mallory. Ejecutó el malware, que se oculta durante la ejecución. Proporciona el nombre del fabricante de la unidad USB que probablemente usó Kevin. Guía de respuesta: Utiliza la correlación temporal para identificar la unidad USB.
Filtro: `index="botsv2" kutekitten usb`

![Pasted image 20260116115911](../Fotos/Pasted%20image%2020260116115911.png)

![Pasted image 20260116115950](../Fotos/Pasted%20image%2020260116115950.png)

Respuesta: Alcor Micro Corp

*Pregunta 18*
¿En qué lenguaje de programación está al menos parte del malware de la pregunta anterior?
Revisando los eventos del filtro pasado encontramos el nombre de usuario de Mallory en su ordenador personal

![Pasted image 20260116120521](../Fotos/Pasted%20image%2020260116120521.png)

Filtro: `index="botsv2" kutekitten mkraeusen`
Field: `name`

![Pasted image 20260116121333](../Fotos/Pasted%20image%2020260116121333.png)

`index="botsv2" kutekitten mkraeusen name=file_events`

![Pasted image 20260116122342](../Fotos/Pasted%20image%2020260116122342.png)

![Pasted image 20260116122358](../Fotos/Pasted%20image%2020260116122358.png)

md5 hash: 72d4d364ed91dd9418d144a2db837a6d

![Pasted image 20260116122556](../Fotos/Pasted%20image%2020260116122556.png)

Respuesta: Pearl

*Pregunta 19*
¿Cuándo se vio por primera vez este malware en la naturaleza? Guía de respuesta: YYY-MM-DD

![Pasted image 20260116122725](../Fotos/Pasted%20image%2020260116122725.png)

Respuesta: 2017-01-17

*Pregunta 20*
El malware que infecta kutekitten utiliza destinos DNS dinámicos para comunicarse con dos servidores C&C poco después de la instalación. ¿Cuál es el nombre de dominio totalmente cualificado (FQDN) del primer destino (alfabético)?

![Pasted image 20260116122936](../Fotos/Pasted%20image%2020260116122936.png)

Respuesta: eidk.duckdns.org

*Pregunta 21*
A partir de la pregunta anterior, ¿cuál es el nombre de dominio totalmente cualificado (FQDN) del segundo servidor C&C contactado alfabéticamente?

![Pasted image 20260116122936](../Fotos/Pasted%20image%2020260116122936.png)

Respuesta: eidk.hopto.org

### Serie 400
*Pregunta 22*
Una agencia federal de seguridad informa que Taedonggang suele hacer spear phishing a sus víctimas con archivos zip que deben abrirse con contraseña. ¿Cuál es el nombre del apego enviado a Frothly por un actor malicioso de Taedonggang?
`index="botsv2" sourcetype="stream:smtp" *.zip`

![500](../Fotos/Pasted%20image%2020260116124026.png)

Respuesta: invoice.zip

*Pregunta 23*
¿Cuál es la contraseña para abrir el archivo zip?
`Raw View` mismo evento

![Pasted image 20260116124429](../Fotos/Pasted%20image%2020260116124429.png)

Respuesta: 912345678

*Pregunta 24*
El grupo APT de Taedonggang cifra la mayor parte de su tráfico con SSL. ¿Cuál es el "emisor SSL" que utilizan para la mayoría de su tráfico? Guía de respuesta: Copia el campo exactamente, incluyendo los espacios

![500](../Fotos/Pasted%20image%2020260116130705.png)

![500](../Fotos/Pasted%20image%2020260116130726.png)

45.77.65.211
Filtro: `index="botsv2" dest_ip"45.77.65.211" SSL`

![400](../Fotos/Pasted%20image%2020260116131042.png)

Field: `ssl_issuer`

![Pasted image 20260116131019](../Fotos/Pasted%20image%2020260116131019.png)

Respuesta: C = US

*Pregunta 25*
¿Qué archivo inusual (para una empresa estadounidense) winsys32.dll provoca que se descargue en el entorno de Frothly?
Filtro: `index="botsv2" winsys32.dll`
Filtro: `index="botsv2" sourcetype="stream:ftp" (get OR retr)`

![600](../Fotos/Pasted%20image%2020260116133104.png)

Dado que copiar y pegar por tema de formato causaba problemas usamos CyberChef

![Pasted image 20260116133204](../Fotos/Pasted%20image%2020260116133204.png)

Respuesta: 나는_데이비드를_사랑한다.hwp

*Pregunta 26*
¿Cuál es el nombre y apellido del pobre inocente que fue implicado en los metadatos del archivo que ejecutó PowerShell Empire en la estación de trabajo de la primera víctima? Ejemplo de respuesta: John Smith
encontraste
Aqui nos entregan 3 links para trabajar:
[Free Automated Malware Analysis Service - powered by Falcon Sandbox - Viewing online file analysis results for 'invoice.doc'](https://hybrid-analysis.com/sample/d8834aaa5ad6d8ee5ae71e042aca5cab960e73a6827e45339620359633608cf1/598155a67ca3e1449f281ac4)
[VirusTotal - File - d8834aaa5ad6d8ee5ae71e042aca5cab960e73a6827e45339620359633608cf1](https://www.virustotal.com/gui/file/d8834aaa5ad6d8ee5ae71e042aca5cab960e73a6827e45339620359633608cf1/detection)
[Analysis invoice.doc (MD5: 3709EEF2D72DE0DE72649EBDAF3E4082) Malicious activity - Interactive analysis ANY.RUN](MD5:%203709EEF2D72DE0DE72649EBDAF3E4082)%20Malicious%20activity%20-%20Interactive%20analysis%20ANY.RUN)%20Malicious%20activity%20-%20Interactive%20analysis%20ANY.RUN)
![Pasted image 20260116140728](../Fotos/Pasted%20image%2020260116140728.png)
Respuesta: Ryan Kovar

*Pregunta 27*
Dentro del documento, ¿qué tipo de puntos se mencionan si encontraste el texto?

![Pasted image 20260116141033](../Fotos/Pasted%20image%2020260116141033.png)

Respuesta: CyberEastEgg

*Pregunta 28*
Para mantener la persistencia en la red Frothly, el APT de Taedonggang configuró varias Tareas Programadas para que se enviaran balizas de vuelta a su servidor C2. ¿Qué página es la más contactada por estas tareas programadas? Ejemplo de respuesta: index.php o images.html
Info: Esto devuelve 9 eventos, los primeros 6 son tareas para configurar actualizaciones automáticas y monitorización, así que los ignoro. Los últimos 3 eventos parecen ser tareas programadas creadas por el atacante:
`index="botsv2" schtasks.exe sourcetype=wineventlog create`

![Pasted image 20260116141723](../Fotos/Pasted%20image%2020260116141723.png)

`HKLM:\Software\Microsoft\Network`
base64

![Pasted image 20260116142443](../Fotos/Pasted%20image%2020260116142443.png)

![Pasted image 20260116142412](../Fotos/Pasted%20image%2020260116142412.png)

Respuesta: process.php