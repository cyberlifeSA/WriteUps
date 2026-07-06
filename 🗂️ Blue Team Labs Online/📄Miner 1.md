- --
- Tags: #wireshark #networkminer #btlo #miner_lab
- --
Según Network Miner, ¿qué intento de exploit se está haciendo en qué protocolo?
Abrimos cada uno de los archivos Pcap en Wireshark y guardamos como formato Pcap más antiguo para poder abrirlo en networkminer
![Pasted image 20260627103549](../Fotos/Pasted%20image%2020260627103549.png)
![Pasted image 20260627104446](../Fotos/Pasted%20image%2020260627104446.png)
Eternalblue, SMB

¿Cuál es la dirección IP de la máquina responsable que realiza el escaneo de red usando Nmap?
![Pasted image 20260627104655](../Fotos/Pasted%20image%2020260627104655.png)
172.16.0.4

Identifica el servidor de Mando y Control. ¿Cuál es el nombre de host, la dirección MAC y los números de puerto de escucha?
![Pasted image 20260627105706](../Fotos/Pasted%20image%2020260627105706.png)
IE11WIN8_1, 080027FEEC1E, 139, 4782

¿Cuáles son las credenciales bancarias de la víctima?
Segundo paquete aplicamos filtro y encontramos POST con credenciales
![Pasted image 20260627130323](../Fotos/Pasted%20image%2020260627130323.png)
`christiww007, christismoneyworld$$`

¿Cuál es la respuesta observada de NTLM para el desafío desde VICTIM Machine?
![Pasted image 20260627124729](../Fotos/Pasted%20image%2020260627124729.png)
46E55566E4DF5613