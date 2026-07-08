- --
- Tags: #splunk #btlo #spilled_bucket_lab
- --

![Pasted image 20260602134907](../Fotos/Pasted%20image%2020260602134907.png)

1) ¿A qué objeto del bucket S3 accedió el atacante? (Formato: nombre del cubo)

![Pasted image 20260602142843](../Fotos/Pasted%20image%2020260602142843.png)

**Respuesta:** developers-configuration

2) ¿Cuál era la IP del atacante asociada en el acceso a S3? [Proporciona la IP sin colmillos] (Formato: XXX[.]XXX[.]XXX[.]XXX)

![Pasted image 20260602150904](../Fotos/Pasted%20image%2020260602150904.png)

**Respuesta:** 18[.]216[.]138[.]52

3) ¿Qué objeto/archivo accedió/descargó el atacante que permitiera acceder a su entorno? (Formato: Directorio/Archivo.extension)

![Pasted image 20260602152015](../Fotos/Pasted%20image%2020260602152015.png)

**Respuesta:** VPN-Profiles/DevelopersProfile_wg0.conf

4) Según la pregunta anterior, ¿qué software está asociado al archivo? Proporciona el nombre del software utilizado para este tipo de archivos. (Formato: Nombre)

`VPN-Profiles/DevelopersProfile_wg0.conf` (Archivo de configuracion de VPN WireGuard)

**Respuesta:** wireguard

5) Usando el archivo mencionado anteriormente, uno de los atacantes se conectó accidentalmente a través del sistema principal, lo que provocó la filtración de su dirección IP. ¿Cuál es la dirección IP del atacante? [Proporciona la IP sin colmillos]

![Pasted image 20260602154132](../Fotos/Pasted%20image%2020260602154132.png)

![Pasted image 20260602154525](../Fotos/Pasted%20image%2020260602154525.png)

**Respuesta:** 122[.]161[.]49[.]105

6) ¿Cuál era la IP privada de la instancia EC2 a la que se conectaba el atacante? (Formato: Dirección IP de la instancia)

![Pasted image 20260602155957](../Fotos/Pasted%20image%2020260602155957.png)

**Respuesta:** 10.0.1.125

7) El atacante realizó una enumeración adicional mientras estaba dentro de la instancia EC2 y encontró un rol que podía usarse más adelante asumiéndolo. ¿Cuál era la ARN del papel? (Formato: Rol ARN)

![Pasted image 20260602163556](../Fotos/Pasted%20image%2020260602163556.png)

![Pasted image 20260602163530](../Fotos/Pasted%20image%2020260602163530.png)

**Respuesta:** arn:aws:iam::764581110688:role/ec2-role

Explicacion:

`userIdentity.sessionContext.sessionIssuer.arn`
ARN del rol/usuario AWS que emitió la sesión usada para realizar la acción.
`ec2-role ✅`

```
arn:aws:iam::764581110688:role/ec2-role
```

Este sí encaja con la narrativa:

- atacante **ya está dentro de EC2**
- hace **enumeración IAM**
- descubre un **IAM Role**
- ese rol **puede ser asumido después (AssumeRole)**

Por eso el lab esperaba:

```
arn:aws:iam::764581110688:role/ec2-role
```
No porque tenga más count.
Sino porque **semánticamente coincide con el rol útil para privilege escalation / lateral movement**.
En cloud IR muchas veces no eliges por frecuencia (`count`), sino por:
> **¿Qué entidad tiene sentido operacional para el atacante?**

8) Utilizar el rol que el atacante dirigió IAM para lograr persistencia en el entorno. Proporciona las APIs utilizadas en orden de uso. (Formato: API 1, API 2, API 3)

![Pasted image 20260602170846](../Fotos/Pasted%20image%2020260602170846.png)

**Respuesta:** CreateUser, CreateAccessKey, AttachUserPolicy

Explicacion:
**eventSource = qué servicio AWS hizo/recibió la acción**  
**eventName = qué acción específica ocurrió** (En CloudTrail, muchos `eventName` representan **llamadas API de AWS**.)

9) Proporcionar el nombre de la identidad IAM creada durante la Persistencia. (Formato: Nombre de Identidad)

![Pasted image 20260602172138](../Fotos/Pasted%20image%2020260602172138.png)

**Respuesta:** web_engg_2 

10) ¿Qué política se asoció a la Identidad después? Proporciona la póliza ARN. (Formato: Política ARN)

![Pasted image 20260602173532](../Fotos/Pasted%20image%2020260602173532.png)

**Respuesta:** arn:aws:iam::aws:policy/AdministratorAccess 

11) Se encontró que otra instancia EC2 era accedida por el atacante usando SSH. Encuentra su dirección IP privada. (Formato: dirección IP)

![Pasted image 20260602175049](../Fotos/Pasted%20image%2020260602175049.png)

**Respuesta:** 10.0.2.32

**_interface_id_** Es una interfaz de red (ENI) en AWS. Las 3 IPs están conectadas a la misma interface_id, por lo que todas pertenecen al mismo servidor EC2. **_10.0.2.32_** es una IP con una interface_id diferente. Además, **_10.0.2.32_** es otro servidor interno, IP privada, mientras que los demás son públicos.

12) A partir de la instancia EC2 anterior, el atacante creó entonces una carcasa inversa. Encuentra la IP y el puerto del Reverse Shell. [Proporcionar la IP Defanged] (Formato: IP del atacante, puerto)

![Pasted image 20260602175827](../Fotos/Pasted%20image%2020260602175827.png)

**Respuesta:** 3[.]15[.]209[.]50, 13337

Un shell inverso suele tener menos eventos logarítmicos, logs que se generan cuando se establece la primera conexión. Y el 13337 es un puerto personalizado típico, normalmente usado para carcasa inversa.