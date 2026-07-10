- --
- Tags: #treathhunting #splunk #tryhackme 
----
## Host-Based IOCs

|**Type**|**Value**|
|---|---|
|NPM Package|`healthchk-lib@1.0.1`|
|Registry Path|`HKCU\Software\Microsoft\Windows\CurrentVersion\Run`|
|Registry Value Name|`Windows Update Monitor`|
|Registry Value Data|`powershell.exe -NoP -W Hidden -EncodedCommand <base64>`|
|Downloaded File Path|`%APPDATA%\SystemHealthUpdater.exe`|
|PowerShell Command|`Invoke-WebRequest -Uri ... -OutFile ...`|
|Process Execution|`powershell.exe -NoP -W Hidden -EncodedCommand ...`|
|Script Artifact|Found in `package.json` under `"postinstall"`|

## Network-Based IOCs

|**Type**|**Value**|
|---|---|
|Download URL|`http://global-update.wlndows.thm/SystemHealthUpdater.exe`|
|Hostname Contacted|`global-update.wlndows.thm`|
|Protocol|HTTP (unencrypted)|
|Port|80|
|Traffic Behavior|Outbound file download to `%APPDATA%` via PowerShell|
# 1. Supply Chain Attack `T1195` → Supply Chain Compromise
**el malware viene dentro de una librería legítima**
```bash
index=* (powershell OR sysmon)
("EncodedCommand" OR "-enc" OR "-NoP" OR "-W Hidden")
```            

![Pasted image 20260507125120](../Fotos/Pasted%20image%2020260507125120.png)

Instalación del paquete malicioso `npm install healthchk-lib@1.0.1`
NPM Package:
Registry Path:
Registry Value Name:
Registry Value Data:
Downloaded File Path:
PowerShell Command:
URL:
# 2. Ejecución automática (postinstall) `T1059` → Command and Scripting Interpreter
```bash
index=* "postinstall"
```

![Pasted image 20260507125616](../Fotos/Pasted%20image%2020260507125616.png)

**Execution (ejecución automática)**  
→ no requiere interacción del usuario

# 3. Ejecución del payload en memoria `T1059` → Command execution `T1027` → Obfuscation / Uso de XOR (ofuscación)`T1140` → Deobfuscate/Decode Files 🔥 PERSISTENCIA
```bash
index=* (powershell OR sysmon)
("EncodedCommand" OR "-enc" OR "-NoP" OR "-W Hidden")
```            

![Pasted image 20260507124508](../Fotos/Pasted%20image%2020260507124508.png)

- **-NoP (NoProfile)** → no carga perfil → evita detección
- **-W Hidden (Window Hidden)** → ejecución invisible
- **-EncodedCommand** → comando en Base64 (ofuscación)
Defense Evasion + Execution

![Pasted image 20260507140456](../Fotos/Pasted%20image%2020260507140456.png)

![Pasted image 20260507192738](../Fotos/Pasted%20image%2020260507192738.png)

El script PowerShell decodificado implementa un loader que desencripta un payload mediante XOR y lo ejecuta en memoria usando `Invoke-Expression`, eliminando rastros para evadir detección.
El malware se asegura de **volver a ejecutarse después del reboot**

---
# Conclusión
El incidente se originó a partir de la instalación del paquete `healthchk-lib@1.0.1` mediante npm, el cual corresponde a un caso de compromiso de la cadena de suministro (_supply chain attack_). Este paquete contenía un script `postinstall` que se ejecutó automáticamente durante la instalación, sin requerir interacción adicional del usuario.

Dicho script lanzó un proceso de PowerShell con comandos ofuscados (`-EncodedCommand`, `-NoProfile`, `-WindowStyle Hidden`), con el objetivo de evadir mecanismos de detección. A través de esta ejecución, se descargó y preparó un payload malicioso en directorios temporales del sistema (AppData\Local\Temp), lo que corresponde a una fase de _staging_.

Posteriormente, el malware utilizó técnicas de _DLL side-loading_, aprovechando el binario legítimo `cleanmgr.exe` para cargar librerías maliciosas desde una ruta controlada por el atacante. El payload fue desencriptado mediante operaciones XOR y ejecutado en memoria utilizando `Invoke-Expression`, evitando así dejar artefactos evidentes en disco.

Finalmente, se identificó persistencia mediante la modificación de claves de registro en `HKU\<SID>\Software\Microsoft\Windows\CurrentVersion\Run`, permitiendo la ejecución automática del malware en cada inicio de sesión del usuario.

Este comportamiento evidencia un ataque avanzado que combina técnicas de ejecución remota, evasión de defensas, persistencia y abuso de herramientas legítimas del sistema.

---

![Pasted image 20260507200118](../Fotos/Pasted%20image%2020260507200118.png)

![Pasted image 20260507195856](../Fotos/Pasted%20image%2020260507195856.png) 

![Pasted image 20260507195924](../Fotos/Pasted%20image%2020260507195924.png)

![Pasted image 20260507200014](../Fotos/Pasted%20image%2020260507200014.png)

![Pasted image 20260507200046](../Fotos/Pasted%20image%2020260507200046.png)

**En Ingles**
**Threath Report**
# Threat Case Report: Compromised NPM Package Leading to Persistence

## Executive Summary

### Overview

An attack campaign was identified involving the installation of a compromised NPM package, "healthchk-lib@1.0.1," which led to the execution of a malicious script and establishment of persistence through registry modifications. This attack chain exploited the supply chain compromise tactic, utilizing PowerShell scripts to download and execute malicious payloads.

### Attack Chain Details

### Stage 1: Initial Access

- **Description**: The attacker gained initial access by deploying a compromised NPM package. Upon installation, a `postinstall.ps1` script was triggered, employing PowerShell with hidden and encoded commands to download and execute a malicious file from `global-update.windows.thm`.
- **Tactic & Technique**: Initial Access (TA0001), Supply Chain Compromise (T1195)
- **Indicators of Compromise (IOC)**:
    - Compromised NPM Package: `healthchk-lib@1.0.1`
    - PowerShell Command: `powershell.exe -NoP -W Hidden -EncodedCommand`
    - Malicious File Path: `APPDATA\SystemHealthUpdater.exe`

### Stage 2: Malicious Script Execution Post Package Installation

- **Description**: The post-install script executed a hidden PowerShell command, downloading `SystemHealthUpdater.exe` into the `%APPDATA%` directory.
- **Tactic & Technique**: Execution (TA0002), Command and Scripting Interpreter (T1059)
- **IOC**:
    - Encoded PowerShell Command: `Invoke-WebRequest -Uri $url -OutFile $dest`

### Stage 3: Registry Persistence

- **Description**: Persistence was achieved by modifying the registry key `HKCU:\Software\Microsoft\Windows\CurrentVersion\Run`. The modification ensured the malicious PowerShell command was executed at each user logon.
- **Tactic & Technique**: Persistence (TA0003), Boot or Logon Autostart Execution (T1547)
- **IOC**:
    - Registry Path: `HKCU:\Software\Microsoft\Windows\CurrentVersion\Run`
    - Registry Value Name: `Windows Update Monitor`
    - Registry Value Data: Encoded PowerShell command for persistence

### Impact and Findings

The attack utilized a compromised NPM package to infiltrate the system, executing a series of PowerShell scripts to maintain persistence via registry modifications. This sophisticated attack vector underscores the vulnerability of supply chains and the need for rigorous package validation processes. Immediate remediation and monitoring actions are recommended to mitigate further exposure and potential lateral movement within the network.

---
**En Espanol**
# Informe de caso de amenaza: Paquete NPM comprometido que genera persistencia

## Resumen ejecutivo

### Descripción general

Se identificó una campaña de ataque que involucró la instalación de un paquete NPM comprometido, "healthchk-lib@1.0.1", lo que condujo a la ejecución de un script malicioso y al establecimiento de persistencia mediante modificaciones del registro. Esta cadena de ataque explotó la táctica de compromiso de la cadena de suministro, utilizando scripts de PowerShell para descargar y ejecutar cargas útiles maliciosas.

### Detalles de la cadena de ataque

### Etapa 1: Acceso inicial

- **Descripción**: El atacante obtuvo acceso inicial mediante la implementación de un paquete NPM comprometido. Tras la instalación, se activó un script `postinstall.ps1`, que empleó PowerShell con comandos ocultos y codificados para descargar y ejecutar un archivo malicioso desde `global-update.windows.thm`.
- **Táctica y técnica**: Acceso inicial (TA0001), Compromiso de la cadena de suministro (T1195)
- **Indicadores de compromiso (IOC)**:

- Paquete NPM comprometido: `healthchk-lib@1.0.1`

- Comando de PowerShell: `powershell.exe -NoP -W Hidden -EncodedCommand`

- Ruta de archivo maliciosa: `APPDATA\SystemHealthUpdater.exe`

### Etapa 2: Ejecución de script malicioso después de la instalación del paquete

- **Descripción**: El script posterior a la instalación ejecutó un comando oculto de PowerShell, descargando `SystemHealthUpdater.exe` en el directorio `%APPDATA%`.
- **Táctica y técnica**: Ejecución (TA0002), Intérprete de comandos y scripts (T1059)
- **IOC**:

- Comando de PowerShell codificado: `Invoke-WebRequest -Uri $url -OutFile $dest`

### Etapa 3: Persistencia en el registro

- **Descripción**: La persistencia se logró modificando la clave del registro `HKCU:\Software\Microsoft\Windows\CurrentVersion\Run`. Esta modificación garantizó la ejecución del comando malicioso de PowerShell en cada inicio de sesión del usuario.
- **Táctica y técnica**: Persistencia (TA0003), Ejecución de inicio automático al arrancar o iniciar sesión (T1547)
- **IOC**:

- Ruta del registro: `HKCU:\Software\Microsoft\Windows\CurrentVersion\Run`

- Nombre del valor del registro: `Windows Update Monitor`

- Datos del valor del registro: Comando de PowerShell codificado para la persistencia

### Impacto y hallazgos

El ataque utilizó un paquete NPM comprometido para infiltrarse en el sistema, ejecutando una serie de scripts de PowerShell para mantener la persistencia mediante modificaciones del registro. Este sofisticado vector de ataque subraya la vulnerabilidad de las cadenas de suministro y la necesidad de procesos rigurosos de validación de paquetes. Se recomiendan acciones inmediatas de remediación y monitorización para mitigar una mayor exposición y el posible movimiento lateral dentro de la red.

![Pasted image 20260507200210](../Fotos/Pasted%20image%2020260507200210.png)

![Pasted image 20260507200511](../Fotos/Pasted%20image%2020260507200511.png)

![Pasted image 20260507200518](../Fotos/Pasted%20image%2020260507200518.png)

