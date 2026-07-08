- --
- Tags: #ejptmachine #cifradocesar #reverseshell 
---

![Pasted image 20250130192204](../../Fotos/Pasted%20image%2020250130192204.png)

![Pasted image 20250130192551](../../Fotos/Pasted%20image%2020250130192551.png)

![Pasted image 20250130192259](../../Fotos/Pasted%20image%2020250130192259.png)

- mateo : hdvbfuadcb

**dcode.fr**

![Pasted image 20250130193640](../../Fotos/Pasted%20image%2020250130193640.png)

- easycrazy

Host descubierto con nmap apenas comenzamos la enumeracion

![Pasted image 20250130193948](../../Fotos/Pasted%20image%2020250130193948.png)

*http://*

![Pasted image 20250130194059](../../Fotos/Pasted%20image%2020250130194059.png)

*https://*

![Pasted image 20250130194301](../../Fotos/Pasted%20image%2020250130194301.png)

![Pasted image 20250130194328](../../Fotos/Pasted%20image%2020250130194328.png)

![Pasted image 20250130195214](../../Fotos/Pasted%20image%2020250130195214.png)

![Pasted image 20250130195236](../../Fotos/Pasted%20image%2020250130195236.png)

![Pasted image 20250130195400](../../Fotos/Pasted%20image%2020250130195400.png)

- Es necesario aplicar permisos de lectura y ejecución mínimo

![Pasted image 20250130195351](../../Fotos/Pasted%20image%2020250130195351.png)

- Cargamos la reverse shell php

/uploads

![Pasted image 20250130195548](../../Fotos/Pasted%20image%2020250130195548.png)

![Pasted image 20250130195559](../../Fotos/Pasted%20image%2020250130195559.png)

![Pasted image 20250130195905](../../Fotos/Pasted%20image%2020250130195905.png)

![Pasted image 20250130195913](../../Fotos/Pasted%20image%2020250130195913.png)

`sudo -u root /usr/bin/php -r "system('/bin/bash');"`

![Pasted image 20250130200307](../../Fotos/Pasted%20image%2020250130200307.png)

`sudo /usr/bin/php -r "pcntl_exec('/bin/sh', ['-p']);"`

- También aplica para darnos root en este caso

