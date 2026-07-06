- --
- Tags: #cyberdefenders #phishstrike_lab #urlhaus #vmray #cyberchef #malwarebaazar #recordedfuturetriage 
- --

Identificar la dirección IP del remitente con valores específicos de SPF y DKIM ayuda a rastrear el origen del correo de phishing. ¿Cuál es la dirección IP del remitente que tiene un valor SPF de falla suave y un valor DKIM de fallo?
![Pasted image 20260705204821](../../Fotos/Pasted%20image%2020260705204821.png)

Comprender la ruta de devolución de un correo electrónico es esencial para rastrear su origen. ¿Cuál es la ruta de devolución especificada en este correo?
![Pasted image 20260705205044](../../Fotos/Pasted%20image%2020260705205044.png)
erikajohana.lopez@uptc.edu.co

Identificar la fuente del malware es fundamental para una mitigación y respuesta eficaz a amenazas. ¿Cuál es la dirección IP del servidor que aloja el archivo malicioso relacionado con la distribución de malware?
![Pasted image 20260705205305](../../Fotos/Pasted%20image%2020260705205305.png)
107.175.247.199

Identificar malware que explota recursos del sistema para la minería de criptomonedas es fundamental para priorizar los esfuerzos de mitigación de amenazas. La URL maliciosa puede transmitir varios tipos de malware. ¿Qué familia de malware es responsable de la minería de criptomonedas?
![Pasted image 20260705210023](../../Fotos/Pasted%20image%2020260705210023.png)
CoinMiner

Identificar las URLs específicas que solicitan el malware es clave para interrumpir sus canales de comunicación y reducir su impacto. Basándonos en el análisis previo de la muestra de malware de criptomonedas, ¿qué URL solicita este malware?
![Pasted image 20260705210920](../../Fotos/Pasted%20image%2020260705210920.png)
Si bien aparece con la extension .png antiguamente la el archivo otro y la extension igual por eso no aparece en nuestra revision, esto habria que validarlo con reportes antiguos
http://ripley.studio/loader/uploads/Hjvnp.png
http://ripley.studio/loader/uploads/Qanjttrbv.jpeg

Comprender las entradas del registro añadidas a la clave de autoejecución por malware es crucial para identificar sus mecanismos de persistencia. Según el análisis de ejemplo de malware BitRAT, ¿cuál es el nombre del ejecutable en el primer valor añadido a la clave de auto-ejecución del registro?
![Pasted image 20260705212416](../../Fotos/Pasted%20image%2020260705212416.png)
Jzwvix.exe

Identificar el hash SHA-256 de archivos descargados de una URL maliciosa es esencial para rastrear y analizar la actividad del malware. Según el análisis de BitRAT, ¿cuál es el hash SHA-256 del archivo descargado y añadido previamente a las claves de autoactivación?
![Pasted image 20260705215726](../../Fotos/Pasted%20image%2020260705215726.png)
bf7628695c2df7a3020034a065397592a1f8850e59f9a448b555bc1c8c639539

Analizar las peticiones HTTP realizadas por malware ayuda a identificar sus patrones de comunicación. ¿Cuál es la URL en la petición HTTP que utiliza el loader para recuperar el malware BitRAT?
![Pasted image 20260705220711](../../Fotos/Pasted%20image%2020260705220711.png)
http://107.175.247.199/loader/server.exe

Introducir un retraso en la ejecución del malware puede ayudar a evadir los mecanismos de detección. ¿Cuál es el retraso (en segundos) causado por el comando PowerShell según el análisis de BitRAT?
![Pasted image 20260705221055](../../Fotos/Pasted%20image%2020260705221055.png)
50

Rastrear los dominios de comando y control (C2) utilizados por malware es esencial para detectar y bloquear actividades maliciosas. ¿Cuál es el dominio C2 utilizado por el malware BitRAT?
![Pasted image 20260705221417](../../Fotos/Pasted%20image%2020260705221417.png)
gh9st.mywire.org

Comprender cómo el malware exfiltra datos es esencial para detectar y prevenir brechas de datos. Según el análisis de AsyncRAT, ¿cuál es el ID de Telegram Bot utilizado por este malware?
[URLhaus | http://107.175.247.199/loader/install.exe](https://urlhaus.abuse.ch/url/2381718/)
[bitrat | 5ca468704e7ccb8e1b37c0f7595c54df4e2f4035345b6e442e8bd4e11c58f791 | Triage™](https://tria.ge/221025-a5kz9sbbcm/behavioral1)
![Pasted image 20260705222725](../../Fotos/Pasted%20image%2020260705222725.png)
![Pasted image 20260705222806](../../Fotos/Pasted%20image%2020260705222806.png)
bot5610920260
