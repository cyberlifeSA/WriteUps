- --
- Tags: #volatility #virustotal #cyberdefenders #ramnit_lab
- --
¿Cuál es el nombre del proceso responsable de la actividad sospechosa?

![Pasted image 20260702194551](../../Fotos/Pasted%20image%2020260702194551.png)

![Pasted image 20260702194440](../../Fotos/Pasted%20image%2020260702194440.png)

**Respuesta:** chromesetup.exe

---
**Nota** : Se puede crear una imagen de un proceso
```bash
python [vol.py](http://vol.py/) -f D:\CTF\test\volatility3\memory.dmp -o "volcar" windows.dumpfile — pid 4628
```
![Pasted image 20260702195131](../../Fotos/Pasted%20image%2020260702195131.png)

![Pasted image 20260702195140](../../Fotos/Pasted%20image%2020260702195140.png)

![Pasted image 20260702195145](../../Fotos/Pasted%20image%2020260702195145.png)
-------

¿Cuál es la ruta exacta del ejecutable para el proceso malicioso?

**Respuesta:** C:\Users\alex\Downloads\ChromeSetup.exe

Identificar las conexiones de red es crucial para comprender la estrategia de comunicación del malware. ¿A qué dirección IP intentó conectarse el malware?

![Pasted image 20260702195425](../../Fotos/Pasted%20image%2020260702195425.png)

**Respuesta:** 58.64.204.181

Para determinar el origen geográfico específico del ataque, ¿qué ciudad está asociada con la dirección IP con la que se comunicó el malware?

![Pasted image 20260702195557](../../Fotos/Pasted%20image%2020260702195557.png)

**Respuesta:** Hong Kong

Los hashes sirven como identificadores únicos para archivos, ayudando a detectar amenazas similares en diferentes máquinas. ¿Cuál es el hash SHA1 del ejecutable de malware?

![Pasted image 20260702200036](../../Fotos/Pasted%20image%2020260702200036.png)

![Pasted image 20260702200401](../../Fotos/Pasted%20image%2020260702200401.png)

**Respuesta:** 280c9d36039f9432433893dee6126d72b9112ad2

Examinar el calendario de desarrollo del malware puede aportar información sobre su despliegue. ¿Cuál es la fecha de compilación del malware?

![Pasted image 20260702205609](../../Fotos/Pasted%20image%2020260702205609.png)

**Respuesta:** 2019-12-01 08:36

Identificar los dominios asociados a este malware es crucial para bloquear futuras comunicaciones maliciosas y detectar cualquier interacción en curso con esos dominios dentro de nuestra red. ¿Puedes proporcionar el dominio conectado al malware?

![Pasted image 20260702210730](../../Fotos/Pasted%20image%2020260702210730.png)

**Respuesta:** dnsnb8.net