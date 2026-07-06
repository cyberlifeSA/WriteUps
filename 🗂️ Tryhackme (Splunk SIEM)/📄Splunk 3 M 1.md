- --
- Tags: #splunk #siem 
- - - 
### AWS y otros eventos
*Pregunta 1*
¿Puedes enumerar los usuarios de IAM que accedieron a un servicio de AWS (con éxito o sin éxito) en el entorno AWS de Frothly?. Guía de respuesta: Comas separadas sin espacios, en orden alfabético. (Ejemplo: Ajackson, Mjones, Tmiller)
Consulta el siguiente enlace para hacerte una idea del **tipo de fuente** que necesitas consultar y qué **campo** de los resultados tendrá la respuesta que buscas.
**Enlace**: [https://docs.AWS.amazon.com/awscloudtrail/latest/userguide/ Cloudtrail-log-file-examples.html](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-log-file-examples.html)
```
index="botsv3" user_type=IAMuser 
|  stats count by user
```
![Pasted image 20260116154951](../Fotos/Pasted%20image%2020260116154951.png)    ![Pasted image 20260116155053](../Fotos/Pasted%20image%2020260116155053.png)
Respuesta: bstoll,btun,splunk_access,web_admin

*Pregunta 2*
¿Qué campo usarías para alertar de que la actividad de la API de AWS ha ocurrido sin autenticación multifactor (MFA)? Guía para responder: Proporciona el camino completo en JSON. (Ejemplo: iceCream.flavors.traditional)
**Enlaces**:
- [https:// aws.amazon.com/premiumsupport/knowledge-center/ s3-bucket-public-access/](https://aws.amazon.com/premiumsupport/knowledge-center/s3-bucket-public-access/)
- [](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail-additional-examples.html#cloudwatch-alarms-for-cloudtrail-no-mfa-ejemplo](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail-additional-examples.html#cloudwatch-alarms-for-cloudtrail-no-mfa-ejemplo](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail-additional-examples.html#cloudwatch-alarms-for-cloudtrail-no-mfa-ejemplo](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail-additional-examples.html#cloudwatch-alarms-for-cloudtrail-no-mfa-ejemplo](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail-additional-examples.html#cloudwatch-alarms-for-cloudtrail-no-mfa-ejemplo](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail-additional-examples.html#cloudwatch-alarms-for-cloudtrail-no-mfa-ejemplo](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail-additional-examples.html#cloudwatch-alarms-for-cloudtrail-no-mfa-ejemplo](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail-additional-examples.html#cloudwatch-alarms-for-cloudtrail-no-mfa-ejemplo](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail-additional-examples.html#cloudwatch-alarms-for-cloudtrail-no-mfa-example)
![Pasted image 20260116155716](../Fotos/Pasted%20image%2020260116155716.png)
Respuesta: userIdentity.sessionContext.attributes.mfaAuthenticated

*Pregunta 3*
¿Cuál es el número de procesador utilizado en los servidores web? Guía de respuesta: Incluye cualquier carácter especial o puntuación. (Ejemplo: El número de procesador del Intel Core i7-8650U es i7-8650U.)
Filtro:
`index="botsv3" sourcetype="osquery:results" amd OR intel`
![Pasted image 20260116155926](../Fotos/Pasted%20image%2020260116155926.png)
o
Filtro: 
```
index="botsv3" sourcetype="osquery:results" amd OR intel 
|  stats count by "columns.cpu_brand"
```
![Pasted image 20260116160104](../Fotos/Pasted%20image%2020260116160104.png)
Respuesta: E5-2676

*Pregunta 4*
Bud hace que un cubo S3 sea accesible públicamente por accidente. ¿Cuál es el ID de evento de la llamada API que permitió el acceso público? Guía de respuesta: Incluye cualquier carácter especial o puntuación.
**Enlace**: [https://docs.aws.amazon.com/AmazonS3/latest/ API/API_PutBucketAcl.html](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketAcl.html)
En el link encontramos esto
![Pasted image 20260116162754](../Fotos/Pasted%20image%2020260116162754.png)
Filtro: `index="botsv3" sourcetype="aws:cloudtrail" http://acs.amazonaws.com/groups/global/AllUsers`
![Pasted image 20260116162950](../Fotos/Pasted%20image%2020260116162950.png)
Respuesta: ab45689d-69cd-41e7-8705-5350402cf7ac

*Pregunta 5*
¿Cuál es el nombre de usuario de Bud?
**Enlace**: [https://docs.aws.amazon.com/AmazonS3/latest/ API/API_PutBucketAcl.html](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketAcl.html)
![Pasted image 20260116164220](../Fotos/Pasted%20image%2020260116164220.png)
Respuesta: bstoll

*Pregunta 6*
¿Cómo se llama el bucket S3 que se hizo accesible públicamente?
**Enlace**: [https://docs.aws.amazon.com/AmazonS3/latest/ API/API_PutBucketAcl.html](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketAcl.html)
![Pasted image 20260116164411](../Fotos/Pasted%20image%2020260116164411.png)
Respuesta: frothlywebcode

*Pregunta 7*
¿Cuál es el nombre del archivo de texto que se subió correctamente al bucket S3 mientras era accesible públicamente? Guía de respuesta: Proporciona solo el nombre del archivo y la extensión, no la ruta completa. (Ejemplo: filename.docx en lugar de /mylogs/web/filename.docx)
**Enlace**: [https://docs.aws.amazon.com/AmazonS3/latest/ API/API_PutObject.html](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObject.html)
Filtro:
```
index="botsv3" sourcetype="aws:s3:accesslogs"
|  stats count by request_uri
```
![Pasted image 20260116164851](../Fotos/Pasted%20image%2020260116164851.png)
Respuesta: OPEN_BUCKET_PLEASE_FIX.txt

*Pregunta 8*
¿Cuál es la FQDN del endpoint que ejecuta una edición diferente del sistema operativo de Windows que los demás?
Info: Busqué "windows" para buscar un campo que tuviera sistema operativo. Lo filtré por tipo de evento=hostmon_inventory y luego usé el recuento de estadísticas para enumerar el sistema operativo y el anfitrión.
Filtro: `index="botsv3" sourcetype=winhostmon windows eventtype=hostmon_inventory`
Filtro ganador: 
```
index="botsv3" sourcetype=winhostmon windows eventtype=hostmon_inventory 
|  stats count by OS host
```
![Pasted image 20260116165352](../Fotos/Pasted%20image%2020260116165352.png)
Respuesta: BSTOLL-L.froth.ly
### Eventos de minería de criptomonedas
*Pregunta 9*
Un punto final espumoso muestra signos de actividad minera de monedas. ¿Cómo se llama el **segundo proceso** que alcanzará el 100 por ciento del tiempo de utilización del procesador de CPU desde esta actividad en este punto final? Guía de respuesta: Incluye cualquier carácter especial o puntuación.
Filtro: `index="botsv3" sourcetype="PerfmonMk:Process" process_cpu_used_percent=100| top process_name process_cpu_used_percent`
![Pasted image 20260116171414](../Fotos/Pasted%20image%2020260116171414.png)
Respuesta: chrome#5

*Pregunta 10*
¿Cuál es el nombre de host corto del único endpoint Frothly que realmente minaría la criptomoneda Monero? (Ejemplo: ahamilton en lugar de ahamilton.mycompany.com)
Filtro: `index="botsv3" sourcetype="PerfmonMk:Process" process_cpu_used_percent=100 process_name="chrome#5"`
![Pasted image 20260116171627](../Fotos/Pasted%20image%2020260116171627.png)
Respuesta: BSTOLL-L

*Pregunta 11*
Utilizando las funciones de orden de eventos de Splunk, ¿cuál es el primer ID de firma visto de la amenaza minero según los datos Symantec Endpoint Protection (SEP) de Frothly?
Filtro:
```
index="botsv3" sourcetype="symantec:ep:security:file" 
|  reverse 
| table CIDS_Signature_ID CIDS_Signature_String _time
```
![Pasted image 20260116173709](../Fotos/Pasted%20image%2020260116173709.png)
Respuesta: 30358

*Pregunta 12*
¿Cómo se llama el ataque?
Filtro:
```
**index="botsv3" sourcetype="symantec:ep:security:file" 
|  reverse 
| table CIDS_Signature_ID CIDS_Signature_String _time**
```
![Pasted image 20260116173720](../Fotos/Pasted%20image%2020260116173720.png)
Respuesta: JSCoinminer Download 8

*Pregunta 13*
Según la web de Symantec, ¿cuál es la gravedad de esta amenaza específica de mineros de monedas?
![Pasted image 20260116174006](../Fotos/Pasted%20image%2020260116174006.png)
Respuesta: Medium

*Pregunta 14* REPASAR
¿Cuál es el nombre abreviado del único endpoint de Frothly que muestra pruebas de haber derrotado la amenaza de las criptomonedas? (Ejemplo: ahamilton en lugar de ahamilton.mycompany.com)
![Pasted image 20260116174401](../Fotos/Pasted%20image%2020260116174401.png)
Respuesta: btun-l

### Más eventos de AWS
*Pregunta 15*
¿Qué clave de acceso de usuario IAM genera los errores más evidentes al intentar acceder a los recursos IAM?
Enlace:
- [https://docs. aws.amazon.com/awscloudtrail/latest/userguide/ rastro de nubes-event-reference-record-contents.html](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-record-contents.html)[](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-record-contents.html)
- [https://community. splunk.com/t5/Splunk-Search/¿Cómo puedo recuperar el contar-o-distinto-contar-de-algunos-valores-campos/m-p/33619](https://community.splunk.com/t5/Splunk-Search/How-can-I-retrieve-count-or-distinct-count-of-some-field-values/m-p/33619)
Filtro:
```
index="botsv3" sourcetype="AWS:CLOUDTRAIL" userIdentity.accessKeyId=* msg=AccessDenied 
|  stats count by userIdentity.accessKeyId
```

![Pasted image 20260116175402](../Fotos/Pasted%20image%2020260116175402.png)
o
Filtro: `index="botsv3" sourcetype="AWS:CLOUDTRAIL" userIdentity.accessKeyId=* msg=AccessDenied`
![Pasted image 20260116175321](../Fotos/Pasted%20image%2020260116175321.png)
Respuesta: AKIAJOGCDXJ5NW5PXUPA

*Pregunta 16*
Bud accidentalmente compromete las claves de acceso de AWS en un repositorio de código externo. Poco después, recibe una notificación de AWS informando de que la cuenta ha sido comprometida. ¿Cuál es el ID del caso de soporte que Amazon abre en su nombre?
Filtro: `index="botsv3" sourcetype="stream:smtp" "*access key*"`
![Pasted image 20260116175744](../Fotos/Pasted%20image%2020260116175744.png)
Respuesta: 5244329601

*Pregunta 17*
Las claves de acceso de AWS constan de dos partes: un ID de clave de acceso (por ejemplo, AKIAIOSFODNN7EXAMPLE) y una clave de acceso secreta (por ejemplo, wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY). ¿Cuál es la clave de acceso secreta de la clave que se filtró al repositorio de código externo?
Filtro: `index="botsv3" sourcetype="stream:smtp" "*access key*"`
![Pasted image 20260116181410](../Fotos/Pasted%20image%2020260116181410.png)
![Pasted image 20260116181419](../Fotos/Pasted%20image%2020260116181419.png)
![Pasted image 20260116181447](../Fotos/Pasted%20image%2020260116181447.png)
![Pasted image 20260116181458](../Fotos/Pasted%20image%2020260116181458.png)
Respuesta: Bx8/gTsYC98T0oWiFhpmdROqhELPtXJSR9vFPNGk

*Pregunta 18*
Usando la clave filtrada, el adversario intenta no autorizado crear una clave para un recurso específico. ¿Cómo se llama ese recurso? Responder a la guía: Una palabra.
Filtro: `index="botsv3" sourcetype="aws:cloudtrail" "userIdentity.accessKeyId"=AKIAJOGCDXJ5NW5PXUPA eventName=CreateAccessKey`
Enlace: [https://docs.aws.amazon.com/ IAM/latest/APIReference/API_CreateAccessKey.html](https://docs.aws.amazon.com/IAM/latest/APIReference/API_CreateAccessKey.html)
![Pasted image 20260116181743](../Fotos/Pasted%20image%2020260116181743.png)
Respuesta:nullweb_admin

*Pregunta 19*
Usando la clave filtrada, el adversario intenta no autorizar describir una cuenta. ¿Cuál es la cadena completa de agente de usuario de la aplicación que originó la solicitud?
`index="botsv3" sourcetype="aws:cloudtrail" "userIdentity.accessKeyId"=AKIAJOGCDXJ5NW5PXUPA"`
![Pasted image 20260116182101](../Fotos/Pasted%20image%2020260116182101.png)
Respuesta: ElasticWolf/5.1.6

### Volviendo a los eventos de endpoint
*Pregunta 20*
¿Cuál es la cadena completa de agente de usuario que subió el archivo de enlace malicioso a OneDrive?
Filtro:
```
sourcetype="ms:o365:management" "OneDrive" Operation=FileUploaded
| table _time src_ip user object UserAgent
```
![Pasted image 20260117135119](../Fotos/Pasted%20image%2020260117135119.png)
Respuesta: Mozilla/5.0 (X11; U; Linux i686; ko-KP; rv: 19.1br) Gecko/20130508 Fedora/1.9.1-2.5.rs3.0 NaenaraBrowser/3.5b4

*Pregunta 21*
¿Cuál era el nombre del archivo adjunto habilitado por macros identificado como malware?
Filtro: `sourcetype="stream:smtp" "attach_filename{}"="Malware Alert Text.txt"`
![622](../Fotos/Pasted%20image%2020260117135923.png)      ![Pasted image 20260117135745](../Fotos/Pasted%20image%2020260117135745.png)
![800](../Fotos/Pasted%20image%2020260117135849.png)
Respuesta: Frothly-Brewery-Financial-Planning-FY2019-Draft.xlsm	

*Pregunta 22*
¿Cuál es el nombre del ejecutable que estaba incrustado en el malware? Guía para responder: Incluye la extensión del archivo. (Ejemplo: explorer.exe)
Filtro: `sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" *xlsm*`
![Pasted image 20260117140539](../Fotos/Pasted%20image%2020260117140539.png)
![Pasted image 20260117140524](../Fotos/Pasted%20image%2020260117140524.png)
Respuesta: HxTsr.exe

*Pregunta 23*
¿Cuál es la contraseña del usuario que fue creada correctamente por el "root" del usuario en el sistema Linux local?
Filtro: `(adduser OR useradd) source="/var/log/auth.log"`
![Pasted image 20260117140848](../Fotos/Pasted%20image%2020260117140848.png)
Filtro: `tomcat7 sourcetype="osquery:results"`
![Pasted image 20260117140942](../Fotos/Pasted%20image%2020260117140942.png)
Respuesta: ilovedavidverve

*Pregunta 24*
¿Cuál es el nombre del usuario que se creó después de que el endpoint fuera comprometido?
Filtro: `source="wineventlog:security" EventCode="4720"`
![700](../Fotos/Pasted%20image%2020260117141141.png)
![Pasted image 20260117141133](../Fotos/Pasted%20image%2020260117141133.png)
Respuesta: svcvnc

*Pregunta 25*
Según la pregunta anterior, ¿a qué grupos se asignó este usuario después de que el endpoint fuera comprometido? Guía de respuesta: Comas separadas sin espacios, en orden alfabético.
Filtro: `source="wineventlog:security" svcvnc EventCode=4732`
![700](../Fotos/Pasted%20image%2020260117141336.png)
![Pasted image 20260117141323](../Fotos/Pasted%20image%2020260117141323.png)
Respuesta: administrators,user

*Pregunta 26*
¿Cuál es el ID del proceso que escucha en un puerto "leet"?
Filtro: `1337 sourcetype="osquery:results" "columns.port"=1337`
![Pasted image 20260117143727](../Fotos/Pasted%20image%2020260117143727.png)
Respuesta: 14356

*Pregunta 27*
¿Cuál es el valor MD5 del archivo descargado en el sistema final de Fyodor y usado para escanear la red de Frothly?
Filtro:
```
host="fyodor-l" sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=1  
| table app  
| reverse
```
![Pasted image 20260117144412](../Fotos/Pasted%20image%2020260117144412.png)
Filtro: `host="fyodor-l" sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=1 hdoor.exe`
![Pasted image 20260117144833](../Fotos/Pasted%20image%2020260117144833.png)
Respuesta: 586EF56F4D8963DD546163AC31C865D7

### Más eventos en los puntos finales
*Pregunta 28*
¿Qué número de puerto usó el adversario para descargar sus herramientas de ataque?
Filtro: `sourcetype="stream:http"`
![Pasted image 20260117145721](../Fotos/Pasted%20image%2020260117145721.png)
Filtro: `sourcetype="stream:http" 3333`
![Pasted image 20260117145912](../Fotos/Pasted%20image%2020260117145912.png)
Filtro: `sourcetype="stream:http" dest_port=3333`
![Pasted image 20260117145953](../Fotos/Pasted%20image%2020260117145953.png)
Respuesta: 3333

*Pregunta 29*
Basándonos en la información recopilada para la pregunta 1, ¿qué archivo se puede inferir que contiene las herramientas de ataque? Guía para responder: Incluye la extensión del archivo.
Filtro: `sourcetype="stream:http" dest_port=3333`
![Pasted image 20260117150958](../Fotos/Pasted%20image%2020260117150958.png)
Respuesta: logos.png

*Pregunta 30*
Durante el ataque, dos archivos son transmitidos remotamente al directorio /tmp del servidor Linux local por el adversario. ¿Cuáles son los nombres de estos archivos? Guía de respuesta: Comas separadas sin espacios, en orden alfabético, incluyen la extensión del archivo cuando corresponda.
Filtro: `earliest=0 /tmp/*.* sourcetype!=lsof NOT phpsessionclean sourcetype="osquery:results" name=pack_fim_file_events`
![Pasted image 20260117151506](../Fotos/Pasted%20image%2020260117151506.png)
Filtro: `earliest=0 colonel.c OR definitelydontinvestigatethisfile.sh OR loot.txt OR blargh.tgz OR suitecrm.sql | reverse`
![Pasted image 20260117152015](../Fotos/Pasted%20image%2020260117152015.png)
Respuesta: colonel.c,definitelydontinvestigatethisfile.sh

*Pregunta 31*
El adversario de Taedonggang envió un correo electrónico a Grace Hoppy presumiendo de la exitosa extracción de datos de clientes. ¿Cuántos correos electrónicos de clientes de Frothly fueron expuestos o revelados?
Filtro: `sourcetype="stream:smtp" *Grace Hoppy* earliest=0 sourcetype!="ms:aad:signin"`
![Pasted image 20260117152710](../Fotos/Pasted%20image%2020260117152710.png)
![800](../Fotos/Pasted%20image%2020260117152746.png)
Respuesta: 8

*Pregunta 32*
¿Cuál es la ruta a la URL que accede el servidor de comando y control? Guía de respuesta: Proporciona el camino completo. (Ejemplo: La ruta completa para la https://imgur.com/a/mAqgt4S/lasd3.jpg URL es /a/mAqgt4S/lasd3.jpg)
Info: Después de filtrar los registros ruidosos de PowerShell, noté que el script asignaba un URI C2 a una variable. Solía extraerlo y revelarlo como el camino completo al que se accedía.`rex``/admin/get.php`
Filtro: NO FUNCIONO ESTE FILTRO REVISAR
```
index=botsv3 earliest=0 source="WinEventLog:Microsoft-Windows-PowerShell/Operational" Message!="PowerShell console*" Message="*/*"  
| rex field=Message "\\$t\\=[\\'\\"](?<c2_uri>[^%5C%5C'%5C%5C")"  
| table c2_uri  
| dedup c2_uri
```
![Pasted image 20260117153634](../Fotos/Pasted%20image%2020260117153634.png)
Respuesta: /admin/get.php

*Pregunta 33*
Al menos dos puntos finales Frothly contactan con la infraestructura de mando y control del adversario. ¿Cuáles son sus nombres de anfitrión cortos? Guía de respuesta: Comas separadas sin espacios, en orden alfabético.
Filtro: `"/news.php" OR "/login/process.php" OR "/admin/get.php"`
![Pasted image 20260117153905](../Fotos/Pasted%20image%2020260117153905.png)
Respuesta: ABUNGST-L,FYODOR-L
