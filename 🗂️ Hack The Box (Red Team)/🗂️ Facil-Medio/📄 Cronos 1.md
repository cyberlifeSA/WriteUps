- ---
- Tags: #ejptmachine #sqlinyection #cron #nettool
----

https://www.youtube.com/watch?v=aqAiBf0q2DE&ab_channel=Bobi%27swHack

`nmap -p- --min-rate 5000 -sCV -n 10.10.10.13 -oN portScan`
![Pasted image 20250102223537](../../Fotos/Pasted%20image%2020250102223537.png)

- Incluimos cronos.htb en el /etc/hosts
![Pasted image 20250102213246](../../Fotos/Pasted%20image%2020250102213246.png)

`dig axfr bank.htb @10.10.10.31`
- Para ver si hay mas hosts
admin.cronos.htb
![Pasted image 20250102213604](../../Fotos/Pasted%20image%2020250102213604.png)

`' or 1=1-- -`
- Test de SQLi, tras la inyeccion logramos acceso 
![Pasted image 20250102213708](../../Fotos/Pasted%20image%2020250102213708.png)

![Pasted image 20250102213737](../../Fotos/Pasted%20image%2020250102213737.png)
- Net Tool v0.1

Podemos seleccionar traceroute o ping
En traceroute si ponemos `;` y luego un comando tenemos automáticamente un RCE por lo que fácilmente podríamos hacer de inmediato una reverse shell
![Pasted image 20250102213906](../../Fotos/Pasted%20image%2020250102213906.png)

Ejecutamos la Reverse Shell
![Pasted image 20250102215511](../../Fotos/Pasted%20image%2020250102215511.png)

![Pasted image 20250102215609](../../Fotos/Pasted%20image%2020250102215609.png)

![Pasted image 20250102215647](../../Fotos/Pasted%20image%2020250102215647.png)

`Maquina Vulnerada`

User Flag: 2ec7ed4ca84a102e19682d7edd165be3

----

*Escalada de Privilegios*

`cat /etc/crontab`
![Pasted image 20250102215925](../../Fotos/Pasted%20image%2020250102215925.png)

Vamos  a la ruta de artisan que se ejecuta como root
![Pasted image 20250102220231](../../Fotos/Pasted%20image%2020250102220231.png)

`cat artisan`
![Pasted image 20250102220305](../../Fotos/Pasted%20image%2020250102220305.png)
![Pasted image 20250102220319](../../Fotos/Pasted%20image%2020250102220319.png)

`ls -la`
![Pasted image 20250102220412](../../Fotos/Pasted%20image%2020250102220412.png)

Haremos una reverse shell, la incluiremos en este archivo y luego como root y tarea cron se ejecutara en el momento correspondiente.

![Pasted image 20250102220613](../../Fotos/Pasted%20image%2020250102220613.png)

`locate reverse shell php`
![Pasted image 20250102220832](../../Fotos/Pasted%20image%2020250102220832.png)

![Pasted image 20250102220822](../../Fotos/Pasted%20image%2020250102220822.png)

![Pasted image 20250102221155](../../Fotos/Pasted%20image%2020250102221155.png)

![Pasted image 20250102221329](../../Fotos/Pasted%20image%2020250102221329.png)

`Maquina Vulnerada`

Root Flag: 819b1bf79c018bfc09c17c5819b69594

---

