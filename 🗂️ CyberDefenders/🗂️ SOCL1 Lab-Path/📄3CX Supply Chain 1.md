- --
- Tags: #virustotal #cyberdefenders #3cxsupplychain_lab
- --
Comprender el alcance del ataque e identificar qué versiones presentan comportamientos maliciosos es crucial para tomar decisiones informadas si estas versiones comprometidas están presentes en la organización. ¿Cuántas versiones de 3CX **en Windows** han sido marcadas como malware?

![Pasted image 20260704164204](../../Fotos/Pasted%20image%2020260704164204.png)
2

Determinar la antigüedad del malware puede ayudar a evaluar el alcance del compromiso y a seguir la evolución de las familias y variantes de malware. ¿Cuál es la hora de creación UTC del malware?`.msi`
![Pasted image 20260704164524](../../Fotos/Pasted%20image%2020260704164524.png)
2023-03-13 06:33

Los archivos ejecutables () se utilizan frecuentemente como cargas útiles principales o secundarias de malware, mientras que las bibliotecas de enlaces dinámicos () suelen cargar código malicioso o mejorar la funcionalidad del malware. Analizar los archivos depositados por el Microsoft Software Installer () es crucial para identificar archivos maliciosos e investigar su potencial completo. ¿Qué DLLs maliciosas se eliminaron por el archivo?`.exe``.dll``.msi``.msi`
![Pasted image 20260704170156](../../Fotos/Pasted%20image%2020260704170156.png)
 ffmpeg.dll,d3dcompiler_47.dll

Reconocer las técnicas de persistencia utilizadas en este incidente es esencial para las estrategias actuales de mitigación y las mejoras futuras en la defensa. ¿Cuál es el ID de la Técnica MITRE que emplean los archivos para cargar la DLL maliciosa?`.msi`
![Pasted image 20260704171410](../../Fotos/Pasted%20image%2020260704171410.png)
T1574

Reconocer el tipo de malware () es esencial para tu investigación, ya que puede ofrecer información valiosa sobre las posibles acciones maliciosas que vas a examinar. ¿Cuál es la categoría de amenaza de las dos DLLs maliciosas?`threat category`
![Pasted image 20260704173029](../../Fotos/Pasted%20image%2020260704173029.png)
trojan

Como analista de inteligencia de amenazas que realiza análisis dinámicos, es fundamental entender cómo el malware puede evadir la detección en entornos virtualizados o sistemas de análisis. Este conocimiento te ayudará a mitigar o abordar eficazmente estas tácticas evasivas. ¿Cuál es el ID MITRE para las técnicas de virtualización/evasión sandbox utilizadas por las dos DLLs maliciosas?
![Pasted image 20260704173122](../../Fotos/Pasted%20image%2020260704173122.png)
T1497

Al realizar análisis de malware e ingeniería inversa, comprender las técnicas antianálisis es vital para evitar perder tiempo. ¿Qué hipervisor es objetivo de las técnicas de antianálisis del archivo?`ffmpeg.dll`
![Pasted image 20260704174636](../../Fotos/Pasted%20image%2020260704174636.png)
VMWare

Identificar el método criptográfico utilizado en malware es crucial para comprender las técnicas empleadas que eluden los mecanismos de defensa y ejecutan plenamente sus funciones. ¿Qué algoritmo de cifrado utiliza el archivo?`ffmpeg.dll`
![Pasted image 20260704173718](../../Fotos/Pasted%20image%2020260704173718.png)
RC4

Como analista, has reconocido algunos TTPs implicados en el incidente, pero identificar al grupo APT responsable te ayudará a buscar sus TTPs habituales y descubrir otras actividades maliciosas potenciales. ¿Qué grupo es responsable de este ataque?
![Pasted image 20260704175332](../../Fotos/Pasted%20image%2020260704175332.png)
Lazarus