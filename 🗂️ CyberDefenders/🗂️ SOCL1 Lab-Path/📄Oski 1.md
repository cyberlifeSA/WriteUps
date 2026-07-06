- --
- Tags: #virustotal  #cyberdefenders #oski_lab #anyrun
- --
![Pasted image 20260703160052](../../Fotos/Pasted%20image%2020260703160052.png)

Determinar el momento de creación del malware puede aportar información sobre su origen. ¿Cuál fue la época de la creación del malware?
![Pasted image 20260703160213](../../Fotos/Pasted%20image%2020260703160213.png)
2022-09-28 17:40

Identificar el servidor de mando y control (C2) con el que se comunica el malware puede ayudar a rastrear hasta el atacante. ¿Con qué servidor C2 se comunica el malware en el archivo PPT?
![Pasted image 20260703160358](../../Fotos/Pasted%20image%2020260703160358.png)
http://171.22.28.221/5c06c05b7b34e8e6.php

Identificar las acciones iniciales del malware tras la infección puede proporcionar información sobre sus objetivos principales. ¿Cuál es la primera biblioteca que solicita el malware tras la infección?
![Pasted image 20260703161046](../../Fotos/Pasted%20image%2020260703161046.png)
sqlite3.dll

Al examinar el [informe proporcionado en Any.run](https://any.run/report/a040a0af8697e30506218103074c7d6ea77a84ba3ac1ee5efae20f15530a19bb/d55e2294-5377-4a45-b393-f5a8b20f7d44), ¿qué clave RC4 utiliza el malware para descifrar su cadena codificada en base64?
![Pasted image 20260703162453](../../Fotos/Pasted%20image%2020260703162453.png)
5329514621441247975720749009

Examinando las técnicas MITRE ATT&CK mostradas en el [informe sandbox de Any.run](https://app.any.run/tasks/d55e2294-5377-4a45-b393-f5a8b20f7d44), identifica la técnica principal MITRE (no las subtécnicas) que utiliza el malware para robar la contraseña del usuario.
![Pasted image 20260703163717](../../Fotos/Pasted%20image%2020260703163717.png)
T1555

Al examinar los procesos hijos que aparecen en el [informe sandbox Any.run](https://app.any.run/tasks/d55e2294-5377-4a45-b393-f5a8b20f7d44), ¿a qué directorio se dirige el malware para eliminar todos los archivos **DLL**?
![Pasted image 20260703163047](../../Fotos/Pasted%20image%2020260703163047.png)
C:\ProgramData

Comprender el comportamiento del malware tras la exfiltración de datos puede ofrecer información sobre sus técnicas de evasión. Analizando los procesos hijos, tras exfiltrar con éxito los datos del usuario, **¿cuántos segundos** tarda el malware en **autoeliminarse**?
![Pasted image 20260703163147](../../Fotos/Pasted%20image%2020260703163147.png)
5