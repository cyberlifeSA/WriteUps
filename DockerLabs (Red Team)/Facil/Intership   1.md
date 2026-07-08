- --
- Tags: #ejptmachine #sqlinyection #rot13 #reverseshell 
---

![Pasted image 20250302152755](../../Fotos/Pasted%20image%2020250302152755.png)

![Pasted image 20250302152811](../../Fotos/Pasted%20image%2020250302152811.png)

![Pasted image 20250302153201](../../Fotos/Pasted%20image%2020250302153201.png)

![Pasted image 20250302153250](../../Fotos/Pasted%20image%2020250302153250.png)

Ahora si podemos interactuar con la web

![Pasted image 20250302153307](../../Fotos/Pasted%20image%2020250302153307.png)

![Pasted image 20250302153336](../../Fotos/Pasted%20image%2020250302153336.png)

![Pasted image 20250302153346](../../Fotos/Pasted%20image%2020250302153346.png)

![Pasted image 20250302153515](../../Fotos/Pasted%20image%2020250302153515.png)

- login.php 

![Pasted image 20250302153717](../../Fotos/Pasted%20image%2020250302153717.png)

![Pasted image 20250302153730](../../Fotos/Pasted%20image%2020250302153730.png)

- spam

`' or 1=1-- -`

![Pasted image 20250302153921](../../Fotos/Pasted%20image%2020250302153921.png)

![Pasted image 20250302153929](../../Fotos/Pasted%20image%2020250302153929.png)

![Pasted image 20250302154110](../../Fotos/Pasted%20image%2020250302154110.png)

![Pasted image 20250302154132](../../Fotos/Pasted%20image%2020250302154132.png)

- pasantes : Valentina Gomez / Pedro Ramirez 
- Clave purpl3

![Pasted image 20250302154321](../../Fotos/Pasted%20image%2020250302154321.png)

![Pasted image 20250302154332](../../Fotos/Pasted%20image%2020250302154332.png)

`ps faux`

![Pasted image 20250302154923](../../Fotos/Pasted%20image%2020250302154923.png)

- Este script lo ejecuta valentina cada 45 segundos, tenemos permisos de escritura en el por lo que lo modificaremos para escalar privilegios a valentina

![Pasted image 20250302155054](../../Fotos/Pasted%20image%2020250302155054.png)

Como el script se ejecuta cada 45 segundos solamente esperamos estando en escucha por el puerto designado

![Pasted image 20250302155237](../../Fotos/Pasted%20image%2020250302155237.png)

![Pasted image 20250302155614](../../Fotos/Pasted%20image%2020250302155614.png)

![Pasted image 20250302155405](../../Fotos/Pasted%20image%2020250302155405.png)

Necesitamos ver si hay algo interesante en la imagen en contrada por lo que para poder pasarlo a nuestra maquina atacante la pasaremos a /tmp y de ahi la compartiremos con `scp`  a nuestra maquina ya que no tenemos disponible python por ejemplo.

`cp ~/profile_picture.jpeg /tmp`

![Pasted image 20250302155721](../../Fotos/Pasted%20image%2020250302155721.png)

![Pasted image 20250302155910](../../Fotos/Pasted%20image%2020250302155910.png)

![Pasted image 20250302155919](../../Fotos/Pasted%20image%2020250302155919.png)

`scp pedro@172.17.0.2:/tmp/profile_picture.jpeg .`

![Pasted image 20250302160022](../../Fotos/Pasted%20image%2020250302160022.png)

`teghide --extract -sf profile_picture.jpeg`

![Pasted image 20250302160341](../../Fotos/Pasted%20image%2020250302160341.png)

- mag1ck

Vemos que escalando con script de pedro a valentina, siendo ya valentina sudo -l no funfiona sin embargo si con ssh directamente logeamos a valentina sabiendo claramente la constrasena, si nos permite hacer `sudo -l`

![Pasted image 20250302160430](../../Fotos/Pasted%20image%2020250302160430.png)

![Pasted image 20250302160453](../../Fotos/Pasted%20image%2020250302160453.png)

----

### Manera fácil y corta 

![Pasted image 20250302160654](../../Fotos/Pasted%20image%2020250302160654.png)

### Manera fácil pero mas larga

![Pasted image 20250302160814](../../Fotos/Pasted%20image%2020250302160814.png)

![Pasted image 20250302160909](../../Fotos/Pasted%20image%2020250302160909.png)
