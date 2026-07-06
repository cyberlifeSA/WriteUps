- --
- Tags: #wireshark #cyberdefenders #psexechunt_lab
- --
Para rastrear eficazmente las actividades del atacante dentro de nuestra red, ¿puedes identificar la dirección IP de la máquina desde la que el atacante obtuvo acceso inicialmente?
![Pasted image 20260701140332](../../Fotos/Pasted%20image%2020260701140332.png)
10.0.0.130

Para entender completamente el alcance de la brecha, ¿puedes determinar el nombre de host de la máquina al que el atacante pivotó primero?
![Pasted image 20260701141301](../../Fotos/Pasted%20image%2020260701141301.png)
![Pasted image 20260701141739](../../Fotos/Pasted%20image%2020260701141739.png)
sales-pc

Conocer el nombre de usuario de la cuenta que el atacante usó para la autenticación nos dará una visión sobre la magnitud de la brecha. ¿Cuál es el nombre de usuario que utiliza el atacante para la autenticación?
![Pasted image 20260701141932](../../Fotos/Pasted%20image%2020260701141932.png)
ssales

Después de averiguar cómo se movió el atacante dentro de nuestra red, necesitamos saber qué hizo en la máquina objetivo. ¿Cómo se llama el ejecutable de servicio que el atacante configuró en el objetivo?
![Pasted image 20260701142055](../../Fotos/Pasted%20image%2020260701142055.png)
PSEXESVC.exe

Necesitamos saber cómo el atacante instaló el servicio en la máquina comprometida para entender las tácticas de movimiento lateral del atacante. Esto puede ayudar a identificar otros sistemas afectados. ¿Qué reparto de red utilizó PsExec para instalar el servicio en la máquina objetivo?
![Pasted image 20260701142338](../../Fotos/Pasted%20image%2020260701142338.png)
ADMIN$

Debemos identificar la cuota de red utilizada para comunicarse entre ambas máquinas. ¿Qué cuota de red usaba PsExec para la comunicación?
![Pasted image 20260701142648](../../Fotos/Pasted%20image%2020260701142648.png)
IPC$

Ahora que tenemos una imagen más clara de las actividades del atacante en la máquina comprometida, es importante identificar cualquier movimiento lateral adicional. ¿Cuál es el nombre de host de la segunda máquina que el atacante apuntó para pivotar dentro de nuestra red?
![Pasted image 20260701143554](../../Fotos/Pasted%20image%2020260701143554.png)
MARKETING-PC