- --
- Tags: #volatility #cyberdefenders #brave_lab #hxd
- --
¿A qué hora se adquirió la imagen de RAM según el sistema sospechoso?
![Pasted image 20260705165406](../../Fotos/Pasted%20image%2020260705165406.png)
2021-04-30 17:52

¿Cuál es el valor hash SHA256 de la imagen RAM?
![Pasted image 20260705165637](../../Fotos/Pasted%20image%2020260705165637.png)
9db01b1e7b19a3b2113bfb65e860fffd7a1630bdf2b18613d206ebf2aa0ea172

¿Cuál es el ID de proceso de **brave.exe**?
![Pasted image 20260705165826](../../Fotos/Pasted%20image%2020260705165826.png)
![Pasted image 20260705165759](../../Fotos/Pasted%20image%2020260705165759.png)
4856

¿Cuántas conexiones de red establecidas había en el momento de la adquisición?
`python3 vol.py -f /home/kali/Downloads/temp_extract_dir/c49-AfricanFalls2/20210430-Win10Home-20H2-64bit-memdump.mem windows.netscan | grep "ESTABLISHED"`
![Pasted image 20260705170633](../../Fotos/Pasted%20image%2020260705170633.png)
10

¿Con qué nombre de dominio Chrome tiene una conexión de red establecida?
`Resolve-DnsName -Name 185.70.41.130 -Type A` (PowerShell)
`dig +short -x 185.70.41.130` (Linux)
`nslookup 185.70.41.130` (Linux)
`host 185.70.41.130` (Linux)
![Pasted image 20260705170923](../../Fotos/Pasted%20image%2020260705170923.png)
protonmail.ch

¿Cuál es el valor hash MD5 del ejecutable del proceso para el **PID 6988**?
![Pasted image 20260705171052](../../Fotos/Pasted%20image%2020260705171052.png)
![Pasted image 20260705171601](../../Fotos/Pasted%20image%2020260705171601.png)
0b493d8e26f03ccd2060e0be85f430af 

¿Puedes identificar la palabra que empieza en el **desplazamiento 0x45BE876** y tiene 6 bytes?
Ctrl+g
![Pasted image 20260705173220](../../Fotos/Pasted%20image%2020260705173220.png)
![Pasted image 20260705174106](../../Fotos/Pasted%20image%2020260705174106.png)
hacker

¿Cuál es la fecha y hora de creación del proceso principal de **powershell.exe**?
![Pasted image 20260705174750](../../Fotos/Pasted%20image%2020260705174750.png)
2021-04-30 17:39

¿Cuál es la ruta completa y el nombre del último archivo abierto en el bloc de notas?
![Pasted image 20260705175013](../../Fotos/Pasted%20image%2020260705175013.png)
![Pasted image 20260705175001](../../Fotos/Pasted%20image%2020260705175001.png)
C:\Users\JOHNDO~1\AppData\Local\Temp\7zO4FB31F24\accountNum

¿Cuánto tiempo usó el sospechoso **el navegador Brave**? (En horas)
*El plugin **`windows.registry.userassist`** de Volatility 3 analiza las claves **UserAssist** del Registro de Windows para mostrar **qué programas ha ejecutado un usuario** y con qué frecuencia.*
![Pasted image 20260705175942](../../Fotos/Pasted%20image%2020260705175942.png)
4