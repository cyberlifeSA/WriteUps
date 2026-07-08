-- --
- Tags: #ejptmachine #library_path_hijacking
---

![Pasted image 20250125160033](../../Fotos/Pasted%20image%2020250125160033.png)

![Pasted image 20250125160138](../../Fotos/Pasted%20image%2020250125160138.png)

![Pasted image 20250125160257](../../Fotos/Pasted%20image%2020250125160257.png)

![Pasted image 20250125160239](../../Fotos/Pasted%20image%2020250125160239.png)

- JIFGHDS87GYDFIGD

`hydra -L /usr/share/wordlists/rockyou.txt -p JIFGHDS87GYDFIGD ssh://172.17.0.2/ -t 10`

![Pasted image 20250125161357](../../Fotos/Pasted%20image%2020250125161357.png)

- login: carlos   password: JIFGHDS87GYDFIGD

![Pasted image 20250125161457](../../Fotos/Pasted%20image%2020250125161457.png)

![Pasted image 20250125161509](../../Fotos/Pasted%20image%2020250125161509.png)

![Pasted image 20250125162725](../../Fotos/Pasted%20image%2020250125162725.png)

![Pasted image 20250125163210](../../Fotos/Pasted%20image%2020250125163210.png)

- Crearemos otro archivo con el mismo nombre para que podamos escribir en el. 

script.py

![Pasted image 20250125163120](../../Fotos/Pasted%20image%2020250125163120.png)

![Pasted image 20250125163348](../../Fotos/Pasted%20image%2020250125163348.png)

`sudo /usr/bin/python3 /opt/script.py`

- **Es necesario colocar la ruta del binario python3 mas la ruta del script si no lanzara errores siempre probar esto**

![Pasted image 20250125164322](../../Fotos/Pasted%20image%2020250125164322.png)