- ---
- Tags: #ejptmachine #sqlinyection #reverseshell 
---

Se debe realizar en maquina kali ya que en el personalizado hay problemas con la tty

![Pasted image 20250118202221](../../Fotos/Pasted%20image%2020250118202221.png)

Reverse shell php
![Pasted image 20250118203159](../../Fotos/Pasted%20image%2020250118203159.png)

Es necesario hacer tratamiento de tty
![Pasted image 20250118204011](../../Fotos/Pasted%20image%2020250118204011.png)

escalamos a pingu luego con sudo -l tenemos el binario dpkg y usamos gtfo bins luego nuevamente !/bin/bash con usuario gladys y ahora somos gladys luego aplicamos esto jugnado con chown que tiene permisos sudo gladys como root
![Pasted image 20250118204914](../../Fotos/Pasted%20image%2020250118204914.png)

![Pasted image 20250118205425](../../Fotos/Pasted%20image%2020250118205425.png)

Tras todo esto solo con hacer sudo su siendo gladys ya seremos root


