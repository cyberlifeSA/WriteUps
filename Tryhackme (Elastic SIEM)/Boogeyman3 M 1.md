- --
- Tags: #elasticsearch 
- --
**TRYACKME**
## 🎯**Objetivos completados del laboratorio
1. **Ejecución inicial del adjunto malicioso**  
    El CEO abrió **`ProjectFinancialSummary_Q3.pdf`**, iniciando la **Stage 1 payload** con **PID 6392** en `WKSTN-0051` (10.10.155.159 / evan.hutchinson).
2. **Implantación del payload en disco**  
    El malware utilizó **`xcopy.exe`** para copiar `review.dat` al directorio temporal del usuario (técnica _Living off the Land_).
3. **Ejecución del payload implantado**  
    Se ejecutó **`rundll32.exe`** cargando `review.dat`, confirmando la ejecución de código malicioso.
4. **Persistencia mediante tarea programada**  
    El atacante creó la tarea **`Review`** usando `Register-ScheduledTask`.
5. **Establecimiento de comunicación C2**  
    PowerShell inició una conexión hacia **165.232.170.151:80**, confirmando canal de _Command & Control_.
6. **Bypass de UAC (escalada local)**  
    El atacante ejecutó **`fodhelper.exe`** para evadir **UAC (User Account Control)** y operar con altos privilegios.
7. **Descarga de herramienta de volcado de credenciales**  
    Se descargó **Mimikatz** desde GitHub:  
    `https://github.com/gentilkiwi/mimikatz/releases/download/2.2.0-20220919/mimikatz_trunk.zip`
8. **Compromiso de nuevas credenciales locales**  
    Se obtuvieron credenciales:  
    **`itadmin : F84769D250EB95EB2D7D8B4A1C5613F2`**
9. **Enumeración de recursos compartidos remotos**  
    El atacante accedió al archivo **`IT_Automation.ps1`** desde `quicklogistics.org`.
10. **Descubrimiento de nuevas credenciales de dominio**  
    Se identificaron credenciales en texto claro:  
    **`QUICKLOGISTICS\allan.smith : Tr!ckyP@ssw0rd987`**
11. **Movimiento lateral a otra estación**  
    El atacante apuntó a la máquina **`WKSTN-1327`** usando las nuevas credenciales.
12. **Ejecución remota y proceso padre identificado**  
    El comando malicioso se ejecutó bajo **`wsmprovhost.exe`**, típico de **WinRM / PowerShell Remoting**.
13. **Nuevo volcado de credenciales en la segunda máquina**  
    Se obtuvieron credenciales locales:  
    **`administrator : 00f80f2538dcb54e7adc715c0e7091ec`**
14. **Ataque DCSync contra el controlador de dominio**  
    En **DC01**, el atacante volcó hashes adicionales, incluyendo la cuenta **`backupda`**.
15. **Preparación para impacto final (ransomware)**  
    Se descargó el binario de ransomware desde:  
    
    `http://ff.sillytechninja.io/ransomboogey.exe

---
*Pregunta 1*
¿Cuál es el PID del proceso que ejecutó la carga útil inicial de la etapa 1?    
**29 y el 30 de agosto de 2023**.
**ProjectFinancialSummary_Q3.pdf**

![Pasted image 20260112155715](../Fotos/Pasted%20image%2020260112155715.png)

Antes de enviar nuestra respuesta, apunta el nombre de host, la dirección IP del host y el nombre de usuario de la estación de trabajo comprometida del CEO. Esto nos ayudará a mantenernos organizados mientras seguimos el ataque.
`WKSTN-0051` / `10.10.155.159` / `evan.hutchinson`
Respuesta: 6392 `ProcessId`

*Pregunta 2*
La carga útil de la etapa 1 intentó implantar un archivo en otra ubicación. ¿Cuál es el valor completo de línea de comandos de esta ejecución?

![Pasted image 20260112160122](../Fotos/Pasted%20image%2020260112160122.png)

Respuesta: "C:\Windows\System32\xcopy.exe" /s /i /e /h D:\review.dat C:\Users\EVAN~1.HUT\AppData\Local\Temp\review.dat (`process.command_line`)

![Pasted image 20260318003433](../Fotos/Pasted%20image%2020260318003433.png)   

![Pasted image 20260318003513](../Fotos/Pasted%20image%2020260318003513.png)

*Pregunta 3*
El archivo implantado fue finalmente utilizado y ejecutado por la carga útil de la etapa 1. ¿Cuál es el valor completo de línea de comandos de esta ejecución?

![Pasted image 20260112160400](../Fotos/Pasted%20image%2020260112160400.png)

Respuesta: "C:\Windows\System32\rundll32.exe" D:\review.dat,DllRegisterServer (`process.command_line`)

![Pasted image 20260318004304](../Fotos/Pasted%20image%2020260318004304.png)  

![Pasted image 20260318004315](../Fotos/Pasted%20image%2020260318004315.png)

*Pregunta 4*
La carga útil de la etapa 1 estableció un mecanismo de persistencia. ¿Cuál es el nombre de la tarea programada creada por el script malicioso?

![Pasted image 20260112160604](../Fotos/Pasted%20image%2020260112160604.png)

Respuesta: Review (`Register-ScheduledTask`)

![Pasted image 20260318005101](../Fotos/Pasted%20image%2020260318005101.png)

*Pregunta 5*
La ejecución del archivo implantado dentro de la máquina ha iniciado una posible conexión C2. ¿Cuál es la IP y el puerto que utiliza esta conexión?
`powershell.exe and event.provider : "Microsoft-Windows-Sysmon" and event.code : "3"`

![Pasted image 20260112162540](../Fotos/Pasted%20image%2020260112162540.png)

Como sabemos por la última pregunta que el atacante está aprovechando **PowerShell,** intentemos acotar nuestra búsqueda para eso. Por suerte para nosotros, **Quick Logistics LLC** había desplegado [**Sysmon**](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) en la estación de trabajo del CEO, lo que nos da ventaja.
La IP externa está presente en la gran mayoría de los registros **de PowerShell** buscados. **Es sospechoso que una estación de trabajo comprometida se conecte a una dirección IP externa a través de PowerShell.**`destination.ip`
Respuesta: 165.232.170.151:80

*Podria ser mejor esta query correlacionando lo que ya sabemos ya puntando a una ejecicion de un host especifico*
`process.name:powershell.exe AND event.code:3 AND process.command_line:*review*`

*Pregunta 6*
El atacante ha descubierto que el acceso actual es un administrador local. ¿Cuál es el nombre del proceso que utiliza el atacante para ejecutar un bypass UAC?
Ahora que hemos identificado el servidor C2, necesitamos entender cómo el atacante escaló privilegios y eludió **el control de cuentas de usuario (UAC).**
Revisaremos el archivo implantado descrito en la pregunta 2 `review.dat`

![Pasted image 20260112164251](../Fotos/Pasted%20image%2020260112164251.png)

Respuesta: fodhelper.exe (`process.command_line / agent.name / process.parent.executable`)

![Pasted image 20260318125537](../Fotos/Pasted%20image%2020260318125537.png)   

![Pasted image 20260318125848](../Fotos/Pasted%20image%2020260318125848.png)

*Pregunta 7*
Al tener acceso a la máquina con altos privilegios, el atacante intentó volcar las credenciales dentro de la máquina. ¿Cuál es el enlace de GitHub que utiliza el atacante para descargar una herramienta de volcado de credenciales?

![Pasted image 20260112170344](../Fotos/Pasted%20image%2020260112170344.png)

Respuesta: ttps://github.com/gentilkiwi/mimikatz/releases/download/2.2.0-20220919/mimikatz_trunk.zip

*Pregunta 8*
Tras volcar con éxito las credenciales dentro de la máquina, el atacante las utilizó para acceder a otra máquina. ¿Cuál es el nombre de usuario y el hash del nuevo par de credenciales?

![Pasted image 20260112170849](../Fotos/Pasted%20image%2020260112170849.png)

Resultado: itadmin:F84769D250EB95EB2D7D8B4A1C5613F2
*Tener en cuenta que agent.name en este caso es donde se ejecuta el proceso y no a la maquina que se esta moviendo*

![Pasted image 20260318133725](../Fotos/Pasted%20image%2020260318133725.png)

*Pregunta 9*
Usando las nuevas credenciales, el atacante intentó enumerar los archivos compartidos accesibles. ¿Cuál es el nombre del archivo al que accede el atacante desde un recurso compartido remoto?
`quicklogistics.org`

![Pasted image 20260112171617](../Fotos/Pasted%20image%2020260112171617.png)

o

![Pasted image 20260112171852](../Fotos/Pasted%20image%2020260112171852.png)

Resultado: IT_Automation.ps1

![Pasted image 20260318134625](../Fotos/Pasted%20image%2020260318134625.png)  

![Pasted image 20260318134745](../Fotos/Pasted%20image%2020260318134745.png)  

![Pasted image 20260318134801](../Fotos/Pasted%20image%2020260318134801.png)

*Pregunta 10*
Tras obtener el contenido del archivo remoto, el atacante utilizó las nuevas credenciales para moverse lateralmente. ¿Cuál es el nuevo conjunto de credenciales descubierto por el atacante?

![Pasted image 20260112172928](../Fotos/Pasted%20image%2020260112172928.png)

o

![Pasted image 20260112172953](../Fotos/Pasted%20image%2020260112172953.png)

Respuesta: QUICKLOGISTICS\allan.smith:Tr!ckyP@ssw0rd987

*Pregunta 11*
¿Cuál es el nombre de host de la máquina objetivo del atacante para su intento de movimiento lateral?

![Pasted image 20260112174726](../Fotos/Pasted%20image%2020260112174726.png)

Respuesta: WKSTN-1327

*Pregunta 12*
Usando el comando malicioso ejecutado por el atacante desde la primera máquina para moverse lateralmente, ¿cuál es el nombre del proceso padre del comando malicioso ejecutado en la segunda máquina comprometida?

![Pasted image 20260112184215](../Fotos/Pasted%20image%2020260112184215.png)

![250](../Fotos/Pasted%20image%2020260112184338.png)

Respuesta: wsmprovhost.exe (`host.hostname / process.command_line / process.parent.executable / process.parent.name`)

*Pregunta 13*
El atacante entonces volcó los hashes en esta segunda máquina. ¿Cuál es el nombre de usuario y el hash de las credenciales recién volcadas?

![Pasted image 20260112184850](../Fotos/Pasted%20image%2020260112184850.png)

Respuesta: administrator:00f80f2538dcb54e7adc715c0e7091ec

*Pregunta 14*
Tras acceder al controlador de dominio, el atacante intentó volcar los hashes mediante un ataque DCSync. Aparte de la cuenta de administrador, ¿qué cuenta volcó el atacante?

![Pasted image 20260112190107](../Fotos/Pasted%20image%2020260112190107.png)

`host.hostname: DC01 and event.provider: Microsoft-Windows-Sysmon and event.code: 1` VER COMO SABER IDENTIFICAR PARA UTILIZAR Y ENCOTNRAR DATOS DE EVENT.PROVIDER CUAL UTILIZAR
Resultado: backupda

*Pregunta 15*
Tras volcar los hashes, el atacante intentó descargar otro archivo remoto para ejecutar ransomware. ¿Cuál es el enlace que utiliza el atacante para descargar el binario del ransomware?

![Pasted image 20260112190416](../Fotos/Pasted%20image%2020260112190416.png)

Respuesta: http://ff.sillytechninja.io/ransomboogey.exe