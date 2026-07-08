- --
- Tags: #volatility #btlo #tuxone_lab
- --
Pregunta 1) ¿Qué versión de Linux está usando la máquina? (Formato: Algo XX.XX)

![Pasted image 20260627203639](../Fotos/Pasted%20image%2020260627203639.png)

**Respuesta:** Ubuntu 18.04

Pregunta 2) ¿Cómo se llama la máquina de la víctima? (Formato: nombre)

`strings -a /home/ubuntu/Desktop/TuxOne/system.mem | grep "hostname"`

![Pasted image 20260627212423](../Fotos/Pasted%20image%2020260627212423.png)

**Respuesta:** nik-tumbo

---

**El archivo `.json`** contiene los **símbolos del kernel** del sistema cuya memoria estás analizando. Es decir, le dice a Volatility **dónde están las estructuras de datos en memoria** para ese kernel específico.

```bash
mkdir /home/ubuntu/Desktop/volatility3-2.4.1/volatiltiy3/volatility3/symbols/linux/ 
cp /home/ubuntu/Desktop/TuxOne/5.4.0-150-generic.json /home/ubuntu/Desktop/volatility3-2.4.1/volatiltiy3/volatility3/symbols/Linux/ 
cd /home/ubuntu/Desktop/volatility3-2.4.1 
ln -s /home/ubuntu/Desktop/TuxOne/system.mem ./system.mem 
alias vol="python3.7 vol.py -f system.mem"
```

![Pasted image 20260628113335](../Fotos/Pasted%20image%2020260628113335.png)

----

Pregunta 3) Parece que el atacante intentaba llamar de nuevo a otra IP interna que ha sido comprometida, ¿cuál es la IP del servidor? (Formato: x.x.x.x)

![Pasted image 20260628120458](../Fotos/Pasted%20image%2020260628120458.png)

**Respuesta:** 192.168.100.186

Pregunta 4) ¿Cómo se llama la carga útil de Python que se está instalando y de qué puerto se recuperó? (Formato: name.py, número de porto)

![Pasted image 20260628120625](../Fotos/Pasted%20image%2020260628120625.png)

**Respuesta:** ragdoll.py, 8888

Pregunta 5) El atacante también extrajo algunos archivos de otra máquina usando el comando SCP. ¿Cómo se llama el directorio del que sacaron los archivos? (Formato: nombre del directorio)

![Pasted image 20260628120851](../Fotos/Pasted%20image%2020260628120851.png)

**Respuesta:** 3viL

Pregunta 6) ¿Cuál es el nombre del archivo .mp4 que se está descargando? (Formato: file.mp4)

![Pasted image 20260628121834](../Fotos/Pasted%20image%2020260628121834.png)

**Respuesta:** Slow1.mp4

Pregunta 7) Uno de esos archivos era un archivo llamado privpyscript.py. ¿Cuál es el nombre del subproceso que el script está intentando cargar? (Formato: /algo/algo)

![Pasted image 20260628122437](../Fotos/Pasted%20image%2020260628122437.png)

![Pasted image 20260628122457](../Fotos/Pasted%20image%2020260628122457.png)

![Pasted image 20260628122518](../Fotos/Pasted%20image%2020260628122518.png)

**Respuesta:** /bin/bash
