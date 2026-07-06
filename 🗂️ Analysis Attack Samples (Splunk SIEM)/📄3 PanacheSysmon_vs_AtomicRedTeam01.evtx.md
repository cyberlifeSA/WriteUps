`C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\AutomatedTestingTools\PanacheSysmon_vs_AtomicRedTeam01.evtx`

```bash
index=attack_lab source=C:\\Logs\\SAMPLES-SPLUNK\\EVTX-ATTACK-SAMPLES-master\\AutomatedTestingTools\\PanacheSysmon_vs_AtomicRedTeam01.evtx
| rex field=Message "(?<RealTime>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}\.\d{3})"
| eval _time=strptime(RealTime,"%Y-%m-%d %H:%M:%S.%3N")
| rex field=Message "(?<Process>[A-Za-z]:\\\\[^\s]+\.exe)"
| rex field=Message "(?<WorkingDir>[A-Za-z]:\\\\[^\s]+\\\\)\s+[A-Z0-9]+\\\\"
| rex field=Message "(?<User>[A-Z0-9]+\\\\[A-Za-z0-9]+)\s+(Low|Medium|High|System)"
| rex field=Message "(?<Integrity>Low|Medium|High|System)"
| rex field=Message "IMPHASH=[^\s]+\s+(?<ParentImage>[A-Za-z]:\\\\[^\s]+\.exe)\s+(?<ParentCommandLine>.+)$"
| rex field=Message "(?<RawCmd>Microsoft Corporation\s+(.+?)\s+[A-Za-z]:\\\\[^\s]+\\\\\s+[A-Z0-9]+\\\\)"
| rex field=Message "(?<ParentCommandLine>C:\\\\Windows\\\\[^\s]+\.exe\s+[-/a-zA-Z0-9\s]+)$"
| eval CommandLine=replace(RawCmd,"Microsoft Corporation\s+","")
| rename ComputerName as Host
| sort 0 + _time
| eval Time=strftime(_time,"%Y-%m-%d %H:%M:%S.%3N")
| table Time ParentImage ParentCommandLine Host User WorkingDir Integrity Process CommandLine Message
```

![Pasted image 20260426122409](../Fotos/Pasted%20image%2020260426122409.png)
- **svchost.exe** → proceso legítimo de Windows (Service Host)
- **consent.exe** → proceso de UAC (User Account Control prompt)

👉 Algo intentó **elevar privilegios (UAC prompt)**

`Appinfo`: 👉 Gestiona solicitudes de elevación de privilegios (UAC)
Ejemplos:
- ejecutar como administrador
- mostrar prompt de consentimiento
- validar permisos elevados

`consent.exe`
- muestra el popup de “¿Permitir que esta app haga cambios?”
- solicita credenciales o confirmación
- gestiona elevación de privilegios

Integrity `system` : ✔ Significa que **el sistema operativo está ejecutando ese proceso**

T1548.002 – Bypass UAC
- Uso de `consent.exe`
- Elevación de privilegios mediante UAC prompt manipulation

![Pasted image 20260426122825](../Fotos/Pasted%20image%2020260426122825.png)
![Pasted image 20260426122838](../Fotos/Pasted%20image%2020260426122838.png)
- **explorer.exe** → shell del usuario (GUI)
- **cmd.exe** → intérprete de comandos

👉 Usuario `IEUser` ejecuta `cmd.exe` desde `explorer.exe`

T1059.003 – Windows Command Shell
- Ejecución de `cmd.exe`
T1204 – User Execution
- Usuario ejecuta manualmente cmd desde explorer


![Pasted image 20260426131050](../Fotos/Pasted%20image%2020260426131050.png)
🔥 Windows está iniciando un “servicio”

- ejecución automática al iniciar Windows
- persistencia (persistency)
- ejecución en contexto SYSTEM
- evasión de usuario

T1050 – New Service
- creación de servicios nuevos en Windows
- usado para persistencia o ejecución automática

![Pasted image 20260426150309](../Fotos/Pasted%20image%2020260426150309.png)
👉 PowerShell ejecutándose en el sistema
👉 carga o uso de una DLL desde TEMP
**PowerShell + DLL en TEMP = patrón de ataque**


![Pasted image 20260426153009](../Fotos/Pasted%20image%2020260426153009.png)
![Pasted image 20260426153026](../Fotos/Pasted%20image%2020260426153026.png)
Se está creando un servicio nuevo en Windows 

`sc.exe create AtomicTestService`
👉 Esto:
- registra servicio en Windows
- define binario a ejecutar
`AtomicService.exe`
👉 Este será el ejecutable del servicio

👉 es **creación de persistencia en Windows**


![Pasted image 20260426151643](../Fotos/Pasted%20image%2020260426151643.png)
👉 Define **qué ejecutable lanza el servicio** (🔥 **El sistema está configurando un servicio de Windows y definiendo qué ejecutable va a lanzar**)
💥 En este caso:
- el servicio “AtomicTestService”
- ejecutará `AtomicService.exe`


![Pasted image 20260426151709](../Fotos/Pasted%20image%2020260426151709.png)
Controla cómo inicia el servicio:

|Valor|Significado|
|---|---|
|2|Auto Start|
|3|Manual|
|4|Disabled|
🔥 Se está configurando un servicio en Windows

T1543.003 – Create or Modify System Process (Windows Service)

🔥 Se está creando/modificando un servicio de Windows para persistencia, definiendo su ejecutable (AtomicService.exe) y su modo de inicio
🔥 Se creó/modificó un servicio de Windows que apunta a un ejecutable controlado por el atacante, pero que **NO se ejecuta automáticamente**.
Persistencia no es “se ejecuta solo”, es “queda instalado para ejecutarse después”


![Pasted image 20260426132913](../Fotos/Pasted%20image%2020260426132913.png)
![Pasted image 20260426132930](../Fotos/Pasted%20image%2020260426132930.png)

![Pasted image 20260426135633](../Fotos/Pasted%20image%2020260426135633.png)
sc.exe - herramienta para controlar servicios de Windows
Permite:
- iniciar servicios (`start`)
- detener servicios (`stop`)
- crear servicios (`create`)
- consultar servicios (`query`)

🔥 “Inicia un servicio llamado AtomicTestService”

🔴 Observaciones clave:
- ejecutado desde CMD (no GUI)
- usa `sc.exe` (herramienta legítima pero abusable)
- servicio externo (`AtomicTestService`)
- contexto usuario IEUser (no SYSTEM directamente aquí)

![Pasted image 20260427082736](../Fotos/Pasted%20image%2020260427082736.png)
🔥 El servicio fue utilizado como mecanismo de ejecución temporal: se inicia, se detiene y luego se elimina completamente del sistema

![Pasted image 20260427083304](../Fotos/Pasted%20image%2020260427083304.png)
*Se intuye*
1. Se crea persistencia (Run key)
2. Se ejecuta lo necesario
3. Se elimina la persistencia

![Pasted image 20260427084114](../Fotos/Pasted%20image%2020260427084114.png)
 ⚙️ `/v "Atomic Red Team"`
👉 nombre del valor
⚙️ `/d C:\Path\AtomicRedTeam.exe`
👉 programa que se ejecutará
⚙️ `/t REG_SZ`
👉 tipo string

🔥Se creó una clave de registro para ejecutar automáticamente un binario al iniciar sesión (persistencia por usuario)
🔥 el proceso se está ejecutando con **nivel de integridad SYSTEM (máximo privilegio)**
¿Por qué aparece como System?
Elevación de privilegios previa
- UAC
- ejecución como admin

![Pasted image 20260427094215](../Fotos/Pasted%20image%2020260427094215.png)
🔥 Se está **compilando código C# (T1121.cs) para generar una DLL en tiempo real**

⚙️ `csc.exe`
👉 **C# Compiler**
compila código fuente `.cs`
⚙️ `/target:library`
👉 salida:
🔥 genera una **DLL** (no un .exe)
 ⚙️ `/r:System.EnterpriseServices.dll`
👉 referencia a librería
necesaria para funcionalidades COM (muy importante aquí)

🔴 **generación de payload en tiempo de ejecución**
🔥 el atacante NO trae el binario listo  
🔥 lo crea en la máquina víctima


![Pasted image 20260427085549](../Fotos/Pasted%20image%2020260427085549.png)
`HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnceEx`
🔥 ejecuta cosas **una sola vez** en el siguiente inicio
👉 persistencia a nivel **sistema completo**
🔥 **Registra una DLL para ejecutarse automáticamente una sola vez mediante RunOnceEx**

🔴 Se está configurando la ejecución automática de una DLL en el próximo arranque del sistema

![Pasted image 20260427093632](../Fotos/Pasted%20image%2020260427093632.png)
 ¿QUÉ ES `regasm.exe`?
👉 **.NET Assembly Registration Utility**
Sirve para **registrar o desregistrar DLLs .NET** en el sistema (COM)
`/U`
👉 **Unregister**
🔴 **Desregistra la DLL del sistema**
🔥 Se está **eliminando el registro de la DLL `T1121.dll`** del sistema
🔥 El objetivo de eliminar (o desregistrar) la DLL es **borrar rastros y evitar que siga activa en el sistema** Sin mayor contexto por ahora

![Pasted image 20260427101533](../Fotos/Pasted%20image%2020260427101533.png)
🔥 Si ves:
- `regasm /U`
- `del .dll`
💥 **fase de cleanup completa del payload**

----------------------------
![Pasted image 20260427102704](../Fotos/Pasted%20image%2020260427102704.png)
🔥 el atacante está probando múltiples formas de ejecutar la DLL

![Pasted image 20260427103615](../Fotos/Pasted%20image%2020260427103615.png)
🔥 Engaña al usuario con un popup  
🔥 el usuario escribe su contraseña  
🔥 el script la roba
🔴 **T1056.001 – Keylogging / Credential Prompting**
Ejemplo:
```bash
powershell -command {
 $cred = $host.UI.PromptForCredential("Windows Security Update","","","")
 $cred.GetNetworkCredential().Password
}
```
![Pasted image 20260427140701](../Fotos/Pasted%20image%2020260427140701.png)

![Pasted image 20260427141324](../Fotos/Pasted%20image%2020260427141324.png)
1 → 🔥 ACTIVADO
“inyección de DLL global en procesos → persistencia + evasión”
Se configuró ejecución automática de DLLs mediante AppInit → persistencia + ejecución

🔹 **AppInit_DLLs**
👉 Es un **mecanismo de Windows** que permite:
**cargar DLLs automáticamente dentro de procesos**

```bash
LoadAppInit_DLLs = 1  
AppInit_DLLs = C:\Tools\MessageBox64.dll
```
👉 🔥 AHÍ sí:
- hay payload (DLL)
- hay mecanismo activo
👉 ✔️ persistencia real

![Pasted image 20260427142746](../Fotos/Pasted%20image%2020260427142746.png)
🔹 `reg.exe import`
👉 importa un archivo `.reg` al registro
**cargando configuraciones de registro predefinidas desde un archivo externo**

-----------
1. *Preparación (Impact / Defense Evasion)*
![Pasted image 20260427143829](../Fotos/Pasted%20image%2020260427143829.png)
🔥 1. `vssadmin.exe delete shadows /all /quiet`

👉 **Qué es:**

- `vssadmin` = herramienta de **Volume Shadow Copy Service**
- maneja snapshots (copias de respaldo del sistema)

👉 **Qué hace:**

- elimina **TODAS las copias de sombra**
- `/quiet` = sin mostrar nada al usuario

👉 **Objetivo:**  
💥 **Impedir recuperación de archivos**

👉 **Traducción mental rápida (clave CDSA):**

“El atacante está destruyendo backups locales”

👉 **Técnica MITRE:**

- **T1490 – Inhibit System Recovery**

Luego de este evento saltaron estas dos , mas abajo hay mas detalle
![Pasted image 20260427150239](../Fotos/Pasted%20image%2020260427150239.png)

----

![Pasted image 20260427144541](../Fotos/Pasted%20image%2020260427144541.png)
🔥 2. `wbadmin.exe delete catalog -quiet`

👉 **Qué es:**

- `wbadmin` = herramienta de backups de Windows

👉 **Qué hace:**

- borra el catálogo de backups del sistema

👉 **Objetivo:**  
💥 eliminar historial de backups → no restaurable

👉 **Otra vez:**

- **T1490 – Inhibit System Recovery**

![Pasted image 20260427145716](../Fotos/Pasted%20image%2020260427145716.png)
🔥 3. `bcdedit.exe /set {default} recoveryenabled no`

👉 **Qué es:**

- `bcdedit` = Boot Configuration Data Editor

👉 **Qué hace:**

- desactiva el entorno de recuperación de Windows

👉 **Objetivo:**  
💥 impedir que el sistema arranque en modo recuperación

![Pasted image 20260427145839](../Fotos/Pasted%20image%2020260427145839.png)
🔥 4. `bcdedit.exe /set {default} bootstatuspolicy ignoreallfailures`

👉 **Qué hace:**

- evita que Windows muestre errores de arranque

👉 **Objetivo:**  
💥 ocultar fallos + evitar modos de recuperación

🔴 T1490 – Inhibit System Recovery

Cuando veas:
- `vssadmin delete`
- `wbadmin delete`
- `bcdedit recoveryenabled no`
👉 AUTOMÁTICO:
“Esto es ransomware behavior o preparación para destrucción”

Después de `vssadmin` aparecen:

- `vds.exe`
- `wbengine.exe`

👉 NO son malware  
👉 son **servicios legítimos reaccionando al cambio**

💡 Eso confirma que:

- el comando **sí tuvo efecto real en el sistema**

----
2. *Limpieza de evidencias*
![Pasted image 20260428092723](../Fotos/Pasted%20image%2020260428092723.png)
 ❌ `del file.txt`
- elimina referencia del archivo
- se puede recuperar (forense)
 ✔ `sdelete file.txt`
- sobrescribe el disco
- **no se puede recuperar fácilmente**

💥 **El atacante está borrando evidencia de forma segura**
👉 **T1070 – Indicator Removal on Host**
Subtécnica:
- **T1070.004 – File Deletion**

3. *Descarga payload
![Pasted image 20260428093613](../Fotos/Pasted%20image%2020260428093613.png)
 ¿Qué es `bitsadmin.exe`?
👉 **BITS (Background Intelligent Transfer Service)**  
Herramienta de Windows BITS
📌 Función legítima:
- descargar/subir archivos en segundo plano
- usada por Windows Update

👉 Traducción directa:
- descarga un archivo desde internet (GitHub)
- lo guarda como:
`C:\Windows\Temp\bitsadmin_flag.ps1`
Aunque aquí sea un `.md` disfrazado como `.ps1`, lo importante es el comportamiento.
👉 **T1105 – Ingress Tool Transfer**
 Significado:
- traer herramientas o payloads al sistema comprometido

![Pasted image 20260428094706](../Fotos/Pasted%20image%2020260428094706.png)
👉 Cuando termine la descarga:
- ejecuta `notepad.exe`
- con argumento `bitsadmin_flag.ps1`
📌 Técnica:
- **T1197 – BITS Persistence / Execution**

![Pasted image 20260428100946](../Fotos/Pasted%20image%2020260428100946.png)
📌 Qué es esto:
- `cscript.exe` → ejecuta VBScript
- `pubprn.vbs` → script legítimo de Windows
- `script:` → carga remoto
👉 Esto es:  
**Execution vía script remoto**
📌 MITRE:
- **T1216 – Signed Script Proxy Execution**

*Para este puento*
En tu caso, highlights:
- `vssadmin`
- `wbadmin`
- `bcdedit`
- `bitsadmin`
- `cscript`
- `sdelete`
👉 Ya con eso sabes:  
**esto es 100% malicioso o simulación de ataque**

![Pasted image 20260428142715](../Fotos/Pasted%20image%2020260428142715.png)
🔥“Conéctate al disco C de la máquina Target usando credenciales de administrador”

![Pasted image 20260428144103](../Fotos/Pasted%20image%2020260428144103.png)Esto hace:
`dir c:\ /b /s .key`
- Busca archivos recursivamente (`/s`)
- En todo el disco `C:\`
- Formato simple (`/b`)
- Que contengan `.key`
`| findstr /e .key`
- Filtra resultados que **terminan en `.key`**


Multiples
![Pasted image 20260428151038](../Fotos/Pasted%20image%2020260428151038.png)
`reg query <ruta_del_registro>`
🔎 Significa:
👉 Consulta (read) claves del registro de Windows
- `reg` = herramienta de registro
- `query` = consultar / leer

RunOnce / RunOnceEx
```bash
HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce
HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnceEx
```
👉 ¿Qué es esto?
- Programas que se ejecutan **una vez al iniciar**
- Usado para:
    - instalación
    - malware
    - persistencia

```bash
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\ShellServiceObjectDelayLoad
```
👉 Esto permite:
- cargar DLLs al iniciar Windows
💡 Muy usado en:
**persistencia stealth (más sigilosa)**

```bash
HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Shell  
HKCU\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Shell
```
👉 Normalmente:
explorer.exe
⚠️ Si ves otra cosa:  
👉 malware ejecutándose al loguearse

```bash
HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Userinit
```
👉 Normal:
userinit.exe
⚠️ Si está modificado:  
👉 ejecución automática de malware

✔ `reg query ...Run / RunOnce / Winlogon / Services`  
👉 Búsqueda de **persistencia** (programas que se ejecutan al inicio)

✔ `reg query ...Explorer\Run / Policies`  
👉 Más lugares donde malware puede esconderse

✔ `Winlogon\Shell` y `Userinit`  
👉 Clásico para detectar si alguien cambió el proceso de login

Está **enumerando (revisando) claves del registro asociadas al arranque de Windows** para identificar **mecanismos de persistencia existentes o posibles puntos donde ejecutar código automáticamente**.

--------------
![Pasted image 20260428151729](../Fotos/Pasted%20image%2020260428151729.png)
👉 **`reg save HKLM\Security security.hive`**
🧠 Traducción tipo profe:
- **`reg save`** → comando para **guardar una copia del registro**
- **`HKLM\Security`** → hive del registro que contiene info sensible (credenciales, políticas de seguridad)
- **`security.hive`** → archivo donde lo guarda

➡️ Está **extrayendo información sensible del sistema**  
➡️ Esto se usa para **credential dumping (robo de credenciales)**

👉 **El atacante enumera mecanismos de persistencia en el registro y luego extrae credenciales guardando el hive de seguridad.**

--------

![Pasted image 20260428213012](../Fotos/Pasted%20image%2020260428213012.png)
🔹 `reg save HKLM\SAM sam.hive`
- **SAM (Security Account Manager)**  
    👉 Base de datos donde Windows guarda **hashes de contraseñas locales**
🔹 `reg save HKLM\SYSTEM system.hive`
- **SYSTEM hive**  
    👉 Contiene la **boot key** (clave necesaria para descifrar los hashes del SAM)
👉 **El atacante está volcando los hives SAM y SYSTEM para extraer hashes de contraseñas y poder crackearlas offline.**
🚨 **ALTAMENTE MALICIOSO**

![Pasted image 20260428213905](../Fotos/Pasted%20image%2020260428213905.png)
`cmd.exe /c "dir c: /b /s .docx | findstr /e .docx"`
📚 Desglose rápido
- **`cmd.exe /c`**
    - Ejecuta el comando y termina
    - `/c = run and exit`
- **`dir c: /b /s .docx`**
    - `dir` = listar archivos
    - `/s` = busca recursivamente (todo el disco)
    - `/b` = formato simple (solo rutas)
    - `.docx` = filtro por archivos Word

👉 **Está buscando documentos Word en todo el sistema para posible recolección de información sensible.**

![Pasted image 20260428215318](../Fotos/Pasted%20image%2020260428215318.png)
👉 **Ejecuta una búsqueda recursiva de archivos `.docx` en todo el disco usando un entorno de cmd controlado y sin interferencias.**

![971](../Fotos/Pasted%20image%2020260428214929.png)
📚 Desglose tipo examen
- **`for /R c:`**
    - Recorre recursivamente todo el disco C  
        👉 loop recursivo
- **`%%f`**
    - Variable que representa cada archivo encontrado
- **`in (*.docx)`**
    - Filtra archivos Word
- **`do copy %%f c:\temp\`**
    - Copia cada archivo encontrado a `C:\temp`
 “Por cada archivo .docx en todo el disco, cópialo a una carpeta centralizada”

---------
![Pasted image 20260429120919](../Fotos/Pasted%20image%2020260429120919.png) ![Pasted image 20260429121827](../Fotos/Pasted%20image%2020260429121827.png)
`HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options`
👉 Se usa para:
- Debugging legítimo ❗
- **Interceptar ejecución de programas** ⚠️
👉 Está creando esto:
osk.exe → Debugger = cmd.exe
💥“Cada vez que se ejecute `osk.exe`, en realidad ejecuta `cmd.exe`”

Qué es `osk.exe`?
- **On-Screen Keyboard (teclado en pantalla)**

👉 **Está secuestrando `osk.exe` para que ejecute `cmd.exe`, permitiendo acceso privilegiado.**

👉 Esto es una técnica de:
- **Persistence** (persistencia)
- **Privilege Escalation** (escalada de privilegios)

- Atacante modifica IFEO
- En login screen abre teclado (`osk.exe`)
- En vez de teclado → **cmd.exe como SYSTEM**
- 🔓 Control total del sistema

👉 **Cuando alguien intenta ejecutar `osk.exe`**, Windows:
en vez de abrir el teclado en pantalla → ejecuta `cmd.exe`
- Si alguien abre el teclado normalmente ✔
- Si lo abren desde la pantalla de login 🔥

👉 Y aquí está lo peligroso:
En la pantalla de login:
- `osk.exe` puede ejecutarse
- corre con privilegios altos (SYSTEM)

🔥 ¿Por qué esto es crítico?
👉 Porque estos binarios:
- Se pueden ejecutar **SIN iniciar sesión**
- Corren con privilegios **SYSTEM**

![Pasted image 20260429122558](../Fotos/Pasted%20image%2020260429122558.png)
📚 Término clave: `sethc.exe`
👉 **`sethc.exe` (Sticky Keys / Teclas especiales)**
- Función de accesibilidad
- Se activa presionando **Shift 5 veces seguidas**
“Cuando alguien presione Shift 5 veces, en vez de Sticky Keys → abre cmd.exe”
🔥 Por qué esto es MÁS peligroso que `osk.exe`
👉 Porque es más fácil de activar:
- No necesitas hacer clic
- Solo presionas **Shift 5 veces** en login

👉 **Está secuestrando Sticky Keys para abrir una consola con privilegios SYSTEM desde la pantalla de login.**

![Pasted image 20260429123041](../Fotos/Pasted%20image%2020260429123041.png)
📚 Término clave: `utilman.exe`
👉 **`utilman.exe` (Utility Manager / Administrador de accesibilidad)**
- Botón de accesibilidad en la pantalla de login
- Permite abrir herramientas como:
    - teclado en pantalla
    - lupa
    - narrador

“Cuando alguien haga clic en el botón de accesibilidad en login, en vez de abrir utilman → abre cmd.exe”

![Pasted image 20260429123216](../Fotos/Pasted%20image%2020260429123216.png)
📚 Término: `magnify.exe`
👉 **`magnify.exe` (Magnifier / Lupa de Windows)**
- Herramienta de accesibilidad
- Se puede lanzar desde la pantalla de login
- Corre con privilegios elevados (SYSTEM en ese contexto)

👉 Está agregando **otro punto de entrada (backdoor)** antes del login
Ahora tiene:
- `sethc.exe` → Shift x5
- `utilman.exe` → botón accesibilidad
- `osk.exe` → teclado pantalla
- `magnify.exe` → lupa

![Pasted image 20260429163243](../Fotos/Pasted%20image%2020260429163243.png)
📚 Término: `narrator.exe`
👉 **`narrator.exe` (Narrador de Windows)**
- Herramienta de accesibilidad (lee texto en voz alta)
- Disponible en la pantalla de login
- Puede ejecutarse con privilegios elevados (SYSTEM en ese contexto)

👉 **Está creando múltiples puntos de acceso SYSTEM pre-login abusando de herramientas de accesibilidad mediante IFEO.**

![Pasted image 20260429163339](../Fotos/Pasted%20image%2020260429163339.png)
👉 **Image File Execution Options (IFEO)**

Todos esos programas tienen algo en común:
👉 Se pueden ejecutar desde la pantalla de login (Winlogon)

|Binario|Función|Cómo se activa|
|---|---|---|
|`sethc.exe`|Sticky Keys|Shift 5 veces|
|`utilman.exe`|Accessibility|Botón abajo derecha|
|`osk.exe`|Teclado en pantalla|Botón accesibilidad|
|`magnify.exe`|Lupa|Accesibilidad|
|`narrator.exe`|Narrador|Accesibilidad|
|`DisplaySwitch.exe`|Pantallas|(menos común)|
👉 Está probando múltiples vectores para **obtener una shell SYSTEM antes del login**

Porque:
- Esos binarios corren como **SYSTEM**
- No requieren autenticación
- Son accesibles desde la pantalla de login

👉 _Persistence + Privilege Escalation mediante IFEO hijacking de binarios accesibles en Winlogon_

![Pasted image 20260429170920](../Fotos/Pasted%20image%2020260429170920.png)
📚 `atbroker.exe`
👉 **Nombre completo:**
- **Inglés:** _Assistive Technology Broker_
- **Español:** Broker de tecnologías de asistencia

🧠 ¿Qué es?

👉 Es un proceso de Windows que:
- Gestiona herramientas de accesibilidad
- Actúa como “intermediario” entre el sistema y apps como:
    - Narrador (`narrator.exe`)
    - Lupa (`magnify.exe`)
    - Teclado (`osk.exe`)

👉 No es una herramienta visible directa como las otras  
👉 Es más “interno”
- También puede activarse desde la pantalla de login indirectamente

--------------
![Pasted image 20260430094534](../Fotos/Pasted%20image%2020260430094534.png)
👉 **Ejecuta código remoto usando `msxsl.exe` descargando XML/XSL desde Internet.**

![Pasted image 20260430100153](../Fotos/Pasted%20image%2020260430100153.png)
**Uso de `wmic.exe`**
- Antes: solo consultas de registro y archivos.
- Ahora:    
```
wmic.exe process /FORMAT:list
```
👉 Está enumerando procesos.

![Pasted image 20260430100334](../Fotos/Pasted%20image%2020260430100334.png)
👉 Está ejecutando código remoto mediante `wmic` usando un script XSL para evadir controles.

🔥 Indicadores claros de malicia
- `/FORMAT:` con URL ❌ (NO normal)
- XSL remoto ❌
- Uso de binario legítimo (`wmic`) ✔️ (LOLBIN)
- AtomicRedTeam path ✔️ (simulación adversaria)

![Pasted image 20260430101022](../Fotos/Pasted%20image%2020260430101022.png)
👉 **Qué hace (1 frase):** Enumera dominios en la red (reconocimiento de red).

![Pasted image 20260430101040](../Fotos/Pasted%20image%2020260430101040.png)
👉 **Qué hace (1 frase):** Lista equipos compartidos en la red (descubrimiento lateral).

👉 **El atacante pasa de persistencia local a reconocimiento de red y ejecución remota usando LOLBins.**

![Pasted image 20260430102133](../Fotos/Pasted%20image%2020260430102133.png)
![Pasted image 20260430102222](../Fotos/Pasted%20image%2020260430102222.png)
👉 Envía **1 paquete ICMP** a la IP `192.168.1.10` y espera máximo **100 ms** por respuesta.
👉 **Está escaneando la red interna para descubrir hosts activos.**

![Pasted image 20260430105114](../Fotos/Pasted%20image%2020260430105114.png)
👉 “Muéstrame qué dispositivos conozco en la red sin tener que escanear.”

![Pasted image 20260430105457](../Fotos/Pasted%20image%2020260430105457.png)
👉 **regsvr32.exe** = herramienta legítima de Windows para registrar DLLs  
👉 Pero aquí se está usando para **ejecutar un script (.sct)**
- `regsvr32.exe` → binario legítimo (LOLBin)
- `/i:<archivo>` → ejecuta script pasado como parámetro
- `.sct` → **Scriptlet (Script Component)**
- `scrobj.dll` → motor que interpreta scripts
MITRE: 👉 T1218.010

![Pasted image 20260430105601](../Fotos/Pasted%20image%2020260430105601.png)
🚨 Evidencia de ejecución real
Fíjate en esto:
```
C:\Windows\System32\calc.exe
```
👉 Eso significa:
🧠 **El script .sct ejecutó calc.exe**
👉 **El archivo RegSvr32.sct contiene código que se ejecutó exitosamente**

![Pasted image 20260430110220](../Fotos/Pasted%20image%2020260430110220.png)
🧠 ¿Qué cambia respecto al anterior?
Antes:
- Ejecutabas un `.sct` **local**
Ahora:  
👉 Ejecutas un `.sct` **remoto desde Internet** 🔥
👉 “Descarga y ejecuta código desde Internet usando un binario legítimo.”
MITRE: 👉 T1218.010

- regsvr32.exe se ejecuta
- Ve el parámetro `/i:https://...`
- Hace una **request HTTP/HTTPS**
- Descarga el `.sct` (script)
- Lo pasa a scrobj.dll
- 👉 El script se **interpreta y ejecuta en memoria**

- `scrobj.dll` = motor (como un navegador)
- `.sct` = código (como una página web con JS)
👉 El motor **ejecuta**, pero no “guarda” el código dentro de sí

👉 Está en el `.sct`:
- Puede venir de disco
- Puede venir de Internet
- Se carga en memoria
- Se interpreta (VBScript/JScript)

🔥 Por qué parece “fileless”
Porque:
- No necesitas `.exe`
- No modificas binarios del sistema
- Solo ejecutas **código interpretado en memoria**

calc.exe -> 👉 “Demostrar que logré ejecutar código.”

![Pasted image 20260430111837](../Fotos/Pasted%20image%2020260430111837.png)
🧠 ¿Qué hace?
👉 Registra (o ejecuta) una DLL en el sistema usando  
regsvr32.exe
📚 Desglose rápido
- `syswow64\regsvr32.exe` → versión **32-bit**
- `/s` → modo silencioso (sin ventanas)
- `AllTheThingsx86.dll` → DLL a cargar

![Pasted image 20260430112712](../Fotos/Pasted%20image%2020260430112712.png)
👉 Está **comprobando la arquitectura del sistema**

![Pasted image 20260430113937](../Fotos/Pasted%20image%2020260430113937.png)
📚 Qué es exactamente
👉 `UserInitMprLogonScript` =  
**variable de entorno especial que Windows evalúa durante el logon**

🔍 Qué hace Windows internamente
Cuando el usuario inicia sesión:
1. Windows carga variables de entorno (`HKCU\Environment`)
2. Detecta `UserInitMprLogonScript`
3. 👉 Ejecuta el contenido como script/comando

👉 “Se configura un comando en el registro para ejecutarse automáticamente en cada inicio de sesión del usuario.”

![Pasted image 20260430114116](../Fotos/Pasted%20image%2020260430114116.png)
🧠 ¿Qué hace?
👉 Usa WinRAR (comando `rar`) para:
- `a` → **add** (crear archivo comprimido)
- `-r` → **recursivo** (incluye subcarpetas)
- `exfilthis.rar` → archivo destino
- `*.docx` → todos los documentos Word

🧠 Traducción operativa
👉 “Busca todos los .docx y los empaqueta en un solo archivo.”

👉 Para **exfiltrar datos más fácilmente**
En vez de enviar:
- 1000 archivos sueltos ❌
Envía:
- 1 archivo comprimido ✅

👉 “El atacante está agrupando documentos para facilitar su exfiltración.”

![Pasted image 20260430114411](../Fotos/Pasted%20image%2020260430114411.png)
🧠 ¿Qué hace cada uno?

1️⃣ `-encode`
👉 Convierte un binario a **Base64 (texto)**
- `c:\file.exe` → archivo original
- `file.txt` → versión codificada
📌 Resultado: un `.txt` que contiene el `.exe` en texto

 2️⃣ `-decode`
👉 Hace lo contrario
- `file.txt` → Base64
- `c:\file.exe` → reconstruye el ejecutable

🧠 Traducción operativa

👉 “Transformo un ejecutable en texto y luego lo vuelvo a armar.”

🎯 1. Evasión
- Algunos controles bloquean `.exe` ❌
- Pero permiten `.txt` ✅

👉 entonces:
- envío `.txt`
- lo reconstruyo en destino

🎯 2. Exfiltración encubierta
- Sacas datos como texto
- Más difícil de inspeccionar

🎯 3. Transferencia de payloads

- Descargar payload en texto
- Reconstruir localmente

⚠️ Red flags claras
- `certutil` con:
    - `-encode`
    - `-decode`
- Uso fuera de contexto de certificados
- Conversión `.exe → .txt`
👉 altamente sospechoso

 🧠 Tipo de técnica
👉 T1027  
👉 T1105

👉 El atacante (o en este caso Red Canary con Atomic Red Team) está comprobando:
- que puede codificar ✔️
- que puede reconstruir ✔️
- que no rompe el archivo ✔️

![Pasted image 20260430120655](../Fotos/Pasted%20image%2020260430120655.png)
🔴 Qué ocurre (resumen rápido)
1. Copian certutil.exe a `%TEMP%`
2. Lo renombran a algo raro (`tcm.tmp`)
3. Lo ejecutan desde ahí

🧠 Paso a paso
1️⃣ Copia del binario
```
copy %windir%\system32\certutil.exe %temp%\tcm.tmp
```

👉 Traducción:
- Toma `certutil.exe`
- Lo deja en:
```
 C:\Users\IEUser\AppData\Local\Temp\tcm.tmp
```

![Pasted image 20260430122316](../Fotos/Pasted%20image%2020260430122316.png)
2️⃣ Ejecución encubierta
```
%temp%\tcm.tmp -decode c:\file.exe file.txt
```

👉 O sea:
- Ejecuta **certutil**, pero:
- con otro nombre
- desde otra ruta
🔥 ¿Cuál es el objetivo?
👉 **Evasión de detección**

`certutil.exe` es:
- legítima ✔️
- pero también **muy vigilada** ⚠️
👉 La renombra para que **no sea reconocida como esa herramienta específica**

![Pasted image 20260430124701](../Fotos/Pasted%20image%2020260430124701.png)
🧠 ¿Qué es `fltmc.exe`?
👉 fltmc.exe  
Herramienta de Windows para gestionar **drivers de filtros** (file system filter drivers).

🧠 ¿Qué es `SysmonDrv`?

👉 Es e driver de Sysmon
- Captura:
    - creación de procesos
    - conexiones de red
    - cambios en archivos
- Envía logs al sistema

🔥 ¿Qué hace el comando?
👉 **Descarga (unload) el driver de Sysmon**
Traducción directa:
👉 “Apaga Sysmon”

![Pasted image 20260430125041](../Fotos/Pasted%20image%2020260430125041.png)
🧠 ¿Qué es `appcmd.exe`?
👉 appcmd.exe  
Herramienta para administrar Internet Information Services (IIS).

🧠 ¿Qué hace exactamente?
👉 Modifica la configuración del sitio:
- sección: `httplogging`
- parámetro: `dontLog:true`

🔥 Traducción real
👉 **Desactiva los logs HTTP del sitio “Default”**

⚠️ Impacto
- El atacante puede usar el servidor web
- Pero **no deja rastro en logs de IIS**
👉 T1562.002

👉 “Voy a usar el servidor web, pero sin que se registren mis accesos.”

 ✔ ¿Qué hace aquí?
- Toma el proceso con PID `3912`
- Inyecta `T1055.dll` dentro de ese proceso
- El código malicioso se ejecuta **dentro de un proceso legítimo**

![Pasted image 20260430130713](../Fotos/Pasted%20image%2020260430130713.png)
🔍 ¿Qué significa?
- `cmd.exe /c`  
    👉 Ejecuta un comando y termina (no deja consola abierta).
- `.\bin\T1055.exe`  
    👉 Ejecuta un binario desde el directorio actual (`C:\AtomicRedTeam\bin\` probablemente).

🧠 ¿Qué es T1055?
Process Injection
👉 **Process Injection (Inyección de procesos)**

✔️ En simple:
Técnica donde un proceso mete código dentro de otro proceso legítimo para ejecutarse ahí.

 🚨 ¿Qué hace este ejecutable?
Ese `T1055.exe` de Atomic Red Team simula:
- Crear o elegir un proceso (ej: `notepad.exe`, `explorer.exe`, etc.)
- Inyectar una DLL o shellcode dentro de ese proceso
- Ejecutar código desde ese proceso “legítimo”

![Pasted image 20260430131550](../Fotos/Pasted%20image%2020260430131550.png)
- `at 13:20`  
    👉 Ejecuta algo a las **13:20**
- `/interactive`  
    👉 Permite que la tarea interactúe con el escritorio del usuario  
    (muy importante, esto hoy casi no se usa legítimamente)
- `cmd`  
    👉 Ejecuta una consola (`cmd.exe`)
👉 Está programando que a las 13:20 se abra una consola interactiva.

![Pasted image 20260430131934](../Fotos/Pasted%20image%2020260430131934.png)
 📖 Desglose:
- **`SCHTASKS`** → herramienta para gestionar tareas programadas
- `/Create` → crear tarea
- `/SC ONCE` → se ejecuta una vez
- `/TN spawn` → nombre de la tarea
- `/TR cmd.exe` → lo que ejecuta
- `/ST 20:10` → hora

 📌 ¿Qué logra?
👉 Ejecutar `cmd.exe` en el futuro → **ejecución diferida controlada**

![Pasted image 20260430132337](../Fotos/Pasted%20image%2020260430132337.png)
📖 Esto agrega:
- `/RU DOMAIN\user` → ejecuta como otro usuario
- `/RP password` → usa credenciales
- `/SC daily` → persistente (diario)

⚠️ Esto ya es serio:
👉 **Persistencia + movimiento lateral potencial**

- **T1053 – Scheduled Task/Job**
Subtécnicas:
- `T1053.005` → Scheduled Task (Windows)

![Pasted image 20260430135404](../Fotos/Pasted%20image%2020260430135404.png)
🔹 ¿Qué es `pcalua.exe`?
📌 Nombre:
- **Program Compatibility Assistant**

📖 Qué significa:
- Herramienta de Windows para ejecutar programas con **configuraciones de compatibilidad**.
👉 Normalmente se usa cuando un programa antiguo no funciona bien.

👉 Esto está mal formado → probablemente prueba/fuzzing o test del Atomic

![Pasted image 20260430135426](../Fotos/Pasted%20image%2020260430135426.png)
👉 Intenta ejecutar algo llamado "Java"

![Pasted image 20260430135452](../Fotos/Pasted%20image%2020260430135452.png)
- Ejecuta un **.cpl (Control Panel item)**
- `javacpl.cpl` → panel de control de Java

🧠 **LOLBins (Living Off The Land Binaries)**
- Binarios legítimos de Windows usados para:
    - ejecutar código
    - evadir detección
    - evitar malware en disco
👉 `pcalua.exe` es uno de ellos

🔹 ¿Por qué un atacante usaría esto?
No es porque quiera abrir Java 😅
👉 Es porque:
- ✔️ Es un binario firmado por Microsoft
- ✔️ Muchas veces no está bloqueado
- ✔️ Puede ejecutar otros binarios indirectamente
- ✔️ Puede evadir controles básicos (AV/EDR simples)

Esto cae en:
- **T1218 – Signed Binary Proxy Execution**
Subtécnica:
- `T1218.x` → uso de binarios firmados para ejecutar payloads

“Está usando un binario legítimo para ejecutar código de forma indirecta (proxy execution)”

![Pasted image 20260430140706](../Fotos/Pasted%20image%2020260430140706.png)
🔥 ¿Qué ocurre realmente?
👉 `forfiles` encuentra `notepad.exe`  
👉 Por cada match ejecuta:

```
calc.exe
```

✔️ Resultado: se abre la calculadora

🔹 ❗ Lo importante (esto es lo que te evalúan)
Esto NO es sobre `notepad`.
👉 Es sobre esto:
Ejecutar un comando arbitrario usando una herramienta legítima

- Se usa un binario legítimo (`forfiles.exe`)
- Para ejecutar otro proceso (`calc.exe`)
- Evadiendo controles básicos

- **T1202 – Indirect Command Execution**
👉 Porque:
- No ejecutas directamente `calc.exe`
- Lo ejecutas **a través de otro binario**

![Pasted image 20260430141136](../Fotos/Pasted%20image%2020260430141136.png)
- `forfiles.exe` ejecutando comandos raros
- Uso de `:` en rutas (ADS)
- Procesos hijos inesperados:
    - `forfiles → calc.exe`
    - `forfiles → payload.exe`

 ✔️ 1. LOLBin
`forfiles.exe` usado para ejecutar comandos

✔️ 2. Evasión
Uso de **ADS (`file:stream`)**

🔹 ¿Qué es ADS?
- Feature de NTFS
- Permite ocultar datos dentro de un archivo

Ejemplo:
```
normal.dll:evil.exe
```
👉 `evil.exe` está oculto dentro de `normal.dll`

✔️ “El atacante demuestra ejecución de código mediante un LOLBin” → perfecto (calc es ejemplo podria ser otra cosa como es un lab sample)

![Pasted image 20260430141307](../Fotos/Pasted%20image%2020260430141307.png)
Muestra **la identidad del usuario actual** en el sistema
“Está haciendo reconocimiento para decidir siguientes pasos (movimiento lateral, privilegios, persistencia)”

![Pasted image 20260430141437](../Fotos/Pasted%20image%2020260430141437.png)
- **gsecdump**

📖 Qué hace:
- Herramienta para **extraer credenciales del sistema**
- Similar a:
    - `pwdump`
    - `secretsdump`
    - Mimikatz

👉 El atacante está intentando:
🧠 **Credential Dumping**
- Sacar hashes de contraseñas
- Obtener acceso a otras cuentas
- Prepararse para:
    - Pass-the-Hash
    - Movimiento lateral

- Técnica: **OS Credential Dumping (T1003)**

![Pasted image 20260430141737](../Fotos/Pasted%20image%2020260430141737.png)
**WCE = Windows Credentials Editor**
- Extrae credenciales en memoria (como hace Mimikatz)
- Output → `output.txt`

👉 Objetivo:  
**Credential Dumping (robo de credenciales)**

![Pasted image 20260430141822](../Fotos/Pasted%20image%2020260430141822.png)
 📌 Concepto: SAM
- **SAM (Security Account Manager)** = base de datos local de usuarios
- Contiene hashes de contraseñas

👉 Este comando:
- Guarda una copia del registro SAM en un archivo

![Pasted image 20260430142426](../Fotos/Pasted%20image%2020260430142426.png)
- Necesario para poder **desencriptar los hashes del SAM**

👉 Relación importante (esto es examen):

- SAM + SYSTEM = puedes crackear hashes offline

![Pasted image 20260430142507](../Fotos/Pasted%20image%2020260430142507.png)
Contiene secretos adicionales (LSA secrets)

👉 Todo esto junto:
- SAM
- SYSTEM
- SECURITY

➡️ = **Full credential extraction offline**

![Pasted image 20260430142547](../Fotos/Pasted%20image%2020260430142547.png)
📌 Concepto: LSASS
- **LSASS (Local Security Authority Subsystem Service)**
- Proceso donde viven credenciales en memoria

👉 Qué hace:
- Dump completo de memoria de LSASS

👉 Objetivo:
- Extraer:
    - passwords en texto plano
    - hashes
    - tickets Kerberos

⚠️ Esto es:  
👉 **Técnica clave: Credential Dumping (T1003)**

![Pasted image 20260430142645](../Fotos/Pasted%20image%2020260430142645.png)
 📌 Concepto: NTDS.dit
- Base de datos de Active Directory

👉 Qué intenta:
- Crear una copia completa del AD

👉 Objetivo:
- Obtener TODOS los usuarios del dominio

![Pasted image 20260430142741](../Fotos/Pasted%20image%2020260430142741.png)
 📌 Concepto: VSS (Volume Shadow Copy)
- Snapshot del disco

👉 Para qué sirve:
- Copiar archivos bloqueados (como NTDS.dit)

![Pasted image 20260430142852](../Fotos/Pasted%20image%2020260430142852.png)
`GLOBALROOT\Device\HarddiskVolumeShadowCopy1`
Concepto: **Volume Shadow Copy (VSS)**
- Snapshot del disco
- Permite acceder a archivos que están bloqueados (como NTDS.dit

👉 Esto es clave:
- Accede al snapshot
- Copia la base de datos del AD

👉 Resultado:
- Dump completo del dominio
“Copia la base de datos completa de Active Directory desde un snapshot del sistema a una ruta accesible” para: Offline credential dumping

🔴 1. ¿Qué es `NTDS.dit`?
📌 Concepto: **NTDS.dit (Active Directory Database)**

- Base de datos de **Active Directory**
- Contiene:
    - usuarios del dominio
    - hashes de contraseñas
    - grupos
    - políticas

👉 En simple:  
**Es el “corazón” del dominio**

![Pasted image 20260430143347](../Fotos/Pasted%20image%2020260430143347.png)
 🔴 1. ¿Qué es `SYSTEM`?
 📌 Concepto: **SYSTEM Hive (Registro de Windows)**
- Parte del registro (`HKLM\SYSTEM`)
- Contiene:
    - BootKey (clave de cifrado)
    - Configuración del sistema

👉 En simple:  
**Es la clave que permite descifrar los hashes**

👉 Igual que antes con NTDS.dit:
- Accede al snapshot (`Volume Shadow Copy`)
- Copia el archivo SYSTEM
- Lo deja en una carpeta accesible\

👉 **NTDS.dit SOLO no sirve completamente**
Necesitas:
- `NTDS.dit` → hashes cifrados
- `SYSTEM` → clave para descifrarlos

![Pasted image 20260430143623](../Fotos/Pasted%20image%2020260430143623.png)
👉 Está **extrayendo el registro del sistema**.
Y si lo conectas con eventos anteriores que mostraste:
- `reg save HKLM\SAM`
- `reg save HKLM\SECURITY`
- `ntds.dit` copy
- `vssadmin create shadow`

💥 Esto forma un patrón clarísimo:
🧠 ATAQUE: Credential Dumping

![Pasted image 20260430143818](../Fotos/Pasted%20image%2020260430143818.png)## 📌 Concepto: **rundll32.exe**
- Ejecuta funciones dentro de DLLs
- Muy usado por Windows… y también por atacantes

📌 Qué significa este caso específico
- Padre: `svchost.exe -k DcomLaunch`
- DLL: `shell32.dll`
- Función: `SHCreateLocalServerRunDll`
- Flag: `-Embedding`

👉 Esto es típico de:
**COM / DCOM execution**

👉 Es ruido comparado con lo anterior (LSASS, NTDS, etc.)

![Pasted image 20260430143921](../Fotos/Pasted%20image%2020260430143921.png)- Abre un archivo `.md` (markdown)
- Ruta:
    - `AtomicRedTeam`
    - técnica `T1003`
👉 Es:
- lectura de documentación
- alguien viendo la técnica

👉 **T1003 = OS Credential Dumping**

“El usuario está leyendo el playbook de cómo hacer credential dumping”