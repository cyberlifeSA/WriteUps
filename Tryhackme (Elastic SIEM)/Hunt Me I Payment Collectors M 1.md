 - --
- Tags: #elasticsearch 
- --
**TRYACKME**
### 🎯**Objetivos completados del laboratorio
1. **Identificación del adjunto inicial**  
    Se detectó que Michael descargó el archivo **`Invoice_AT_2023-227.zip`**, punto inicial del ataque (vector de entrada).
2. **Análisis del contenido del ZIP**  
    El ZIP contenía el archivo **`Payment_Invoice.pdf.lnk.lnk`**, un acceso directo malicioso (técnica de _masquerading_).
3. **Ejecución del payload inicial**  
    El archivo `.lnk` ejecutó **`powershell.exe`**, iniciando la fase de ejecución remota.
4. **Descarga de herramienta de acceso remoto**  
    PowerShell descargó **Powercat** desde  
    `https://raw.githubusercontent.com/besimorhino/powercat/master/powercat.ps1` para obtener una _reverse shell_.
5. **Establecimiento de conexión con el atacante**  
    La estación de trabajo se conectó al atacante mediante el **puerto 19282**.
6. **Enumeración básica del sistema**  
    El atacante ejecutó **`systeminfo.exe`**, primer binario nativo usado para reconocimiento del host.
7. **Enumeración del dominio**  
    Se descargó **PowerView.ps1** desde  
    `https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerView/powerview.ps1`.
8. **Montaje de recurso compartido malicioso**  
    El atacante asignó el recurso **`SSF-FinancialRecords`** a la máquina de la víctima.
9. **Copia de datos desde el recurso compartido**  
    Los archivos fueron copiados a  
    **`C:\Users\michael.ascot\Downloads\exfiltration`**.
10. **Identificación del archivo sensible extraído**  
    Se confirmó la extracción de **`ClientPortfolioSummary.xlsx`**.
11. **Preparación para la exfiltración**  
    El atacante comprimió la información en **`exfilt8me.zip`**.
12. **Técnica de exfiltración identificada (MITRE)**  
    Se utilizó **`nslookup.exe`** para exfiltrar datos → **MITRE ATT&CK T1048 (Exfiltration Over Alternative Protocol)**.
13. **Identificación del servidor atacante**  
    Se analizó el dominio del servidor que recibió la información exfiltrada.
14. **Reconstrucción de datos exfiltrados**  
    Se reconstruyó el archivo adicional y se obtuvo la bandera final:  
    **`THM{1497321f4f6f059a52dfb124fb16566e}`**

---
*Pregunta 1*
¿Cómo se llamaba el archivo adjunto ZIP que Michael descargó?
`*.zip`

![Pasted image 20260113132836](../Fotos/Pasted%20image%2020260113132836.png)

Respuesta: Invoice_AT_2023-227.zip

*Pregunta 2*
¿Cuál era el archivo contenido que Michael extrajo del archivo adjunto?

![Pasted image 20260113134459](../Fotos/Pasted%20image%2020260113134459.png)

Ojo

![Pasted image 20260113184644](../Fotos/Pasted%20image%2020260113184644.png)

Respuesta: Payment_Invoice.pdf.lnk.lnk

*Pregunta 3*
¿Cómo se llamaba el proceso de línea de comandos que apareció a partir del archivo adjunto extraído?

![Pasted image 20260113134133](../Fotos/Pasted%20image%2020260113134133.png)

Respuesta: powershell.exe

*Pregunta 4*
¿Qué URL usó el atacante para descargar una herramienta que estableciera una conexión inversa de shell?
Expandiendo el `process.command_line` veremos la url de donde se descarga la herramienta

![Pasted image 20260113140422](../Fotos/Pasted%20image%2020260113140422.png)

Respuesta: https://raw.githubusercontent.com/besimorhino/powercat/master/powercat.ps1

*Pregunta 5*
¿En qué puerto se conectó la estación de trabajo al atacante?

![Pasted image 20260113140404](../Fotos/Pasted%20image%2020260113140404.png)

Respuesta: 19282

*Pregunta 6*
¿Cuál fue el primer binario nativo de Windows que el atacante ejecutó para la enumeración del sistema tras obtener acceso remoto?
`winlog.computer_name` `user.name` `process.command_line` `file.path`

![Pasted image 20260113141931](../Fotos/Pasted%20image%2020260113141931.png)

Respuesta: systeminfo.exe

![Pasted image 20260318185332](../Fotos/Pasted%20image%2020260318185332.png)

*Pregunta 7*
¿Cuál es la URL del script que descarga el atacante para enumerar el dominio?
Columnas: `file.path` / `process.name` / `process.command_file` / `process.parent.command_line` / `user.name`
Filtro: PowerView.ps1

![Pasted image 20260113221007](../Fotos/Pasted%20image%2020260113221007.png)

Respuesta: https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerView/powerview.ps1

![Pasted image 20260318185831](../Fotos/Pasted%20image%2020260318185831.png)

*Pregunta 8*
¿Cuál era el nombre del archivo compartido que el atacante asignó a la estación de trabajo de Michael?
Filtro: "SSF-FinancialRecords" (No es lo mas optimo ya que en teoria esto no lo sabemos pero por `net.exe` que lo encontremos buscando eso nos deberia dar pistas)

![Pasted image 20260113221642](../Fotos/Pasted%20image%2020260113221642.png)

Respuesta: SSF-FinancialRecords

*Pregunta 9*
¿A qué directorio copió el atacante el contenido del archivo compartido?
`event.code:11`

![Pasted image 20260113222914](../Fotos/Pasted%20image%2020260113222914.png)

Respuesta: C:\Users\michael.ascot\Downloads\exfiltration

![Pasted image 20260318221433](../Fotos/Pasted%20image%2020260318221433.png)   

![Pasted image 20260318221511](../Fotos/Pasted%20image%2020260318221511.png)

![Pasted image 20260318222306](../Fotos/Pasted%20image%2020260318222306.png)

*Pregunta 10*
¿Cuál era el nombre del archivo de Excel que el atacante extrajo del recurso compartido?
Es la misma ventana que la anterior pero igual podemos aplicar filtro
Filtro: `winlog.event_id : 11 AND file.name: *.xlsx`

![Pasted image 20260113223232](../Fotos/Pasted%20image%2020260113223232.png)

Resultado: ClientPortfolioSummary.xlsx

*Pregunta 11*
¿Cuál era el nombre del archivo de archivo que el atacante creó para prepararse para la exfiltración?
Misma ventana anterior 
Filtro: `winlog.event_id : 11`

![Pasted image 20260113223647](../Fotos/Pasted%20image%2020260113223647.png)

Respuesta: exfilt8me.zip

*Pregunta 12*
¿Cuál es el ID MITRE de la técnica que el atacante utilizó para exfiltrar los datos?
Se utiliza `nslookup.exe`(**DNS Exfiltration**) para enviar datos al receptor como tecnica en especial aplicada y con eso investigamos en MITRE el ID

![Pasted image 20260113225959](../Fotos/Pasted%20image%2020260113225959.png)

Respuesta: T1048

*Pregunta 13*
¿Cuál era el dominio del servidor del atacante que recuperó los datos exfiltrados?

![Pasted image 20260113225938](../Fotos/Pasted%20image%2020260113225938.png)

Respuesta: haz4rdw4re.io

*Pregunta 14*
El atacante extrajo un archivo adicional de la estación de trabajo de la víctima. ¿Cuál es la bandera que recibes tras reconstruir el archivo?
Filtro: `nslookup.exe`
Primera parte del base64: VEhNezE0OTczMjFmNGY2ZjA1OWE1Mm

![Pasted image 20260113230820](../Fotos/Pasted%20image%2020260113230820.png)

Segunda parte del base64: RmYjEyNGZiMTY1NjZlfQ==

![Pasted image 20260113230750](../Fotos/Pasted%20image%2020260113230750.png)

![Pasted image 20260113231037](../Fotos/Pasted%20image%2020260113231037.png)  

![Pasted image 20260113231049](../Fotos/Pasted%20image%2020260113231049.png)

Resultado: THM{1497321f4f6f059a52dfb124fb16566e}



