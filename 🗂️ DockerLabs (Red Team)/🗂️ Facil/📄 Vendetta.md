- --
- Tags: #ejptmachine #sqlinyection #hydra #medusa 
---

![Pasted image 20250222201933](../../Fotos/Pasted%20image%2020250222201933.png)

![Pasted image 20250222202116](../../Fotos/Pasted%20image%2020250222202116.png)

error con hydra
`hydra -l V -P /usr/share/wordlists/rockyou.txt mysql://172.17.0.2 -t 64`
![Pasted image 20250222204848](../../Fotos/Pasted%20image%2020250222204848.png)

error con medusa
![Pasted image 20250222204822](../../Fotos/Pasted%20image%2020250222204822.png)

Existe algún mecanismo que bloquea la fuerza bruta por lo que daremos vuelta el diccionario para probar

`tac /usr/share/wordlists/rockyou.txt >> invertido.txt`
- daremos la vuelta al diccionario.
`cat invertido.txt | tr -cd '[:alnum:]\n' > invertido2.txt`
- Y ahora quitaremos todos los numeros que no nos van a servir.



**LA MAQUINA DA MUCHOS ERRORES CON LO DE MYSQL**