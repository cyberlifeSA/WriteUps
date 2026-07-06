`C:\Logs\SAMPLES-SPLUNK\EVTX-ATTACK-SAMPLES-master\Credential Access\ACL_ForcePwd_SPNAdd_User_Computer_Accounts.evtx`

```bash
index=attack_lab source="C:\\Logs\\SAMPLES-SPLUNK\\EVTX-ATTACK-SAMPLES-master\\Credential Access\\ACL_ForcePwd_SPNAdd_User_Computer_Accounts.evtx"
| sort 0 + _time
| rex field=Message "Propietario:\s(?<propietario>[^\r\n]+)"
| table _time, EventCode, ComputerName, host, User, propietario, splunk_server, Message
```

![Pasted image 20260506144834](../Fotos/Pasted%20image%2020260506144834.png)
🔴EVENTO INICIAL CRÍTICO
🔹 EventCode 1102
- **Qué es:** _Audit Log Cleared_ (borrado de logs)
- **Usuario:** `bob`
- **Significado:**
    - Intento de **borrar evidencia**
    - Esto casi siempre es **malicioso** en un DC
    
👉 **Nota para apuntes:**
1102 = indicador fuerte de intrusión (Defense Evasion)

![Pasted image 20260506151502](../Fotos/Pasted%20image%2020260506151502.png)
🟠 2. ACCESO A OBJETOS AD (RECON / PREPARACIÓN)
🔹 EventCode 4662
- **Qué es:** _An operation was performed on an object_
- **Traducción:** acceso a objetos de Active Directory
- Aparece con:
    - `Control Access`
    - `Write Property`

👉 **Interpretación:**
- `bob` está interactuando con objetos de AD
- No es solo lectura → hay **modificación**

Es un evento generado en el **Domain Controller** relacionado con **Active Directory**
🔹 EventCode 4662
👉 _“An operation was performed on an object”_
`bob` **NO solo se comunica** con el objeto  
👉 `bob` está **MODIFICANDO propiedades del objeto de Active Directory**

![Pasted image 20260506151302](../Fotos/Pasted%20image%2020260506151302.png)

![Pasted image 20260506162153](../Fotos/Pasted%20image%2020260506162153.png)
🔴 3. MODIFICACIÓN DE GPO (PERSISTENCIA / PRIV ESC)
🔹 EventCode 5136
- **Qué es:** _Directory Service Object Modified_
- Objeto:
```
CN=POLICIES,CN=SYSTEMClase: groupPolicyContainer
```
- Atributo modificado:
```
versionNumber
```
👉 **Qué significa esto:**
- Está modificando una **GPO (Group Policy Object)**

Modificar `versionNumber`:
- Fuerza a los clientes a **reaplicar la GPO**
- Se usa cuando:
    - Se cambia una GPO maliciosa
    - Se quiere propagar configuración

👉 **Esto NO es normal repetidamente**

5136 sobre GPO = posible persistencia vía Group Policy
`bob` está modificando una **Group Policy (GPO)**

![Pasted image 20260506164906](../Fotos/Pasted%20image%2020260506164906.png)
🔴 4. CAMBIOS EN AUDITORÍA (DEFENSE EVASION)
🔹 EventCode 4719
- **Qué es:** _System audit policy changed_
👉 Cambios:
- Logon
- Object Access
- Directory Service Access
👉 **Significado:**
- El atacante está:
    - Reduciendo visibilidad
    - O manipulando qué se registra
👉 **Nota:**
4719 después de 1102 = patrón claro de evasión

![Pasted image 20260506171006](../Fotos/Pasted%20image%2020260506171006.png)
🔴 5. MODIFICACIÓN DE USUARIO (POSIBLE PRIV ESC)
🔹 EventCode 4738
- **Qué es:** _User account changed_
- Actor: `bob`
- Target: `alice`

👉 Aunque no muestra atributos:
- Igual es sospechoso
- Puede ser:
    - cambio de permisos
    - delegación
    - flags ocultos
👉 **Nota:**
4738 sin cambios visibles ≠ benigno

`bob` **modificó la cuenta de `alice`**
👉 Dentro de su sesión válida:
- `bob` ya está autenticado
- tiene permisos (o los consiguió)
- usa esos permisos para **cambiar atributos de `alice`**

![Pasted image 20260507095537](../Fotos/Pasted%20image%2020260507095537.png)
🔴6. CAMBIO DE CUENTA DE EQUIPO
🔹 EventCode 4742
- **Qué es:** _Computer account changed_
👉 **Significado:**
- Modificación de máquina (`ALICE$`)
- Puede ser:
    - SPN manipulation
    - delegación
    - movimiento lateral

---
🔥 Cadena completa:
1. `1102` → borra logs
2. `4662` → accede a AD
3. `5136` → modifica GPO
4. `4719` → cambia auditoría
5. `4738` → modifica usuario
6. `4742` → modifica máquina

👉 Es un patrón típico de:
✔️ Ataque en Active Directory
Probable:
- **Persistence via GPO**
- **Privilege Escalation**
- **Defense Evasion**