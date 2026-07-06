- --
- Tags: #wireshark #reverse_engineering #forensics_tools #btlo #anakus_lab
- --

*P1) Usando Detect it Easy, ¿cuál es el hash SHA256 del malware en cuestión (lnrvmp.dll) y en qué lenguaje estaba escrito? No dudes en utilizar VirusTotal para obtener más información (Formato: SHA256, Idioma)*
![Pasted image 20260620154938](../Fotos/Pasted%20image%2020260620154938.png)
![Pasted image 20260620154540](../Fotos/Pasted%20image%2020260620154540.png)
fb20fc4e474369e6d8626be3ef6e79027c2db3ba0a46f7b68c6981d6859d6d32, cpp

*P2) El valor total de entropía en Detect it Easy nos da una indicación general de la aleatoriedad en todo el archivo, pero la presencia de una sección altamente entrópica indica una parte del archivo que contiene datos comprimidos—empaquetada. Normalmente, una entropía superior a 7,2 se considera maliciosa, ¿cuál es el nombre de esta sección en cuestión? (Formato: .xxxx)*
![Pasted image 20260620155213](../Fotos/Pasted%20image%2020260620155213.png)
.rsrc

*P3) Dado el nivel de entropía del archivo, su reputación en VirusTotal y sus características extrañas, está claro que eso es malicioso. Sin embargo, a veces los atacantes utilizan los nombres de empresas de seguridad en su malware para eludir la detección. En este caso, ¿qué nombre de producto está imitando este malware? (Formato: Nombre del producto)*
![Pasted image 20260620155802](../Fotos/Pasted%20image%2020260620155802.png)
Sophos Anti-Virus

*P4) Usando SigCheck, veamos si el archivo ha sido firmado con un certificado de firma de código, demostrando su validez. Incluye el estado "Verificado" y la "fecha de firma", también conocida como Fecha de Enlace (Formato: Estado, X:XX PM X/X/XXXX)*
![Pasted image 20260620161213](../Fotos/Pasted%20image%2020260620161213.png)
Unsigned, 4:59 PM 9/1/2022

*P5) Veamos el supuesto "hataker.exe" de malware. A juzgar por su hash, no es evidente en la mayoría de las plataformas de malware, aunque no excluye su malicia. Activa Windows Defender y ejecuta el malware hasta que lo detecte. ¿Cuál es el nombre dado a este supuesto programa tipo troyano malicioso? (Formato: Troyano: nombre completo)*
![Pasted image 20260620161630](../Fotos/Pasted%20image%2020260620161630.png)
![Pasted image 20260620161741](../Fotos/Pasted%20image%2020260620161741.png)
![Pasted image 20260620162120](../Fotos/Pasted%20image%2020260620162120.png)
Trojan:Wub32/Meterpreter.RPZIMTB

*P6) Curiosamente, ahora que conocemos el tipo de troyano para el "hataker.exe", podemos asumir con seguridad que el atacante planeaba iniciar una conexión desde el punto final de la víctima a su servidor de mando y control. ¿Cómo se llama este método? (Formato: xxxxxxx xxxxx)*
reverse shell

*P7) Siguiendo el mismo contexto, vamos a analizar más el malware. ¿Qué dominio dinámico está usando el malware hataker.exe para establecer una conexión de vuelta al sistema del atacante? (Formato: Nombre de dominio)*
![Pasted image 20260620164334](../Fotos/Pasted%20image%2020260620164334.png)
0.TCP.NGROK.IO

*P8) Usando Timeline Explorer, mira los "Registros de Amenazas" del incidente, ¿cuántas alertas de alto riesgo se registraron? (Formato: Conteo)*
![Pasted image 20260620170715](../Fotos/Pasted%20image%2020260620170715.png)
6

*P9) ¿Cuáles son los dos "Títulos de Regla" con mayor número dentro del grupo de alertas de alto riesgo, en orden respectivo? (Formato: Título de la Regla 1, Título de la Regla 2)*
![Pasted image 20260620171021](../Fotos/Pasted%20image%2020260620171021.png)
Important Log File Cleared, Important Windows Service Terminated With Error

*P10) Examinar el último "Título de Regla" para el grupo de nivel de alertas de alto riesgo: ¿a qué ID MITRE corresponde esto y cuál es el TgtGrp en cuestión? (Formato: TechniqueID, TgtGrp)*
![Pasted image 20260620171850](../Fotos/Pasted%20image%2020260620171850.png)
T1098, Administrators

*P11) Mirando las alertas de nivel medio de riesgo, ¿cuál es el "Título de la Regla" y el recuento del grupo de alerta popular? (Formato: Título de la regla, Conteo)*
![Pasted image 20260620173351](../Fotos/Pasted%20image%2020260620173351.png)
Potentially Malicious PsSh, 457

*P12) ¿Qué función se utilizó para el ataque con spray de contraseña? También indica su ID MITRE para este tipo de ataque (Formato: powershell-function, TXXXX.xxx)*
![Pasted image 20260620173950](../Fotos/Pasted%20image%2020260620173950.png)
![Pasted image 20260620174033](../Fotos/Pasted%20image%2020260620174033.png)Invoke-LocalPasswordSpray, T1110.003

