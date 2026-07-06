- --
- Tags: #cyberchef #btlo #macroni_lab #active_lab
- --
Investigacion
![Pasted image 20260629153049](../Fotos/Pasted%20image%2020260629153049.png)

![Pasted image 20260629153417](../Fotos/Pasted%20image%2020260629153417.png)

![Pasted image 20260629153337](../Fotos/Pasted%20image%2020260629153337.png)

Preguntas:

¿Qué tipo de cifrado contiene el comando extraído de PowerShell?
![Pasted image 20260629154420](../Fotos/Pasted%20image%2020260629154420.png)
base64

![Pasted image 20260629155531](../Fotos/Pasted%20image%2020260629155531.png)

Después de descifrar la primera capa de cifrado. Obtenemos otra capa de cifrado. Si se descifra, ¿qué tipo de datos de archivo será?
![Pasted image 20260629160343](../Fotos/Pasted%20image%2020260629160343.png)
Gzip

Al guardar el archivo y extraer el contenido. Vemos una tercera capa de cifrado presente. ¿Qué variable de bytes del array tiene esos datos?
![Pasted image 20260629161242](../Fotos/Pasted%20image%2020260629161242.png)
var_code

El mismo array de bytes está siendo XOR con un valor decimal. ¿Qué pasa?
![Pasted image 20260629161958](../Fotos/Pasted%20image%2020260629161958.png)
35

¿Puedes descifrar esta capa de ofuscación y encontrar la IP C2?
![Pasted image 20260629170734](../Fotos/Pasted%20image%2020260629170734.png)176.103.56.89