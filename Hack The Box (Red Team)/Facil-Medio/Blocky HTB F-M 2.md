-- -
- Tags: #ejptmachine #wordpress 
--- -
### ¿Qué habilidades puedes practicar con Blocky?

1. **Reconocimiento y escaneo de puertos:**
    - Al igual que en la mayoría de las máquinas de Hack The Box, el primer paso es realizar un **escaneo de puertos** para identificar los servicios y puertos abiertos. Utilizando herramientas como **Nmap** podrás obtener una visión clara de los servicios disponibles, lo que te permitirá planificar tu ataque.
2. **Explotación de vulnerabilidades web:**
    - En **Blocky**, uno de los principales vectores de ataque es una vulnerabilidad en una aplicación web. Deberás realizar un **reconocimiento web** exhaustivo para identificar posibles **vulnerabilidades** como **inyección SQL (SQLi)**, **cross-site scripting (XSS)**, o **autenticación débil**.
    - Este tipo de vulnerabilidad te permitirá obtener acceso al sistema o **obtener credenciales** que te ayuden en etapas posteriores del proceso.
3. **Escalada de privilegios en Linux:**
    - Una vez que consigas acceso inicial al sistema, tendrás que realizar una **escalada de privilegios** para obtener privilegios más altos en el sistema **Linux**. Buscarás **permisos incorrectos** en archivos importantes o configuraciones erróneas en servicios que puedan permitirte elevar tus privilegios.
    - También es común que se necesite **abuso de configuraciones de Sudo** o el aprovechamiento de **vulnerabilidades locales** para lograr la escalada.

----

`nmap -p- --open -sCV --min-rate 5000 -n -vvv 10.10.10.37`

![Pasted image 20241125145149](../../Fotos/Pasted%20image%2020241125145149.png)

`wpscan --url blocky.htb --enumerate --ignore-main-redirect`

![Pasted image 20241125154818](../../Fotos/Pasted%20image%2020241125154818.png)

![Pasted image 20241125154838](../../Fotos/Pasted%20image%2020241125154838.png)

- Tenemos un usuario identificado.

`gobuster dir -u blocky.htb -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt`

![Pasted image 20241125155305](../../Fotos/Pasted%20image%2020241125155305.png)

/plugins

![Pasted image 20241125155754](../../Fotos/Pasted%20image%2020241125155754.png)

Descargamos ambos archivos

![Pasted image 20241130225402](../../Fotos/Pasted%20image%2020241130225402.png)

Descomprimimos blockycore.jar

![Pasted image 20241130225639](../../Fotos/Pasted%20image%2020241130225639.png)

![Pasted image 20241130225833](../../Fotos/Pasted%20image%2020241130225833.png)

![Pasted image 20241130225823](../../Fotos/Pasted%20image%2020241130225823.png)

Vemos en .java

![Pasted image 20241130230557](../../Fotos/Pasted%20image%2020241130230557.png)

localhost
root
8YsqfCTnvxAUeduzjNSXe22

![Pasted image 20241130230807](../../Fotos/Pasted%20image%2020241130230807.png)

- Utilizamos las posibles credenciales encontradas

![Pasted image 20241130230847](../../Fotos/Pasted%20image%2020241130230847.png)

Base de datos
Worpress
wp_users

![Pasted image 20241130231557](../../Fotos/Pasted%20image%2020241130231557.png)

notch
`$P$BiVoTj899ItS1EZnMhqeqVbrZI4Oq0/`
notch@blockcraftfake.com

![Pasted image 20241130231840](../../Fotos/Pasted%20image%2020241130231840.png)

- La clave no nos toma asique usaremos la que empleamos en myadminphp para root
8YsqfCTnvxAUeduzjNSXe22

![Pasted image 20241130231912](../../Fotos/Pasted%20image%2020241130231912.png)

`Maquina Vulnerada`

![Pasted image 20241130231949](../../Fotos/Pasted%20image%2020241130231949.png)

user flag: 1ecd94dbe04fb7d09bc4e0a108cc2576

----

*Escalada de privilegios 1*

`find / -type f -perm -4000 2>dev/null`

![Pasted image 20241130232343](../../Fotos/Pasted%20image%2020241130232343.png)

`sudo -u root /usr/bin/sudo su`

![Pasted image 20241130232451](../../Fotos/Pasted%20image%2020241130232451.png)

![Pasted image 20241130232516](../../Fotos/Pasted%20image%2020241130232516.png)

root flag: fe6f1104c1e0ee5e1138b2f343fec350

----

*Escalada de privilegios 2*

`sudo -i`

![Pasted image 20241130232843](../../Fotos/Pasted%20image%2020241130232843.png)





