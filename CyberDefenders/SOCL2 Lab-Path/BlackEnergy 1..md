- --
- Tags: #volatility #cyberdefenders #blackenergy_lab
- --

¿Qué perfil de volatilidad sería el mejor para esta máquina?

![Pasted image 20260704204842](../../Fotos/Pasted%20image%2020260704204842.png)

A partir de los datos:
- `NtMajorVersion = 5`
- `NtMinorVersion = 1` → Windows XP
- `CSDVersion = 3` → SP3
- `Is64Bit = False` → x86.

Profile equivalente: WinXPSP3x86 (Al haber usado volatility 3 el CSDVersion lanza 3 pero en realidad es 2 ya que se espera usar Volatility2)

¿Cuántos procesos se ejecutaban cuando se adquirió la imagen?

![Pasted image 20260704205628](../../Fotos/Pasted%20image%2020260704205628.png)

**Respuesta:** 19

¿De qué es el ID del proceso?`cmd.exe`

![Pasted image 20260704205726](../../Fotos/Pasted%20image%2020260704205726.png)

**Respuesta:** 1960

¿Cuál es el nombre del proceso más sospechoso?

![Pasted image 20260704205854](../../Fotos/Pasted%20image%2020260704205854.png)

**Respuesta:** rootkit.exe

¿Qué proceso muestra la mayor probabilidad de inyección de código?

La firma **MZ** corresponde al encabezado característico de los archivos **PE (Portable Executable)**, utilizado por ejecutables y bibliotecas DLL para identificar el inicio del archivo. Si esta firma aparece en una región de memoria donde no debería estar, especialmente dentro de un proceso como **svchost.exe**, es un indicio de que un archivo PE pudo haber sido inyectado en dicho proceso. Esta técnica permite que un atacante ejecute código malicioso utilizando un proceso legítimo del sistema como cobertura, dificultando su detección por soluciones antivirus tradicionales y herramientas de monitoreo del comportamiento. Por ello, la presencia de la firma **MZ** en este contexto constituye un fuerte indicador de una inyección de código en memoria mediante la carga de un ejecutable o DLL dentro de un proceso confiable.

![Pasted image 20260704210516](../../Fotos/Pasted%20image%2020260704210516.png)

![Pasted image 20260704210456](../../Fotos/Pasted%20image%2020260704210456.png)

Hay un archivo extraño mencionado en el proceso reciente. Proporciona la ruta completa de ese archivo.
El archivo `\Device\HarddiskVolume1\WINDOWS\system32\drivers\str.sys` se encuentra ubicado en el directorio `**system32\drivers**`, una ruta utilizada habitualmente para almacenar controladores legítimos de Windows. No obstante, el nombre `**str.sys**` resulta llamativo por no seguir la nomenclatura habitual de los controladores del sistema, lo que puede convertirlo en un elemento de interés durante un análisis forense y justificar una revisión más detallada para determinar si se trata de un archivo legítimo o potencialmente malicioso.

![Pasted image 20260704211028](../../Fotos/Pasted%20image%2020260704211028.png)

![Pasted image 20260704211302](../../Fotos/Pasted%20image%2020260704211302.png)

**Respuesta:** C:\WINDOWS\system32\drivers\str.sys

¿Cuál es el nombre del archivo DLL inyectado cargado desde el proceso reciente?

![Pasted image 20260704214649](../../Fotos/Pasted%20image%2020260704214649.png)

**Respuesta:** msxml3r.dll

¿Cuál es la dirección base de la DLL inyectada?

La salida del plugin **malfind** evidencia una región de memoria con comportamiento sospechoso. En particular, se destaca un bloque que inicia en la dirección base **0x980000**, la cual corresponde al punto donde se encuentra cargado un módulo inyectado. La detección del encabezado **MZ** (representado como **4D 5A en hexadecimal**) en esa ubicación confirma que dicha zona contiene un archivo de tipo **Portable Executable (PE)**, como puede ser una DLL o un ejecutable, lo que sugiere posible inyección de código en memoria.

![Pasted image 20260704223702](../../Fotos/Pasted%20image%2020260704223702.png)

**Respuesta:** 0x980000