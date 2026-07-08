- --
- Tags: #wireshark #cyberdefenders #clueskyransomware_lab #cyberchef 
- --

Conocer la IP de origen del ataque permite a los equipos de seguridad responder rápidamente a posibles amenazas. ¿Puedes identificar la IP de origen responsable de la posible actividad de escaneo de puertos?

![](../../Fotos/Pasted%20image%2020260707172519.png)

**Respuesta:** 87.96.21.84

Durante la investigación, es esencial determinar la cuenta a la que se dirige el atacante. ¿Puedes identificar el nombre de usuario de la cuenta a la que se dirigía?

![](../../Fotos/Pasted%20image%2020260707173956.png)

**Respuesta:** sa

Necesitamos determinar si el atacante logró acceder. ¿Puedes proporcionar la contraseña correcta que descubra el atacante?

![](../../Fotos/Pasted%20image%2020260707174147.png)

**Respuesta:** cyb3rd3f3nd3r$

Los atacantes suelen cambiar algunos ajustes para facilitar el movimiento lateral dentro de una red. ¿Qué configuración activó el atacante para controlar aún más al host objetivo y ejecutar más órdenes?

![](../../Fotos/Pasted%20image%2020260707174651.png)
El ID de evento `15457` es un evento de auditoría de seguridad generado por Microsoft SQL Server cuando se modifica la `configuración xp_cmdshell` configuración.

**Respuesta:** xp_cmdshell

La inyección de procesos es utilizada a menudo por atacantes para escalar privilegios dentro de un sistema. ¿En qué proceso inyectó el atacante el C2 para obtener privilegios administrativos?

![](../../Fotos/Pasted%20image%2020260707182214.png)

![](../../Fotos/Pasted%20image%2020260707182330.png)

**Respuesta:** winlogon.exe

Tras la escalada de privilegios, el atacante intentó descargar un archivo. ¿Puedes identificar la URL de este archivo descargado?

![](../../Fotos/Pasted%20image%2020260707184454.png)

**Respuesta:** http://87.96.21.84/checking.ps1

Entender qué grupo de Identificador de Seguridad (SID) verifica el script malicioso para verificar los privilegios del usuario actual puede proporcionar información sobre las intenciones del atacante. ¿Puedes proporcionar el SID específico del grupo que se está revisando?

![](../../Fotos/Pasted%20image%2020260707184831.png)

**Respuesta:** S-1-5-32-544

Windows Defender desempeña un papel fundamental en la defensa frente a las amenazas cibernéticas. Si un atacante lo desactiva, el sistema se vuelve más vulnerable a ataques posteriores. ¿Cuáles son las claves de registro que utiliza el atacante para desactivar las funcionalidades de Windows Defender? Proporciónalos en el mismo orden en que se ha encontrado.

![](../../Fotos/Pasted%20image%2020260707185401.png)

**Respuesta:** DisableAntiSpyware,DisableRoutinelyTakingAction,DisableRealtimeMonitoring,SubmitSamplesConsent,SpynetReporting

¿Puedes determinar la URL del segundo archivo descargado por el atacante?

![](../../Fotos/Pasted%20image%2020260707185541.png)

**Respuesta:** http://87.96.21.84/del.ps1

Identificar tareas maliciosas y entender cómo se usaron para la persistencia ayuda a reforzar las defensas frente a futuros ataques. ¿Cuál es el nombre completo de la tarea creada por el atacante para mantener la persistencia?

![](../../Fotos/Pasted%20image%2020260707191218.png)

**Respuesta:** \Microsoft\Windows\MUI\LPupdate

Según tu análisis del segundo archivo malicioso, ¿cuál es el ID MITRE de la táctica principal que intenta lograr el segundo archivo?

![](../../Fotos/Pasted%20image%2020260707192359.png)

**Respuesta:** TA0005

¿Cuál es el script de PowerShell invocado que usa el atacante para volcar credenciales?

![](../../Fotos/Pasted%20image%2020260707192743.png)

**Respuesta:** Invoke-PowerDump.ps1

Comprender qué credenciales han sido comprometidas es esencial para evaluar la magnitud de la brecha de datos. ¿Cuál es el nombre del archivo de texto guardado que contiene las credenciales volcadas?

![](../../Fotos/Pasted%20image%2020260707200536.png)

![](../../Fotos/Pasted%20image%2020260707200558.png)

**Respuesta:** hashes.txt

Conociendo a los hosts objetivo durante la fase de reconocimiento del atacante, el equipo de seguridad puede priorizar sus esfuerzos de remediación en estos hosts específicos. ¿Cómo se llama el archivo de texto que contiene los hosts descubiertos?

![](../../Fotos/Pasted%20image%2020260707201229.png)

**Respuesta:** extracted_hosts.txt

Tras el volcado de hash, el atacante intentó desplegar ransomware en el host comprometido, propagándolo al resto de la red mediante actividades previas de movimiento lateral usando SMB. Se te proporciona la muestra de ransomware para un análisis más detallado. ¿Al realizar un análisis conductual, cómo se llama el archivo de la nota de rescate?

![](../../Fotos/Pasted%20image%2020260707203745.png)

![](../../Fotos/Pasted%20image%2020260707203815.png)

![](../../Fotos/Pasted%20image%2020260707205027.png)

**Respuesta:** # DECRYPT FILES BLUESKY #

En algunos casos, existen herramientas de descifrado disponibles para familias específicas de ransomware. Identificar el apellido puede conducir a una posible solución de descifrado. ¿Cómo se llama esta familia de ransomware?

![](../../Fotos/Pasted%20image%2020260707204943.png)

**Respuesta:** BlueSky