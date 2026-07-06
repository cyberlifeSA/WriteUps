`C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Command and Control\bits_openvpn.evtx`

```bash
index=attack_lab source="C:\\Logs\\SAMPLES-SPLUNK\\EVTX-ATTACK-SAMPLES-master\\Command and Control\\bits_openvpn.evtx"
| sort 0 + _time
| rex field=Message "Propietario:\s(?<propietario>[^\r\n]+)"
| table _time, EventCode, ComputerName, host, User, propietario, splunk_server, Message
```

![Pasted image 20260505122856](../Fotos/Pasted%20image%2020260505122856.png)
Son logs del servicio **Background Intelligent Transfer Service (BITS)**
 📌 BITS (Background Intelligent Transfer Service)
- Servicio de Windows que descarga archivos en segundo plano
- Usado por:
    - Windows Update
    - OneDrive
    - Google Update
- Funciona con jobs (trabajos de transferencia)

📌 Código: `0x80072EE7`
- Significado:  
    → No puede resolver el nombre DNS
👉 Traducción simple:
La máquina NO puede traducir el dominio (ej: g.live.com) a una IP

![Pasted image 20260505123014](../Fotos/Pasted%20image%2020260505123014.png)
🧠 4. Interpretación (esto es lo que te evalúan en CDSA)
🔹 Comportamiento observado:
- Múltiples intentos de descarga
- Fallan todos con el mismo error
- Se reintenta constantemente (cada ~10 segundos)
- Finalmente el job se cancela

🔹 Servicios involucrados:
- Google Update
- OneDrive
- Microsoft (fonts, config)

*Aquí es donde debes pensar como analista:*
✔️ Indicadores de benigno:
- URLs legítimas:
    - `clients2.google.com`
    - `g.live.com`
    - `fs.microsoft.com`
- Procesos válidos:
    - `GoogleUpdate.exe`
    - `OneDriveStandaloneUpdater.exe`
    - `svchost.exe`
- Usuarios del sistema:
    - `NT AUTHORITY\SYSTEM`
    - `LOCAL SERVICE`
👉 Esto apunta a actividad **legítima**

 ⚠️ Pero lo sospechoso es:
- Fallo constante de DNS
- Múltiples servicios afectados

👉 Eso NO es normal

 *🧩 Posibles causas:*
1. 🔌 **Problema de red**
    - Sin conexión a internet
    - DNS mal configurado
2. 🛡️ **Bloqueo intencional**
    - Firewall
    - Proxy
    - DNS sinkhole
3. 🧪 **Entorno de laboratorio**
    - Muy típico en máquinas tipo:
        - `MSEDGEWIN10`
    - Sin salida a internet
4. 🚨 **Defensa o evasión**
    - Malware que bloquea updates (menos probable aquí)

Los eventos analizados corresponden al servicio BITS intentando realizar múltiples descargas legítimas desde servicios de Google y Microsoft. Sin embargo, todas las transferencias fallan con el código 0x80072EE7, indicando un problema de resolución DNS. Este comportamiento sugiere una falla de conectividad o una restricción de red, más que actividad maliciosa directa.

**Servicios afectados**

|Servicio|Proceso|Tipo|
|---|---|---|
|Google Update|`GoogleUpdate.exe`|Tercero|
|OneDrive Updater|`OneDriveStandaloneUpdater.exe`|Cloud|
|Windows Font / Config|`svchost.exe`|Sistema|
|Microsoft Maps|`svchost.exe`|Sistema|
|BITS (core)|`svchost.exe`|Infraestructura|
 ![Pasted image 20260505125121](../Fotos/Pasted%20image%2020260505125121.png)
 ![Pasted image 20260505125151](../Fotos/Pasted%20image%2020260505125151.png)
 ![Pasted image 20260505125209](../Fotos/Pasted%20image%2020260505125209.png)
 
 🔁 Ciclo repetitivo:
1. BITS crea job
2. Intenta descargar
3. ❌ Falla (`0x80072EE7`)
4. Reintenta
5. Cancela
6. 🔁 Vuelve a crear job

*Esto NO es solo “falla DNS” Un sistema en estado persistente de fallo de conectividad con reintentos automáticos de servicios críticos*

**Posible root cause**
Ordenado por probabilidad:
1. 🧪 **Máquina sin salida a internet (LAB)**
2. 🌐 **DNS mal configurado**
3. 🔥 **Firewall / proxy bloqueando tráfico**
4. 🛑 **DNS sinkhole (entorno defensivo)**

✔️ BITS puede ser usado por malware  
❌ Aquí no aplica porque:

- URLs legítimas
- Procesos legítimos
- Sin ejecución rara
- Sin rutas sospechosas

![Pasted image 20260505131906](../Fotos/Pasted%20image%2020260505131906.png)
- Uso de ruta en:
    - `AppData\Local\{GUID}`
- BITS creando jobs sobre rutas no típicas de instalación

 **🚨 6. ¿Por qué parece sospechoso?**
Porque:
- BITS es técnica de ataque (Living Off The Land)
- Muchos retries parecen C2 beaconing

![Pasted image 20260505141813](../Fotos/Pasted%20image%2020260505141813.png)*Cambio de comportamiento*
```
código 0x0 → SUCCESS
```
✔️ Ahora sí hay conexión

![Pasted image 20260505141952](../Fotos/Pasted%20image%2020260505141952.png)
🚨 4. Evento MÁS IMPORTANTE (posible detección real)
```
BITS inició descarga desde:http://r3---sn-5hnedn7z.gvt1.com/.../GoogleUpdateSetup.exe
```
🔎 Traducción:
- Descarga de binario
- Dominio de Google (CDN legítimo)

---
*⚠️ Cosas que debes cuestionar*

1. **Muchos errores DNS seguidos**
    - Puede ser:
        - Máquina sin internet (lab)
        - DNS bloqueado (defensa)
        - O evasión (malware esperando conexión)
2. **Uso intensivo de BITS**
    - Técnica MITRE:
        ```
        T1197 – BITS Jobs
        ```
    - Malware usa BITS porque:
        - Es sigiloso
        - Usa procesos confiables
3. **Descarga de ejecutables**
    ```
    GoogleUpdateSetup.exechrome_updater.exe
    ```
    👉 Normal, pero también **puede ser suplantado**

---
Para contar **descargas exitosas en BITS**, buscas:
```
Evento ID 4 → "Se completó el trabajo de transferencia"
```
Y normalmente acompañado de:
```
Código de estado: 0x0 (SUCCESS)
```
![Pasted image 20260505144431](../Fotos/Pasted%20image%2020260505144431.png)
![Pasted image 20260505144649](../Fotos/Pasted%20image%2020260505144649.png)
![Pasted image 20260505155315](../Fotos/Pasted%20image%2020260505155315.png)

Se observa actividad del servicio BITS generando múltiples trabajos de transferencia asociados a procesos legítimos como GoogleUpdate.exe y OneDriveStandaloneUpdater.exe. Inicialmente, los intentos fallan con el código 0x80072EE7, indicando problemas de resolución DNS. Posteriormente, la conectividad se restablece y se completan descargas exitosas desde dominios legítimos. No se identifican indicadores claros de compromiso, aunque el uso intensivo de BITS representa una técnica común en ataques tipo Living-off-the-Land.

---
![Pasted image 20260506115448](../Fotos/Pasted%20image%2020260506115448.png)Primer bloque (12:55:52) → 🔥 comportamiento CRÍTICO a analizar
Proceso: `GoogleUpdate.exe`
Eventos:
- `3` → crea job
- `5` → cancela
- `61` → falla DNS (`0x80072EE7`)

![Pasted image 20260506131702](../Fotos/Pasted%20image%2020260506131702.png)
la misma que arrba
![Pasted image 20260506131728](../Fotos/Pasted%20image%2020260506131728.png)
**Notepad.exe NO debería:**
- Crear jobs BITS ❌
- Descargar archivos ❌
👉 Esto rompe completamente el comportamiento esperado.
- `notepad.exe` → usado como launcher para BITS
👉 Esto es típico en:
- malware
- scripts ofensivos
- técnicas de evasión
🔍 Evaluación:
- Imgur es legítimo ✔️
- Pero en ciberseguridad:
    - se usa para **payload staging**
    - ocultar datos en imágenes (steganography)
👉 Contexto + notepad = **sospechoso**

Se identificaron trabajos BITS iniciados por `notepad.exe`, descargando recursos desde dominios externos (Imgur), lo cual no corresponde al comportamiento esperado del sistema y podría indicar ejecución de herramientas maliciosas o técnicas LOLBins.

![Pasted image 20260506141335](../Fotos/Pasted%20image%2020260506141335.png)
```
Ruta de proceso: C:\Windows\SysWOW64\notepad.exe
```
👉 Indica **el proceso que solicitó la creación del job BITS**  
(es decir, quien llamó a la API de BITS)
✔ Eso indica directamente el proceso que creó el job BITS.

---
 🔴 1. Uso anómalo de proceso
- `notepad.exe` creando jobs BITS  
    ➡️ **Esto es anómalo y sospechoso**

 🔴 2. Descarga desde dominio externo no típico
- `imgur.com`  
    ➡️ No es infraestructura del sistema

🟡 3. Archivos tipo `.png`
- Podrían ser:
    - imágenes normales ✔️
    - payload oculto (stego / staging) ⚠️

👉 **NO hay evidencia suficiente de C2 (Command & Control)**  
👉 **SÍ hay evidencia de comportamiento sospechoso (posible abuso de BITS + LOLBins)**