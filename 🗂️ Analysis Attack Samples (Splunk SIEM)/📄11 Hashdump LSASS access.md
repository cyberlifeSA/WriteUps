`source="C:\\Logs\\SAMPLES-SPLUNK\\EVTX-ATTACK-SAMPLES-master\\Credential Access\\CA_hashdump_4663_4656_lsass_access.evtx"`

```bash
index=attack_lab source="C:\\Logs\\SAMPLES-SPLUNK\\EVTX-ATTACK-SAMPLES-master\\Credential Access\\CA_hashdump_4663_4656_lsass_access.evtx"
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
| table Time Host Integrity Process Message
```

![Pasted image 20260517122709](../Fotos/Pasted%20image%2020260517122709.png)
## ¿Qué es `cscript.exe`?
`cscript.exe` = Windows Script Host en consola.
Se usa para ejecutar:
- `.vbs`
- `.js`
- scripts administrativos
Pero también es muy abusado por malware.

# IOC

## Acceso solicitado
```
Leer desde la memoria del procesoMáscara de acceso: 0x10
```
La máscara:
```
0x10
```
corresponde a:
```
PROCESS_VM_READ
```
# ¿Qué significa?
El proceso intentó leer memoria de LSASS.
Eso es exactamente lo que hace:
- Mimikatz
- ProcDump
- comsvcs.dll
- herramientas de hashdump
para extraer credenciales.
# Accesos solicitados
```
DELETEREAD_CONTROLWRITE_DACWRITE_OWNERSYNCHRONIZE
```
Esto es muy sospechoso para un proceso normal como `cscript.exe`.

# Contexto ofensivo
El atacante probablemente ejecutó:
- un VBScript
- JS script
- LOLBin technique
- wrapper de Mimikatz
- script de dumping
usando `cscript.exe`.

# MITRE 
Credential Access
T1003.001 — OS Credential Dumping: LSASS Memory

| Indicador         | Relevancia                  |
| ----------------- | --------------------------- |
| `lsass.exe`       | objetivo clásico de dumping |
| `PROCESS_VM_READ` | lectura de memoria          |
| `cscript.exe`     | LOLBin sospechoso           |
| Event ID 4656     | handle request              |
| Event ID 4663     | object access               |
| Access Mask 0x10  | lectura de memoria LSASS    |