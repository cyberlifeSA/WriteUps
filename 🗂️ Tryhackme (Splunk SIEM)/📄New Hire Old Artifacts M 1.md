- --
- Tags: #splunk #siem 
- - -
*Pregunta 1*
Un visor de contraseñas del navegador web se ejecutó en la máquina infectada. ¿Cuál es el nombre del binario? Entra en el camino completo.
Filtro: `C:`
![Pasted image 20260118144908](../Fotos/Pasted%20image%2020260118144908.png)
Respuesta: C:\Users\FINANC~1\AppData\Local\Temp\11111.exe
![Pasted image 20260323104647](../Fotos/Pasted%20image%2020260323104647.png)

*Pregunta 2*
¿Cuál es el nombre de la empresa?  
Filtro: `company`
![Pasted image 20260118145304](../Fotos/Pasted%20image%2020260118145304.png)
o
Filtro: `password viewer`
![Pasted image 20260118145341](../Fotos/Pasted%20image%2020260118145341.png)
Respuesta: NirSoft

*Pregunta 3*
Otro binario sospechoso que se ejecutaba desde la misma carpeta se ejecutaba en la estación de trabajo. ¿Cómo se llamaba el binario? ¿Cuál es su nombre original de archivo? (**formato: archivo.xyz, archivo.xyz**)  
Filtro: `ioniclarge.exe`
![Pasted image 20260118151008](../Fotos/Pasted%20image%2020260118151008.png)      ![Pasted image 20260118151022](../Fotos/Pasted%20image%2020260118151022.png)
![Pasted image 20260118151058](../Fotos/Pasted%20image%2020260118151058.png)
Respuesta: ioniclarge.exe,PalitExplorer.exe

*Pregunta 4*
El binario de la pregunta anterior hizo dos conexiones salientes a una dirección IP maliciosa. ¿Cuál era la dirección IP? Introduce la respuesta en formato defan.  
Filtro: `ioniclarge.exe`
![Pasted image 20260118152512](../Fotos/Pasted%20image%2020260118152512.png)
Respuesta: 2[.]56[.]59[.]42

*Pregunta 5*
El mismo binario hizo algún cambio en una clave de registro. ¿Cuál fue el camino clave?  
Filtro: `ioniclarge.exe EventCode=12`
![Pasted image 20260118153248](../Fotos/Pasted%20image%2020260118153248.png)
Respuesta: HKLM\SOFTWARE\Policies\Microsoft\Windows Defender

*Pregunta 6*
Algunos procesos fueron eliminados y los binarios asociados fueron eliminados. ¿Cuáles eran los nombres de los dos binarios? (**formato: archivo.xyz, archivo.xyz**)  
Filtro: `taskill`
![Pasted image 20260118153935](../Fotos/Pasted%20image%2020260118153935.png)
![Pasted image 20260118153917](../Fotos/Pasted%20image%2020260118153917.png)
Respuesta: WvmIOrcfsuILdX6SNwIRmGOJ.exe

*Pregunta 7*
El atacante ejecutaba varios comandos dentro de una sesión de PowerShell para cambiar el comportamiento de Windows Defender. ¿Cuál fue la última orden ejecutada en la serie de comandos similares?  
Filtro:
```
powershell 
| table _time CommandLine 
| dedup CommandLine
```
![Pasted image 20260118155136](../Fotos/Pasted%20image%2020260118155136.png)
Respuesta: powershell WMIC /NAMESPACE:\\root\Microsoft\Windows\Defender PATH MSFT_MpPreference call Add ThreatIDDefaultAction_Ids=2147737394 ThreatIDDefaultAction_Actions=6 Force=True
![Pasted image 20260323114650](../Fotos/Pasted%20image%2020260323114650.png)

*Pregunta 8*
Basándonos en la respuesta anterior, ¿cuáles fueron los cuatro IDs que estableció el atacante? Introduce la respuesta en orden de ejecución. (formato: 1º, 2º, 3º, 4º)  
![Pasted image 20260118155345](../Fotos/Pasted%20image%2020260118155345.png)
![Pasted image 20260118155336](../Fotos/Pasted%20image%2020260118155336.png)
Respuesta: 2147735503,2147737010,2147737007,2147737394

*Pregunta 9*
Otro binario malicioso se ejecutó en la estación de trabajo infectada desde otra ubicación de AppData. ¿Cuál fue el camino completo hacia el binario?  
Filtro: `index=* C:\\Users\\Finance01\\AppData`
![Pasted image 20260118155827](../Fotos/Pasted%20image%2020260118155827.png)
Respuesta: C:\Users\Finance01\AppData\Roaming\EasyCalc\EasyCalc.exe
![Pasted image 20260323123602](../Fotos/Pasted%20image%2020260323123602.png) ![Pasted image 20260323123632](../Fotos/Pasted%20image%2020260323123632.png)
![Pasted image 20260323123656](../Fotos/Pasted%20image%2020260323123656.png)

*Pregunta 10*
¿Cuáles eran las DLL que se cargaron desde el binario de la pregunta anterior? Introduce las respuestas en orden alfabético. (formato: file1.dll,file2.dll,file3.dll)  
Filtro: `EasyCalc dll`
![Pasted image 20260118160411](../Fotos/Pasted%20image%2020260118160411.png)
Respuesta: nw_elf.dll, ffmpeg.dll, nw.dll

