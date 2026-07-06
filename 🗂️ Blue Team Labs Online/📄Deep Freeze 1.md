- --
- Tags: #splunk #btlo #deep_freeze_lab
- --

![Pasted image 20260601235822](../Fotos/Pasted%20image%2020260601235822.png)

![Pasted image 20260602001151](../Fotos/Pasted%20image%2020260602001151.png)
*1) El equipo de TI aplicó accidentalmente una política excesivamente permisiva a la cuenta de usuario AWS del sujeto, en lugar de una política específica. ¿Qué política se aplicó y cuál es el nombre de usuario del sujeto? (Formato: Nombre de usuario, Nombre de la Política)*
CodyShaddock, GrantS3FullAccess

*2) ¿Cuál es el nombre de usuario de la cuenta que hizo este cambio? (Formato: Nombre de usuario)*
TimSmithADMIN

*3) ¿Qué cubos S3 se pusieron a disposición del sujeto? (Formato: Bucket1, Bucket2)*
lab-bucket-sensitive/, lab-bucket-general/

*4) ¿Cuál es el ID de la sección de preparación de la Matriz de Amenazas Internas más relevante para esta actividad? (Formato: PRXXX)*
[Increase Privileges | Preparation](https://insiderthreatmatrix.org/articles/AR3/sections/PR024)
![Pasted image 20260602001730](../Fotos/Pasted%20image%2020260602001730.png)
PR004

*5) ¿Qué dirección IP usó el sujeto al enumerar los cubos S3 y qué nombre de evento está presente? (Formato: X.X.X.X, NombreDeEvento)*
![Pasted image 20260602001455](../Fotos/Pasted%20image%2020260602001455.png)
203.0.113.46, ListBuckets

*6) ¿Qué tipo de recuperación se utilizó y cuál es el nombre del evento? (Formato: xxxx, NombreDeEvento)*
![Pasted image 20260602002711](../Fotos/Pasted%20image%2020260602002711.png)
bulk ,RestoreObject

*7) ¿Qué cubo fue objetivo en la solicitud de recuperación? (Formato: NombreCubo)*
lab-bucket-sensitive

*8) ¿Cuál es la marca de tiempo de la solicitud de recuperación? (Formato: YYY-MM-DDTHH:MM:SS)*
2024-11-29T12:30:00Z

*9) ¿Cuáles son los nombres de los archivos descargados por el sujeto? (Formato: archivo1.ext, archivo2.ext)*
![Pasted image 20260602002734](../Fotos/Pasted%20image%2020260602002734.png)  ![Pasted image 20260602003051](../Fotos/Pasted%20image%2020260602003051.png) 
project-orion-CONFIDENTIAL.zip, project-chimera-CONFIDENTIAL.zip"

*10) ¿Cuál es el ID de la sección de preparación de la Matriz de Amenazas Internas más relevante para esta actividad? (Formato: PRXXX)*
![Pasted image 20260602003628](../Fotos/Pasted%20image%2020260602003628.png)
PR025

*11) ¿Qué política se eliminó de la cuenta de usuario del sujeto y cuál es el nombre del evento? (Formato: PolicyName, EventoName)*
![Pasted image 20260602004109](../Fotos/Pasted%20image%2020260602004109.png)
GrantS3FullAccess, DeleteUserPolicy

*12) ¿Cuál es el ID de sección anti-forense de la Matriz de Amenazas Internas más relevante para esta actividad? (Formato: AFXXX)*
![Pasted image 20260602004335](../Fotos/Pasted%20image%2020260602004335.png)
AF015
