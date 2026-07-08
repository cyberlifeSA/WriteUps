- --
- Tags: #cyberdefenders #ulysses_lab #volatility #mount 
- --
Necesitamos en Linux hacer montura o utilizar una herramienta equivalente ya que la idea era utilizar FTK Imager sin embargo por el entorno simplemente hicimos monturas.

El atacante estaba realizando un ataque de fuerza bruta. ¿Qué cuenta activó la alerta?

![Pasted image 20260706204238](../../Fotos/Pasted%20image%2020260706204238.png)

`/var/log/auth.log`

![Pasted image 20260706204213](../../Fotos/Pasted%20image%2020260706204213.png)

**Respuesta:** Ulysses

Durante la investigación de los registros. ¿Cuántos intentos fallidos de inicio de sesión fueron alertados por el mismo usuario?

`/var/log/auth.log`

![Pasted image 20260706204741](../../Fotos/Pasted%20image%2020260706204741.png)

**Respuesta:** 32

¿Qué tipo de sistema se ejecuta en el servidor objetivo?

`/etc/issue`

![Pasted image 20260706204955](../../Fotos/Pasted%20image%2020260706204955.png)

**Respuesta:** Debian GNU/Linux 5.0

¿Cuál es la dirección IP de la víctima?

`/var/lib/dhcp3/dhclient.eth0.leases`

`http://mnt/Linux/var/log/syslog`

![Pasted image 20260706205555](../../Fotos/Pasted%20image%2020260706205555.png)

**Respuesta:** 192.168.56.102

¿Cuáles son las dos direcciones IP del atacante?

En la pregunta 2 vimos multiples intentos de acceso (Brute Force) desde una ip especifica.192.168.56.1

![Pasted image 20260706210936](../../Fotos/Pasted%20image%2020260706210936.png)

`sudo grep -r -F -l '192.168.56.1' /mnt/Linux/var/log`

`cat /mnt/Linux/var/log/exim4/rejectlog`

![Pasted image 20260706211214](../../Fotos/Pasted%20image%2020260706211214.png)

![Pasted image 20260706213131](../../Fotos/Pasted%20image%2020260706213131.png)

**Respuesta:** 192.168.56.1,192.168.56.101

¿Cuál es el número PID del servicio que estaba ejecutándose en el servidor?`nc`

`vol.py --profile=LinuxDebian5_26x86 -f victoria-v8.memdump.img linux_pslis`

![Pasted image 20260706220437](../../Fotos/Pasted%20image%2020260706220437.png)

**Respuesta:** 2169

¿Qué servicio se explotó para acceder al sistema?

`vol.py --profile=LinuxDebian5_26x86 -f victoria-v8.memdump.img linux_bash`

- **apt-get remove exim4** → Elimina el paquete principal de Exim4 (servidor de correo).
- **apt-get remove exim4-base** → Elimina el paquete base de Exim4.
- **apt-get remove exim4-daemon-light** → Elimina la versión ligera del demonio de Exim4.
- **dpkg -l | grep exim** → Lista paquetes instalados relacionados con exim (para verificar).
- **apt-get remove exim4-config** → Elimina la configuración de Exim4.
- **dpkg --purge** → (incompleto en la imagen) Normalmente se usa para purgar (eliminar completamente) paquetes.
- **pwd** → Muestra en qué carpeta estás.
- **apt-get remove exim** → Elimina paquetes restantes de exim.
- **mkdir exim4** → Crea una carpeta llamada exim4.
- **dpkg -i exim4-config_4.69-9_all.deb** → Instala el paquete de configuración de Exim4 desde archivo .deb.
- **cd exim4/** → Entra en la carpeta que creó.
- **scp ...** → Copia archivos desde otra máquina (IP 192.168.56.1) hacia la carpeta actual.
- **dpkg -i exim4-base_4.69-9_i386.deb** → Instala el paquete base (versión 32 bits).
- **dpkg -i --ignore-depends=...** → Instala el paquete principal ignorando dependencias.
- **/etc/init.d/networking restart** → Reinicia el servicio de red.
- **ifconfig** → Muestra configuración de interfaces de red.
- **halt** → Apaga la máquina.
- **apt-get install openssh-server** → Instala el servidor SSH.
- **scp ...** → Copia más archivos desde la otra máquina.
- **dpkg -i exim4-daemon-light_4.69-9_i386.deb** → Instala el demonio ligero de Exim4.
- **rm -rf exim4/** → Borra la carpeta exim4 y todo su contenido.
- **vi .bash_history**, **vi .ssh/known_hosts**, etc. → Edita archivos de historial y configuración SSH.
- **update-exim4.conf** → Actualiza la configuración de Exim4.

![Pasted image 20260706222453](../../Fotos/Pasted%20image%2020260706222453.png)

**Respuesta:** Exim4

¿Cuál es el número CVE de vulnerabilidades explotadas?

IOCs : Puerto 25 - Version de Exim4 - Versión de la distribución Debian - SSH RCE

**Respuesta:** CVE-2010-4344

Durante este ataque, el atacante descargó dos archivos al servidor. Proporciona el nombre del archivo comprimido.

`/var/log/exim4/rejectlog`

![Pasted image 20260706223914](../../Fotos/Pasted%20image%2020260706223914.png)

**Respuesta:** rk.tar

Durante la investigación, dos puertos participaron en el proceso de exfiltración de datos. ¿Qué puerto usó el comando para la exfiltración?`nc`

`vol.py --profile=LinuxDebian5_26x86 -f victoria-v8.memdump.img linux_bash`

![Pasted image 20260706224231](../../Fotos/Pasted%20image%2020260706224231.png)

`vol.py --profile=LinuxDebian5_26x86 -f victoria-v8.memdump.img linux_netstat`

![Pasted image 20260706224326](../../Fotos/Pasted%20image%2020260706224326.png)

**Respuesta:** 8888

¿Qué puerto intentó bloquear el atacante en el cortafuegos?

![Pasted image 20260706224844](../../Fotos/Pasted%20image%2020260706224844.png)

`tar -xvf rk.tar 'rk/install.sh'`

![Pasted image 20260706225004](../../Fotos/Pasted%20image%2020260706225004.png)

![Pasted image 20260706224945](../../Fotos/Pasted%20image%2020260706224945.png)

**Respuesta:** 45295