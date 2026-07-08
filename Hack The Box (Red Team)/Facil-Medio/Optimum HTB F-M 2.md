- ---
- Tags: #ejptmachine #rce #metasploit 
--- --
### ¿Qué habilidades puedes practicar con Optimum?

1. **Reconocimiento web:**
    - Comenzarás con **Nmap** para escanear los puertos de la máquina y ver qué servicios están expuestos.
    - Luego, te enfocarás en la **enumeración de directorios** en la aplicación web. Herramientas como **Gobuster** o **Dirbuster** te serán útiles para descubrir archivos o directorios ocultos en el servidor web.
2. **Explotación de vulnerabilidades web:**
    - La máquina se basa en una **inyección SQL** en un **formulario web vulnerable**, lo cual es muy relevante para la **eJPTv2**.
    - Practicarás cómo encontrar y explotar **vulnerabilidades SQLi** manualmente, así como el uso de herramientas como **sqlmap** para automatizar el proceso de explotación.
3. **Escalada de privilegios en Linux:**
    - Después de obtener acceso al sistema, la **escalada de privilegios** es clave. En **Optimum**, tendrás que explorar el sistema operativo **Linux** y buscar configuraciones erróneas que te permitan aumentar tus privilegios, como archivos con permisos incorrectos o vulnerabilidades en configuraciones de **Sudo**.
    - El uso de herramientas como **LinPEAS** o realizar búsquedas manuales de posibles vectores de escalada son fundamentales aquí.

---

`nmap -p- --min-rate 5000 -sCV -n 10.10.10.8 -oN portScan`

![Pasted image 20241129222625](../../Fotos/Pasted%20image%2020241129222625.png)

![Pasted image 20241129223157](../../Fotos/Pasted%20image%2020241129223157.png)

![Pasted image 20241129223857](../../Fotos/Pasted%20image%2020241129223857.png)
 
 CVE_-2014-6287 httpfileserver 2.3 remote code execution
 
![Pasted image 20241129225227](../../Fotos/Pasted%20image%2020241129225227.png)

![Pasted image 20241129225332](../../Fotos/Pasted%20image%2020241129225332.png)

![Pasted image 20241129225340](../../Fotos/Pasted%20image%2020241129225340.png)

![Pasted image 20241130145140](../../Fotos/Pasted%20image%2020241130145140.png)

`Maquina Vulnerada`

![Pasted image 20241129225551](../../Fotos/Pasted%20image%2020241129225551.png)

user flag : d4d0476a3d240d3b0f6c374e9d61fc28

---
*Escalada de Privilegios*

Posibilidades de escalada de privilegios utilizaremos un modulo de metasploit.
En la sesión que tenemos abierta la mandaremos a background y abriremos otra consola de metasploit

![Pasted image 20241129230826](../../Fotos/Pasted%20image%2020241129230826.png)

![Pasted image 20241129230909](../../Fotos/Pasted%20image%2020241129230909.png)

- No obtenemos nada por ahora.

exploit(windows/local/ms16_032_secondary_logon_handle_privesc)
- Este exploit permite el acceso también a la maquina pero la sesión de meterpreter muere muy rapido tras ingresar.

----

![Pasted image 20241129233924](../../Fotos/Pasted%20image%2020241129233924.png)

CORRECCION 

![Pasted image 20241129234132](../../Fotos/Pasted%20image%2020241129234132.png)

- Descargamos el .py

![Pasted image 20241129234237](../../Fotos/Pasted%20image%2020241129234237.png)

- Ponemos la ip de maquina atacante

-----

`windows/local/ms16_032_secondary_logon_handle_privesc`

![Pasted image 20241130150332](../../Fotos/Pasted%20image%2020241130150332.png)

![Pasted image 20241130150721](../../Fotos/Pasted%20image%2020241130150721.png)

![Pasted image 20241130150805](../../Fotos/Pasted%20image%2020241130150805.png)

![Pasted image 20241130150901](../../Fotos/Pasted%20image%2020241130150901.png)

root flag: b13df8e932a7deb474e9b9c52dcd26e5

La escalada d eprivilegios se podia hacer manualmente igualmente.

---

*Preguntas*

Accede a la clave del registro donde se almacenan las credenciales: Usa el siguiente comando para leer la clave del registro relacionada con el inicio de sesión automático:
`reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon"`

![Pasted image 20241130154100](../../Fotos/Pasted%20image%2020241130154100.png)

![Pasted image 20241130154415](../../Fotos/Pasted%20image%2020241130154415.png)

- `AutoAdminLogon` (debería estar en `1` si el inicio de sesión automático está habilitado).
- `DefaultUsername` (debería contener el nombre de usuario `kostas`).
- `DefaultPassword` (contendrá la contraseña si está almacenada allí).

Si `DefaultPassword` está presente, deberías poder ver la contraseña directamente en el valor de esta clave.

---

*Dato 1*
Si logras escalar privilegios a **SYSTEM** o un usuario con privilegios administrativos, podrías cambiar la contraseña de **Kostas** o crear un nuevo usuario para obtener acceso. El comando `net user` te permitirá cambiar la contraseña:
`net user kostas nueva_contraseña`
- Esto cambiará la contraseña del usuario **Kostas** a `nueva_contraseña`.

*Dato 2*
Si tienes acceso a la línea de comandos y **reg query** o **PowerShell** está disponible, puedes buscar directamente en el registro para obtener las credenciales de inicio de sesión automático de **Kostas**. Si tienes privilegios más altos, herramientas como **mimikatz** pueden facilitar la obtención de contraseñas de usuarios.

