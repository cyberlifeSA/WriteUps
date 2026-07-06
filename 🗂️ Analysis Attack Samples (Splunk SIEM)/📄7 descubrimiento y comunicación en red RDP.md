`C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Command and Control\DE_sysmon-3-rdp-tun.evtx`

Trabajable
```bash
index=attack_lab source="C:\\Logs\\SAMPLES-SPLUNK\\EVTX-ATTACK-SAMPLES-master\\Command and Control\\DE_sysmon-3-rdp-tun.evtx"
| eval Time=strftime(_time,"%Y-%m-%d %H:%M:%S.%3N")
| rex field=Message "Usuario:\s(?<User>[^\r\n]+)"
| rex field=Message "Dominio:\s(?<Domain>[^\r\n]+)"
| rex field=Message "Dirección de red de origen:\s(?<SrcIP>[^\r\n]+)"
| rename ComputerName as Host
| sort 0 _time
| table Time Host User Domain SrcIP Message
```

Un poco mas seccionada por field un poco
```bash
index=attack_lab source="C:\\Logs\\SAMPLES-SPLUNK\\EVTX-ATTACK-SAMPLES-master\\Command and Control\\DE_sysmon-3-rdp-tun.evtx"
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

![Pasted image 20260504124824](../Fotos/Pasted%20image%2020260504124824.png)
`plink.exe` = cliente de **SSH por línea de comandos** (parte de PuTTY)

 🧠 Análisis de parámetros
### 🔹 `10.0.2.18`
👉 IP remota (atacante o servidor C2)
### 🔹 `-P 80`
👉 Puerto 80 (HTTP)
⚠️ **sospechoso**
- SSH normalmente usa 22
- Aquí usan 80 → evasión (parece tráfico web)
### 🔹 `-C`
👉 Compression (compresión)
### 🔹 `-R 127.0.0.3:4444:127.0.0.2:3389`
🔥 **ESTO ES LO MÁS IMPORTANTE**
#### Traducción:
- `-R` = Remote Port Forwarding
👉 Significa:
```
Desde el servidor remoto:127.0.0.3:4444 → redirige a → 127.0.0.2:3389 (RDP)
```
“Abrir un puerto en el servidor remoto y conectarlo a algo local”

| Parte       | Significado              |
| ----------- | ------------------------ |
| `127.0.0.3` | IP en el servidor remoto |
| `4444`      | Puerto que se abre       |
| `127.0.0.2` | IP en la máquina víctima |
| `3389`      | Puerto RDP               |
El atacante abre el puerto **4444 en su servidor**  
y todo lo que llegue ahí lo envía al **RDP (3389)** de la víctima

```bash
ATACANTE (10.0.2.18)
        │
        │ conecta a puerto 4444
        ▼
[SSH túnel]
        ▼
VICTIMA (127.0.0.2:3389)
        │
        ▼
RDP activo
```

La máquina víctima se conecta por SSH al atacante y abre un túnel que permite acceder a su RDP desde el puerto 4444 usando el usuario “test”.

👉 El **4444 vive en el atacante**  
👉 El **3389 vive en la víctima**
Pero:  
👉 el túnel SSH los une

 🔹 **SSH**
 📌 ¿Puede usar puerto 80?
👉 **Sí, SSH puede funcionar en cualquier puerto**, incluido el 80.

🚨 ¿Por qué usar puerto 80?
👉 Para evadir controles:
- Firewalls suelen permitir:
    - 80 (HTTP)
    - 443 (HTTPS)
- Bloquean:
    - 22 (SSH)
👉 Entonces el atacante hace:
“Disfrazo SSH como tráfico web”

![Pasted image 20260504150719](../Fotos/Pasted%20image%2020260504150719.png)
 ⚠️ Punto clave
👉 `UI0Detect.exe` **NO es el ataque**
👉 Es un **efecto secundario**
🧠 Entonces… ¿qué indica realmente?
 🔴 Indica que:
- Hay **actividad interactiva inusual**
- Un proceso/servicio intentó mostrar algo
- Puede estar relacionado con acceso remoto

![Pasted image 20260504153550](../Fotos/Pasted%20image%2020260504153550.png)
- `smss` → crea sesiones
- `csrss` → maneja interfaz
- `winlogon` → login
- `LogonUI` → pantalla de login

✔️ Todo esto es **normal**

![Pasted image 20260504155405](../Fotos/Pasted%20image%2020260504155405.png)
🧠 **DLLs cargadas**
```
VBoxDisp.dll (Oracle)vga.dll (Microsoft)
```
👉 Máquina virtual (VirtualBox)
✔️ Normal en lab

![Pasted image 20260504155608](../Fotos/Pasted%20image%2020260504155608.png)## 🔥 **TSTheme.exe**
```
Parent: svchost.exe -k DcomLaunchUser: PC01\IEUserIntegrity: High
```
 🔎 Qué significa esto
👉 `TSTheme.exe` está relacionado con:
- **Terminal Services (RDP)**
🔥 Traducción real:
Se está levantando un entorno gráfico para una sesión remota (RDP)

🔹 **ms-wbt-server**
📌 Qué es
`ms-wbt-server` = nombre del servicio para:
👉 **RDP (Remote Desktop Protocol)**  
👉 Traducción: _Escritorio Remoto_
`ms-wbt-server = RDP = puerto 3389`

Esto ocurre después de que viste:
- `plink.exe` (túnel SSH) ✔️
- `ms-wbt-server` (RDP) ✔️
👉 Entonces esto confirma:
🔥 **El atacante logró abrir una sesión RDP**

`ms-wbt-server` 👉 Que hay **tráfico asociado a RDP (puerto 3389)**

![Pasted image 20260504155830](../Fotos/Pasted%20image%2020260504155830.png)
 🔎 Qué es LLMNR
👉 **LLMNR**
- Resolución de nombres en red local
- Alternativa a DNS 
- Alternativa cuando falla DNS
**⚠️ Esto es clave en pentesting**
LLMNR se usa para:
- **Name resolution poisoning**
- Capturar hashes (NTLM)
👉 Muy típico en ataques tipo:
- Responder
- MITM

🔴 Acceso remoto exitoso mediante túnel SSH + sesión RDP (NO NORMAL)

![Pasted image 20260504172539](../Fotos/Pasted%20image%2020260504172539.png)
La máquina está intentando resolver nombres en la red local
*LLMNR es peligroso porque permite:*
- **LLMNR poisoning**
- **Captura de hashes NTLM**
👉 *Herramientas típicas:*
- **Responder**
- **Inveigh**

👉 Resolver **nombres → IP**

![Pasted image 20260504172821](../Fotos/Pasted%20image%2020260504172821.png)
*Tráfico NetBIOS*
👉 Esto es:
- Comunicación SMB (compartición de archivos)
- Inicio de sesión en red
⚠️ **Importante**
Después de acceso inicial, esto puede indicar:
- Enumeración de red
- Movimiento lateral
- Intento de autenticación

La máquina está intentando establecer una **sesión SMB (compartición de red)** con otra máquina

![Pasted image 20260504172954](../Fotos/Pasted%20image%2020260504172954.png)
*Tráfico WS-Discovery*
- Descubrimiento de dispositivos en red
- Similar a SSDP
Muchos eventos:
```BASH
udp multicast  
ws-discovery
```
👉 El sistema está buscando otros dispositivos/servicios en la red

👉 Descubrir **servicios/dispositivos**

🔴 **actividad de descubrimiento y comunicación en red**
🔴 La máquina comprometida está interactuando con la red interna (posible fase de post-explotación)

