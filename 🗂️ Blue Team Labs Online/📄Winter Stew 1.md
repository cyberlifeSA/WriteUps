- --
- Tags: #wireshark #btlo #winterstew_lab
- --
P1) La máquina anfitriona en la captura tiene la mayor cantidad de tráfico, ¿cuál es la IP del endpoint del anfitrión? (Formato: X.X.X.X)
![Pasted image 20260623173249](../Fotos/Pasted%20image%2020260623173249.png)
192.168.1.120

P2) Los ingenieros en el lugar nos dicen que esta máquina se conecta a la DMZ pero también tiene acceso a internet. ¿Eso significa que el endpoint debe tener dos [en blanco] de red? (Formato: String)
interfaces

P3) hmm, si hay dos de esos, eso debe significar que hay otra IP. Si yo fuera un atacante, probablemente empezaría con uno de los primeros pasos de la cadena de eliminación cibernética. ¿Qué herramienta de reconocimiento de red se utiliza y cuál es la cadena en la columna de información donde aparece el nombre por primera vez? (Formato: herramienta, XXXX /XXX XXXX/X.X)
```bash
frame matches "(?i)nmap"
```
![Pasted image 20260623174610](../Fotos/Pasted%20image%2020260623174610.png)
nmap POST /sdk HTTP/1.1

P4) Esto puede ayudarnos a obtener la otra IP de la máquina anfitriona (Formato: X.X.X.X)
![Pasted image 20260623175209](../Fotos/Pasted%20image%2020260623175209.png)
192.168.90.3

P5) Viendo lo que ocurrió antes del escaneo nmap, parece que hubo otro tipo de escaneo. Solo necesitamos el nombre genérico y no la herramienta (Formato: escaneo xxx)
![Pasted image 20260623180302](../Fotos/Pasted%20image%2020260623180302.png)
arp scan

P6) ¿Cuál es la IP del endpoint en la DMZ con nuestro anfitrión? (Formato: X.X.X.X)
![Pasted image 20260623181044](../Fotos/Pasted%20image%2020260623181044.png)
192.168.90.5

P7) Según el escaneo del Q3, ¿qué dos puertos están abiertos? (Formato: puerto, puerto)
![Pasted image 20260623182130](../Fotos/Pasted%20image%2020260623182130.png)
8009, 8080

P8) ¿Qué protocolo se ejecuta en el puerto superior? (Formato: protocolo)
![Pasted image 20260623184056](../Fotos/Pasted%20image%2020260623184056.png)
http

P9) Sabemos que la planta tiene software de operaciones ejecutándose en ese endpoint. ¿Cuál es la URL de inicio de sesión y cuáles son las credenciales a las que acceder? (Formato: http://x.x.x.x:port/something, usuario:password)
![Pasted image 20260623190527](../Fotos/Pasted%20image%2020260623190527.png)
http://192.168.90.5:8080/ScadaBR/login.htm,  admin:admin

P10) Oh, no, esto es malo. Parece que Glacier podría haberse conectado a la red. Voy a cortar el cable entre OT y TI, pero ¿puedes encontrar el número de paquete que corresponde a la redirección después de introducir las credenciales? (Formato: Número)
23280

```powershell
Red Interna                DMZ                     (Posible Zona SCADA / OT)
192.168.1.0/24       192.168.90.0/24
     │                       │
  192.168.1.120  ──────  192.168.90.3   ← Atacante (usa esta IP)
     (Monitoreo)               │
                               ↓
                          192.168.90.5 (SCADA)
```