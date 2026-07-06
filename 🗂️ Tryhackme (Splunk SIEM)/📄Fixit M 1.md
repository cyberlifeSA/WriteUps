- --
- Tags: #splunk #siem 
- - -
**Importante**: El objetivo de este laboratorio es que parte sin todos los Fields que necesitas para completarlo de un inicio el trabajo fuerte es crear un archivo que llame a que aparezcan las columnas necesarias para poder responder cada respuesta del laboratorio ese es el punto fuerte por que el resto es extremadamente facil lo que te piden, revisarlo si en algun punto creemos necesario afianzar este conocimiento mas alla.

*Pregunta 1*
¿Cuál es el camino completo del directorio de la app FIXIT?  
![Pasted image 20260118132702](../Fotos/Pasted%20image%2020260118132702.png)   ![Pasted image 20260118134016](../Fotos/Pasted%20image%2020260118134016.png)
Respuesta: /opt/splunk/etc/apps/fixit

*Pregunta 2*
¿Qué estrofa usaremos para definir el Límite de Evento en este caso de Evento de múltiples líneas?
Filtro: `index=main Network-log`
![400](../Fotos/Pasted%20image%2020260118133603.png)
![400](../Fotos/Pasted%20image%2020260118133550.png)
Respuesta: BREAK_ONLY_BEFORE

*Pregunta 3*
En el inputs.conf, ¿cuál es la ruta completa del script network-logs?  
![Pasted image 20260118133916](../Fotos/Pasted%20image%2020260118133916.png)
Respuesta: /opt/splunk/etc/apps/fixit/bin/network-logs

*Pregunta 4*
¿Qué patrón regex nos ayudará a definir el inicio del evento?
Enlace: [regex101: build, test, and debug regex](https://regex101.com/)
reges ejemplo tryhackme para trabajar y ponerlos en el enlace:
```c
[Network-log]: User named Johny Bil from Development department accessed the resource Cybertees.THM/about.html from the source IP 192.168.0.1 and country 
Japan at: Thu Sep 28 00:13:46 2023
[Network-log]: User named Johny Bil from Marketing department accessed the resource Cybertees.THM/about.html from the source IP 192.168.2.2 and country 
Japan at: Thu Sep 28 00:13:46 2023
[Network-log]: User named Johny Bil from HR department accessed the resource Cybertees.THM/about.html from the source IP 10.0.0.3 and country 
Japan at: Thu Sep 28 00:13:46 2023
```
Respuesta: `\[Network-log\]`

*Pregunta 5*
¿Cuál es el dominio capturado?
![Pasted image 20260118134700](../Fotos/Pasted%20image%2020260118134700.png)
Respuesta: Cybertees.THM

---
*Pregunta 6*
¿Cuántos países aparecen en los registros?
Respuesta: 12

*Pregunta 7*
¿Cuántos departamentos aparecen en los registros?  
Respuesta: 6

*Pregunta 8*
¿Cuántos nombres de usuario aparecen en los registros?
Respuesta: 28

*Pregunta 9*
¿Cuántas IPs de origen se capturan en los registros?
Respuesta: 52

Para estas 5 preguntas hay que crear un archivo y luego reiniciar el splunk se deja walkthrough para revisarlo a fondo en cuanto confirmemos que es necesario pasar a esto:
Enlace: https://regex101.com/
Paso a paso: [TryHackMe Room — Arregla. Soluciona el problema del análisis de registros y analiza... | por Haircutfish | Medio](https://medium.com/@haircutfish/tryhackme-room-fixit-b3df2c4dd313)
La idea es que una vez creado el archivo como se menciona y reiniciado el splunk podemos filtrar por nuevos Fields y directamente encontrar la respuesta que buscamos.
![Pasted image 20260118140801](../Fotos/Pasted%20image%2020260118140801.png)
**Otra forma, esto es clave:**
![Pasted image 20260118140938](../Fotos/Pasted%20image%2020260118140938.png)

---
*Pregunta 10*
¿Qué archivos de configuración se usaron para solucionar nuestro problema? [Orden alfabético: Archivo1, archivo2, archivo3]  
Respuesta: fields.conf, props.conf, transforms.conf

*Pregunta 11*
¿Cuáles son los dos principales países desde los que el usuario Robert intentó acceder al dominio? [Respuesta entre comas y en orden alfabético][Formato: Country1, Country2]  
![Pasted image 20260118141344](../Fotos/Pasted%20image%2020260118141344.png)  ![Pasted image 20260118141350](../Fotos/Pasted%20image%2020260118141350.png)  ![Pasted image 20260118141406](../Fotos/Pasted%20image%2020260118141406.png)
Respuesta: Canada, United States

*Pregunta 12*
¿Qué usuario accedió a la secret-document.pdf en la web?
Filtro: `index=main Domain=*secret-document.pdf`
![Pasted image 20260118141948](../Fotos/Pasted%20image%2020260118141948.png)   ![Pasted image 20260118142030](../Fotos/Pasted%20image%2020260118142030.png)
Respuesta: Sarah Hall