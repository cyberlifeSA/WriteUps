- --
- Tags: #sqlitebrowser #cyberdefenders #krakenkeylogger_lab #LECmd #zimmerman #timelineexplorer
- --

¿Cuál es la aplicación de mensajería web que usó el empleado para hablar con el atacante?

`/Users/OMEN/AppData/Local/Microsoft/Windows/Notifications`

![](../../Fotos/Pasted%20image%2020260708115645.png)

![](../../Fotos/Pasted%20image%2020260708160603.png)

**Respuesta:** Telegram

¿Cuál es la contraseña del archivo ZIP protegido que el atacante envió al empleado?

![](../../Fotos/Pasted%20image%2020260708161119.png)

**Respuesta:** @1122d

¿Qué dominio utilizó el atacante para descargar la segunda etapa del malware?

![](../../Fotos/Pasted%20image%2020260708170638.png)

Para tener el resultado esperado hay que utilizar la version concreta de LECmd 1.5.0.0 ya que con la ultima el resultado se corta, al menos en mi caso.

![](../../Fotos/Pasted%20image%2020260708172351.png)

![](../../Fotos/Pasted%20image%2020260708172114.png)

**aht1.sen/hi/coucys.erstmaofershma//s:tpht**

```python
input_string = 'aht1.sen/hi/coucys.erstmaofershma//s:tpht'  
output_string = ''  
# Reverse the input string  
input_string = input_string[::-1]  
# Iterate through the reversed string  
for i in range(0, len(input_string), 2):  
try:  
# Take two characters at a time  
tmp = input_string[i] + input_string[i + 1]  
# Reverse the two characters  
tmp = tmp[::-1]  
# Append the reversed characters to the output string  
output_string += tmp  
except IndexError:  
# If there's an odd number of characters, just append the last one  
output_string += input_string[i]  
print(output_string)
```

**Respuesta:** [https://masherofmasters.cyou/chin/se1.hta](https://masherofmasters.cyou/chin/se1.hta)

¿Cuál es el nombre del comando que el atacante inyectó usando uno de los LOLAPPS instalados en la máquina para lograr persistencia?

![](../../Fotos/Pasted%20image%2020260708174445.png)

![](../../Fotos/Pasted%20image%2020260708174427.png)

**Respuesta:** jlhgfjhdflghjhuhuh

¿Cuál es la ruta completa del archivo malicioso que el atacante utilizó para lograr persistencia?


![](../../Fotos/Pasted%20image%2020260708175755.png)

**Respuesta:** C:\Users\OMEN\AppData\Local\Temp\templet.lnk

¿Cuál es el nombre de la aplicación que el atacante utilizó para la exfiltración de datos?

`/OMEN/AppData/Roaming/AnyDesk/ad.trace`

![](../../Fotos/Pasted%20image%2020260709005243.png)

**Respuesta:** Anydesk

¿Cuál es la dirección IP del atacante?

En este caso lo hemos realizado revisando el archivo `ad.trace` manualmente pero la idea es utilizar alguna herramienta como **Timeline Explorer**

![](../../Fotos/Pasted%20image%2020260709004034.png)

![](../../Fotos/Pasted%20image%2020260709004148.png)

![](../../Fotos/Pasted%20image%2020260709004244.png)

**Respuesta:** 57.128.101.75