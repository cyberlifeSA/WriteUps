- --
- Tags: #splunk #siem 
- - -
*Pregunta 1*
¿Puedes identificar la ubicación del ransomware?
Info: **Paso 1:** Basándonos en la pregunta 2, hemos determinado que se trata de un evento de creación de archivo, así que lo descubrimos en el artículo siguiente ID de evento utilizado para filtrar este tipo de evento.
[https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
Buscamos en el EventoCode=11 en el campo de búsqueda y pulsamos Todo el tiempo en el desplegable de la esquina superior izquierda, luego hacemos clic en la imagen de la sección de campos interesantes. Al mirarla podemos ver la sección resaltada: cmd.exe ejecutable está almacenado en una ubicación extraña.
Filtro: `EventCode=11`
![Pasted image 20260119163712](../Fotos/Pasted%20image%2020260119163712.png)
Respuesta: c:\Users\Administrator\Documents\cmd.exe
![Pasted image 20260322142911](../Fotos/Pasted%20image%2020260322142911.png) ![Pasted image 20260322142847](../Fotos/Pasted%20image%2020260322142847.png)

*Pregunta 2*
¿Cuál es el ID de evento Sysmon para el evento relacionado de creación de archivos?  

Respuesta: 11

*Pregunta 3*
¿Puedes encontrar el hash MD5 del ransomware?  
![Pasted image 20260119164004](../Fotos/Pasted%20image%2020260119164004.png)
Respuesta: 290C7DFB01E50CEA9E19DA81A781AF2C

*Pregunta 4*
¿Qué archivo se guardó en varias carpetas?
Filtro: `Image="c:\\Users\\Administrator\\Documents\\cmd.exe" EventCode=11`
![Pasted image 20260119165332](../Fotos/Pasted%20image%2020260119165332.png)
Respuesta: readme.txt

*Pregunta 5*
¿Cuál fue el comando que usó el atacante para añadir un nuevo usuario al sistema comprometido?
Filtro: `/add*`
![Pasted image 20260119165539](../Fotos/Pasted%20image%2020260119165539.png)
Filtro: `/add*`
![Pasted image 20260119165640](../Fotos/Pasted%20image%2020260119165640.png)
Respuesta: net user /add securityninja hardToHack123$

*Pregunta 6*
El atacante migró el proceso para mejorar la persistencia. ¿Qué es la imagen de proceso migrada (ejecutable) y cuál es la imagen original del proceso (ejecutable) cuando el atacante accedió al sistema?
Info: Podemos usar Sysmon Event ID 8 Create Remote Thread, que detecta cuando un proceso crea un hilo en otro proceso. En la sección de campos interesantes, haz clic en TargetImage y mostrará dos eventos; haz clic en el segundo y encontrarás las ubicaciones de origen y destino como abajo de la instantánea.
Filtro: `EventCode=8`
![Pasted image 20260119170326](../Fotos/Pasted%20image%2020260119170326.png)
Click y filtramos: `EventCode=8 TargetImage="C:\\Windows\\System32\\wbem\\unsecapp.exe"`
![Pasted image 20260119170626](../Fotos/Pasted%20image%2020260119170626.png)
![Pasted image 20260119170606](../Fotos/Pasted%20image%2020260119170606.png)
Respuesta: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe,C:\Windows\System32\wbem\unsecapp.exe

*PowerShell → inyecta código → unsecapp.exe*
![Pasted image 20260323143415](../Fotos/Pasted%20image%2020260323143415.png) ![Pasted image 20260322144947](../Fotos/Pasted%20image%2020260322144947.png) ![Pasted image 20260322145015](../Fotos/Pasted%20image%2020260322145015.png)

*Pregunta 7*
El atacante también recuperó los hashes del sistema. ¿Cuál es la imagen del proceso utilizada para obtener los hashes del sistema?  
Info: Usando solo el filtro EventCode=8, comprueba la Imagen Objetivo y mostrará dos eventos; si nos referimos a la salida de la búsqueda usada en la pregunta 6, podemos ver que se produce una segunda migración de proceso entre unsecapp.exe y lsass.exe
Filtro: `EventCode=8`
![Pasted image 20260119171358](../Fotos/Pasted%20image%2020260119171358.png)
Respuesta: C:\Windows\System32\lsass.exe
![Pasted image 20260322153133](../Fotos/Pasted%20image%2020260322153133.png)

*Pregunta 8*
¿Qué es el web shell que el exploit se desplegó en el sistema?  
Filtro: `*.aspx*`
![Pasted image 20260119171638](../Fotos/Pasted%20image%2020260119171638.png)
Respuesta: i3gfPctK1c2x.aspx

*Pregunta 9*
¿Cuál es la línea de comandos que ejecutó este web shell?  
Filtro: `i3gfPctK1c2x.aspx`
![Pasted image 20260119171807](../Fotos/Pasted%20image%2020260119171807.png)
Respuesta: attrib.exe -r \\\\win-aoqkg2as2q7.bellybear.local\C$\Program Files\Microsoft\Exchange Server\V15\FrontEnd\HttpProxy\owa\auth\i3gfPctK1c2x.aspx

*El comando utiliza attrib.exe con el parámetro -r para eliminar el atributo de solo lectura de un archivo .aspx ubicado en un recurso administrativo remoto (C$), lo que permite al atacante modificar o utilizar un posible webshell en el servidor Exchange.*

✔️ **Sí, la webshell ya existe en el servidor**  
✔️ Este comando es para **habilitar/modificarla**  
✔️ **Sí, el host ya está comprometido**

1. Explota Exchange (RCE)  
2. Sube webshell (.aspx) 👈 YA PASÓ  
3. Ajusta atributos (attrib -r) 👈 ESTÁS AQUÍ  
4. Ejecuta comandos vía webshell  
5. Se mueve lateral / roba credenciales

*Pregunta 10*
¿Qué tres CVEs aprovechó este exploit? Proporciona la respuesta en orden ascendente.
Respuesta: CVE-2018-13374,CVE-2018-13379,CVE-2020-0796

```bash
1. CVE-2018-13374 → lee archivos del sistema  
2. CVE-2018-13379 → roba credenciales VPN  
3. CVE-2020-0796 → ejecuta código en Windows (RCE)
```

🔴 CVE-2018-13374
- Path traversal en Fortinet VPN
🔑 Qué es
- **SSRF (Server-Side Request Forgery)**
- Permite hacer peticiones internas en Exchange
🔗 En tu lab
👉 Es la fase donde el atacante **entra al servidor Exchange sin credenciales**
✔️ Antes de que veas el `.aspx`  
✔️ No siempre es visible en logs simples

✔️ *El atacante gana acceso inicial al Exchange*

----
🔴 CVE-2018-13379
- Robo de credenciales en Fortinet
🔑 Qué es
- **File disclosure** → exposición de archivos sensibles
- Especialmente:
    - credenciales
    - sesiones VPN
🔗 En el lab
👉 El atacante obtiene:
usuarios + contraseñas

✔️ *acceso válido (credenciales reales)*

---
🔴 CVE-2020-0796
- Vulnerabilidad SMB (gusano)
🔑 Qué es
- Vulnerabilidad en SMB
- Permite:
    - ejecución remota (RCE)
    - movimiento lateral
🔗 En el lab
👉 Aquí ya pasa esto:
- ejecución de comandos
- acceso a otros hosts
- posible despliegue de malware

✔️ Fase: **explotación / ejecución**

