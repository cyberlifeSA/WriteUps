- --
- Tags: #elasticsearch 
- --
**TRYACKME**
## 🎯 Objetivos trabajados en el laboratorio 
1. **Analizar tráfico HTTP con Elastic**  
    → Uso de campos clave como `response.status`, `http.url`, `User-Agent` e IPs para entender el comportamiento web.
2. **Identificar actividad anómala desde una IP**  
    → Detección de **tráfico inusual** concentrado en una sola dirección (`transaction.remote_address`).
3. **Reconocer herramientas de reconocimiento (Reconnaissance)**  
    → Identificación de escáneres y enumeradores como **Nmap** y **Gobuster** mediante `User-Agent`.
4. **Detectar enumeración de directorios**  
    → Uso de múltiples errores **404** para confirmar **fuerza bruta de rutas**.
5. **Localizar recursos válidos y sensibles (200 OK)**  
    → Descubrimiento de archivos y directorios interesantes, incluyendo **flags**.
6. **Identificar paneles administrativos expuestos**  
    → Detección de `/admin-login.php` como superficie de ataque crítica.
7. **Detectar ataques de fuerza bruta (Brute Force)**  
    → Identificación de **Hydra** como herramienta de ataque mediante `User-Agent`.
8. **Extraer credenciales desde logs**  
    → Decodificación de **Basic Auth en Base64** para obtener usuario y contraseña.
9. **Confirmar compromiso del servidor web**  
    → Identificación de **web shell PHP** subida por el atacante.
10. **Analizar ejecución remota de comandos (RCE)**  
    → Confirmación del primer comando ejecutado (`whoami`).
11. **Detectar vulnerabilidad de LFI (Local File Inclusion)**  
    → Acceso a archivos sensibles del sistema (`/etc/phpmyadmin/config-db.php`).
12. **Monitorear abuso de herramientas administrativas**  
    → Uso indebido de **phpMyAdmin** para acceso y manipulación de datos.
13. **Detectar exfiltración de bases de datos**  
    → Identificación de exportación de la base `customer_credit_cards`.
14. **Detectar manipulación de datos sensibles

----
`response.status`
`http.url`
`request.headers.User-Agent`
`transactions.remote_address`
![Pasted image 20260111125518](../Fotos/Pasted%20image%2020260111125518.png)
Cantidad inusual de tráfico desde una IP en un solo día.
`transaction.remote_address`

*Pregunta 1*
¿Cuál fue el primer escáner que el atacante ejecutó contra el servidor web?
![Pasted image 20260320130245](../Fotos/Pasted%20image%2020260320130245.png)
![Pasted image 20260111125632](../Fotos/Pasted%20image%2020260111125632.png)
Respuesta: 10.0.2.15
![Pasted image 20260320130345](../Fotos/Pasted%20image%2020260320130345.png)

`transaction.remote_address`
![Pasted image 20260320130431](../Fotos/Pasted%20image%2020260320130431.png)

*Pregunta 2*
¿Cuál era el Agente de Usuario de la herramienta de enumeración de directorios que el atacante utilizaba en el servidor web?
![Pasted image 20260111125934](../Fotos/Pasted%20image%2020260111125934.png)
Respuesta: Nmap scripting engine
![Pasted image 20260320131506](../Fotos/Pasted%20image%2020260320131506.png) ![Pasted image 20260320131532](../Fotos/Pasted%20image%2020260320131532.png)

*Pregunta 3*
¿Cuál era el Agente de Usuario de la herramienta de enumeración de directorios que el atacante utilizaba en el servidor web?
Aqui filtramos por el codigo de estado 404 ya que como es una iteracion probablemente tiene muchos errores
![Pasted image 20260111130530](../Fotos/Pasted%20image%2020260111130530.png)
Respuesta: Mozilla/5.0 (Gobuster)

*Pregunta 4*
En total, ¿cuántos recursos solicitados en el servidor web no encontró el atacante?
![Pasted image 20260111130801](../Fotos/Pasted%20image%2020260111130801.png)
Respuesta: 1867

*Pregunta 5*
¿Cuál es la bandera bajo el directorio interesante que encontró el atacante?
Editar el filtro response.status de 404 a 200
Añade Mozilla/5.0 (Gobuster) al filtro haciendo clic en el signo + que obtenemos al pasar el cursor encima.
![Pasted image 20260111133428](../Fotos/Pasted%20image%2020260111133428.png)
Respuesta: a76637b62ea99acda12f5859313f539a

*Pregunta 6*
¿Qué página de inicio de sesión descubrió el atacante usando la herramienta de enumeración de directorios?
Como no hemos encontrado ninguna página de inicio de sesión al filtrar usando 200,  podemos incluirla como excepción. También, obviamente el 404
/admin-login.php
![Pasted image 20260111134213](../Fotos/Pasted%20image%2020260111134213.png)
Respuesta: /admin-login.php

*Pregunta 7*
¿Cuál era el agente de usuario de la herramienta de fuerza bruta que usó el atacante en el panel de administración?
![Pasted image 20260111135742](../Fotos/Pasted%20image%2020260111135742.png)
Respuesta: Mozilla/4.0 (Hydra)
![Pasted image 20260320133951](../Fotos/Pasted%20image%2020260320133951.png)

*Pregunta 8*
¿Qué combinación de nombre de usuario y contraseña usó el atacante para acceder a la página de administración?
Conocemos el User-Agent de la herramienta de fuerza bruta, agrégalo al filtro. Como el atacante obtuvo acceso, sabemos que la respuesta no será 401,  así que añade eso al filtro.
![Pasted image 20260111162648](../Fotos/Pasted%20image%2020260111162648.png)
![Pasted image 20260111163218](../Fotos/Pasted%20image%2020260111163218.png)
`request.headers.Authorization`
Esta en base64: YWRtaW46dGh4MTEzOA==
Respuesta: admin:thx1138

*Pregunta 9*
¿Qué flag estaba incluido en el archivo que el atacante subió desde el directorio de administradores?
![Pasted image 20260111163805](../Fotos/Pasted%20image%2020260111163805.png)
![Pasted image 20260111163945](../Fotos/Pasted%20image%2020260111163945.png)
![Pasted image 20260111165156](../Fotos/Pasted%20image%2020260111165156.png)
Respuesta: THM{ecb012e53a58818cbd17a924769ec447}

*Pregunta 10*
¿Cuál fue el primer comando que ejecutó el atacante en la web shell?
Buscar: easy-simple-php-webshell.php (este es el archivo subido que pudimos ver)
Aqui ya podemos identificar la vuln es una web shell caragada en php confirmada no solo por el nombre.
![Pasted image 20260111165522](../Fotos/Pasted%20image%2020260111165522.png)
Respuesta: whoami

*Pregunta 11*
¿De qué ubicación de archivo en el servidor web extrajo el atacante las credenciales de la base de datos usando **la Inclusión Local de Archivos**?
Usando la vista que teniamos y viendo el LFI
![Pasted image 20260111170741](../Fotos/Pasted%20image%2020260111170741.png)
Segunda vista filtrando por URL
![Pasted image 20260111170809](../Fotos/Pasted%20image%2020260111170809.png)
Respuesta: /etc/phpmyadmin/config-db.php

*Pregunta 12*
¿Qué **directorio** usó el atacante para acceder al gestor de bases de datos?
Respuesta: /phpmyadmin/

*Pregunta 13*
¿Cuál era el nombre de la base de datos que **exportó** el atacante?
![Pasted image 20260111173013](../Fotos/Pasted%20image%2020260111173013.png)
Hubo muchas solicitudes de las que esperaba.  
Pero pocos de ellos captaron mi atención.  
**[export.php , import.php , tbl_replace.php]**
Como nuestro atacante exportó la base de datos, vamos a **comprobarlo export.php** primero.
![Pasted image 20260111173448](../Fotos/Pasted%20image%2020260111173448.png)
Respuesta: db=customer_credit_cards (customer_credit_cards)

*Pregunta 14*
¿Qué bandera inserta el atacante en la base de datos?
![Pasted image 20260111175457](../Fotos/Pasted%20image%2020260111175457.png)
Respuesta: /phpmyadmin/tbl_replace.php

*URLENCODEADO:*

{"transaction":{"time":"26/Jul/2023:14:34:38 +0000","transaction_id":"ZMEu-vKt6ajeD0PzMR7vPQAAAAc","remote_address":"10.0.2.15","remote_port":53812,"local_address":"10.0.2.4","local_port":80},"request":{"request_line":"POST /phpmyadmin/tbl_replace.php HTTP/1.1","headers":{"Host":"slingway.thm","User-Agent":"Mozilla/5.0 (X11; Linux x86_64; rv:102.0) Gecko/20100101 Firefox/102.0","Accept":"*/*","Accept-Language":"en-US,en;q=0.5","Accept-Encoding":"gzip, deflate","Content-Type":"application/x-www-form-urlencoded; charset=UTF-8","X-Requested-With":"XMLHttpRequest","Content-Length":"1263","Origin":"http://slingway.thm","Connection":"keep-alive","Cookie":"pma_lang=en; pmaUser-1=%7B%22iv%22%3A%22h%2BFmToYQdMIViXsCj7A12g%3D%3D%22%2C%22mac%22%3A%22d6989b2b9c917a030a4679efa20a251ac011e64f%22%2C%22payload%22%3A%22vFA911kd5bZnmSlwnSITmA%3D%3D%22%7D; phpMyAdmin=esa6gngv8pvn5dlot5gubb5ias; pmaAuth-1=%7B%22iv%22%3A%22z2ScJB2BNBtS2B4cNQSN4w%3D%3D%22%2C%22mac%22%3A%220cf7496df089f46fc87332ac608c121b67ad58ce%22%2C%22payload%22%3A%22c7%2Bk1PjjvYgkqzlVg%2BapS%2Bg5Jpta9LAPcrO9AE5R%5C%2F1c%3D%22%7D"},"body":["ajax_request=true&ajax_page_request=true&db=customer_credit_cards&table=credit_cards&goto=tbl_sql.php&err_url=tbl_sql.php%3Fdb%3Dcustomer_credit_cards%26amp%3Btable%3Dcredit_cards&sql_query=&token=302e562342217c5d6258344222294172&fields_name%5Bmulti_edit%5D%5B0%5D%5B4400d338d951ef295f9ce6f8e78015fb%5D=card_number&funcs%5Bmulti_edit%5D%5B0%5D%5B4400d338d951ef295f9ce6f8e78015fb%5D=&fields%5Bmulti_edit%5D%5B0%5D%5B4400d338d951ef295f9ce6f8e78015fb%5D=000&fields_name%5Bmulti_edit%5D%5B0%5D%5B1b8e5adf4b36ee546c8adca1ba7c0141%5D=cardholder_name&funcs%5Bmulti_edit%5D%5B0%5D%5B1b8e5adf4b36ee546c8adca1ba7c0141%5D=&fields%5Bmulti_edit%5D%5B0%5D%5B1b8e5adf4b36ee546c8adca1ba7c0141%5D=**c6aa3215a7d519eeb40a660f3b76e64c**&fields_name%5Bmulti_edit%5D%5B0%5D%5B1a9d849ff5a68997176b6144236806ae%5D=expiration_date&funcs%5Bmulti_edit%5D%5B0%5D%5B1a9d849ff5a68997176b6144236806ae%5D=&fields%5Bmulti_edit%5D%5B0%5D%5B1a9d849ff5a68997176b6144236806ae%5D=000&fields_name%5Bmulti_edit%5D%5B0%5D%5B05f59f175b8961c00305e4ee7c88f9f2%5D=cvv&funcs%5Bmulti_edit%5D%5B0%5D%5B05f59f175b8961c00305e4ee7c88f9f2%5D=&fields%5Bmulti_edit%5D%5B0%5D%5B05f59f175b8961c00305e4ee7c88f9f2%5D=000&submit_type=insert&after_insert=back&_nocache=1690382078218295119&token=302e562342217c5d6258344222294172"]},"response":{"protocol":"HTTP/1.1","status":200,"headers":{"Set-Cookie":"phpMyAdmin=esa6gngv8pvn5dlot5gubb5ias; path=/phpmyadmin/; HttpOnly","Expires":"Wed, 26 Jul 2023 14:34:38 +0000","Cache-Control":"no-store, no-cache, must-revalidate,  pre-check=0, post-check=0, max-age=0","Last-Modified":"Wed, 26 Jul 2023 14:34:38 +0000","Set-Cookie":"back=deleted; expires=Thu, 01-Jan-1970 00:00:01 GMT; Max-Age=0; path=/phpmyadmin/","Set-Cookie":"pmaUser-1=%7B%22iv%22%3A%22riO2GJY1EBtBYnvjq0H%5C%2FTQ%3D%3D%22%2C%22mac%22%3A%22fd72264704405df1a9548cac890a51d662f112ce%22%2C%22payload%22%3A%22%5C%2Fk7YUyet982nF6sPsEloag%3D%3D%22%7D; expires=Fri, 25-Aug-2023 14:34:38 GMT; Max-Age=2592000; path=/phpmyadmin/; HttpOnly","Set-Cookie":"pmaAuth-1=%7B%22iv%22%3A%22le9LYke0%5C%2FrJ0oYnh3IYxHg%3D%3D%22%2C%22mac%22%3A%2202fbd75ce678964befbd37c74e53741d2c914272%22%2C%22payload%22%3A%229lkXCvaHLfc7aTmFMLs52ziT7kB64i2thWbEGYMS6J8%3D%22%7D; path=/phpmyadmin/; HttpOnly","X-ob_mode":"1","Pragma":"no-cache","X-Content-Type-Options":"nosniff","Content-Encoding":"gzip","Vary":"Accept-Encoding","Content-Length":"5047","Keep-Alive":"timeout=5, max=100","Connection":"Keep-Alive","Content-Type":"application/json; charset=UTF-8"}},"audit_data":{}}

*URLDECODEADO:*  A partir de esto podemos entender que el atacante añadió credenciales de tarjeta de crédito

{"transaction":{"time":"26/Jul/2023:14:34:38+0000","transaction_id":"ZMEu-vKt6ajeD0PzMR7vPQAAAAc","remote_address":"10.0.2.15","remote_port":53812,"local_address":"10.0.2.4","local_port":80},"request":{"request_line":"POST/phpmyadmin/tbl_replace.phpHTTP/1.1","headers":{"Host":"slingway.thm","User-Agent":"Mozilla/5.0(X11;Linuxx86_64;rv:102.0)Gecko/20100101Firefox/102.0","Accept":"*/*","Accept-Language":"en-US,en;q=0.5","Accept-Encoding":"gzip,deflate","Content-Type":"application/x-www-form-urlencoded;charset=UTF-8","X-Requested-With":"XMLHttpRequest","Content-Length":"1263","Origin":"http://slingway.thm","Connection":"keep-alive","Cookie":"pma_lang=en;pmaUser-1={"iv":"h+FmToYQdMIViXsCj7A12g==","mac":"d6989b2b9c917a030a4679efa20a251ac011e64f","payload":"vFA911kd5bZnmSlwnSITmA=="};phpMyAdmin=esa6gngv8pvn5dlot5gubb5ias;pmaAuth-1={"iv":"z2ScJB2BNBtS2B4cNQSN4w==","mac":"0cf7496df089f46fc87332ac608c121b67ad58ce","payload":"c7+k1PjjvYgkqzlVg+apS+g5Jpta9LAPcrO9AE5R\/1c="}"},"body":["ajax_request=true&ajax_page_request=true&db=customer_credit_cards&table=credit_cards&goto=tbl_sql.php&err_url=tbl_sql.php?db=customer_credit_cards&amp;table=credit_cards&sql_query=&token=302e562342217c5d6258344222294172&fields_name[multi_edit][0][4400d338d951ef295f9ce6f8e78015fb]=card_number&funcs[multi_edit][0][4400d338d951ef295f9ce6f8e78015fb]=&fields[multi_edit][0][4400d338d951ef295f9ce6f8e78015fb]=000&fields_name[multi_edit][0][1b8e5adf4b36ee546c8adca1ba7c0141]=cardholder_name&funcs[multi_edit][0][1b8e5adf4b36ee546c8adca1ba7c0141]=&fields[multi_edit][0][1b8e5adf4b36ee546c8adca1ba7c0141]=c6aa3215a7d519eeb40a660f3b76e64c&fields_name[multi_edit][0][1a9d849ff5a68997176b6144236806ae]=expiration_date&funcs[multi_edit][0][1a9d849ff5a68997176b6144236806ae]=&fields[multi_edit][0][1a9d849ff5a68997176b6144236806ae]=000&fields_name[multi_edit][0][05f59f175b8961c00305e4ee7c88f9f2]=cvv&funcs[multi_edit][0][05f59f175b8961c00305e4ee7c88f9f2]=&fields[multi_edit][0][05f59f175b8961c00305e4ee7c88f9f2]=000&submit_type=insert&after_insert=back&_nocache=1690382078218295119&token=302e562342217c5d6258344222294172"]},"response":{"protocol":"HTTP/1.1","status":200,"headers":{"Set-Cookie":"phpMyAdmin=esa6gngv8pvn5dlot5gubb5ias;path=/phpmyadmin/;HttpOnly","Expires":"Wed,26Jul202314:34:38+0000","Cache-Control":"no-store,no-cache,must-revalidate,pre-check=0,post-check=0,max-age=0","Last-Modified":"Wed,26Jul202314:34:38+0000","Set-Cookie":"back=deleted;expires=Thu,01-Jan-197000:00:01GMT;Max-Age=0;path=/phpmyadmin/","Set-Cookie":"pmaUser-1={"iv":"riO2GJY1EBtBYnvjq0H\/TQ==","mac":"fd72264704405df1a9548cac890a51d662f112ce","payload":"\/k7YUyet982nF6sPsEloag=="};expires=Fri,25-Aug-202314:34:38GMT;Max-Age=2592000;path=/phpmyadmin/;HttpOnly","Set-Cookie":"pmaAuth-1={"iv":"le9LYke0\/rJ0oYnh3IYxHg==","mac":"02fbd75ce678964befbd37c74e53741d2c914272","payload":"9lkXCvaHLfc7aTmFMLs52ziT7kB64i2thWbEGYMS6J8="};path=/phpmyadmin/;HttpOnly","X-ob_mode":"1","Pragma":"no-cache","X-Content-Type-Options":"nosniff","Content-Encoding":"gzip","Vary":"Accept-Encoding","Content-Length":"5047","Keep-Alive":"timeout=5,max=100","Connection":"Keep-Alive","Content-Type":"application/json;charset=UTF-8"}},"audit_data":{}}

Respuesta: c6aa3215a7d519eeb40a660f3b76e64c