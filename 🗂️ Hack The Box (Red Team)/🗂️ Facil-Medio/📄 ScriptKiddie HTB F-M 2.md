-- -
- Tags: #ejptmachine #python3 #metasploit 
--- -
### ¿Qué habilidades puedes practicar con ScriptKiddie?

1. **Reconocimiento y escaneo de puertos:**
    - Al igual que en la mayoría de las máquinas de Hack The Box, el primer paso es realizar un **escaneo de puertos** para identificar los servicios y puertos abiertos. Utilizando herramientas como **Nmap** podrás obtener una visión clara de los servicios disponibles, lo que te permitirá planificar tu ataque.
2. **Explotación de vulnerabilidades web:**
    - En **ScriptKiddie**, uno de los principales vectores de ataque es una vulnerabilidad en una aplicación web. Deberás realizar un **reconocimiento web** exhaustivo para identificar posibles **vulnerabilidades** como **inyección SQL (SQLi)**, **cross-site scripting (XSS)**, o **autenticación débil**.
    - Este tipo de vulnerabilidad te permitirá obtener acceso al sistema o **obtener credenciales** que te ayuden en etapas posteriores del proceso.
3. **Escalada de privilegios en Linux:**
    - Una vez que consigas acceso inicial al sistema, tendrás que realizar una **escalada de privilegios** para obtener privilegios más altos en el sistema **Linux**. Buscarás **permisos incorrectos** en archivos importantes o configuraciones erróneas en servicios que puedan permitirte elevar tus privilegios.
    - También es común que se necesite **abuso de configuraciones de Sudo** o el aprovechamiento de **vulnerabilidades locales** para lograr la escalada.

----
### Forma 1

`nmap -p- --min-rate 5000 -n -sCV 10.10.10.226 -oN portScan`
![Pasted image 20241207151713](../../Fotos/Pasted%20image%2020241207151713.png)

![Pasted image 20241207151832](../../Fotos/Pasted%20image%2020241207151832.png)
![Pasted image 20241207151840](../../Fotos/Pasted%20image%2020241207151840.png)

![Pasted image 20241207152947](../../Fotos/Pasted%20image%2020241207152947.png)
set LHOST tun0
![Pasted image 20241207155128](../../Fotos/Pasted%20image%2020241207155128.png)
run
![Pasted image 20241207153104](../../Fotos/Pasted%20image%2020241207153104.png)
genero el archivo
![Pasted image 20241207153317](../../Fotos/Pasted%20image%2020241207153317.png)
![Pasted image 20241207153341](../../Fotos/Pasted%20image%2020241207153341.png)
cargamos
![Pasted image 20241207153612](../../Fotos/Pasted%20image%2020241207153612.png)

![Pasted image 20241207154012](../../Fotos/Pasted%20image%2020241207154012.png)
Tratamiento
![Pasted image 20241207155512](../../Fotos/Pasted%20image%2020241207155512.png)
![Pasted image 20241207155836](../../Fotos/Pasted%20image%2020241207155836.png)

`Maquina Vulnerada`

----

*Escalada de Privilegios*

Meteremos una Reverse Shell en el archivo hackers
Nos ponemos en escucha una vez metamos la reverse shell en el archivo hackers nos generara la consola en la ventana donde estemos en escucha.
![Pasted image 20241207162024](../../Fotos/Pasted%20image%2020241207162024.png)

![Pasted image 20241207162113](../../Fotos/Pasted%20image%2020241207162113.png)

![Pasted image 20241207162150](../../Fotos/Pasted%20image%2020241207162150.png)
- Podemos usar metasploit con sudo

![Pasted image 20241207162353](../../Fotos/Pasted%20image%2020241207162353.png)
![Pasted image 20241207162430](../../Fotos/Pasted%20image%2020241207162430.png)
user flag: c13a4c651adedf7ea8fb93491d2035b1
root flag: 91a8f95178775e5ef64619088b2de350


----

*Preguntas:*

What is the 2020 CVE ID for a command injection vulnerability in `msfvenom`?
CVE-2020-7384