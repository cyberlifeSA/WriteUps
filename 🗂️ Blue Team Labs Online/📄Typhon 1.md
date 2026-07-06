- --
- Tags: #wireshark #btlo #typhon_lab
- --
P1) ¿Cuál es el dominio y la dirección IP a los que el programa intenta conectarse? (Formato: Dominio, IP)
![Pasted image 20260622154026](../Fotos/Pasted%20image%2020260622154026.png)
www.b4s1lisk.xyz, 159.65.12.25

P2) ¿Qué es la ruta de petición HTTP y la cadena codificada que se devuelve en la respuesta cuando el programa se comunica con el servidor? (Formato: Camino, Cuerda)
![Pasted image 20260622154205](../Fotos/Pasted%20image%2020260622154205.png)
/join,U3RvbmVHbGFyZSA=

P3) Intenta imitar la respuesta del servidor. Hay un script en la sección de Herramientas que puedes usar, pero tendrás que hacer algunas modificaciones antes de que funcione. Considera cómo el programa localiza el servidor y cómo podrías redirigirlo a tu script. Una vez hecho esto, el binario extraerá varios archivos. ¿Cuál es el nombre de la DLL que se extrae? (Formato: Nombre de archivo)
![Pasted image 20260622154757](../Fotos/Pasted%20image%2020260622154757.png)
![Pasted image 20260622155257](../Fotos/Pasted%20image%2020260622155257.png)
![Pasted image 20260622155601](../Fotos/Pasted%20image%2020260622155601.png)
![Pasted image 20260622160203](../Fotos/Pasted%20image%2020260622160203.png)
![Pasted image 20260622194054](../Fotos/Pasted%20image%2020260622194054.png)
![Pasted image 20260622194123](../Fotos/Pasted%20image%2020260622194123.png)
![Pasted image 20260622194257](../Fotos/Pasted%20image%2020260622194257.png)
typhon.dll

P4) ¿Qué LOLBAS se aprovecha para ejecutar el DLL? (Formato: LOLBAS Name)
![Pasted image 20260622194537](../Fotos/Pasted%20image%2020260622194537.png)
rundll32.exe

P5) Durante la primera ejecución del DLL, se llama a una función específica y se descarta un archivo. ¿Cómo se llama esta función y cuál es el nombre del archivo que se ha eliminado? (Formato: Función, Nombre de archivo)
![Pasted image 20260622200743](../Fotos/Pasted%20image%2020260622200743.png)
![Pasted image 20260622201918](../Fotos/Pasted%20image%2020260622201918.png)
yurei, spoolsvl.exe

P6) El programa se replica a sí mismo en un directorio oculto. ¿Cuál es el nombre del archivo replicado y cuál es su hash MD5? (Formato: Nombre de archivo, Hash)
![Pasted image 20260622202234](../Fotos/Pasted%20image%2020260622202234.png)![Pasted image 20260622202452](../Fotos/Pasted%20image%2020260622202452.png)
![Pasted image 20260622202902](../Fotos/Pasted%20image%2020260622202902.png)
dwm.exe, 2C21D810BDD449C3668092AC62B3B896

P7) ¿Qué LOLBAS utiliza el binario para ejecutar la DLL en segundo plano repetidamente, y cuál es el nombre de la tarea programada que crea? (Formato: LOLBAS, NombreTask)
![Pasted image 20260622203436](../Fotos/Pasted%20image%2020260622203436.png)
schtasks, TenguTask

P8) Después de que el comando se ejecute repetidamente en segundo plano, el programa intenta subir un archivo a un servicio de almacenamiento en línea específico. ¿Cómo se llama este servicio? (Formato: Nombre del servicio de almacenamiento)
![Pasted image 20260622204716](../Fotos/Pasted%20image%2020260622204716.png)
![Pasted image 20260622204732](../Fotos/Pasted%20image%2020260622204732.png)
dropbox

P9) Para mantener la persistencia, ¿bajo qué clave de registro crea la entrada binaria? (Formato: Clave del Registro)
![Pasted image 20260622205429](../Fotos/Pasted%20image%2020260622205429.png)
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run

P10) ¿Cuál es el nombre de la entrada del registro añadida? (Formato: Nombre de la entrada)
![Pasted image 20260622205211](../Fotos/Pasted%20image%2020260622205211.png)
Typhon