- --
- Tags: #elasticsearch #blue_team #siem 
-- -
# Resources

**EVENT ID**
[SIEM/Notable-Event-IDs-Sigma.md at master · TonyPhipps/SIEM](https://github.com/TonyPhipps/SIEM/blob/master/Notable-Event-IDs-Sigma.md)
[SIEM/Notable-Event-IDs-Sigma.md at master · TonyPhipps/SIEM](https://github.com/TonyPhipps/SIEM/blob/master/Notable-Event-IDs-Sigma.md)

*Practica Labs Blue Team*
[GitHub - ChickenLoner/Awesome-Splunk-and-Elastic-SIEM-Practice-Labs: This repository dedicated to collect SIEM practice labs (Splunk and Elastic) from various cybersecurity training platforms](Splunk%20and%20Elastic)%20from%20various%20cybersecurity%20training%20platforms)%20from%20various%20cybersecurity%20training%20platforms)
[GitHub - ChickenLoner/Awesome-Splunk-and-Elastic-SIEM-Practice-Labs: This repository dedicated to collect SIEM practice labs (Splunk and Elastic) from various cybersecurity training platforms](Splunk%20and%20Elastic)%20from%20various%20cybersecurity%20training%20platforms)%20from%20various%20cybersecurity%20training%20platforms)

*Winlogbeat Fields* Son los **campos crudos/específicos de Windows**.

[Winlogbeat fields | Beats](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-winlog)

*ECS Fields* ECS **normaliza** Winlogbeat, Sysmon, Zeek, etc. Para que "hablen" el mismo idioma

[ECS fields | Beats](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs)

*Zeek logs* Zeek = **visión de red**, no del host.

[ Beats](https://www.elastic.co/docs/reference/beats/filebeat/exported-fields-zeek|Zeek (Bro) fields | Beats](Bro)%20fields%20)

*System Monitor (Sysmon) logs* **telemetría profunda del host**.

[Sysmon - Sysinternals | Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)

*PowerShell logs Aquí detectas **PowerView, Mimikatz, IEX, EncodedCommand**.*

[Hunting for Malicious PowerShell using Script Block Logging | Splunk](https://www.splunk.com/en_us/blog/security/hunting-for-malicious-powershell-using-script-block-logging.html)

*Lateral Tool Transfer*

[Lateral Tool Transfer, Technique T1570 - Enterprise | MITRE ATT&CK®](https://attack.mitre.org/techniques/T1570/)

*Boot or logon autostart execution*
[Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder, Sub-technique T1547.001 - Enterprise | MITRE ATT&CK®](https://attack.mitre.org/techniques/T1547/001/)

*WinRM for Lateral Movement*
[WinRM for Lateral Movement | Red Team Notes](https://www.ired.team/offensive-security/lateral-movement/t1028-winrm-for-lateral-movement)

*Windows Security Log Event ID 4624*
[Windows Security Log Event ID 4624 - An account was successfully logged on](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=4624)

*Windows Security Log Event ID 4732*
[Windows Security Log Event ID 4732 - A member was added to a security-enabled local group](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=4732)

*Windows Security Log Event ID 4733*
[Windows Security Log Event ID 4733 - A member was removed from a security-enabled local group](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=4733)

----
# **Log Types**
# Tipos de Logs

`Windows audit logs` (categorizados bajo las ventanas de patrón índice*)
Normalmente bajo `windows-*`
**Valor defensivo:** ⭐⭐⭐⭐  
Muy buenos para:
Detectar **movimiento lateral**Ataques de **credenciales**
Persistencia básica
`System Monitor (Sysmon) logs` (también dentro de las ventanas de patrones de índice*, más sobre [Sysmon aquí](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon))
También bajo `windows-*` (pero con campos Sysmon)
**Valor defensivo:** ⭐⭐⭐⭐⭐  
Ideal para:
Detectar **malware**
**Living off the Land (LotL)**
Ataques avanzados
`PowerShell logs` (indexado también en Windows*, más información sobre los registros de PowerShell [aquí](https://www.splunk.com/en_us/blog/security/hunting-for-malicious-powershell-using-script-block-logging.html))
Bajo `windows-*`
**Valor defensivo:** ⭐⭐⭐⭐⭐  
Clave para detectar:
- **Ataques fileless**
- C2 en PowerShell
Uso de `Invoke-Expression`, `DownloadString`, etc.
`Zeek logs`, [una herramienta de monitorización de seguridad de red](https://www.elastic.co/guide/en/beats/filebeat/current/exported-fields-zeek.html) (clasificada bajo el patrón de índice Zeek*)
Bajo `zeek-*`
**Valor defensivo:** ⭐⭐⭐⭐⭐  
Excelente para:
- Detectar **C2**
- Exfiltración de datos
- Actividad anómala en red
### Sospechoso

**Hay ciertas palabras que en un entorno normal casi nunca aparecen juntas. Si las ves, sospecha de inmediato:**

| **Palabra/Comando**             | **Qué significa en "humano"** | **Por qué es alarmante**                                                        |
| ------------------------------- | ----------------------------- | ------------------------------------------------------------------------------- |
| **`iwr` / `wget`**              | "Traer algo de internet"      | ¿Por qué un usuario descargaría algo de una web externa en la terminal?         |
| **`iex` / `Invoke-Expression`** | "Ejecutar ahora mismo"        | Se usa para correr virus directamente en la memoria sin guardarlos en el disco. |
| **`Bypass`**                    | "Saltarse las reglas"         | Se usa para desactivar las políticas de seguridad de PowerShell.                |
| **`GitHub` / `raw...`**         | Una web de código             | Los empleados normales no bajan herramientas de GitHub para trabajar.           |
| **`Hidden`**                    | "Escondido"                   | Intentan que no se vea la ventana negra de la terminal.                         |
# **Introduction to Threat Hunting & Hunting With Elastic Hack The Box**
# Filtros y Columnas Utilizados
**Filtros y agregados como Fields** (Columnas del Panel izquierdo)
⏱️ *Tiempo:* `@timestamp` → Fecha y hora del evento
🖥️ *Host:* `host.name` → Máquina que generó el evento `host.hostname` → Nombre del host `agent.hostname` → Quien envia el Log
👤 *Usuario:* `user.id` → ID único del usuario `user.name` → Nombre del usuario
🌐 *Red:* `source.ip` → IP de origen `destination.ip` → IP de destino `destination.port` → Puerto de destino
⚙️ *Proceso:* `process.pid` → Identificador del proceso (PID) `process.name` → Nombre del proceso `process.args` → Argumentos del proceso
📁 *Archivos:* `file.path` → Ruta completa del archivo `file.name` → Nombre del archivo
🧾 *Evento (clasificación):* `event.category` → Categoría general del evento `event.type` → Tipo específico de evento
🎯 *Acción y resultado:* `action / event.action` → Acción realizada `event.outcome` → Resultado (success / failure) `status / response.code` → Código de estado o error
🆔 *Identificadores:* `event.code` → Código numérico del evento `event_id` → ID único del evento
📜 *Contenido del log:* `message` → Resumen del evento `log.original` → Log original completo
🌍 *DNS:* `dns.question.name` → Dominio consultado `dns.answers.data` → Respuesta DNS
### Filtros Utilizados Lab 1 Cazando Stuxbot
**Orden Cronológico utilizado**
**Filtro**
`event.code:15 AND file.name:*invoice.one` → Creación de archivo detectada por Sysmon  
`event.code:11 AND file.name:invoice.one*` → Archivo escrito o modificado en disco  
`source.ip:192.168.28.130 AND dns.question.name:*` → Consultas DNS desde IP sospechosa  
`event.code:1 AND process.command_line:*invoice.one*` → Ejecución de proceso con ese archivo  
`event.code:1 AND process.parent.name:"ONENOTE.EXE"` → Proceso iniciado desde OneNote  
`event.code:1 AND process.parent.command_line:*invoice.bat*` → Ejecución encadenada mediante script  
`process.pid:"9944" and process.name:"powershell.exe"` → Actividad de instancia específica de PowerShell  
`process.name:"default.exe"` → Ejecución de binario sospechoso  
`process.name:"SharpHound.exe"` → Enumeración de Active Directory  
`process.hash.sha256:018d37cbd3878258c29db3bc3f2988b6ae688843801b9abc28e6151141ab66d4` → Hash asociado a malware conocido  
`(event.code:4624 OR event.code:4625) AND winlog.event_data.LogonType:3 AND source.ip:192.168.28.130` → Intentos de autenticación de red desde esa IP  
`file.name:"default.exe" AND file.name:*VBS*` → Ejecutable relacionado con script VBS  
`process.name:"mimikatz.exe"` y buscamos `process.arg` → Herramienta usada para robo de credenciales  
`process.name:"powershell.exe" AND process.args:*` → Comandos ejecutados vía PowerShell
#### Explicación campos tras revisar un evento en concreto en Lab 1 Cazando Stuxbot
**Campos dentro de un evento**
`process.command_line` → Binario y argumentos usados en la ejecución  
`process.executable` → Ruta completa del ejecutable real  
`process.parent.command_line` → Forma en que se lanzó el proceso padre  
`process.parent.executable` → Binario que inició el proceso actual  
`process.parent.args` → Argumentos del proceso padre  
`process.args` → Argumentos del proceso hijo
### Filtros y Columnas utilizados Lab 1 Skill Assesment
**Filtro**
`event.code:11 AND file.directory:"C:\\Users\\Public"` → Escritura de archivos en carpeta pública  
`event.code:11 AND file.directory:"C:\\Users\\Public" AND file.extension:"exe"` → Drop de ejecutables en ubicación accesible  
`event.code:13 AND message:"*Run*"` → Persistencia mediante claves Run  
`event.code:4648 AND message:"*powershell.exe*"` → Uso de credenciales explícitas en PowerShell  
`event.code:4624 AND winlog.event_data.TargetUserName:"administrator"` → Login exitoso como administrador
### Filtros y Columnas utilizados Create new Dashboard / Visualization
**Filtro**
`event.code:4625 AND winlog.event_data.SubStatus:0xC0000072` → Fallos por cuenta deshabilitada  
`event.code:4625 AND winlog.event_data.SubStatus:0xC0000072 AND @timestamp >= "2023-03-03T00:00:00.000Z" AND @timestamp <= "2023-03-06T23:59:59.999Z"` → Fallos en rango temporal definido  
`event.code:4625 AND user.name:admin*` → Intentos fallidos contra cuentas admin  
`winlog.logon.type` → Tipo de autenticación utilizada  
`group.name` → Grupo de seguridad involucrado  
`NOT user.name:*$ AND winlog.channel.keyword:Security` → Exclusión de cuentas de sistema
**Columnas**
`user.name.keyword` → Usuario que intentó autenticarse  
`host.hostname.keyword` → Equipo donde ocurrió el evento  
`winlog.logon.type.keyword` → Tipo de inicio de sesión  
`related.ip.keyword` → IP origen de la conexión  
`winlog.event_data.MemberSid.keyword` → Usuario afectado por cambios de grupo  
`winlog.event_data.SubjectUserName` → Usuario que realizó la acción  
`event.action.keyword` → Acción registrada en el sistema
### Filtros y Columnas Intentos fallidos de inicio de sesion (Todos los usuarios)
**Columnas**
`user.name.keyword` → Usuario objetivo  
`host.hostname.keyword` → Equipo destino  
`winlog.logon.type.keyword` → Método de autenticación  
`count of records` → Total de intentos
**Filtro**
`NOT user.name:*$ AND winlog.channel.keyword:Security` → Exclusión de cuentas máquina
### Filtros y Columnas Intentos fallidos de inicio de sesion (Usuarios deshabilitados)
**Filtro**
`winlog.event_Data.SubStatus:0xc0000072` → Fallos por cuenta deshabilitada
### Filtros y Columnas Inicio de sesión RDP exitoso relacionado con cuentas de servicio
**Columnas**
`user.name.keyword` → Cuenta de servicio autenticada  
`host.hostname.keyword` → Host destino  
`related.ip.keyword` → Origen remoto de conexión
Confirmación RDP efectuado → LogonType 10 confirmado
### Filtros y Columnas Usuarios añadidos o eliminados de un grupo local (dentro de un plazo específico)
**Filtro**
`group.name:administrators` → Grupo local privilegiado  
`event.code:4732,4733` → Alta o eliminación de miembros
**Columnas**
`user.name.keyword` → Usuario que ejecutó la acción  
`winlog.event_data.MemberSid.keyword` → Usuario añadido o eliminado  
`group.name.keyword` → Grupo afectado  
`event.action.keyword` → Acción realizada  
`host.hostname.keyword` → Equipo donde ocurrió el evento
### Filtros y Columnas Skill Assessment
**Filtro**
`event.action:"logon-failed" AND user.name:"sql-svc1"` → Fallos en cuenta servicio  
`event.action:"logon-failed" AND user.name:"Anni"` → Fallos de usuario específico  
`event.action:"logon-failed" AND user.name:*admin*` → Ataques a cuentas administrativas  
`winlog.event_data.LogonType:10 AND user.name:*svc*` → RDP usando cuentas servicio  
`group.name:"Administrators"` → Actividad sobre grupo privilegiado  
`winlog.event_data.LogonType:10 user.name:*admin*` → RDP como administrador  
`winlog.event_data.LogonType:10 AND user.name:*admin*` → Confirmación de sesión RDP
### **Maquinas Relacionadas** 
### Filtros Maquina Slingshot Tryhackme
**Filtro**
`response.status` → Código de respuesta HTTP (200, 401, 404, etc.)  
`http.url` → URL completa solicitada al servidor  
`request.headers.User-Agent` → Herramienta o navegador que realiza la petición  
`transactions.remote_address` → IP origen que inicia la conexión  
`request.headers.Authorization` → Credenciales enviadas (Basic, Bearer, Base64)
### Filtros Maquina Itsybitsy Tryhackme
**Filtro**
`user_agent` → Identifica la herramienta o software utilizado  
`host` → Dominio o servidor destino de la conexión  
`uri` → Recurso específico solicitado dentro del servidor
### Filtros Maquina Boogeyman3 Tryhackme
**Campo Útil**  
`ProcessId` → Identificador único del proceso  
`Register-ScheduledTask` → Creación de tarea programada (persistencia)
**Filtro**  
`powershell.exe and event.provider:"Microsoft-Windows-Sysmon" and event.code:3` → Conexiones de red iniciadas por PowerShell  
`event.code:1` → Creación de procesos (ejecución)  
`host.hostname:DC01 and event.provider:Microsoft-Windows-Sysmon and event.code:1` → Procesos creados en el controlador de dominio
**Columna**  
`destination.ip` → IP destino de la conexión  
`agent.name` → Agente que recolecta el evento  
`host.hostname` → Equipo donde ocurrió el evento  
`process.command_line` → Comando exacto ejecutado  
`process.parent.executable` → Ruta del ejecutable padre  
`process.parent.name` → Nombre del proceso padre
### Filtros Maquina Hunt Me I Payment Collectors Tryhackme
**Filtro**
`nslookup.exe` → DNS lookup / Consulta DNS: usado legítimamente para resolución de nombres, pero también para exfiltración de datos vía DNS  
`*.zip` → Archivos comprimidos: identifica creación o acceso a ZIP (preparación de exfiltración)  
`event.code:11` → File creation / Creación de archivos (Sysmon): detecta escritura de archivos en disco  
`winlog.event_id : 11 AND file.name: *.xlsx` → Creación de archivos Excel (posible robo de información sensible)  
`winlog.event_id : 11` → Eventos de creación de archivos en el sistema
**Columna**
`process.command_line` → Línea de comandos completa del proceso ejecutado (contexto exacto)  
`winlog.computer_name` → Nombre del equipo que generó el evento  
`user.name` → Usuario bajo el cual se ejecutó la acción  
`process.command_line` → Comando ejecutado (útil para reconstruir la actividad maliciosa)  
`file.path` → Ruta completa del archivo creado, modificado o accedido  
`process.name` → Nombre del proceso responsable de la acción  
`process.parent.command_line` → Comando del proceso padre (origen de la ejecución)
### Filtros Maquina Hunt Me II Typo Squatters Tryhackme
**Filtro**
`process.name: chrome.exe AND *zip*`
`*7zipp*`
`event.code:1 AND 7z.ps1`
`event.code:1 AND Invoke-Nanodump.ps1`
`event.code:1 AND mimikatz*`
`event.code:1 AND *net1`
`event.code:1 AND anna.jones`
`process.command_line:*eb1892cb0a163e122bc71be173c66fed*`
`process.name: *bomb.exe* AND event.code: 11`
**Columna**
`dns.resolved_ip`
`process.parent.name` 
`process.name`
`file.name` 
`process.pid`
`process.command_line` 
`message`
`user.name`
`agent.hostname`
**Surround Documents**
`surround documents`
# Lab 1 / Cazando Stuxbot

FILTRO
````shell-session
event.code:15 AND file.name:*invoice.one
````
event.code:15 (creacion de archivo / FileCreateStreamHash) *Se introduce información dentro de un archivo que YA EXISTE*  
Podrías ampliar para ver **qué proceso creó el ADS**:
`process.name:*` o `process.executable:*`

FILTRO
```shell-session
event.code:11 AND file.name:invoice.one*
```
FileCreate *Creacion real del archivo*

FILTRO
```shell-session
source.ip:192.168.28.130 AND dns.question.name:*
```
**todas las consultas DNS realizadas por esa IP**.
`*`asegura que **el campo existe** (no importa el dominio)
**ELIMINAR RUIDO EN ESTA BUSQUEDA PUNTUAL**
![700](../Fotos/Pasted%20image%2020260108200054.png)
![ 400](../Fotos/Pasted%20image%2020260108200210.png)

-----
![400](../Fotos/Pasted%20image%2020260108200810.png)
1. `nav-edge.smartscreen.microsoft.com`
2. `file.io`
3. `mail.google.com`
 + Gmail + SmartScreen = **cadena de ataque sospechosa** (abrió email → link a file.io → descarga malware).

---
Vista general (nada reelevante)
![800](../Fotos/Pasted%20image%2020260108201504.png)

----
FILTRO
```shell-session
event.code:1 AND process.command_line:*invoice.one*
```
Con este filtro **SOLO ves ejecución**.
1 Process creation / Registro cuando **se crea / ejecuta un proceso**
O sea: un proceso fue ejecutado usando algo relacionado con `invoice.one`.
process.command_line: **No filtra por tipo de evento**, solo por **contenido del comando**.
![Pasted image 20260108203558](../Fotos/Pasted%20image%2020260108203558.png)

![800](../Fotos/Pasted%20image%2020260108201734.png)
[Winlogbeat fields | Beats](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-winlog)  [ECS fields | Beats](https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs)

FILTRO
```shell-session
event.code:1 AND process.parent.name:"ONENOTE.EXE"
```
Busca **procesos que fueron creados** (**ejecutados**) y cuyo **proceso padre** fue **OneNote**.
![500|800](../Fotos/Pasted%20image%2020260108204144.png)

**FIJARSE MUY BIEN EN LAS HORAS QUE SEAN CONGRUENTES**
Punto 4  Importante y segundo punto detallado en el texto
![700|800](../Fotos/Pasted%20image%2020260108204718.png)
Ahora podemos establecer una conexión entre "OneNote.exe", el sospechoso "invoice.one" y la ejecución de "cmd.exe" que inicia "invoice.bat" desde una ubicación temporal (muy probable debido a su adjunto dentro del archivo de OneNote). La pregunta ahora es, ¿ha provocado algo más este guion por lotes? Busquemos si un proceso padre con un argumento de línea de comandos apuntando al archivo batch ha generado algún proceso hijo con la siguiente consulta.
1. ONENOTE.EXE (binario padre) abre invoice.one
2. ONENOTE.EXE lanza cmd.exe
3. cmd.exe ejecuta invoice.bat
- **process.executable** → proceso que estamos analizando
- **process.parent.executable** → proceso que **lo creó**
*EXPLICACION*
1️⃣ Usuario abre el archivo malicioso
Usuario abre el archivo malicioso ONENOTE.EXE Evidencia PUNTO 1 `process.parent.executable` Acción PUNTO 2 `process.parent.command_line`
2️⃣ OneNote lanza el intérprete de comandos
PUNTO 3 cmd.exe `process.executable`
3️⃣ CMD ejecuta un archivo batch
PUNTO 4 `process.command_line`
4️⃣ Origen del `.bat`
`AppData\Local\Temp\OneNote\16.0\Exported\`
Esto indica que: El `.bat` fue **extraído automáticamente** desde el archivo `.one`

CAMPOS QUE VEMOS EN LA FOTO ANTERIOR EXPLICADOS
`process.command_line`
- **Qué es**: Cómo se ejecutó el proceso.
- **Incluye**: binario + argumentos.
- **Inglés / Español**: Command line / Línea de comandos.
`process.executable`
- **Qué es**: El **binario real** que se ejecutó.
- **Ruta completa** del `.exe`.
- **Inglés / Español**: Executable / Ejecutable.
`process.parent.command_line`
- **Qué es**: Cómo se ejecutó el **proceso padre**.
- Muestra **con qué argumentos** se lanzó el padre.
- **Inglés / Español**: Parent command line / Línea de comandos del padre.
`process.parent.executable`
- **Qué es**: El **binario del proceso padre**.
- Indica **quién lanzó** el proceso actual.
- **Inglés / Español**: Parent executable / Ejecutable padre.

-----
FILTRO
```shell-session
event.code:1 AND process.parent.command_line:*invoice.bat*
```

![800](../Fotos/Pasted%20image%2020260108210333.png)

`process.pid` Field Columna Agregada

![800](../Fotos/Pasted%20image%2020260108211036.png)

FILTRO
```shell-session
process.pid:"9944" and process.name:"powershell.exe"
```
`event.code` Field Columna Agregada
Filtra eventos donde
- El **PID del proceso** es **9944**
- El **nombre del proceso** es **powershell.exe**  (siguiendo el mismo caso en el que estamos trabajando en este lab como vimos mas arriba)

![800](../Fotos/Pasted%20image%2020260108220715.png)

Esto **sigue una sola instancia** de PowerShell (PID 9944) y muestra **todas sus acciones**.
- PowerShell **no solo se ejecutó**
- **Hizo varias acciones distintas**
Todo ligado al evento que venimos dando seguimiento 

`file.path` Ruta del archivo creado o usado
`dns.question.name` Nombre de dominio consultado en DNS
`destination.ip` IP de destino a la que se conecta

`process.name` → `powershell.exe` (default, pero confirma).[](https://rootdse.org/posts/understanding-sysmon-events/)​
`event.code` → `22, 3, 11` (Sysmon: DNS, file create, file mod).[](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)​
`dns.question.name` → `Password Spray`, `Ngrok`, `pastesite.com`.[](https://www.elastic.co/docs/reference/ecs/ecs-dns)​
`destination.ip` → `18.158.249.xx`, `104.47.xx`.[](https://www.elastic.co/guide/en/apm/server/7.5/exported-fields-ecs.html)​
`file.name` / `file.path` → Scripts dumpados como `DomainPasswordSpray.ps1`.[](https://rootdse.org/posts/understanding-sysmon-events/)

![800](../Fotos/Pasted%20image%2020260108221742.png)

- **Fila 1**: `powershell.exe` + **event.code 22** (Sysmon DNS query) + **Password Spray** → Consulta DNS a `18.158.249.xx` (Ngrok).
- **Fila 3**: `powershell.exe` + **EXE file on disk** + `script dumped on disk` → **Dropper de malware** desde payload.[](https://rootdse.org/posts/understanding-sysmon-events/)​
- **Fila 4**: `powershell.exe` + **EXE files on threat intel** + persistence → **Ejecutable en Threat Intel** (malware conocido) con **persistencia**. ⚠️
- **Fila 6**: **DNS for Ngrok** → **C2 via Ngrok** (túnel para evadir firewalls).[](https://www.elastic.co/docs/reference/ecs/ecs-dns)​
- **Fila 9**: `pastesite.com` + **Created some cmdline files after initial stager** → **Descarga stage 2** desde pastebin-like.

**TTPs (MITRE ATT&CK)**:
- **TA0002 Execution** (PS1 dump).
- **TA0011 C2** (Ngrok DNS).
- **TA0003 Persistence** + **T1071 AppData**.
- **TA0008 Lateral** (Password Spray).

Nuevas columnas: Obtener información sobre la dirección IP de destino que acabamos de descubrir.
`source.ip`
`destination.ip`
`destination.port`

![800](../Fotos/Pasted%20image%2020260108233840.png)

Curiosamente, la actividad parece haberse extendido hasta el día siguiente. La razón de la terminación de la actividad no está clara... ¿Hubo algún cambio en la propiedad intelectual C2? ¿O simplemente se detuvo el ataque? Al inspeccionar las consultas DNS para "ngrok.io", encontramos que la IP devuelta () ha cambiado efectivamente. Ten en cuenta que el campo se añadió como columna.

`dns.answers.data` Se agrega Columna
`dns.question.name: *ngrok.io*` Se agrega Filtro
 *Verás cambio de IP C2* La IP recién descubierta también indica que las conexiones continuaron de forma constante durante los días siguientes

![800](../Fotos/Pasted%20image%2020260108235009.png)   

![250](../Fotos/Pasted%20image%2020260108235144.png)

Por tanto, es evidente que existe una actividad sostenida de la red, y podemos deducir que el C2 ha sido accedido de forma continua.

*Ahora, en cuanto al archivo ejecutable subido "default.exe" anterior – ¿alguna vez se ejecutó?*
`process.name` Se agrega Columna
`process.args` Se agrega Columna
`event.code` Se agrega Columna
`file.path` Se agrega Columna
`destination.ip` Se agrega Columna
`dns.question.name` Se agrega Columna

FILTRO
```shell-session
process.name:"default.exe"
```

![800](../Fotos/Pasted%20image%2020260108235611.png)

De hecho, se ha ejecutado: podemos discernir al instante que el ejecutable inició consultas DNS para Ngrok y estableció conexiones con las direcciones IP C2. También subió dos archivos: "svchost.exe" y "SharpHound.exe". SharpHound es una herramienta reconocida para diagramar Active Directory e identificar rutas de ataque para escalar. En cuanto a svchost.exe, no estamos seguros: ¿es otro agente malicioso? El nombre implica que intenta imitar el archivo legítimo svchost, que forma parte del sistema operativo Windows.
C:\Users\Public\SharpHound.exe **BloodHound collector**: Enumera AD (usuarios/grupos/permisos).
C:\Users\bob\AppData\Local\Temp\svchost.exe **Malware disfrazado**: svchost legítimo está en System32. Este en Temp = **persistencia/escalada**.|
Tal vez podria ser util esto no estoy seguro:  `file.path: *SharpHound.exe OR *svchost.exe AND NOT file.path: *System32*`

*Si subimos el tiempo, hay más actividad de este ejecutable, incluyendo la subida de "payload.exe", un archivo VBS y repetidas subidas de "svchost.exe".*
En este punto, nos queda una pregunta: ¿ejecutó SharpHound? ¿El atacante adquirió información sobre Active Directory? Podemos investigar esto con la siguiente consulta (ya que era un archivo ejecutable en disco).

FILTRO /COLUMNA
```shell-session
process.name:"SharpHound.exe"
```
`process.args` Se agrega Columna

![800](../Fotos/Pasted%20image%2020260109014150.png)

Sysmon ha marcado "default.exe" con un hash de archivo (campo) que coincide con el que aparece en el informe de Threat Intel. Esto nos lleva a cuestionar si este ejecutable ha sido detectado en otros dispositivos dentro del entorno. Hagamos una búsqueda amplia.

FILTRO
````shell-session
process.hash.sha256:018d37cbd3878258c29db3bc3f2988b6ae688843801b9abc28e6151141ab66d4
````
En este momento tenemos las siguientes columnas 
Para cumplir con lo que mencionamos en la descripción del paso anterior agregamos la columna:
`host.hostname` Se agrega Columna

![800](../Fotos/Pasted%20image%2020260109014751.png)

Resumido:  para ver si este ejecutable ha sido detectado en otros dispositivos dentro del entorno.
Se han encontrado archivos con este valor hash en WS001 y PKI, lo que indica que el atacante también ha vulnerado al menos el servidor PKI. También parece que un archivo de puerta trasera ha sido colocado bajo el perfil del usuario "svc-sql1", lo que sugiere que la cuenta de este usuario probablemente está comprometida.


![800](../Fotos/Pasted%20image%2020260109015451.png)

Ampliando la primera instancia de ejecución "default.exe" en PKI, observamos que el proceso principal era "PSEXESVC", un componente de PSExec de SysInternals – una herramienta a menudo utilizada para ejecutar comandos de forma remota, frecuentemente empleada para movimientos laterales en brechas de Active Directory.
`process.parent.args` (proceso principal)
`process.args` (proceso hijo)

Estos son columnas quizas con nombre personalizado aun no lo se me falta experiencia pero tenerlo en cuenta quizas si quizas no revisarlo
`time`
`event.code`
`agent.hostname`
`user.name`
FILTRO
```shell-session
(event.code:4624 OR event.code:4625) AND winlog.event_data.LogonType:3 AND source.ip:192.168.28.130
```

![800](../Fotos/Pasted%20image%2020260109020053.png)

---
**Reflexion**
¿Cómo se comprometió la contraseña de "svc-sql1"? La única explicación plausible según los datos disponibles hasta ahora es posiblemente el script PowerShell subido anteriormente, aparentemente diseñado para la Brutadura por Contraseña. Sabemos que esto se subió en WS001, así que podemos comprobar si hay intentos exitosos o fallidos de contraseña desde esa máquina, excluyendo los de Bob, el usuario de esa máquina (y la propia máquina).
Los resultados son bastante intrigantes: dos intentos fallidos de conseguir la cuenta de administrador, aproximadamente en la época en que se detectó la actividad sospechosa inicial. Posteriormente, hubo numerosos intentos exitosos de inicio de sesión para "svc-sql1". Parece que intentaron descifrar la contraseña del administrador pero fracasaron. Sin embargo, dos días después, el día 28, observamos intentos exitosos con svc-sql1.

![Pasted image 20260109020412](../Fotos/Pasted%20image%2020260109020412.png)

Información extra proporcionada de perplexity unicamente
1. **Discover** → Pega query en **KQL bar**.
2. **Añade columnas**: `winlog.event_data.TargetUserName`, `winlog.event_data.WorkstationName`.
3. **Ve**: Usuario objetivo, máquina origen → **Mapa movimiento lateral**.
4. **Timeline**: Correlaciona con `psexec.exe` events.

-------
*Pregunta 1*
Navega hasta http://[IP objetivo]:5601 y sigue mientras buscamos a Stuxbot. En la parte donde default.exe está bajo investigación, se menciona un archivo VBS. Introduce su nombre completo como respuesta, incluyendo la extensión.
Utilizamos: `file.name:"default.exe" AND file.name:*VBS*`
Respuesta: XceGuhkzaTrOy.vbs

*Pregunta 2*
Stuxbot subió y ejecutó a mimikatz. Proporciona los argumentos del proceso (qué viene después mimikatz.exe, ...) como respuesta.
Utilizamos: `process.name:"mimikatz.exe"` y buscamos `process.arg`
Respuesta: lsadump::dcsync /domain:eagle.local /all /csv, exit

*Pregunta 3*
Se ha cargado algo de código PowerShell en la memoria que escanea/apunta a los composiciones de red. Aprovecha los registros disponibles de PowerShell para identificar de qué herramienta de hacking popular proviene este código. Formato de respuesta (una palabra): `P____V___`
`process.name:"powershell.exe" AND process.args:*`
 🔍 QUÉ fragmentos copiar (muy importante)
`Get-NetUser`
`Get-NetComputer`
`Invoke-ShareFinder`
`Get-Domain`
`function Get-NetGroup`
👉 Esos nombres son **huellas digitales**.

 Ejemplo práctico
En un log ves algo como:
`function Get-NetUser {     param(...)`
Copias:
`function Get-NetUser`
Lo buscas en Google/GitHub 👉  
Respuesta: PowerView (PowerSploit)

# Lab 1 Skill Assessment

`Hunt 1`: Crear una consulta KQL para buscar ["Transferencia lateral de herramienta"](https://attack.mitre.org/techniques/T1570/) a . Introduce el contenido del campo en el documento relacionado con una herramienta transferida que empieza por "r" como respuesta.`C:\Users\Public` (cual es el u suario?)
Filtro: `event.code: 11 AND file.directory: "C:\\Users\\Public"`
Filtro: `event.code: 11 and file.directory: "C:\\Users\\Public" and file.extension: "exe"`
Respuesta: svc-sql1

![800](../Fotos/Pasted%20image%2020260110100631.png)

`Hunt 2`: Crea una consulta KQL para buscar ["Ejecución automática de arranque o inicio de sesión: Claves de ejecución del registro / Carpeta de arranque".](https://attack.mitre.org/techniques/T1547/001/) Introduce el contenido del campo en el documento relacionado con la primera acción de persistencia basada en el registro como tu respuesta.`registry.value`
Filtro: `event.code: 13 and message : "*Run*"`
Respuesta: LgvHsviAUVTsIN

`Hunt 3`: Crea una consulta KQL para buscar ["PowerShell Remoting for Lateral Movement".](https://www.ired.team/offensive-security/lateral-movement/t1028-winrm-for-lateral-movement) Introduce el contenido del campo en el documento relacionado con el movimiento lateral basado en remoto de PowerShell hacia DC1.`winlog.user.name`
Filtro: `event.code: 4648 AND message : "*powershell.exe*"`
Filtro: `event.code: 4624 and winlog.event_data.TargetUserName: "administrator"`
Filtro definitivo de la respuesta 1 hit: `event.code: 4624 and winlog.event_data.TargetUserName: "administrator"`
Respuesta: 	svc-sql1

# **Security Monitoring & SIEM Fundamentals**

# Create new Dashboard ~ Create new Visualization

[Hack The Box - Academy](https://academy.hackthebox.com/module/211/section/2255)

`event.code:4625 AND winlog.event_data.SubStatus:0xC0000072`
La consulta KQL filtra los datos en Kibana para mostrar eventos que tienen el código de evento de Windows 4625 (intentos fallidos de inicio de sesión) y el valor SubStatus de 0xC0000072 para cuentas desabilitadas.

![Pasted image 20251220201308](../Fotos/Pasted%20image%2020251220201308.png)

`event.code:4625 AND winlog.event_data.SubStatus:0xC0000072 AND @timestamp >= "2023-03-03T00:00:00.000Z" AND @timestamp <= "2023-03-06T23:59:59.999Z"`
Mediante esta consulta, los analistas SOC pueden identificar intentos fallidos de inicio de sesión contra cuentas deshabilitadas que tuvieron lugar entre el 3 y el 6 de marzo de 2023

`event.code:4625 AND user.name: admin*`
La consulta Kibana KQL filtra los datos en Kibana para mostrar eventos que tienen el código de evento de Windows 4625 (intentos fallidos de inicio de sesión) y donde el nombre de usuario comienza con "admin", como "admin", "administrator", "admin123", etc.

`winlog.logon.type`

![Pasted image 20251220201449](../Fotos/Pasted%20image%2020251220201449.png)

`group.name`

![Pasted image 20251220201846](../Fotos/Pasted%20image%2020251220201846.png)

`NOT user.name: *$ AND winlog.channel.keyword: Security`
- No se contabilicen registros no relacionados ya cuando hay filtros de por medio etc.

![Pasted image 20251220202642](../Fotos/Pasted%20image%2020251220202642.png)

-----

![Pasted image 20251220200445](../Fotos/Pasted%20image%2020251220200445.png)

![Pasted image 20251220200640](../Fotos/Pasted%20image%2020251220200640.png)

----

`user.name.keyword`
`values 1000`

`host.hostname.keyword`
`values 1000`
(Connected To) Si esque le asignames Display Names

`winlog.logon.type.keyword`
`values 1000`

`related.ip.keyword`
`value 1000`
(Conected from) Si esque le asignames Display Names

----

`winlog.event_data.MemberSid.keyword`
¿Qué usuario fue añadido o eliminado del grupo? Cual usuario fue correspondida la accion

`winlog.event_data.SubjectUserName`
Usuario que inicio sesion

`event.action.keyword`
¿El usuario fue añadido o eliminado del grupo? La accion

![Pasted image 20251220201344](../Fotos/Pasted%20image%2020251220201344.png)

Aplicando Display Name

![Pasted image 20251220201201](../Fotos/Pasted%20image%2020251220201201.png)

----
Filtros usados en Ejercicio 3 HTB

![Pasted image 20251220202130](../Fotos/Pasted%20image%2020251220202130.png)

-----
Que no muestre

![Pasted image 20251220201009](../Fotos/Pasted%20image%2020251220201009.png)

![500](../Fotos/Pasted%20image%2020260110125724.png)    

![Pasted image 20260110125740](../Fotos/Pasted%20image%2020260110125740.png)

*Pregunta 1*
Navega hasta http://[IP objetivo]:5601, haz clic en el interruptor de navegación lateral y luego en "Panel de control". Explora la visualización refinada que creamos o la visualización "Intentos fallidos de inicio de sesión [Todos los usuarios]", si está disponible, e introduce el número de inicios de sesión de la cuenta sql-svc1 como respuesta.
Respuesta: 2
# Intentos fallidos de inicio de session (Todos los usuarios)
`user.name.keyword` y damos en `count of records`

![Pasted image 20260110142314](../Fotos/Pasted%20image%2020260110142314.png)

Agregamos `host.hostname.keyword`

![Pasted image 20260110142816](../Fotos/Pasted%20image%2020260110142816.png)

**Importante**
`user.name.keyword` El nombre de usuario de las personas que inician sesión
`host.hostname.keyword` La máquina en la que se produjo el intento de inicio de sesión.
`count of records` El número de veces que ha ocurrido el evento

Save and Return (sale en la esquina superior derecha de la foto anterior) : Resultado

![Pasted image 20260110143314](../Fotos/Pasted%20image%2020260110143314.png)

Save : No olvidemos guardar también el panel de control. Podemos hacerlo simplemente haciendo clic en el botón "Guardar".

![Pasted image 20260110143436](../Fotos/Pasted%20image%2020260110143436.png)

*Damos en edit para volver y hacer unas nuevas busquedas como veremos a continuacion abajo:*
`user.name.keyword`
`host.hostname.keyword`
`winlog.logon.type.keyword` Tipo de loggin
`count of records`

![Pasted image 20260110150048](../Fotos/Pasted%20image%2020260110150048.png)

```shell-session
NOT user.name: *$ AND winlog.channel.keyword: Security
```

![Pasted image 20260110150258](../Fotos/Pasted%20image%2020260110150258.png)

# Intentos fallidos de inicio de session (Usuarios deshabilitados)

La única diferencia con la de *Todos los usuarios* es el filtro `winlog.event_Data.SubStatus: 0xc0000072`

![Pasted image 20260110162748](../Fotos/Pasted%20image%2020260110162748.png)

*Pregunta 1*
Navega hasta http://[IP objetivo]:5601, haz clic en el interruptor de navegación lateral y luego en "Panel de control". Crea una nueva visualización o edita la visualización "Intentos fallidos de inicio de sesión [Usuario deshabilitado]", si está disponible, para que incluya los datos de intentos fallidos relacionados con usuarios deshabilitados, incluyendo el tipo de inicio de sesión. ¿Cuál es el tipo de inicio de sesión en el documento devuelto?
Respuesta: `interactive`

*Pregunta 2*
 Navega hasta http://[IP objetivo]:5601, haz clic en el interruptor de navegación lateral y luego en "Panel de control". Crea una nueva visualización o edita la visualización "Intentos fallidos de inicio de sesión [Solo usuarios administradores]", si está disponible, para que incluya los datos de intentos fallidos de inicio de sesión donde el campo de nombre de usuario contiene la palabra clave "admin" en cualquier parte de él. ¿Qué deberías especificar después de user.name: en la consulta KQL?
 Respuesta: `*admin*`
 
![Pasted image 20260110163405](../Fotos/Pasted%20image%2020260110163405.png)

# Inicio de sesión RDP exitoso relacionado con cuentas de servicio

![600](../Fotos/Pasted%20image%2020260110165103.png)

![Pasted image 20260110165043](../Fotos/Pasted%20image%2020260110165043.png)

---

`user.name.keyword`
`host.hostname.keyword`
`related.ip.keyword` (Vendría a ser la maquina que inicio la conexión)

![Pasted image 20260110165502](../Fotos/Pasted%20image%2020260110165502.png)

Confirmación RDP efectuado

![600](../Fotos/Pasted%20image%2020260110165830.png)


# Usuarios añadidos o eliminados de un grupo local (dentro de un plazo específico)

**FILTROS:**
`group.name: administrators`
`event.code: 4732, 4733`
- 4732 Se añadió un miembro a un grupo local con seguridad.
- 4733 Un miembro fue retirado de un grupo local con seguridad.

**COLUMNAS:**
`user.name.keyword` Nombre del usuario que realizó la acción
`winlog.event_data.MemberSid.keyword` SID (Security Identifier) del usuario añadido o afectado en un grupo.
`group.name.keyword` A qué grupo se añadió/eliminó un usuario. Nombre del grupo de seguridad afectado. A qué grupo se añadió/eliminó un usuario
`event.action.keyword` Acción específica realizada.
`host.hostname.keyword` Nombre del equipo donde ocurrió el evento.

![Pasted image 20260110174305](../Fotos/Pasted%20image%2020260110174305.png)

![Pasted image 20260110174322](../Fotos/Pasted%20image%2020260110174322.png)

**FECHA** 

![Pasted image 20260110174811](../Fotos/Pasted%20image%2020260110174811.png)    

![Pasted image 20260110174818](../Fotos/Pasted%20image%2020260110174818.png)    

![Pasted image 20260110174825](../Fotos/Pasted%20image%2020260110174825.png)  

Aquí volvimos al dashboard original y bajamos simplemente

![Pasted image 20260110191743](../Fotos/Pasted%20image%2020260110191743.png)

*Pregunta 1*
Navega hasta http://[IP objetivo]:5601, haz clic en el interruptor de navegación lateral y luego en "Panel de control". Extiende la visualización que creamos o la visualización "Usuario añadido o eliminado de un grupo local", si está disponible, e introduce la fecha común en la que se produjeron todos los eventos devueltos como respuesta. Formato de respuesta: 20XX-0X-0X
Respuesta: 2023-03-05

# Skill Assesment

`Visualization 1: Failed logon attempts (All users)`
Una visualización así podría revelar posibles ataques de fuerza bruta. Es importante identificar a cualquier usuario individual con numerosos intentos fallidos o, quizás, varios usuarios conectándose al mismo dispositivo endpoint (o desde). Sin embargo, los datos actuales no apuntan a ningún escenario de este tipo. Sin embargo, hay una anomalía que se nota. **Pista**: Está relacionado con la cuenta "sql-svc1".
*Pregunta 1*
Navega hasta http://[IP objetivo]:5601, haz clic en el interruptor de navegación lateral y luego en "Panel de control". Revisa la visualización "Intentos fallidos de inicio de sesión [Todos los usuarios]" del panel de control "SOC-Alerts". Elige una de las siguientes como respuesta: "Nada sospechoso", "Consulta con Operaciones de TI", "Escala a un analista de nivel 2/3"
FILTRO: `event.action:"logon-failed" AND user.name:"sql-svc1"`
RESPUESTA: Consulta con Operaciones de TI

![Pasted image 20260110201614](../Fotos/Pasted%20image%2020260110201614.png)


`Visualization 2: Failed logon attempts (Disabled user)`
Parece que hay un incidente en el que el usuario "Anni" ha intentado autenticarse, a pesar de que la cuenta estaba desactivada.
*Pregunta 2*
Navega hasta http://[IP objetivo]:5601, haz clic en el interruptor de navegación lateral y luego en "Panel de control". Revisa la visualización "Intentos fallidos de inicio de sesión [Usuario deshabilitado]" del panel de control de "SOC-Alerts". Elige una de las siguientes como respuesta: "Nada sospechoso", "Consulta con Operaciones de TI", "Escala a un analista de nivel 2/3"
FILTRO: `event.action:"logon-failed" AND user.name:"Anni"`
RESPUESTA: Escala a un analista de nivel 2/3

![Pasted image 20260110201854](../Fotos/Pasted%20image%2020260110201854.png)

`Visualization 3: Failed logon attempts (Admin users only)`
**Pista**: Comprueba si todos los eventos tuvieron lugar en estaciones de trabajo de acceso privilegiado (PAWs) o controladores de dominio.
*Pregunta 3*
Navega hasta http://[IP objetivo]:5601, haz clic en el interruptor de navegación lateral y luego en "Panel de control". Revisa la visualización "Intentos fallidos de inicio de sesión [Solo usuarios administradores]" del panel de control "SOC-Alerts". Elige una de las siguientes como respuesta: "Nada sospechoso", "Consulta con Operaciones de TI", "Escala a un analista de nivel 2/3"
FILTRO: `event.action:"logon-failed" AND user.name:*admin*`
RESPUESTA: Nothing suspicious
Todos los eventos en PAWs / DCs  
**Nada sospechoso**

![Pasted image 20260110202716](../Fotos/Pasted%20image%2020260110202716.png)

`Visualization 4: RDP logon for service account`
Las cuentas de servicio en este entorno cumplen una función muy especializada. ¿Notas algo que despierte sospechas?
*Pregunta 4*
Navega hasta http://[IP objetivo]:5601, haz clic en el interruptor de navegación lateral y luego en "Panel de control". Revisa la visualización "Inicio de sesión RDP para la cuenta de servicio" en el panel de control "SOC-Alerts". Elige una de las siguientes como respuesta: "Nada sospechoso", "Consulta con Operaciones de TI", "Escala a un analista de nivel 2/3"
FILTRO: `winlog.event_data.LogonType:10 AND user.name:*svc*`
RESPUESTA: Escalate to a Tier 2/3 analyst
**Service account + RDP = INCIDENTE**
No importa:
- Si fue interno
- Si fue “solo una vez”
- Si fue “para probar”
**Siempre se escala**

![Pasted image 20260110203416](../Fotos/Pasted%20image%2020260110203416.png)

![Pasted image 20260110203406](../Fotos/Pasted%20image%2020260110203406.png)

`Visualization 5: User added or removed from a local group`
Un administrador ha incorporado a una persona (que solo está representada por el valor SID) en el grupo de "Administradores". ¿Deberías escalar a un analista de nivel 2/3 o consultar primero con el departamento de Operaciones de TI?
*Pregunta 5*
Navega hasta http://[IP objetivo]:5601, haz clic en el interruptor de navegación lateral y luego en "Panel de control". Revisa la visualización "Usuario añadido o eliminado de un grupo local" en el panel de control de "SOC-Alerts". Elige una de las siguientes como respuesta: "Nada sospechoso", "Consulta con Operaciones de TI", "Escala a un analista de nivel 2/3"
FILTRO: `group.name:"Administrators"`
RESPUESTA: Consult with IT Operations

![Pasted image 20260110204131](../Fotos/Pasted%20image%2020260110204131.png)

`Visualization 6: Admin logon not from PAW`
Los administradores deberían utilizar exclusivamente los PAWs para conexiones remotas de servidores. ¿Deberías escalar a un analista de nivel 2/3 o consultar primero con el departamento de Operaciones de TI?
*Pregunta 6*
Navega hasta http://[IP objetivo]:5601, haz clic en el interruptor de navegación lateral y luego en "Panel de control". Revisa la visualización "Inicio de sesión de administrador no de PAW" en el panel de control de "SOC-Alerts". Elige una de las siguientes como respuesta: "Nada sospechoso", "Consulta con Operaciones de TI", "Escala a un analista de nivel 2/3"
FILTRO: `winlog.event_data.LogonType:10 user.name:*admin*`
RESPUESTA: Consult with IT Operations
Un administrador **NO debería**:
- Conectarse desde una workstation normal
- Usar su cuenta admin fuera de PAW
- Saltarse el control de acceso privilegiado
Posibles escenarios:
- Credenciales admin comprometidas
- Movimiento lateral
- Malas prácticas graves
- Acceso desde equipo no endurecido
Igualmente aun no hay ataque confirmado asique se consulta

`Visualization 7: SSH Logins`
Recuerda que la cuenta de usuario raíz normalmente no se usa.
*Pregunta 7*
Navega hasta http://[IP objetivo]:5601, haz clic en el interruptor de navegación lateral y luego en "Panel de control". Revisa la visualización "SSH Logins" del panel de control "SOC-Alerts". Elige una de las siguientes como respuesta: "Nada sospechoso", "Consulta con Operaciones de TI", "Escala a un analista de nivel 2/3"
FILTRO: `winlog.event_data.LogonType:10 AND user.name:*admin*`
RESPUESTA: Escalate to a Tier 2/3 analyst

![Pasted image 20260110211901](../Fotos/Pasted%20image%2020260110211901.png)

# **Triad**
# Triaging Process

- **Revisión inicial:** Examinar la alerta, metadatos y registros asociados.
- **Clasificación:** Determinar gravedad e impacto según política.
- **Correlación:** Relacionar con otras alertas o incidentes; buscar IOCs.
- **Enriquecimiento:** Recolectar contexto extra (red, archivos, memoria, inteligencia de amenazas).
- **Evaluación de riesgo:** Analizar impacto y probabilidad de daño.
- **Respuesta:** Tomar acción: resolver, investigar o escalar.
- **Escalamiento:** Notificar a equipos internos o externos si es crítico.
- **Monitoreo y cierre:** Supervisar hasta resolución y documentar lecciones aprendidas.
# **Comandos para probar en examen**
# Comando para emplear

**Orden**
Tiempo
→ Autenticación
→ IP
→ Proceso
→ Archivo
→ Persistencia
→ Credenciales
→ Lateral
→ C2
## 1️⃣ Delimitación temporal y alcance (reducción de ruido)
**Objetivo:** reducir millones de eventos a una ventana investigable.
**Buscar**
- Ventanas temporales anómalas
- Hosts más ruidosos
- Picos de eventos
**Comandos**
`@timestamp >= "2023-03-01T00:00:00.000Z" AND @timestamp <= "2023-03-07T23:59:59.999Z" 
`host.hostname:*`
**Alertas**
- A1 – Pico anómalo de eventos (FP/TP)
- A2 – Host con volumen anormal de logs (TP si coincide con ataque)
## 2️⃣ Autenticaciones fallidas y exitosas (entrada inicial)
**Objetivo:** detectar fuerza bruta, abuso de cuentas o acceso inicial.
**Buscar**
- Fallos repetidos
- Cuentas admin / servicio
- LogonType sospechosos
**Comandos**
`event.code:4625` 
`event.code:4624 AND winlog.event_data.LogonType:3`
`event.code:4624 AND winlog.event_data.LogonType:10`
`event.code:4625 AND user.name:*admin*`

![Pasted image 20260317184905](../Fotos/Pasted%20image%2020260317184905.png)

**Alertas**
- A3 – Fuerza bruta genérica (FP/TP)
- A4 – Ataque a cuenta admin (TP)
- A5 – Login RDP exitoso tras múltiples fallos (TP)
## 3️⃣ Identificación de IPs sospechosas (pivot principal)
**Objetivo:** identificar origen del ataque.
**Buscar**
- IPs repetidas
- IPs externas
- IPs con múltiples tipos de eventos
**Comandos**
`source.ip:*` 
`(event.code:4624 OR event.code:4625) AND source.ip:*`
**Alertas**
- A6 – IP externa autenticándose (TP)
- A7 – IP pivot con auth + procesos + DNS (TP)
## 4️⃣ Creación y ejecución de procesos (payload inicial)
**Objetivo:** ver **qué se ejecutó y desde dónde**.
**Buscar**
- Sysmon Event ID 1
- Office / OneNote / Script como padre
**Comandos**
`event.code:1`
`event.code:1 AND process.parent.name:("ONENOTE.EXE" OR "WINWORD.EXE" OR "EXCEL.EXE")`
`process.name:"powershell.exe"`
**Alertas**
- A8 – Documento ejecutando código (TP)
- A9 – OneNote como dropper (TP)
- A10 – PowerShell lanzado por Office (TP)
## 5️⃣ PowerShell y LOLBins (Living off the Land)
**Objetivo:** detectar ejecución encubierta.
**Buscar**
- Descargas remotas
- Argumentos sospechosos
- LOLBins comunes
**Comandos**
`process.name:"powershell.exe" AND process.args:*`
`process.command_line:*download*`
`process.name:("bitsadmin.exe" OR "certutil.exe" OR "mshta.exe")`
**Alertas**
- A11 – PowerShell descargando payload (TP)
- A12 – Uso de LOLBins para evasión (TP)
## 6️⃣ Escritura de archivos y drop de binarios
**Objetivo:** confirmar infección real.
**Buscar**
- Escritura en rutas públicas
- Ejecutables recién creados
**Comandos**
`event.code:11` 
`event.code:11 AND file.directory:"C:\\Users\\Public"`
`event.code:11 AND file.extension:"exe"`
**Alertas**
- A13 – Drop de binario en carpeta pública (TP)
- A14 – Escritura sospechosa post-ejecución (TP)
## 7️⃣ Persistencia (el atacante quiere quedarse)
**Objetivo:** detectar mecanismos de anclaje.
**Buscar**
- Run Keys
- Scheduled Tasks
- Servicios
**Comandos**
`event.code:13 AND message:"*Run*"`
`Register-ScheduledTask`
`event.code:4698`
**Alertas**
- A15 – Persistencia vía Run Key (TP)
- A16 – Scheduled Task maliciosa (TP)
## 8️⃣ Credenciales y escalada de privilegios
**Objetivo:** determinar impacto real.
**Buscar**
- Mimikatz
- LSASS
- Credenciales explícitas
**Comandos**
`process.name:"mimikatz.exe"`
`event.code:4648`
`process.command_line:*lsass*`
**Alertas**
- A17 – Dump de LSASS (TP)
- A18 – Uso de credenciales explícitas (TP)
## 9️⃣ Movimiento lateral
**Objetivo:** ver propagación del ataque.
**Buscar**
- RDP
- SMB
- Herramientas de lateral movement
**Comandos**
`winlog.event_data.LogonType:10`
`event.code:4624 AND related.ip:*`
`process.name:"psexec.exe"`
**Alertas**
- A19 – Movimiento lateral vía RDP (TP)
- A20 – Uso de PsExec (TP)
## 🔟 C2 y exfiltración
**Objetivo:** confirmar compromiso completo.
**Buscar**
- DNS sospechoso
- HTTP extraño
- Servicios públicos
**Comandos**
`dns.question.name:*`
`http.url:*`
`request.headers.User-Agent:*`
`destination.ip:*`
**Alertas**
- A21 – Dominio C2 sospechoso (TP)
- A22 – User-Agent anómalo (TP)
- A23 – Comunicación con IP externa persistente (TP)
## 🔎 Alertas adicionales (para superar 27)
**Pivot avanzado**
`process.executable:"C:\\Users\\*"`
`NOT process.executable:"C:\\Windows\\System32\\*"`
**Alertas**
- A24 – Binario fuera de rutas estándar (TP)
- A25 – Masquerading de binarios legítimos (TP)
- A26 – Proceso sin padre esperado (TP)
- A27 – Encadenamiento anómalo de procesos (TP)

