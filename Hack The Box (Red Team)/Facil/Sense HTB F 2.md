-- -
- Tags: #ejptmachine #aci #metasploit 
--- --
### ¿Qué habilidades puedes practicar con Sense?

1. **Reconocimiento y escaneo de puertos:**
    - Al igual que en la mayoría de las máquinas de Hack The Box, el primer paso es realizar un **escaneo de puertos** para identificar los servicios y puertos abiertos. Utilizando herramientas como **Nmap** podrás obtener una visión clara de los servicios disponibles, lo que te permitirá planificar tu ataque.
2. **Explotación de vulnerabilidades web:**
    - En **Sense**, uno de los principales vectores de ataque es una vulnerabilidad en una aplicación web. Deberás realizar un **reconocimiento web** exhaustivo para identificar posibles **vulnerabilidades** como **inyección SQL (SQLi)**, **cross-site scripting (XSS)**, o **autenticación débil**.
    - Este tipo de vulnerabilidad te permitirá obtener acceso al sistema o **obtener credenciales** que te ayuden en etapas posteriores del proceso.
3. **Escalada de privilegios en Linux:**
    - Una vez que consigas acceso inicial al sistema, tendrás que realizar una **escalada de privilegios** para obtener privilegios más altos en el sistema **Linux**. Buscarás **permisos incorrectos** en archivos importantes o configuraciones erróneas en servicios que puedan permitirte elevar tus privilegios.
    - También es común que se necesite **abuso de configuraciones de Sudo** o el aprovechamiento de **vulnerabilidades locales** para lograr la escalada.

Vuln: Authenticated Command Injection

---

`nmap -p- -sCV --min-rate 5000 -n 10.10.10.60 -oN portScan`

![Pasted image 20241129172631](../../Fotos/Pasted%20image%2020241129172631.png)

`gobuster dir -u https://10.10.10.60/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -k -x txt `

![Pasted image 20241129175951](../../Fotos/Pasted%20image%2020241129175951.png)

`dirsearch -u https://10.10.10.60 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 500 -f -e txt`

![Pasted image 20241129180017](../../Fotos/Pasted%20image%2020241129180017.png)

![Pasted image 20241129181458](../../Fotos/Pasted%20image%2020241129181458.png)

- changelog.txt
- system-users.txt

![Pasted image 20241129180119](../../Fotos/Pasted%20image%2020241129180119.png)

- rohit
- pfsense ( calve default para servisio Pfsense)

![Pasted image 20241129181404](../../Fotos/Pasted%20image%2020241129181404.png)

- Una vez logeados vemos la version que podria ser interesante.

`searchsploit -m php/webapps/43560.py `

![Pasted image 20241129182253](../../Fotos/Pasted%20image%2020241129182253.png)

![Pasted image 20241129193558](../../Fotos/Pasted%20image%2020241129193558.png)

![Pasted image 20241129193646](../../Fotos/Pasted%20image%2020241129193646.png)

`Maquina Vulnerada`
user flag: 8721327cc232073b40d27d9c17e7348b
root flag: d08c32a5d4f8c8b10e76eb51a69f1a86

CVE-2016-10709
- Remote authenticated users
