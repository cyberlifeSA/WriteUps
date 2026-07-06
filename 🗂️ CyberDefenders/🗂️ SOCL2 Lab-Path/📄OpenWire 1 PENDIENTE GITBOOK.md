- --
- Tags: #wireshark #cyberdefenders #openwire_lab
- --
Al identificar la IP C2, podemos bloquear el tráfico hacia y desde esta IP, ayudando a contener la brecha y evitar una mayor exfiltración de datos o ejecución de comandos. ¿Puedes proporcionar la IP del servidor C2 que se comunicó con nuestro servidor?
![Pasted image 20260704225955](../../Fotos/Pasted%20image%2020260704225955.png)
146.190.21.92

Los puntos de entrada iniciales son críticos para rastrear el vector de ataque hacia atrás. ¿Cuál es el número de puerto del servicio que explotó el adversario?
![Pasted image 20260704230507](../../Fotos/Pasted%20image%2020260704230507.png)
61616

Siguiendo la pregunta anterior, ¿cuál es el nombre del servicio que se ha considerado vulnerable?
![Pasted image 20260704230901](../../Fotos/Pasted%20image%2020260704230901.png)Apache ActiveMQ

La infraestructura del atacante suele implicar múltiples componentes. ¿Cuál es la IP del segundo servidor C2?
![Pasted image 20260704232804](../../Fotos/Pasted%20image%2020260704232804.png)

Los atacantes suelen dejar rastros en el disco. ¿Cómo se llama el ejecutable de shell inverso que se deja caer en el servidor?
docker

¿Qué clase Java invocó el archivo XML para ejecutar el exploit?
![Pasted image 20260704233445](../../Fotos/Pasted%20image%2020260704233445.png)java.lang.ProcessBuilder

Para entender mejor la vulnerabilidad de seguridad específica explotada, ¿puedes identificar el identificador CVE asociado a esta vulnerabilidad?
  ![Pasted image 20260704233703](../../Fotos/Pasted%20image%2020260704233703.png)
CVE-2023-46604

El proveedor abordó la vulnerabilidad añadiendo un [paso de validación](https://github.com/apache/activemq/pull/1098/commits/3eaf3107f4fb9a3ce7ab45c175bfaeac7e866d5b) para asegurar que solo se puedan instanciar clases válidas, evitando la explotación. ¿En qué **clase y método Java** se añadió este paso de validación?`Throwable`
El parche aplicado a esta vulnerabilidad introduce una validación adicional en la clase **`BaseDataStreamMarshaller`**, con el objetivo de comprobar de forma más estricta si la clase que se está intentando instanciar corresponde al tipo **`Throwable`**. Esta verificación extra permite corregir la falla de seguridad al evitar instanciaciones no válidas o potencialmente maliciosas.
![Pasted image 20260704234436](../../Fotos/Pasted%20image%2020260704234436.png)
BaseDataStreamMarshaller.createThrowable