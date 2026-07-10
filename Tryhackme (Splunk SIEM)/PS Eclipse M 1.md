- --
- Tags: #splunk #siem 
- - -

[TryHackMe — PD Eclipse : Resumen | por J Linton | Medio](https://medium.com/@evigmorke.linton/tryhackme-ps-eclipse-write-up-877b39d8b2a2)

*Pregunta 1*
Se ha descargado un binario sospechoso en el endpoint. ¿Cómo se llamaba el binario?
Info: Filtré usando `Index=*`, y fui directamente al filtro "**Image**" a la izquierda, originalmente para ver cuántos comandos de PowerShell se usaron, ya que PowerShell es muy frecuente por atacantes una vez dentro de una máquina. También encontré un ejecutable muy fuera de lugar listado allí. Este ejecutable se guarda en una carpeta \Temp\, que también es un buen IOC para confirmar que probablemente se trata de una actividad maliciosa.
Filtro: `index=*`

![Pasted image 20260117194226](../Fotos/Pasted%20image%2020260117194226.png)

Respuesta: OUTSTANDING_GUTTER.exe

*Pregunta 2*
¿De qué dirección se descargó el binario? **Añade http://** a tu respuesta y desactiva la URL.
Filtro: `OUTSTANDING_GUTTER.exe`

![Pasted image 20260117194554](../Fotos/Pasted%20image%2020260117194554.png)

Filtro: `powershell.exe`

![Pasted image 20260117194649](../Fotos/Pasted%20image%2020260117194649.png)

![Pasted image 20260117194946](../Fotos/Pasted%20image%2020260117194946.png)

Respuesta: hxxp[://]886e-181-215-214-32[.]ngrok[.]io

*Pregunta 3*
¿Qué ejecutable de Windows se utilizó para descargar el binario sospechoso? Entra en el camino completo.
Info: analizamos la misma alerta anterior
Filtro: `powershell.exe`

![Pasted image 20260117195815](../Fotos/Pasted%20image%2020260117195815.png)

Respuesta: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe

*Pregunta 4*
¿Qué comando se ejecutó para configurar el binario sospechoso y que se ejecutara con privilegios elevados?
Filtro: `powershell.exe OUTSTANDING_GUTTER.exe`

![Pasted image 20260117200353](../Fotos/Pasted%20image%2020260117200353.png)

Respuesta: C:\Windows\system32\schtasks.exe" /Create /TN OUTSTANDING_GUTTER.exe /TR C:\Windows\Temp\COUTSTANDING_GUTTER.exe /SC ONEVENT /EC Application /MO *[System/EventID=777] /RU SYSTEM /f

*Pregunta 5*
¿Qué permisos usará el binario sospechoso? ¿Cuál era la orden para ejecutar el binario con privilegios elevados? **(Formato:** **Usuario + + + Línea de comandos)**
Info: esto hace llamado directamente a NT AUTHORITY/SYSTEM REPASAR
Filtro: `powershell.exe OUTSTANDING_GUTTER.exe`

![Pasted image 20260117202121](../Fotos/Pasted%20image%2020260117202121.png)

Respuesta: NT AUTHORITY\SYSTEM;"C:\Windows\system32\schtasks.exe" /Run /TN OUTSTANDING_GUTTER.exe

*Pregunta 6*
El binario sospechoso conectado a un servidor remoto. ¿A qué dirección se conectaba? **Añade http://** a tu respuesta y desactiva la URL.
Filtro: `OUTSTANDING_GUTTER.exe`

![Pasted image 20260117202459](../Fotos/Pasted%20image%2020260117202459.png)

![Pasted image 20260117202643](../Fotos/Pasted%20image%2020260117202643.png)

Respuesta: hxxp[://]9030-181-215-214-32[.]ngrok[.]io

*Pregunta 7*
Se descargaba un script de PowerShell en la misma ubicación que el binario sospechoso. ¿Cómo se llamaba el archivo?
Filtro: `ps1`

![Pasted image 20260117202815](../Fotos/Pasted%20image%2020260117202815.png)

![700](../Fotos/Pasted%20image%2020260117202942.png)

Respuesta: script.ps1

*Pregunta 8*
El guion malicioso fue marcado como malicioso. ¿Cuál crees que era el nombre real del script malicioso?
Filtro: `ps1`

![Pasted image 20260117203102](../Fotos/Pasted%20image%2020260117203102.png)

![Pasted image 20260117203131](../Fotos/Pasted%20image%2020260117203131.png)

Respuesta: BlackSun.ps1

*Pregunta 9*
Se guardó una nota de ransomware en disco, que puede servir como IOC. ¿Cuál es el camino completo por el cual se salvó la nota de rescate?
Filtro: `txt`

![Pasted image 20260117203235](../Fotos/Pasted%20image%2020260117203235.png)

Respuesta: C:\Users\keegan\Downloads\vasg6b0wmw029hd\BlackSun_README.txt

*Pregunta 10*
El script guardaba un archivo de imagen en disco para reemplazar el fondo de pantalla del escritorio del usuario, que también puede servir como IOC. ¿Cuál es el recorrido completo de la imagen?
Filtro: BlackSun

![Pasted image 20260117203538](../Fotos/Pasted%20image%2020260117203538.png)

Respuesta: C:\Users\Public\Pictures\blacksun.jpg