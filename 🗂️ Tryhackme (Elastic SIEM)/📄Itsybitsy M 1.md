- --
- Tags: #elasticsearch 
- --
**TRYACKME**
## 🎯 Objetivos logrados en la máquina
1. **Cuantificar eventos en un periodo específico**  
    → Se contabilizaron **1482 eventos en marzo de 2022**, validando manejo de filtros temporales en Elastic.
2. **Identificar un usuario sospechoso mediante análisis de IP**  
    → Se detectó la IP **192.166.65.53** como origen anómalo (`source.ip`).
3. **Detectar Living Off The Land (LOLBins)**  
    → Identificación de **bitsadmin** (binario legítimo de Windows) usado para descargar payloads maliciosos.
4. **Identificar infraestructura C2 encubierta**  
    → Detección de **pastebin.com** como servidor de **Command and Control (C2)** usando tráfico web legítimo.
5. **Extraer la URL completa del C2**  
    → Reconstrucción de la conexión C2: `pastebin.com/yTg0Ah6a`.
6. **Identificar el recurso remoto accedido**  
    → Detección del archivo **secret.txt** alojado en el servicio de intercambio.
7. **Confirmar exfiltración / staging de información sensible**  
    → Obtención de un **código secreto en formato THM{…}**, confirmando compromiso del host.

----
*Pregunta 1*
¿Cuántos eventos se devolvieron en marzo de 2022?
![Pasted image 20260111202901](../Fotos/Pasted%20image%2020260111202901.png)
Respuesta: 1482

*Pregunta 2*
¿Cuál es la IP asociada al usuario sospechoso en los registros?
Nos enfocamos en la que tiene menor porcentaje parece ser sospechoso
![Pasted image 20260111203602](../Fotos/Pasted%20image%2020260111203602.png)
Respuesta: 192.166.65.53 (`source.ip`)

*Pregunta 3*
La máquina del usuario usaba un binario legítimo de Windows para descargar un archivo desde el servidor C2. ¿Cuál es el nombre del binario?
![Pasted image 20260111203637](../Fotos/Pasted%20image%2020260111203637.png)
Respuesta: bitsadmin (`user_agent`) Es un **header HTTP** que indica **qué herramienta, navegador o script** está realizando la solicitud al servidor. - **Herramientas de ataque** (Gobuster, Nmap, Hydra, curl) ETC

*Pregunta 4*
La máquina infectada se conectó a un famoso sitio de intercambio de archivos en este periodo, que también actúa como servidor C2 utilizado por los autores del malware para comunicarse. ¿Cómo se llama el sitio de intercambio de archivos?
![Pasted image 20260111205134](../Fotos/Pasted%20image%2020260111205134.png)
Respuesta: pastebin.com (`host`)
![Pasted image 20260319200558](../Fotos/Pasted%20image%2020260319200558.png)

*Pregunta 5*
¿Cuál es la URL completa del C2 al que está conectado el host infectado?
![Pasted image 20260111205840](../Fotos/Pasted%20image%2020260111205840.png)
Respuesta: pastebin.com/yTg0Ah6a (`uri`)
![Pasted image 20260319201131](../Fotos/Pasted%20image%2020260319201131.png)

*Pregunta 6*
Se accedió a un archivo en el sitio de intercambio de archivos. ¿Cuál es el nombre del archivo al que se accede?
![Pasted image 20260111210410](../Fotos/Pasted%20image%2020260111210410.png)
Resultado: secret.txt (Accedemos a la web y vemos el archivo)

*Pregunta 7*
El archivo contiene un código secreto con el formato `THM{_____}`.
![Pasted image 20260111210750](../Fotos/Pasted%20image%2020260111210750.png)