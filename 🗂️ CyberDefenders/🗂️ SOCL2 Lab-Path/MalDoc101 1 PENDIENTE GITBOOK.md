- --
- Tags: #cyberdefenders #maldoc101_lab #oledump #olevba
- --
En este documento hay macros que contienen múltiples flujos. Proporciona el número del más alto.

![Pasted image 20260706161356](Pasted%20image%2020260706161356.png)

El comando permite identificar que el archivo corresponde a un **Documento Compuesto V2 (Compound Document V2)**, un formato basado en **CFB (Compound File Binary)**. Además, proporciona información relevante, como el sistema operativo con el que fue creado el documento, la fecha y hora de la última modificación y la versión de la aplicación utilizada. Estos metadatos pueden ser útiles para determinar el origen del archivo y detectar posibles modificaciones.

A continuación, se utiliza la herramienta **oledump** para examinar los flujos de datos contenidos en el archivo CFB. Esta utilidad, desarrollada en Python, permite extraer e inspeccionar los flujos incrustados en documentos de Microsoft Office. Al ejecutar el comando `python oledump.py sample.bin`, se obtiene un listado con todos los flujos disponibles, indicando su índice, tamaño y tipo. Aquellos identificados con la letra **M** corresponden a macros VBA, las cuales requieren un análisis detallado, ya que frecuentemente son utilizadas para incorporar código malicioso.

**M (mayúscula):** Este indicador señala que el flujo contiene una macro VBA con atributos y código comprimido. Para examinar su comportamiento es necesario descomprimir dicho código, ya que es allí donde se encuentra la lógica de la macro. La presencia de este indicador suele ser un aspecto relevante durante el análisis, debido a que las macros comprimidas pueden ocultar código malicioso.

**m (minúscula):** Este indicador identifica un flujo que incluye únicamente los atributos de una macro, pero no contiene código VBA ejecutable. En estos casos, el flujo almacena información descriptiva o de configuración relacionada con la macro, sin incorporar instrucciones que puedan ejecutarse.

En este contexto, los **atributos** corresponden a metadatos asociados a una macro VBA. Estos pueden incluir información como el nombre de la macro, su descripción y otras propiedades utilizadas para su identificación o configuración. Aunque forman parte de la estructura del proyecto VBA, no contienen el código encargado de ejecutar acciones o realizar operaciones.

![Pasted image 20260706162208](Pasted%20image%2020260706162208.png)

**Respuesta:** 16

¿Qué evento se utiliza para iniciar la ejecución de los macros?

**`olevba`** es una herramienta de la suite **oletools** que se utiliza para **extraer y analizar macros VBA** contenidas en documentos de Microsoft Office. Es muy utilizada en análisis de malware porque muchas campañas maliciosas emplean documentos con macros para ejecutar código en el equipo de la víctima.

![Pasted image 20260706163901](Pasted%20image%2020260706163901.png)

![Pasted image 20260706163816](Pasted%20image%2020260706163816.png)

**Respuesta:** Document_open

¿Qué familia de malware intentaba eliminar este maldoc?

![Pasted image 20260706164820](Pasted%20image%2020260706164820.png)

![Pasted image 20260706172027](Pasted%20image%2020260706172027.png)

**Respuesta:** emotet

¿Qué flujo es responsable del almacenamiento de la cadena codificada en base64?

![Pasted image 20260706172239](Pasted%20image%2020260706172239.png)

![Pasted image 20260706172558](Pasted%20image%2020260706172558.png)

El flujo **Macros/roubhaol/i09/o** llama la atención debido a su tamaño relativamente grande, lo que puede indicar la presencia de información ofuscada o codificada que requiere un análisis más detallado.

![Pasted image 20260706174809](Pasted%20image%2020260706174809.png)

**Respuesta:** 34

Este documento contiene un formulario de usuario. Proporciona el nombre.

![Pasted image 20260706175520](Pasted%20image%2020260706175520.png)

**Respuesta:** roubhaol

Este documento contiene una cadena codificada en Base64 ofuscada; ¿Qué valor se usa para rellenar (o ofuscar) esta cadena?

![Pasted image 20260706174705](Pasted%20image%2020260706174705.png)

![Pasted image 20260706174637](Pasted%20image%2020260706174637.png)

![Pasted image 20260706180705](Pasted%20image%2020260706180705.png)

![Pasted image 20260706180955](Pasted%20image%2020260706180955.png)

**Respuesta:**  `2342772g3&*gs7712ffvs626fq`

¿Qué programa ejecuta la cadena codificada en Base64?

![Pasted image 20260706184700](Pasted%20image%2020260706184700.png)

![Pasted image 20260706184730](Pasted%20image%2020260706184730.png)

**Respuesta:** powershell

¿Qué clase WMI se utiliza para crear el proceso de lanzamiento del troyano?

![Pasted image 20260706185120](Pasted%20image%2020260706185120.png)

**Respuesta:** Win32_Process

Se contactó con varios dominios para descargar un troyano. Proporciona el primer FQDN según la pista proporcionada.

![Pasted image 20260706185533](Pasted%20image%2020260706185533.png)

**Respuesta:** haoqunkong.com