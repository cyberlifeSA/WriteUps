`c:\\logs\\samples-splunk\\evtx-attack-samples-master\\command and control\\de_rdp_tunnel_5156.evtx`

```bash
index=attack_lab source="c:\\logs\\samples-splunk\\evtx-attack-samples-master\\command and control\\de_rdp_tunnel_5156.evtx"
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

**Ordenado al revez** (en esta primera parte las primeras fotos son las ultimas y en la segunda van en orden ya que el filtro no sorteo bien de mas viejo a nuevo aun aplicadose, seguramente por tema de parseo)

![Pasted image 20260502130811](../Fotos/Pasted%20image%2020260502130811.png)
- Puerto: **1900 UDP**
- Uso:
    - UPnP (dispositivos en red)
    - descubrimiento automático

Un servicio de Windows (svchost) está recibiendo tráfico multicast de descubrimiento

🧠 **Puerto 5357**
👉 Este puerto es:
🔥 **WS-Discovery / Web Services for Devices (WSD)**
Relacionado con:
- impresoras
- dispositivos de red
- discovery interno

🔥 Windows está descubriendo dispositivos en red usando protocolos legítimos (SSDP → WSD)

🚨 ¿Es malicioso?
❌ Normalmente NO
Esto es típico de:
- Windows Network Discovery
- servicios UPnP
- impresoras/red

#🧩 2. Puerto origen → `3702`
👉 Clave directa de examen:
**3702 = WS-Discovery (Web Services Discovery)**
📌 Qué hace:
- descubre impresoras
- dispositivos en red
- servicios Windows

 🧩 4. Puerto destino → `53614`
👉 Puerto alto (ephemeral)
📌 Qué significa:
- asignado dinámicamente
- usado por el servicio que recibe la respuesta

🟢 **100% normal** "Ruido del sistema"

👉 “Los eventos corresponden a tráfico legítimo de descubrimiento de servicios en red (SSDP y WS-Discovery), generado por servicios del sistema bajo svchost.exe, utilizando multicast en IPv4 e IPv6 y comunicación loopback para procesamiento interno.”

---
![Pasted image 20260502133846](../Fotos/Pasted%20image%2020260502133846.png)
👉 “El servicio bajo svchost.exe inicia una comunicación saliente desde loopback hacia un grupo multicast (239.255.255.250) usando WS-Discovery (UDP/3702), con el objetivo de anunciar o descubrir servicios en la red local.”

 *🟢 NORMAL (lo que tienes)*
✔ multicast  
✔ puertos 1900 / 3702  
✔ svchost  
✔ tráfico local + loopback
👉 Esto es:
🔵 **Service Discovery legítimo**

*🔴 SOSPECHOSO sería si vieras:*
- svchost → IP externa rara (internet)
- puertos raros (no 1900/3702)
- procesos hijos extraños
- correlación con:
    - `powershell`
    - `cmd`
    - `winrm`
    - `rundll32`

![Pasted image 20260502134445](../Fotos/Pasted%20image%2020260502134445.png)
🔹 `AtBroker.exe`
📌 Qué es:
- **Accessibility Tool Broker**
- Maneja herramientas de accesibilidad en Windows
⚠️ `AtBroker.exe` es un **LOLBIN potencial**
Porque:
✔ corre como SYSTEM  
✔ puede lanzar otros binarios  
✔ se ejecuta en contexto privilegiado

🔹 Usuario:
```
S-1-5-18 → SYSTEM
```
👉 Esto ya es 🔥 alto **privilegio**

🔹 Token:
```
TokenElevationTypeDefault (1)
```
📌 Significa:
Token completo (sin restricciones)
👉 Traducción:  
✔ proceso con privilegios máximos  
✔ no está limitado por UAC

👉 necesitas identificar:
- ¿Quién lanzó `AtBroker.exe`?

 🟢 CASO NORMAL
`AtBroker.exe` es legítimo cuando:
- usuario abre herramientas de accesibilidad
- login screen (Winlogon)
- RDP sessions
- utilman.exe usage
🔴 CASO SOSPECHOSO
Si ves:
- fuera de contexto (sin interacción de usuario)
- parent raro (`cmd`, `powershell`, `rundll32`)
- correlación con ejecución previa (como tu `runtests.bat`)
👉 entonces:
🔥 **posible abuso para escalación o ejecución**

| SID                  | Cuenta          | Privilegios |
| -------------------- | --------------- | ----------- |
| `S-1-5-18`           | **SYSTEM**      | 🔥 MÁXIMO   |
| `S-1-5-19`           | Local Service   | Bajo        |
| `S-1-5-20`           | Network Service | Medio       |
| `S-1-5-21-...-500`   | Administrator   | Alto        |
| `S-1-5-21-...-1000+` | Usuario normal  | Variable    |

![Pasted image 20260502135746](../Fotos/Pasted%20image%2020260502135746.png)
👉 un proceso de **RDP** fue inicializado o está activo
Porque:
✔ `rdpclip.exe` solo aparece cuando hay sesión RDP  
✔ o cuando se inicializa el entorno de escritorio remoto

🧠 Qué hace:
- Permite copiar/pegar entre:
    - tu PC local
    - la sesión remota
👉 Ejemplo:
- copias texto en tu PC
- lo pegas dentro del RDP

👉 Pregunta obligatoria SOC:
¿Quién lanzó `rdpclip.exe`?

![Pasted image 20260502141613](../Fotos/Pasted%20image%2020260502141613.png)
`TSTheme.exe`
👉 ¿Qué es?
- Servicio relacionado con:
    - **Remote Desktop Services (RDP)**
    - manejo de temas visuales en sesiones remotas
    - Maneja los **temas visuales** en sesiones remotas
    - Aplica estilos (colores, ventanas, etc.) cuando te conectas por RDP
    - 👉 En simple: “Hace que la sesión remota se vea ‘bonita’”
👉 Contexto:
- `TSTheme.exe` + SYSTEM + entorno RDP  
    ✔ comportamiento normal

---
**Ordenado Cronologicamente**

A continuacion sucede:
```bash
smss.exe
 └── csrss.exe
      └── winlogon.exe
           └── LogonUI.exe
```
- `smss.exe` → inicia la sesión del sistema
- `csrss.exe` → maneja consola/threads
- `winlogon.exe` → gestiona login
- `LogonUI.exe` → interfaz de login (pantalla)
- SID: `S-1-5-18` → **LOCAL SYSTEM (máximo privilegio)**

![Pasted image 20260502165058](../Fotos/Pasted%20image%2020260502165058.png)

![Pasted image 20260502165042](../Fotos/Pasted%20image%2020260502165042.png)

![Pasted image 20260502165021](../Fotos/Pasted%20image%2020260502165021.png)

![Pasted image 20260502165002](../Fotos/Pasted%20image%2020260502165002.png)

----
![Pasted image 20260502165536](../Fotos/Pasted%20image%2020260502165536.png)
![Pasted image 20260502165553](../Fotos/Pasted%20image%2020260502165553.png)
👉 “Se confirma uso de credenciales explícitas porque el evento muestra que un proceso bajo contexto SYSTEM intentó autenticarse como otro usuario (`admin01`), lo que implica que se proporcionaron credenciales manualmente (Event ID 4648).”

LogonType: 10 (RemoteInteractive (RDP) / 👉 Hubo un **login por RDP**)

👉 “Se intentó iniciar sesión con credenciales explícitas”
Esto significa:
- Se usaron credenciales manualmente
- Ejemplo típico:
    - `runas`
    - RDP
    - tareas programadas

![Pasted image 20260502165752](../Fotos/Pasted%20image%2020260502165752.png)
👉 “Se asignaron privilegios especiales”
Usuario: `admin01`
Privilegios:
- `SeDebugPrivilege` → puede inyectar procesos
- `SeImpersonatePrivilege` → **potencial PrivEsc (Potato attacks)**
- `SeTakeOwnershipPrivilege`
- `SeBackupPrivilege`
- etc.

👉 🔥 En un CTF: Esto ya es **post-exploitation viable**

![Pasted image 20260502165940](../Fotos/Pasted%20image%2020260502165940.png)
🔹 Tráfico de `lsass.exe`
Puerto destino: **88**
👉 🔥 ¿Qué es?
👉 **Kerberos (autenticación en dominio)**
📌 Traducción:
👉 El host está hablando con un **Domain Controller**
IP destino:
- `10.0.2.15`

![Pasted image 20260502170138](../Fotos/Pasted%20image%2020260502170138.png)
🔹 Tráfico puerto 445
👉 SMB
👉 Comunicación interna de red (shares, autenticación)

---
**Ordenar al terminar los logs y ponerlo al final esto con todo**
```bash
[1] BOOT DEL SISTEMA  
└─ smss.exe (Session Manager)  
└─ csrss.exe  
└─ winlogon.exe  
└─ LogonUI.exe
```
👉 Sistema inicia con cuenta:  SID: S-1-5-18 (LOCAL SYSTEM)
```bash
[2] ESPERA DE LOGIN
 └─ winlogon.exe activo
     └─ LogonUI mostrando pantalla
```
```bash
[3] INTENTO DE AUTENTICACIÓN
 └─ winlogon.exe
     └─ "Se intentó iniciar sesión con credenciales explícitas"
         └─ Usuario: admin01
         └─ Origen: 127.0.0.1
```
👉 Uso de credenciales manuales (RDP / runas)
```bash
[4] LOGIN EXITOSO
 └─ Tipo de logon: 10 (RemoteInteractive - RDP)
     └─ Usuario: admin01
     └─ Sesión creada
```
```bash
[5] ASIGNACIÓN DE PRIVILEGIOS
 └─ admin01 obtiene:
     ├─ SeDebugPrivilege
     ├─ SeImpersonatePrivilege
     ├─ SeBackupPrivilege
     ├─ SeTakeOwnershipPrivilege
     └─ etc.
```
👉 Usuario con privilegios ALTOS
```bash
[6] CREACIÓN DE ENTORNO DE SESIÓN RDP
 └─ winlogon.exe
     └─ rdpclip.exe (clipboard)
     └─ TSTheme.exe (temas RDP)
     └─ AtBroker.exe (accesibilidad)
```
👉 Confirmación clara de sesión RDP activa
```bash
[7] ACTIVIDAD DE AUTENTICACIÓN EN RED
 └─ lsass.exe
     └─ Conexiones a puerto 88 (Kerberos)
         └─ Destino: 10.0.2.15 (Domain Controller)
```
👉 Validación contra dominio
```bash
[8] ACTIVIDAD DE RED ADICIONAL
 └─ System (PID 4)
     └─ Conexión a puerto 445 (SMB)
```
```bash
[9] SERVICIOS DE RED (BACKGROUND)
 └─ svchost.exe
     ├─ SSDP → 239.255.255.250:1900
     ├─ WS-Discovery → 239.255.255.250:3702
     ├─ IPv6 multicast → ff02::c
     └─ tráfico local (127.0.0.1)
```
👉 Descubrimiento de red (NORMAL)

---
*Hasta el momento*
```bash
BOOT
 ↓
smss → csrss → winlogon → LogonUI
 ↓
LOGIN (RDP)
 ↓
admin01 autenticado
 ↓
Privilegios altos asignados
 ↓
Procesos RDP (rdpclip, TSTheme)
 ↓
lsass → Kerberos (DC)
 ↓
SMB / red interna
 ↓
svchost → multicast normal
```

---
![Pasted image 20260503140516](../Fotos/Pasted%20image%2020260503140516.png)
Repetidos eventos de lsass.exe del host accedido al DC

![Pasted image 20260503140721](../Fotos/Pasted%20image%2020260503140721.png)
`plunk.exe` : herramienta para realizar conexiones ssh legitima pero en el contexto puede ser malicioso
Habiendo una conexion externa al hsot  y luego el uso de lsass y luego ver esto podria ser una **posible actividad de motivimiento lateral**

![Pasted image 20260503141032](../Fotos/Pasted%20image%2020260503141032.png)
`UI0Detect.exe`: Se crea proceso de ubicacion correcta, bajo cuenta de SYSTEM, es registro normal de actividad

![Pasted image 20260503141519](../Fotos/Pasted%20image%2020260503141519.png)
Este evento el encabezado indica basicamente qu el firewall de windows reviso y permitio esta conexion

![Pasted image 20260503141759](../Fotos/Pasted%20image%2020260503141759.png)
Descubrimiento de red mediante broadcast

![Pasted image 20260503153531](../Fotos/Pasted%20image%2020260503153531.png)