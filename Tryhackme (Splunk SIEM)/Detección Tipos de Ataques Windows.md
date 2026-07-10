- --
- Tags: #cdsa_htb #splunk 
- ---
# Splunk Fundamentals

## Resources (revisar he ir borrando de esta lista)

[Splunk - De cero a experto](https://www.udemy.com/course/splunk-para-dummies/?couponCode=CP260518ALMXLD)
[Splunk Core Certified User Exámenes 2026](https://www.udemy.com/course/splunk-core-certified-user-examenes-2026/?couponCode=CP260518ALMXLD)
[Splunk Core Certified Power User Exámenes 2026](https://www.udemy.com/course/splunk-core-certified-power-user-examenes-2026/?couponCode=CP260518ALMXLD)
[Técnicas de Caza de Amenazas](https://www.udemy.com/course/tecnicas-de-caza-de-amenazas/?couponCode=CP260518ALMXLD)

SPLUNK dataset
[Boss of the SOC v3 Dataset Released! | Splunk](https://www.splunk.com/en_us/blog/security/botsv3-dataset-released.html)

SPLUNK documentation
[About the Search Tutorial | Splunk Enterprise (last updated 2025-07-04T03:45:32.593Z)](last%20updated%202025-07-04T03:45:32.593Z)))

SPLUNK tutorials
[Splunk Tutorial](https://www.tutorialspoint.com/splunk/index.htm)

**Nuevo**
Web Certs
[TheCyberCerts | Security Certification Roadmap](https://thecybercerts.com/)

BTLO Practice Labs
[Blue Team Labs Online - Cyber Range](https://blueteamlabs.online/)

stats

![500](../Fotos/Pasted%20image%2020260523125519.png)

timechart
chart

![700](../Fotos/Pasted%20image%2020260525102555.png)

timewrap
eval (calculations) fields+operators+functions

![500](../Fotos/Pasted%20image%2020260523133345.png)

![500](../Fotos/Pasted%20image%2020260524190240.png)

![Pasted image 20260524211221](../Fotos/Pasted%20image%2020260524211221.png)

bin
sort (-|+)
rename
Data normalization

![500](../Fotos/Pasted%20image%2020260524195622.png)

validate

![500](../Fotos/Pasted%20image%2020260524205748.png)

*Dentro de eval*
searchmatch

![600](../Fotos/Pasted%20image%2020260524213723.png)

cidrmatch

![600](../Fotos/Pasted%20image%2020260524215040.png)

match

![600](../Fotos/Pasted%20image%2020260524215243.png)

replace function

![600](../Fotos/Pasted%20image%2020260524230400.png)

![600](../Fotos/Pasted%20image%2020260524230557.png)

...
fieldfortmat (utiliza las mismas funciones que las de eval)
ejemplo sin fieldformat

![700](../Fotos/Pasted%20image%2020260525102120.png)

ejemplo con fieldformat

![Pasted image 20260525102200](../Fotos/Pasted%20image%2020260525102200.png)

fillnull (command)

![500](../Fotos/Pasted%20image%2020260525102343.png)

example

![600](../Fotos/Pasted%20image%2020260525102436.png)

example without fillnull

![700](../Fotos/Pasted%20image%2020260525102555.png)

example with fillnull

![700](../Fotos/Pasted%20image%2020260525102731.png)

example with fullnull + value

![700](../Fotos/Pasted%20image%2020260525102821.png)

where (command - support eval expression's)

![600](../Fotos/Pasted%20image%2020260525102939.png)

![500](../Fotos/Pasted%20image%2020260525102951.png)

![600](../Fotos/Pasted%20image%2020260525103034.png)

![700](../Fotos/Pasted%20image%2020260525104317.png)

![600](../Fotos/Pasted%20image%2020260525104558.png)

(no usar * ya que lo toma como matematico en cambio usar % como reemplazo)

![500](../Fotos/Pasted%20image%2020260525104715.png)

where + isnull (Muéstrame solo las filas donde el campo `sum` está vacío o no existe.)

![700](../Fotos/Pasted%20image%2020260525105031.png)

![700](../Fotos/Pasted%20image%2020260525105051.png)

appendpipe (va luego del comando stats)

![500](../Fotos/Pasted%20image%2020260525131209.png)
 
 ![800](../Fotos/Pasted%20image%2020260525132603.png)

 aqui agregamos un `as count` extra

 ![800](../Fotos/Pasted%20image%2020260525132820.png)

 +sort

![800](../Fotos/Pasted%20image%2020260525132856.png)

![800](../Fotos/Pasted%20image%2020260525132919.png)

![800](../Fotos/Pasted%20image%2020260525132935.png)

Pueden haber campos username vacios por lo que hay que crear nuevos fields

![Pasted image 20260525133015](../Fotos/Pasted%20image%2020260525133015.png)

![Pasted image 20260525133132](../Fotos/Pasted%20image%2020260525133132.png)

Resultado (especie de label) Estás resolviendo el problema de que algunos eventos no tienen `username`, entonces creas un valor alternativo para que la tabla se vea clara.

![Pasted image 20260525133148](../Fotos/Pasted%20image%2020260525133148.png)

mas desarrollo de pipeline

![800](../Fotos/Pasted%20image%2020260525135200.png)

mas desarrollo de pipeline

![Pasted image 20260525135232](../Fotos/Pasted%20image%2020260525135232.png)

eventstats (command)

![500](../Fotos/Pasted%20image%2020260525143242.png)

example

![600](../Fotos/Pasted%20image%2020260525143322.png)

exercise 

![800](../Fotos/Pasted%20image%2020260525143709.png)

![400](../Fotos/Pasted%20image%2020260525143729.png)

![800](../Fotos/Pasted%20image%2020260525144017.png)

- **`stats`** → resume/agrupa y **pierdes los eventos originales**.
- **`eventstats`** → calcula estadísticas, **pero mantiene los eventos originales** y añade el resultado como un nuevo campo.

![800](../Fotos/Pasted%20image%2020260525151058.png)

![800](../Fotos/Pasted%20image%2020260525152140.png)

![500](../Fotos/Pasted%20image%2020260525152213.png)

example 2

![500](../Fotos/Pasted%20image%2020260525152314.png)

1 

![Pasted image 20260525161511](../Fotos/Pasted%20image%2020260525161511.png)

2

![Pasted image 20260525161537](../Fotos/Pasted%20image%2020260525161537.png)

3

![Pasted image 20260525161722](../Fotos/Pasted%20image%2020260525161722.png)

4

![Pasted image 20260525161814](../Fotos/Pasted%20image%2020260525161814.png)

5

*strftime* (String Format Time - Convierte una fecha/hora a un formato legible.)
`%A` = **nombre completo del día de la semana**
|Fecha|Resultado|
|2026-05-25|Monday|
|2026-05-26|Tuesday|
|2026-05-27|Wednesday|

![Pasted image 20260525161855](../Fotos/Pasted%20image%2020260525161855.png)

6

![Pasted image 20260525162204](../Fotos/Pasted%20image%2020260525162204.png)

7

*tostring*
`tostring(totalSales, "commas")`
Convierte un número a **string (texto)** con separadores de miles.
`totalSales = 1500000` with `tostring(totalSales, "commas")` = `1,500,000`

En Splunk, el **punto `.`** significa **concatenación de strings** (unir texto).
Ejemplo:
```
"Hola " . "Mundo"
```
Resultado:
```
Hola Mundo
```
Entonces:
```
"$" . tostring(totalSales,"commas")
```
significa:
**agrega `$` delante del número formateado**.

Antes `2500000`
Despues $ + tostring `$2,500,000`

![Pasted image 20260525162245](../Fotos/Pasted%20image%2020260525162245.png)

streamstats

![500](../Fotos/Pasted%20image%2020260525162920.png)

![650](../Fotos/Pasted%20image%2020260525163223.png)

![850](../Fotos/Pasted%20image%2020260525163408.png)

Aqui se trata de enumerar intentos de alcanzar servidor FTP remoto ya que se indica que dentro de la organizacion no se esta pudiendo realizar esta operacion y piden un informe de los intentos por ejemplo

![1000](../Fotos/Pasted%20image%2020260525164124.png)

xyseries (utiliza estadisticas para mostrarlos en tablas) lo mismo que hace chart y timecahrt pero aqui se procesan aun mas los datos

![Pasted image 20260525174654](../Fotos/Pasted%20image%2020260525174654.png)

![Pasted image 20260525175528](../Fotos/Pasted%20image%2020260525175528.png)

![500](../Fotos/Pasted%20image%2020260525175556.png)

![Pasted image 20260525180419](../Fotos/Pasted%20image%2020260525180419.png)

![Pasted image 20260525180428](../Fotos/Pasted%20image%2020260525180428.png)

![Pasted image 20260525180539](../Fotos/Pasted%20image%2020260525180539.png)

![Pasted image 20260525180549](../Fotos/Pasted%20image%2020260525180549.png)

untable (**Convierte columnas en filas**.)
`untable` **despivotea** una tabla.
Convierte **columnas → filas**.
Es básicamente el **opuesto de `chart` / `xyseries`**.

![500](../Fotos/Pasted%20image%2020260525181047.png)

![500](../Fotos/Pasted%20image%2020260525210915.png)

![Pasted image 20260525181417](../Fotos/Pasted%20image%2020260525181417.png)

![Pasted image 20260525181947](../Fotos/Pasted%20image%2020260525181947.png)

![Pasted image 20260525182422](../Fotos/Pasted%20image%2020260525182422.png)

untable VS xyseries

![1100](../Fotos/Pasted%20image%2020260525201922.png)

stats

![700](../Fotos/Pasted%20image%2020260525202206.png)

chart x, y

![700](../Fotos/Pasted%20image%2020260525202231.png)

stats + xyseries (same result that upper)
es basicamente la primera columna es el x y bajo el x se aplican los multiples campos o resultados que podria tener el y en este caso status los cuales se pueden delimitar y cuantificar con count logicamente

![700](../Fotos/Pasted%20image%2020260525202355.png)

Los códigos HTTP (`200`, `400`, `404`...) se transformaron en nombres de columnas.
**modifica el campo llamado `200`**. `200/(1024*1024)`
1024 * 1024 = 1048576 Eso equivale a **1 MB** (1 Megabyte).
Entonces: `bytes / 1048576`
Supón que en la columna `200` tienes:
```
2097152
```
bytes.
Entonces:
```
2097152 / 1048576
```
=
```
2
```
MB.

![800](../Fotos/Pasted%20image%2020260525205227.png)

round

![800](../Fotos/Pasted%20image%2020260525205547.png)

more clean

![800](../Fotos/Pasted%20image%2020260525205617.png)

`+ xyseries`

![800](../Fotos/Pasted%20image%2020260525205714.png)

other explanation

![800](../Fotos/Pasted%20image%2020260525205745.png)

explanation with chart 
suma los valores de price como revenue y luego lanza x por product_name e y como clientip donde revenue esta contenido en la tabla

![800](../Fotos/Pasted%20image%2020260525210011.png) 

![72](../Fotos/Pasted%20image%2020260525210210.png)

userther=f (for delete the other column)

![800](../Fotos/Pasted%20image%2020260525210259.png)

addtotals (adds a new column with title Total like a sum x product)

![800](../Fotos/Pasted%20image%2020260525210351.png)

sort 5 total (por ordenar acendente to decending order x default minum to  more and with minum sign - we can order decending for more to minum values)

![800](../Fotos/Pasted%20image%2020260525210444.png)

untable

![800](../Fotos/Pasted%20image%2020260525210657.png)

xyseries (es como volver a dejarlo con chart)

![800](../Fotos/Pasted%20image%2020260525210947.png)

aqui le agregamos addtotals y sort para algo mas especifico

![800](../Fotos/Pasted%20image%2020260525211045.png)


| foreach
![Pasted image 20260526113928](../Fotos/Pasted%20image%2020260526113928.png)

this is the same for host's

![Pasted image 20260526114053](../Fotos/Pasted%20image%2020260526114053.png)

same result's

![Pasted image 20260526114009](../Fotos/Pasted%20image%2020260526114009.png)

Esto que hacemos podria perfectamente hacerse con xyseries

![Pasted image 20260526114639](../Fotos/Pasted%20image%2020260526114639.png)

Modifying Field values with | eval

![800](../Fotos/Pasted%20image%2020260526120302.png)

![800](../Fotos/Pasted%20image%2020260526120555.png)

same expression form 1

![800](../Fotos/Pasted%20image%2020260526124227.png)

same expression form 2

![800](../Fotos/Pasted%20image%2020260526124306.png)

same expression form 3

![800](../Fotos/Pasted%20image%2020260526124925.png)

![800](../Fotos/Pasted%20image%2020260526125039.png)

![400](../Fotos/Pasted%20image%2020260526125054.png)

![800](../Fotos/Pasted%20image%2020260526125201.png)

Eval conversion functions
tostring() (conversion de eval que utiliza 2 argumentos, primer argfumento es un valor numero y luego puede ser commas hex o duration (X, Y)) (para las commas el unico requerimiento es que el primer argumento tenga mas de 3 digitos) (hex permite pasar los valores numeros a hexadecimal) (duration es para pasar los valores numeros a formato d ehoras, minutos y segundos)
tonumber()

![800](../Fotos/Pasted%20image%2020260526130642.png)

uso de tostring + commas 

![900](../Fotos/Pasted%20image%2020260526130822.png)

podemos hacer esto c on los otros dos fields mas que hemos creado con stats 

![800](../Fotos/Pasted%20image%2020260526130911.png)

![Pasted image 20260526131012](../Fotos/Pasted%20image%2020260526131012.png)

![500](../Fotos/Pasted%20image%2020260526131524.png)

range = máximo − mínimo (esto sirve para saber cuanto duro algun evento)

![800](../Fotos/Pasted%20image%2020260526132022.png)

![600](../Fotos/Pasted%20image%2020260526134224.png)

![500](../Fotos/Pasted%20image%2020260526134336.png)

![500](../Fotos/Pasted%20image%2020260526134431.png)

Eval text function
upper() (Convierte texto a **MAYÚSCULAS**.)
lower() (Convierte texto a **minúsculas**.)
substr() (Extrae una **subcadena** (parte de un texto).) **Ojo con esta**

![700](../Fotos/Pasted%20image%2020260526135419.png)

![900](../Fotos/Pasted%20image%2020260526140239.png)

![Pasted image 20260526141248](../Fotos/Pasted%20image%2020260526141248.png)

Eval coalesce function
coalesce () ("Usa el primer campo que tenga dato.") (La idea es **unificar / normalizar** dos posibles nombres de campo en **un solo campo común**.)

![500](../Fotos/Pasted%20image%2020260526141432.png)

![700](../Fotos/Pasted%20image%2020260526141453.png)

![500](../Fotos/Pasted%20image%2020260526141628.png)

**Creating knowledge objects**
Field aliases
tags:

![350](../Fotos/Pasted%20image%2020260526171008.png)

calculated fields
Los resultados de los comands eval pueden escribirse en campos nuevos o bien reemplazar los campos existentes 
conversiond e bytes a megabits , veremos como hacer que splunk lo haga automaticamente

![Pasted image 20260526171556](../Fotos/Pasted%20image%2020260526171556.png)

```bash
settings
field
calcualted fields - new field
destination app search
name: bandwidth
eval expression: round(sc_bytes/1024/1024,2)
save
```
![Pasted image 20260526171826](../Fotos/Pasted%20image%2020260526171826.png)

Event Types
first

![500](../Fotos/Pasted%20image%2020260526173737.png)

second

![Pasted image 20260526173752](../Fotos/Pasted%20image%2020260526173752.png)

thrird

![500](../Fotos/Pasted%20image%2020260526173813.png)

four

![500](../Fotos/Pasted%20image%2020260526173725.png)

build event types

![300](../Fotos/Pasted%20image%2020260526173954.png)

![300](../Fotos/Pasted%20image%2020260526174009.png)

![300](../Fotos/Pasted%20image%2020260526174027.png)

![300](../Fotos/Pasted%20image%2020260526174103.png)

![300](../Fotos/Pasted%20image%2020260526174130.png)

![300](../Fotos/Pasted%20image%2020260526174140.png)

![Pasted image 20260526174200](../Fotos/Pasted%20image%2020260526174200.png)

![Pasted image 20260526174209](../Fotos/Pasted%20image%2020260526174209.png)

![Pasted image 20260526174228](../Fotos/Pasted%20image%2020260526174228.png)

![600](../Fotos/Pasted%20image%2020260526174232.png)

Macros
settings / advancesearch

![300](../Fotos/Pasted%20image%2020260526180328.png)

![Pasted image 20260526180351](../Fotos/Pasted%20image%2020260526180351.png)

save

![Pasted image 20260526180458](../Fotos/Pasted%20image%2020260526180458.png)

![500](../Fotos/Pasted%20image%2020260526180520.png)

![800](../Fotos/Pasted%20image%2020260526180526.png)

![Pasted image 20260526180648](../Fotos/Pasted%20image%2020260526180648.png)

adding arguments

![700](../Fotos/Pasted%20image%2020260526180738.png)

![800](../Fotos/Pasted%20image%2020260526180803.png)

workflow actions (create links to interact with external resources or narrow search)
GET POST
settings
fields
workflow action (GET)

![395](../Fotos/Pasted%20image%2020260526181452.png)

![400](../Fotos/Pasted%20image%2020260526181526.png) 

![Pasted image 20260526181550](../Fotos/Pasted%20image%2020260526181550.png)

workflow action (POST)

![700](../Fotos/Pasted%20image%2020260526182731.png)

Search Time Operations Sequence

![200](../Fotos/Pasted%20image%2020260526185529.png)

Creating field extractions
Esto se puede realizar mediante expresiones regulares o delimitadores
el extractor de campos generara expresiones regulares de forma automatica mediante muestras que le proporciones, los delimitadores cuando los eventos continen campos que estan separados por un caracter
Hay 3 formas de acceder al extractor de campos:
FORMA 1
settings
fields
field extractions
open field extractor

![Pasted image 20260526230539](../Fotos/Pasted%20image%2020260526230539.png)

FORMA 2
al final de la columna de fields esta:
extract new fields

![Pasted image 20260526230530](../Fotos/Pasted%20image%2020260526230530.png)

FORMA 3 (La mas facil de extraer un campo)
Desde el menu de event actions

![300](../Fotos/Pasted%20image%2020260526230613.png)


# Pasos de Deteccion y Analisis Splunk BlackBox CDSA - Aplica para Elastic

- ¿Qué usuario se comporta raro?
- ¿Qué host hace cosas que no debería?
- ¿Qué volumen es sospechoso?

![Pasted image 20260318224630](../Fotos/Pasted%20image%2020260318224630.png)

### RECONOCIMIENTO INICIAL

![Pasted image 20260317131325](../Fotos/Pasted%20image%2020260317131325.png)   

![Pasted image 20260317131427](../Fotos/Pasted%20image%2020260317131427.png)   

![Pasted image 20260317131446](../Fotos/Pasted%20image%2020260317131446.png)

### DETECCIÓN DE ANOMALÍAS BÁSICAS

![Pasted image 20260317131510](../Fotos/Pasted%20image%2020260317131510.png)   

![Pasted image 20260317131524](../Fotos/Pasted%20image%2020260317131524.png)   

![Pasted image 20260317131538](../Fotos/Pasted%20image%2020260317131538.png)

*Revisar si hay enumeracion con ps1 programas descargados o powershell systeminfo.exe, etc*

### ACCOUNT ATTACKS

![Pasted image 20260317131652](../Fotos/Pasted%20image%2020260317131652.png)   

![Pasted image 20260317131704](../Fotos/Pasted%20image%2020260317131704.png)

### KERBEROS ATTACKS

![Pasted image 20260317131735](../Fotos/Pasted%20image%2020260317131735.png)   

![Pasted image 20260317131746](../Fotos/Pasted%20image%2020260317131746.png)

### LATERAL MOVEMENT

![Pasted image 20260317131808](../Fotos/Pasted%20image%2020260317131808.png)   

![Pasted image 20260317131858](../Fotos/Pasted%20image%2020260317131858.png)   

![Pasted image 20260317131907](../Fotos/Pasted%20image%2020260317131907.png)

### DOMAIN DOMINATION

![Pasted image 20260317131926](../Fotos/Pasted%20image%2020260317131926.png)   

![Pasted image 20260317131936](../Fotos/Pasted%20image%2020260317131936.png)

### NETWORK / EXFILTRATION

![Pasted image 20260317132005](../Fotos/Pasted%20image%2020260317132005.png)   

![Pasted image 20260317132011](../Fotos/Pasted%20image%2020260317132011.png)   

![Pasted image 20260317132022](../Fotos/Pasted%20image%2020260317132022.png)

### RANSOMWARE 

![Pasted image 20260317132109](../Fotos/Pasted%20image%2020260317132109.png)   

![Pasted image 20260317132115](../Fotos/Pasted%20image%2020260317132115.png)

## Consejos

👉 Explica:
- Qué viste
- Por qué es sospechoso
- Qué ataque representa
- Qué impacto tiene

- **Baseline** → comportamiento normal
- **Anomalía** → desviación
- **Pivot** → cambiar foco (user/IP/host)
- **Correlación** → unir eventos
- **TTPs** → técnicas del atacante
- **Lateral Movement** → moverse entre máquinas
- **Persistence** → mantenerse en el sistema
- **Impact** → daño final

**En el examen asegúrate de haber cubierto:**
✔ Usuarios sospechosos  
✔ IPs sospechosas  
✔ Fallos de login  
✔ Kerberos activity  
✔ Movimiento lateral  
✔ Actividad en dominio  
✔ Tráfico de red  
✔ Posible exfiltración  
✔ Impacto (si existe)

**Correcto:**

![Pasted image 20260317134618](../Fotos/Pasted%20image%2020260317134618.png)

**Incorrecto:**

![Pasted image 20260317134512](../Fotos/Pasted%20image%2020260317134512.png) 

![Pasted image 20260317134506](../Fotos/Pasted%20image%2020260317134506.png) 

![Pasted image 20260317134522](../Fotos/Pasted%20image%2020260317134522.png)

# Resumen Conceptual

ETW → motor interno de logging avanzado de Windows  
SilkETW / SilkService → herramientas para capturar eventos ETW  
Microsoft-Windows-LDAP-Client → provider que registra consultas LDAP  
YARA → reglas para detectar patrones sospechosos  
SOC Analyst → usa todo esto para detectar enumeración y ataques

- `_time` → momento exacto en que ocurrió el evento
- `earliest` → inicio del rango de tiempo que se busca
- `latest` → final del rango de tiempo que se busca
- `EventCode` → identificador del tipo de evento de Windows (ej. login, creación de proceso)
- `source` → tipo de log o origen del registro (Security, Sysmon, etc.)
- `SourceName` → servicio o aplicación de Windows que generó el evento
- `Message` → descripción completa del evento
- `name` → nombre del evento o categoría del evento
- `user` → usuario que ejecutó la acción o proceso
- `Account_Name` → nombre de la cuenta involucrada en el evento
- `Account_Domain` → dominio al que pertenece la cuenta
- `SubjectUserName` → usuario que inicia o provoca el evento
- `TargetUserName` → usuario objetivo del evento (por ejemplo en autenticaciones)
- `src` → dirección IP origen desde donde se origina la actividad
- `dest` → host o sistema destino donde ocurre el evento
- `Source_Network_Address` → IP origen registrada directamente en logs de Windows
- `Target_Server_Name` → servidor al que se intenta acceder o autenticar
- `Workstation_Name` → nombre del equipo desde el que se realiza la acción
- `Image` → ruta completa del ejecutable que se ejecutó
- `ProcessId` → identificador del proceso que se ejecutó
- `ParentProcessId` → identificador del proceso que lanzó el proceso actual
- `ParentImage` → ejecutable que inició el proceso actual
- `CommandLine` → comando exacto que se ejecutó con el proceso
- `LogonType` → tipo de inicio de sesión (local, red, RDP, servicio, etc.)
- `Failure_Reason` → motivo por el cual falló una autenticación
- `AuthenticationPackageName` → método de autenticación utilizado (NTLM, Kerberos, etc.)
- `SearchFilter` → filtro utilizado en consultas LDAP contra Active Directory
- `DistinguishedName` → ruta completa del objeto dentro del directorio LDAP
- `ObjectClass` → tipo de objeto dentro de Active Directory (usuario, grupo, equipo)
- `count` → número total de eventos en una agregación
- `dc(field)` → número de valores únicos de un campo (distinct count)
- `values(field)` → lista de valores únicos que aparecen en un campo
- `min(_time)` → primer evento registrado en el grupo analizado
- `max(_time)` → último evento registrado en el grupo analizado

# Detección de Reconocimiento de Usuarios/Dominio Común   /   Detecting Common User/Domain Recon

### Resumen
Se refiere a **detectar cuando un atacante está recolectando información sobre usuarios y el dominio de Active Directory antes de atacar**

---

Dentro del dominio pueden existir:
- usuario → `juan@corp.local`
- computador → `PC-VENTAS.corp.local`
- servidor → `SRV-DB.corp.local`
Todos **pertenecen al mismo dominio** y se autentican contra el **Domain Controller**.

**Las herramientas/comandos nativos comunes utilizados para el reconocimiento de dominios incluyen:**
- `whoami /all`
- `wmic computersystem get domain`
- `net user /domain`
- `net group "Domain Admins" /domain`
- `arp -a`
- `nltest /domain_trusts`

**BloodHound** es una herramienta de análisis para Active Directory **(AD Mapear)**.
**SharpHound** es el recopilador de datos para BloodHound **(Extraer información sobre usuarios, grupos, permisos, sesiones y equipos.)**.

**Con esta lista de filtros LDAP, la actividad de Bloodhound puede detectarse de forma más eficiente.**
![Table listing recon tools and their LDAP filters. Tools include Metasploit's enum_ad_user_comments, enum_ad_computers, enum_ad_groups, enum_ad_managedby_groups, and PowerView's Get-NetComputer, Get-NetUser, Get-DFSSHareV2, Get-NetOU, Get-DomainSearcher, with corresponding LDAP queries.|550](https://cdn.services-k8s.prod.aws.htb.systems/content/modules/233/image59.png)
#### Detección de reconocimiento apuntando a ejecutables nativos de Windows (Osea .exes)

```bash
index=main source="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" EventID=1 earliest=1690447949 latest=1690450687
| search process_name IN (arp.exe,chcp.com,ipconfig.exe,net.exe,net1.exe,nltest.exe,ping.exe,systeminfo.exe,whoami.exe) OR (process_name IN (cmd.exe,powershell.exe) AND process IN (*arp*,*chcp*,*ipconfig*,*net*,*net1*,*nltest*,*ping*,*systeminfo*,*whoami*))
| stats values(process) as process, min(_time) as _time by parent_process, parent_process_id, dest, user
| where mvcount(process) > 3
```
#### Detección de reconocimiento atacando a Bloodhound

```shell
index=main earliest=1690195896 latest=1690285475 source="WinEventLog:SilkService-Log"
| spath input=Message 
| rename XmlEventData.* as * 
| table _time, ComputerName, ProcessName, ProcessId, DistinguishedName, SearchFilter
| sort 0 _time
| search SearchFilter="*(samAccountType=805306368)*"
| stats min(_time) as _time, max(_time) as maxTime, count, values(SearchFilter) as SearchFilter by ComputerName, ProcessName, ProcessId
| where count > 10
| convert ctime(maxTime)
```

**¿Qué es `samAccountType=805306368`?**
Ese valor en Active Directory corresponde a:
👉 Objetos tipo **usuario**
O sea:
Estás detectando procesos que hacen consultas LDAP buscando usuarios del dominio.

**Herramientas típicas que hacen esto:**
- BloodHound
- SharpHound
- Scripts PowerShell de recon

- BloodHound (a través de SharpHound)
- Scripts PowerShell de enumeración
- Rubeus en algunos contextos
- Cualquier herramienta de AD recon

necesitan listar usuarios del dominio.
Y una forma común de hacerlo es:
(samAccountType=805306368)

**Tambien hay procesos legitimos que lo usan**

![400](../Fotos/Pasted%20image%2020260302134832.png)

**Query utilizada en laboratorio**
Original
```bash
index=main source="WinEventLog:SilkService-Log"
| spath input=Message  
| rename XmlEventData.* as *  
| table _time, ComputerName, ProcessName, ProcessId, DistinguishedName, SearchFilter  
| sort 0 _time  
| search SearchFilter="*(**samAccountType=805306368**)*"
| stats min(_time) as _time, max(_time) as maxTime, count, values(SearchFilter) as SearchFilter by ComputerName, ProcessName, ProcessId  
| where count > 10  
| convert ctime(maxTime)
```
`where count > 10` : mostrar procesos que hicieron la búsqueda más de diez veces
Cambiada para el lab
```bash
index=main source="WinEventLog:SilkService-Log"
| spath input=Message  
| rename XmlEventData.* as *  
| table _time, ComputerName, ProcessName, ProcessId, DistinguishedName, SearchFilter  
| sort 0 _time  
| search SearchFilter="*(**samAccountType=805306368**)*"
| stats min(_time) as _time, max(_time) as maxTime, count, values(SearchFilter) as SearchFilter by ComputerName, ProcessName, ProcessId  
| where count > 1
| convert ctime(maxTime)
```
`where count > 1` : mostrar procesos que hicieron la búsqueda más de una vez
# Detección de la pulverización por contraseña   /   Detecting Password Spraying

### Resumen

**Password Spraying** es un **ataque de autenticación** donde el atacante prueba **una misma contraseña en muchas cuentas diferentes**.

---
Se prueba **una misma contraseña** contra **muchas cuentas distintas**.

Registros de eventos que pueden ayudar en la detección de la aplicación de contraseñas incluyen:
- `4625 - Failed Logon`
- `4768 and ErrorCode 0x6 - Kerberos Invalid Users`
- `4768 and ErrorCode 0x12 - Kerberos Disabled Users`
- `4776 and ErrorCode 0xC0000064 - NTLM Invalid Users`
- `4776 and ErrorCode 0xC000006A - NTLM Wrong Password`
- `4648 - Authenticate Using Explicit Credentials`
- `4771 - Kerberos Pre-Authentication Failed`

**SPL Query empleada en lab:**
```shell
index=main earliest=0 latest=now source="WinEventLog:Security" EventCode=4625
| bin span=15m _time
| stats values(user) as Users, dc(user) as dc_user by src, Source_Network_Address, dest, EventCode, Failure_Reason
```
`earliest=0 latest=now`(en todo momento)
`bin span=15m _time` Agrupa los eventos en bloques de 15 minutos.
`values(user)` : Lista **los usuarios que fallaron login**.
`dc(user)` : Cuenta **usuarios distintos**.
`src` : Aquí **`src` es el nombre del host origen**. (Nota: `src` : origen de la actividad registrada y `host` : el equipo que generó el log)
`Source_Network_Address` : Dirección IP desde la que se hizo el login. Este campo **viene directamente del evento de Windows**. Dirección IP desde la que se hizo el login.
# Detección de ataques similares a los de un respondedor   /   Detecting Responder-like Attacks

### Resumen
```bash
**Responder** es una herramienta usada en pentesting para **capturar hashes NTLM en la red**.

Qué es un NTLM hash  
Es el hash de la contraseña de un usuario de Windows.

Para qué se usa  
Cuando un usuario se autentica, Windows no envía la contraseña,  
envía una prueba basada en ese hash.
```


---

**Responder** es una herramienta ofensiva muy usada en redes Windows.
👉 Responder es un framework que permite hacer:
- LLMNR poisoning
- NBT-NS poisoning
- WPAD poisoning
- Captura de hashes NTLM
**Puede ser cualquier herramienta que:**
- Escuche peticiones broadcast en la red.
- Responda haciéndose pasar por otro equipo.
- Capture hashes NTLM.

![500](../Fotos/Pasted%20image%2020260302160805.png)     

![Pasted image 20260302160922](../Fotos/Pasted%20image%2020260302160922.png)   

![Pasted image 20260302163744](../Fotos/Pasted%20image%2020260302163744.png)

```shell
index=main earliest=1690290078 latest=1690291207 SourceName=LLMNRDetection
| table _time, ComputerName, SourceName, Message
```
[El ID de evento Sysmon 22](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=90022) también puede utilizarse para rastrear consultas DNS asociadas a compartidos de archivos inexistentes o mal tipados.

```shell
index=main earliest=1690290078 latest=1690291207 EventCode=22 
| table _time, Computer, user, Image, QueryName, QueryResults
```
Además, recuerda que [el Evento 4648](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4648) puede usarse para detectar inicios de sesión explícitos en compartidos de archivos fraudulentos que los atacantes podrían usar para recopilar credenciales legítimas de usuario.

```shell
index=main earliest=1690290814 latest=1690291207 EventCode IN (4648) 
| table _time, EventCode, source, name, user, Target_Server_Name, Message
| sort 0 _time
```

**Query utilizada para el laboratorio:**
```bash
index=main EventCode=22
| search QueryResults="*10.10.0.221*" | table _time, Computer, user, Image, QueryName, QueryResults
```
- Estás viendo eventos de **DNS Query** (Sysmon 22) que resolvieron hacia **10.10.0.221**.
Se usa porque **Responder** funciona **provocando resoluciones de nombres falsas** para capturar credenciales.
Tu búsqueda usa eventos **DNS Query** de **Sysmon** específicamente **Sysmon Event ID 22**.

*Este tipo de búsqueda se usa para detectar:*
- malware resolviendo C2
- acceso a file shares sospechosos
- movimientos laterales SMB
- resolución DNS previa a conexión

![Pasted image 20260314135228](../Fotos/Pasted%20image%2020260314135228.png) 

![Pasted image 20260314133904](../Fotos/Pasted%20image%2020260314133904.png)   

![Pasted image 20260314133919](../Fotos/Pasted%20image%2020260314133919.png)

![Pasted image 20260314135533](../Fotos/Pasted%20image%2020260314135533.png)  

![477](../Fotos/Pasted%20image%2020260314135628.png)  

![Pasted image 20260314135339](../Fotos/Pasted%20image%2020260314135339.png)

# Detección de kerberoasting/AS-REProasting   /   Detecting Kerberoasting/AS-REProasting

### Resumen

#### Concepto de ambos ataques
```bash
Kerberoasting  
- Requiere cuenta en el dominio  
- Ataca cuentas con SPN  
- Solicita TGS  
- Extrae hash del ticket del servicio  
  
AS-REP Roasting  
- No requiere autenticación  
- Ataca cuentas sin pre-auth  
- Solicita AS-REP  
- Extrae hash del usuario  

Ambos  
- Ataques contra Kerberos  
- Obtienen hashes para crackear offline
```

```bash
Kerberoasting vs AS-REP Roasting  
  
KERBEROASTING  
Idea  
El atacante solicita tickets de servicio (TGS) de cuentas con SPN para crackear su contraseña offline.  
  
Requisitos  
- Tener una cuenta válida en el dominio.  
- La cuenta objetivo debe tener un SPN (service account).  
  
Flujo  
1. Usuario autenticado solicita TGS al KDC.  
2. El KDC responde con un TGS.  
3. Ese TGS está cifrado con la contraseña de la cuenta de servicio.  
4. El atacante lo guarda.  
5. Lo crackea offline.  
  
Objetivo  
Contraseñas de cuentas de servicio.  
  
Evento típico  
TGS requests (Kerberos Event ID 4769).  
  
---  
  
AS-REP ROASTING  
Idea  
El atacante obtiene directamente un AS-REP de una cuenta que tiene deshabilitada la pre-authentication.  
  
Requisitos  
- La cuenta debe tener **"Do not require Kerberos preauthentication"** habilitado.  
  
Flujo  
1. El atacante envía AS-REQ con un username.  
2. La cuenta NO requiere pre-auth.  
3. El KDC responde con AS-REP.  
4. Ese AS-REP contiene un hash cifrado.  
5. El atacante lo crackea offline.  
  
Objetivo  
Contraseñas de cuentas mal configuradas.  
  
Evento típico  
AS-REP responses (Kerberos Event ID 4768).  
  
---  
  
DIFERENCIA CLAVE  
  
Kerberoasting  
- Ataca **cuentas con SPN**  
- Requiere **usuario autenticado**  
- Extrae **TGS**  
  
AS-REP Roasting  
- Ataca **cuentas sin pre-authentication**  
- **No necesita autenticación**  
- Extrae **AS-REP**  
  
---  
  
POR QUÉ ESTÁN EN LA MISMA CATEGORÍA  
  
Ambos son ataques de:  
  
Credential Access + Offline Password Cracking  
  
porque el atacante:  
1. obtiene un ticket Kerberos  
2. extrae un hash  
3. lo crackea offline
```


**Se detecta por:**
`request_type=AS`  
`sin pre-auth`  
`errores Kerberos específicos`

- **AS-REQ (Authentication Service Request)**  
    → Petición que hace un cliente al **KDC** para autenticarse.

- **AS-REP (Authentication Service Reply)**  
    → Respuesta del **KDC** con el **TGT (Ticket Granting Ticket)**

---

`Kerberoasting` es una técnica dirigida a cuentas de servicio en entornos Active Directory para extraer y descifrar sus hashes de contraseñas.
El ataque explota la forma en que los tickets de servicio de Kerberos están cifrados y el uso de contraseñas débiles o fácilmente descifrables para las cuentas de servicio

*Durante el proceso de autenticación Kerberos, se generan varios eventos relacionados con la seguridad en el Registro de Eventos de Windows cuando un usuario se conecta a un servidor MSSQL:*
- `Event ID 4768 (Kerberos TGT Request)`: Ocurre cuando la estación de trabajo cliente solicita un TGT al KDC, generando este evento en el registro de seguridad del controlador de dominio.
- `Event ID 4769 (Kerberos Service Ticket Request)`: Generado después de que el cliente recibe el TGT y solicita un TGS para el SPN del servidor MSSQL.
- `Event ID 4624 (Logon)`: Iniciado sesión en el registro de seguridad del servidor MSSQL, indicando un inicio de sesión exitoso una vez que el cliente inicia una conexión al servidor MSSQL y inicia sesión usando la cuenta de servicio con el SPN para establecer la conexión.

**Solicitudes TGS benignas**
```shell
index=main earliest=1690388417 latest=1690388630 EventCode=4648 OR (EventCode=4769 AND service_name=iis_svc) 
| dedup RecordNumber 
| rex field=user "(?<username>[^@]+)"
| table _time, ComputerName, EventCode, name, username, Account_Name, Account_Domain, src_ip, service_name, Ticket_Options, Ticket_Encryption_Type, Target_Server_Name, Additional_Information
```

- `| dedup RecordNumber`: Esto elimina eventos duplicados basados en el campo.`RecordNumber`
- `| rex field=user "(?<username>[^@]+)"`: Esto extrae la porción del campo usando una expresión regular y la almacena en un nuevo campo llamado .`username``user``username`

**Deteccion de kerboroasting - Consulta SPN**
```shell
index=main earliest=1690448444 latest=1690454437 source="WinEventLog:SilkService-Log" 
| spath input=Message 
| rename XmlEventData.* as * 
| table _time, ComputerName, ProcessName, DistinguishedName, SearchFilter 
| search SearchFilter="*(&(samAccountType=805306368)(servicePrincipalName=*)*"
```

**Deteccion de kerboroasting - Solicitudes TGS**
```shell
index=main earliest=1690450374 latest=1690450483 EventCode=4648 OR (EventCode=4769 AND service_name=iis_svc)
| dedup RecordNumber
| rex field=user "(?<username>[^@]+)"
| bin span=2m _time 
| search username!=*$ 
| stats values(EventCode) as Events, values(service_name) as service_name, values(Additional_Information) as Additional_Information, values(Target_Server_Name) as Target_Server_Name by _time, username
| where !match(Events,"4648")
```
- `| dedup RecordNumber`: Elimina eventos duplicados basados en el campo.`RecordNumber`
`| rex field=user "(?<username>[^@]+)"`: Extrae la porción del cuerpo usando una expresión regular y la almacena en un nuevo cuerpo llamado .`username`
`| search username!=*$`: 

![300](../Fotos/Pasted%20image%2020260302183125.png)   

![Pasted image 20260314145508](../Fotos/Pasted%20image%2020260314145508.png)   

![Pasted image 20260314145523](../Fotos/Pasted%20image%2020260314145523.png)

**Detección de kerberoasting mediante transacciones - Solicitudes TGS**
```shell
index=main earliest=1690450374 latest=1690450483 EventCode=4648 OR (EventCode=4769 AND service_name=iis_svc)
| dedup RecordNumber
| rex field=user "(?<username>[^@]+)"
| search username!=*$ 
| transaction username keepevicted=true maxspan=5s endswith=(EventCode=4648) startswith=(EventCode=4769) 
| where closed_txn=0 AND EventCode = 4769
| table _time, EventCode, service_name, username
```
**Desglose**
🔹 4769 hacia servicio `iis_svc`
Ticket de servicio Kerberos solicitado (TGS request).
 🔹 4648
Logon usando credenciales explícitas (ej: `runas`, scripts, abuso lateral).
- `rex field=user "(?<username>[^@]+)"`
Ejemplo"
user: admin@corp.local  
username: admin
- `search username!=*$`
Elimina cuentas de máquina.
Solo deja usuarios humanos.
- `| transaction username keepevicted=true maxspan=5s endswith=(EventCode=4648) startswith=(EventCode=4769)`: Agrupa los eventos según el campo. La opción incluye eventos que no cumplen los criterios de la transacción. La opción establece la duración máxima de una transacción en 5 segundos. Las opciones y especifican que las transacciones deben comenzar con un evento con y terminar con un evento con .`transactions``username``keepevicted=true``maxspan=5s``endswith=(EventCode=4648)``startswith=(EventCode=4769)``EventCode 4769``EventCode 4648`

![350](../Fotos/Pasted%20image%2020260302190439.png)

- `| where closed_txn=0 AND EventCode = 4769`: Filtra los resultados para incluir solo las transacciones que no están cerradas () y que tienen un de .`closed_txn=0``EventCode``4769`

*Se intenta detectar:Abuso de ticket seguido de autenticación explícita*
#### AS-REPRoasting

De forma similar al Kerberoasting, la fase inicial del AS-REPRoasting consiste en identificar cuentas de usuario con delegación no restringida activada o cuentas sin preautenticación, lo cual puede detectarse mediante la monitorización LDAP.
La autenticación Kerberos contiene un atributo en la parte de información adicional del evento que indica si la pre-autenticación está habilitada para una cuenta.`Event ID 4768 (TGT Request)``PreAuthType`
```shell
index=main earliest=1690392745 latest=1690393283 source="WinEventLog:SilkService-Log" 
| spath input=Message 
| rename XmlEventData.* as * 
| table _time, ComputerName, ProcessName, DistinguishedName, SearchFilter 
| search SearchFilter="*(samAccountType=805306368)(userAccountControl:1.2.840.113556.1.4.803:=4194304)*"
```
#### Detectando AS-REPrOasting - Solicitudes TGT para cuentas con pre-autenticación desactivada

```shell
index=main earliest=1690392745 latest=1690393283 source="WinEventLog:Security" EventCode=4768 Pre_Authentication_Type=0
| rex field=src_ip "(\:\:ffff\:)?(?<src_ip>[0-9\.]+)"
| table _time, src_ip, user, Pre_Authentication_Type, Ticket_Options, Ticket_Encryption_Type
```

*Detecting Kerberoasting - SPN Consultying*
**Query utilizada en laboratorio:**
```bash
index=main EventCode=4648 OR (EventCode=4769 AND service_name=iis_svc)
| rex field=user "(?<username>[^@]+)"
| search username!=*$
| stats values(EventCode) as Events, values(service_name) as service_name, values(Additional_Information) as Additional_Information, values(Target_Server_Name) as Target_Server_Name by _time, username
| where !match(Events,"4648")
```

# Detección de Pass-the-Hash   /   Detecting Pass-the-Hash

### Resumen

**Pass-the-Hash (PtH)**  
Es una técnica donde el atacante **usa directamente el hash NTLM de una contraseña** para autenticarse en lugar de conocer la contraseña real.

**Cómo funciona el ataque**
```bash
Pass-the-Hash (PtH)  
  
Idea:  
Usar el hash NTLM en lugar de la contraseña para autenticarse.  
  
Cómo ocurre:  
1. Atacante roba hash NTLM  
2. Usa el hash para autenticarse  
3. Accede a otros sistemas  
  
Indicadores en logs:  
EventID 4624  
LogonType 9  
LogonProcess seclogo
```
- Permite **movimiento lateral** dentro del dominio.
- Un solo hash puede dar acceso a **muchas máquinas**.

- **Pass-the-Hash** → usa **NTLM directamente para autenticarse**
- **Pass-the-Ticket** → usa **TGT/TGS ya robado**
- **Overpass-the-Hash** → usa **hash NTLM para generar un TGT nuevo**
---
```shell
index=main earliest=1690450708 latest=1690451116 source="WinEventLog:Security" EventCode=4624 Logon_Type=9 Logon_Process=seclogo
| table _time, ComputerName, EventCode, user, Network_Account_Domain, Network_Account_Name, Logon_Type, Logon_Process
```

```shell
index=main earliest=1690450689 latest=1690451116 (source="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=10 TargetImage="C:\\Windows\\system32\\lsass.exe" SourceImage!="C:\\ProgramData\\Microsoft\\Windows Defender\\platform\\*\\MsMpEng.exe") OR (source="WinEventLog:Security" EventCode=4624 Logon_Type=9 Logon_Process=seclogo)
| sort _time, RecordNumber
| transaction host maxspan=1m endswith=(EventCode=4624) startswith=(EventCode=10)
| stats count by _time, Computer, SourceImage, SourceProcessId, Network_Account_Domain, Network_Account_Name, Logon_Type, Logon_Process
| fields - count
```
- `RecordNumber` = **identificador único del evento dentro del índice**
- Se usa para **ordenar eventos con la misma hora** y asegurar **consistencia en transacciones o análisis cronológico**
`MsMpEng.exe` : Esto excluye Windows Defender porque Defender accede a LSASS de forma legitima

**Query utilizada en laboratorio:**
```bash
index=main source="WinEventLog:Security" EventCode=4624 Logon_Type=9 Logon_Process=seclogo
| table _time, ComputerName, EventCode, user, Network_Account_Domain, Network_Account_Name, Logon_Type, Logon_Process
```
`user` : cuenta local que ejecuta el proceso de login
`Netowrk_Account_Domain` : dominio de la cuenta usada para autenticarse
`Network_Account_Name` : usuario cuyas credenciales se están utilizando
`Logon_Type` : tipo de inicio de sesión. 9 = NewCredentials (Se usa normalmente con runas /netonly)
`Logon_Process` : proceso de Windows que gestionó el login

![500](../Fotos/Pasted%20image%2020260303125437.png)  

![500](../Fotos/Pasted%20image%2020260303125647.png)  

![300](../Fotos/Pasted%20image%2020260303125449.png)

Ejemplo de significado: El equipo ORANGE creó un login tipo NewCredentials usando las credenciales de RAUL_LYNN mediante el proceso runas

![Pasted image 20260314154920](../Fotos/Pasted%20image%2020260314154920.png)

# Detección de Pass the Ticket   /   Detecting Pass-the-Ticket

### Resumen

```bash
Pass-the-Ticket (PtT)  
  
Idea:  
Usar un ticket Kerberos robado para autenticarse.  
  
Cómo ocurre:  
1. Atacante roba TGT o TGS  
2. Inserta el ticket en su sesión  
3. Accede a servicios como el usuario  
  
Indicadores:  
EventID 4769  
Uso anómalo de tickets Kerberos
```

- **Pass-the-Hash** → usa **NTLM directamente para autenticarse**
- **Pass-the-Ticket** → usa **TGT/TGS ya robado**
- **Overpass-the-Hash** → usa **hash NTLM para generar un TGT nuevo**
---
**Query utilizada en laboratorio:**
```bash
index=main earliest=1690392405 latest=1690451745 source="WinEventLog:Security" user!=*$ EventCode IN (4768,4769,4770) 
| rex field=user "(?<username>[^@]+)"
| rex field=src_ip "(\:\:ffff\:)?(?<src_ip_4>[0-9\.]+)"
| transaction username, src_ip_4 maxspan=10h keepevicted=true startswith=(EventCode=4768)
| where closed_txn=0
| search NOT user="*$@*"
| table _time, ComputerName, username, src_ip_4, service_name, category
```
`startswith=4768` : La transacción debe comenzar con solicitud de TGT
`where closed_txn=0` : mostrar transacciones que NO se cerraron correctamente. 4768 ocurrió pero no hubo eventos posteriores.
`search NOT user="*$@*"` :  Elimina formatos extraños de cuentas

![Pasted image 20260314162743](../Fotos/Pasted%20image%2020260314162743.png)  

![Pasted image 20260314161457](../Fotos/Pasted%20image%2020260314161457.png)   

![Pasted image 20260314161514](../Fotos/Pasted%20image%2020260314161514.png)

![Pasted image 20260314162710](../Fotos/Pasted%20image%2020260314162710.png)

# Detección de sobrepaso por el hash   /   Detecting Overpass-the-Hash

### Resumen

```bash
Overpass-the-Hash  
  
Idea:  
Usar hash NTLM para obtener ticket Kerberos.  
  
Cómo ocurre:  
1. Atacante roba hash NTLM  
2. Usa herramienta (Mimikatz/Rubeus)  
3. Solicita TGT al KDC  
4. Usa Kerberos normalmente  
  
Indicadores:  
EventID 4768  
TGT generado desde host anómalo  
NTLM → Kerberos
```

- **Pass-the-Hash** → usa **NTLM directamente para autenticarse**
- **Pass-the-Ticket** → usa **TGT/TGS ya robado**
- **Overpass-the-Hash** → usa **hash NTLM para generar un TGT nuevo**
---

**Identificar Pass the Hash utilizando Splunk**
```shell
index=main earliest=1690450708 latest=1690451116 source="WinEventLog:Security" EventCode=4624 Logon_Type=9 Logon_Process=seclogo
| table _time, ComputerName, EventCode, user, Network_Account_Domain, Network_Account_Name, Logon_Type, Logon_Process
```

![380](../Fotos/Pasted%20image%2020260309224723.png)

Como ya se ha mencionado, podemos mejorar la búsqueda anterior añadiendo acceso a memoria LSASS a la mezcla de la siguiente manera.

**Las siguientes 2 querys sirven para detectar overpas the hash ya que correlaciona LSASS access Sysmon10 + Logon type 9 y la ultima utilizadae en el laboratorio tambien porque mira actividad kerberos anomala en el puerto 88.**

**Detección de Pass-the-Hash**
```shell
index=main earliest=1690450689 latest=1690451116 (source="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=10 TargetImage="C:\\Windows\\system32\\lsass.exe" SourceImage!="C:\\ProgramData\\Microsoft\\Windows Defender\\platform\\*\\MsMpEng.exe") OR (source="WinEventLog:Security" EventCode=4624 Logon_Type=9 Logon_Process=seclogo)
| sort _time, RecordNumber
| transaction host maxspan=1m endswith=(EventCode=4624) startswith=(EventCode=10)
| stats count by _time, Computer, SourceImage, SourceProcessId, Network_Account_Domain, Network_Account_Name, Logon_Type, Logon_Process
| fields - count
```
- `RecordNumber` = **identificador único del evento dentro del índice**
- Se usa para **ordenar eventos con la misma hora** y asegurar **consistencia en transacciones o análisis cronológico**
`MsMpEng.exe` : Esto excluye Windows Defender porque Defender accede a LSASS de forma legitima

**Query utilizada en laboratorio**
*Esta query detecta procesos distintos de lsass.exe conectándose al puerto Kerberos (88)*

Eso suele indicar:
- herramientas de ataque Kerberos
- extracción de tickets
- abuso de autenticación

```bash
index=main earliest=0 latest=now source="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" (EventCode=3 dest_port=88 Image!=*lsass.exe) OR EventCode=1
| eventstats values(process) as process by process_id
| where EventCode=3
| stats count by _time, Computer, dest_ip, dest_port, Image, process
| fields - count
```
Pero excluye:
`Image!=*lsass.exe`
Porque normalmente **LSASS es quien habla con Kerberos**.

![200](../Fotos/Pasted%20image%2020260314193301.png)   

![Pasted image 20260314193329](../Fotos/Pasted%20image%2020260314193329.png)

# Detectando Billetes Dorados/Billetes de Plata   /   Detecting Golden Tickets/Silver Tickets

### Resumen

```bash
Golden Ticket
1. El atacante roba el **hash de la cuenta `krbtgt`** (cuenta especial que **firma todos los TGT** en Kerberos).
2. Con ese hash **puede crear TGT falsos** (tickets de autenticación).
3. Como el TGT parece válido, el **KDC lo acepta**.
4. El atacante puede **pedir TGS para cualquier servicio del dominio**.
5. Resultado: **acceso casi total al dominio**.

Silver Ticket
1. El atacante roba el **hash de una cuenta de servicio con SPN** (por ejemplo un servicio SQL o IIS).
2. Con ese hash **crea directamente un TGS falso**.
3. Ese TGS se presenta **directamente al servicio**, sin pasar por el KDC.
4. Resultado: **acceso solo a ese servicio específico**.

Idea clave para recordar (muy importante para CDSA)
- krbtgt → firma los TGT → Golden Ticket
- SPN de servicio → firma TGS → Silver Ticket

Forma rápida de pensarlo

- Golden Ticket = control del sistema de tickets (krbtgt)
    
- Silver Ticket = acceso a un servicio específico.
```

---
#### Detectar billetes dorados con splunk (otro enfoque más de billetes por superar)   /   Detectando Billetes Dorados/Billetes de Plata
**Usar primero**
```shell
index=main earliest=1690451977 latest=1690452262 source="WinEventLog:Security" user!=*$ EventCode IN (4768,4769,4770) 
| rex field=user "(?<username>[^@]+)"
| rex field=src_ip "(\:\:ffff\:)?(?<src_ip_4>[0-9\.]+)"
| transaction username, src_ip_4 maxspan=10h keepevicted=true startswith=(EventCode=4768)
| where closed_txn=0
| search NOT user="*$@*"
| table _time, ComputerName, username, src_ip_4, service_name, category
```
`where closed_txn=0` : Busca transacciones incompletas **Ejemplo sospechoso:** 4769 sin 4768

```bash
Flujo normal:

4768 → TGT solicitado  
↓  
4769 → TGS solicitado  
↓  
acceso al servicio
```

`keepevicted=true` : no eliminar transacciones incompletas. Porque normalmente `transaction` elimina las que no se cerraron correctamente. **Se quieren mantener** porque pueden indicar **actividad sospechosa**.
`| where closed_txn=0` :  `closed_txn` indica si la transacción se cerró correctamente. 1 transacción completa 0 transacción incompleta

![250](../Fotos/Pasted%20image%2020260315105146.png)

#### Detección de tickets plata con splunk mediante correlación de usuario   /   Detectando Billetes Dorados/Billetes de Plata
**detectar usuarios nuevos sospechosos Pero no detecta Golden Ticket directamente.**
```shell
index=main latest=1690448444 EventCode=4720
| stats min(_time) as _time, values(EventCode) as EventCode by user
| outputlookup users.csv
```

````shell
index=main latest=1690545656 EventCode=4624
| stats min(_time) as firstTime, values(ComputerName) as ComputerName, values(EventCode) as EventCode by user
| eval last24h = 1690451977
| where firstTime > last24h
```| eval last24h=relative_time(now(),"-24h@h")```
| convert ctime(firstTime)
| convert ctime(last24h)
| lookup users.csv user as user OUTPUT EventCode as Events
| where isnull(Events)
````

#### Detectar tickets plateados con Splunk al seleccionar privilegios especiales asignados a un nuevo usuario
**Special privileges assigned Sirve para ver escalamiento de privilegios, no tickets Kerberos.**
````shell
index=main latest=1690545656 EventCode=4672
| stats min(_time) as firstTime, values(ComputerName) as ComputerName by Account_Name
| eval last24h = 1690451977 
```| eval last24h=relative_time(now(),"-24h@h") ```
| where firstTime > last24h 
| table firstTime, ComputerName, Account_Name 
| convert ctime(firstTime)
````

**Query utilizada en el laboratorio**
**Logon con credenciales explícitas**
```bash
index=main Account_Name=barbi EventCode=4648
```

# Detección de ataques de delegación no restringida/delegación restringida   /   Detecting Unconstrained Delegation/Constrained Delegation Attacks

### Resumen

```bash
# Delegation (Delegación en Active Directory)  
  
Delegation = una configuración de Active Directory que permite que  
un servidor actúe en nombre de un usuario frente a otro servicio.  
  
Escenario normal:  
  
User → Web Server → SQL Server  
  
El Web Server pide acceso al SQL Server **como si fuera el usuario**.  
  
Esto se usa para aplicaciones que necesitan acceder a otros servicios  
sin pedirle al usuario autenticarse otra vez.  
  
------------------------------------------------  
  
# Unconstrained Delegation  
  
Configuración peligrosa.  
  
Cuando un usuario se autentica en ese servidor:  
  
1. El DC envía el TGT del usuario al servidor  
2. El servidor guarda ese TGT en memoria  
3. El servidor puede usar ese TGT para autenticarse en cualquier servicio  
  
Problema:  
  
Si el atacante compromete ese servidor → puede robar el TGT.  
  
Resultado:  
puede autenticarse como ese usuario en **cualquier servicio del dominio**.  
  
------------------------------------------------  
  
# Constrained Delegation  
  
Configuración más segura.  
  
El servidor **solo puede actuar como el usuario hacia servicios específicos**.  
  
Ejemplo:  
  
WebServer puede delegar solo hacia:  
  
- SQLServer  
- FileServer  
  
No puede pedir tickets para otros servicios.  
  
------------------------------------------------  
  
# Diferencia clave  
  
Unconstrained Delegation  
→ el servidor puede usar el usuario para **cualquier servicio**  
  
Constrained Delegation  
→ el servidor solo puede usar el usuario para **servicios específicos**  
  
------------------------------------------------  
  
# Idea para recordar (CDSA)  
  
Delegation = servicio usa la identidad del usuario.  
  
Unconstrained → muy peligroso  
Constrained → limitado a servicios definidos
```

![250](../Fotos/Pasted%20image%2020260310143730.png)   

![Pasted image 20260315112820](../Fotos/Pasted%20image%2020260315112820.png)

#### Detección de ataques de delegación sin restricciones
*ESTE COMANDO BUSCA COMANDOS O EJECUCION DE RECONOCIMIENTO DE DELEGACION NO RESTRINGIDA QUE HACE EL ATACANTE*
```shell
index=main earliest=1690544538 latest=1690544540 source="WinEventLog:Microsoft-Windows-PowerShell/Operational" EventCode=4104 Message="*TrustedForDelegation*" OR Message="*userAccountControl:1.2.840.113556.1.4.803:=524288*" 
| table _time, ComputerName, EventCode, Message
```
`TrustedForDelegation` :  Este atributo existe en **Active Directory**. `TRUSTED_FOR_DELEGATION` **Unconstrained Delegation**
`Message="*userAccountControl:1.2.840.113556.1.4.803:=524288*"` :  Esto es un **filtro LDAP avanzado**. 524288 = `TRUSTED_FOR_DELEGATION` que está dentro del atributo `userAccountControl`(Este es el flag). Esto también busca **delegación no restringida**.
`4104` : powershell script block logging. Esto es muy importante porque permite ver **comandos maliciosos completos**.

![Pasted image 20260315112445](../Fotos/Pasted%20image%2020260315112445.png)

#### Detección de ataques de delegación restringida - Aprovechando los registros de PowerShell
```shell
index=main earliest=1690544553 latest=1690562556 source="WinEventLog:Microsoft-Windows-PowerShell/Operational" EventCode=4104 Message="*msDS-AllowedToDelegateTo*" 
| table _time, ComputerName, EventCode, Message
```
`4104` : Se refiere a la **ejecución de scripts o comandos de PowerShell**. Si quieres monitorear o investigar posibles ataques internos, **4104 es el evento clave** porque revela lo que PowerShell hizo en la máquina.
Busca dentro del campo `Message` los eventos que contengan la cadena `msDS-AllowedToDelegateTo` es un **atributo de Active Directory** que indica a qué servicios puede delegar una cuenta o computadora sus credenciales. Es un indicador de **posibles vectores de ataque internos**, especialmente cuando la delegación no está correctamente restringida.

![600](../Fotos/Pasted%20image%2020260310161319.png)

#### Detección de ataques de delegación restringida - Aprovechamiento de registros de Sysmon
```shell
index=main earliest=1690562367 latest=1690562556 source="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" 
| eventstats values(process) as process by process_id
| where EventCode=3 AND dest_port=88
| table _time, Computer, dest_ip, dest_port, Image, process
```
`eventstats` calcula estadísticas y las agrega **a cada evento**, sin reducir la cantidad de eventos.
`values(process) as process by process_id` → agrupa los eventos por `process_id` (ID del proceso) y guarda los nombres de los procesos asociados a ese ID en un campo llamado `process`.

**Resumen:** Permite **ver qué procesos están relacionados con un mismo ID**, útil cuando quieres conectar eventos de red con el proceso que los generó.

**Explicacion de `values(process) as process by process_id`** : “Para cada `process_id`, toma el nombre del proceso y cópialo a todos los eventos que tengan ese mismo `process_id`.”

![Pasted image 20260315114637](../Fotos/Pasted%20image%2020260315114637.png)  

![Pasted image 20260315114645](../Fotos/Pasted%20image%2020260315114645.png)   

![Pasted image 20260315114705](../Fotos/Pasted%20image%2020260315114705.png)

**Query utilizada en laboratorio**
**→ detecta reconocimiento de Unconstrained Delegation en Active Directory mediante PowerShell.**
```bash
index=main earliest=0 latest=now source="WinEventLog:Microsoft-Windows-PowerShell/Operational" EventCode=4104 Message="*TrustedForDelegation*" OR Message="*userAccountControl:1.2.840.113556.1.4.803:=524288*" 
| table _time, ComputerName, EventCode, Message
```

# Detección de DCSync/DCShadow   /   Detecting DCSync/DCShadow

### Resumen

```bash
# DCSync  
  
Idea:  
  
El atacante **simula ser un Domain Controller** y le pide al DC real  
los hashes de las cuentas del dominio.  
  
Proceso:  
  
1. El atacante obtiene privilegios altos (Domain Admin o replicación).  
2. Usa herramientas como mimikatz.  
3. Envía una petición de replicación al Domain Controller.  
4. El DC cree que es otro DC legítimo.  
5. Envía hashes de usuarios (incluido Administrator y krbtgt).  
  
Resultado:  
El atacante obtiene hashes del dominio completo.  
  
Concepto clave:  
El ataque usa **replicación de Active Directory**.  
  
------------------------------------------------  
  
# DCShadow  
  
Idea:  
  
El atacante **registra un Domain Controller falso** en el dominio  
para modificar objetos de Active Directory.  
  
Proceso:  
  
1. El atacante crea un DC falso.  
2. Inserta cambios maliciosos en AD.  
3. Replica esos cambios al DC real.  
  
Ejemplos de cambios:  
  
- añadir privilegios a usuarios  
- modificar permisos  
- agregar cuentas admin  
  
Resultado:  
El atacante modifica Active Directory sin generar logs normales.
```

I**dentificar DCSync usando Splunk.**
```shell
index=main earliest=1690544278 latest=1690544280 EventCode=4662 Message="*Replicating Directory Changes*"
| rex field=Message "(?P<property>Replicating Directory Changes.*)"
| table _time, user, object_file_name, Object_Server, property
```
`4662` : El evento indica que **un usuario o proceso accedió o modificó un objeto de Active Directory**.

![Pasted image 20260315130758](../Fotos/Pasted%20image%2020260315130758.png)   

![Pasted image 20260315130814](../Fotos/Pasted%20image%2020260315130814.png)

 **Identificar DCShadow, usando Splunk.**
 *Buscar modificación de cuentas de equipo*
```shell
index=main earliest=1690623888 latest=1690623890 EventCode=4742
| rex field=Message "(?P<gcspn>XX\/[a-zA-Z0-9\.\-\/]+)" 
| table _time, ComputerName, Security_ID, Account_Name, user, gcspn 
| search gcspn=*
```
Reemplazar XX
`EventCode=4742` :**se modificó una cuenta de computadora** en Active Directory. Los **Domain Controllers son cuentas de computadora**, por lo que este evento es clave.
`rex field=Message "(?P<gcspn>XX\/[a-zA-Z0-9\.\-\/]+)"` : Extrae un **SPN (Service Principal Name)** del mensaje. Ejemplo: GC/server.domain.local (**SPN Identificador de servicio usado por **Kerberos**.)
`search gcspn=*` : > mostrar **solo eventos donde sí se encontró ese SPN**.

![430](../Fotos/Pasted%20image%2020260315203144.png)    

![Pasted image 20260315205026](../Fotos/Pasted%20image%2020260315205026.png)

**Query utilizada en laboratorio (DCShadow)** 
 *Buscar modificación de cuentas de equipo*
 Para ver:
- quién hizo el cambio
- qué máquina fue modificada
- qué SPN apareció
```bash
index=main EventCode=4742 
| rex field=Message "(?P<gcspn>GC\/[a-zA-Z0-9\.\-\/]+)" 
| table _time, ComputerName, Security_ID, Account_Name, user, gcspn 
| search gcspn=*
```

*La query busca cambios en cuentas de computadora que agregan SPN de Domain Controller, lo que puede indicar que un atacante está intentando registrar un DC falso para ejecutar DCShadow*

**Clave:**

![Pasted image 20260315222041](../Fotos/Pasted%20image%2020260315222041.png)

---
#### Fundamental visto en CDSA Deteccion DCSYNC
- **Account:** `management` (HTBDEFENSE\management)
- **SID:** `S-1-5-21-1294279326-831216692-757370779-1107`
- **Logon ID (DCSync session):** `0x3E5FA41`
- **Evidence:** Security EventCode=4662 on WIN-HHSGPJM30S2 at `2023-09-11T18:50:56` — `management` account performed Directory Service access with all three DCSync GUIDs:
    
    - `{1131f6aa-9c07-11d1-f79f-00c04fc2dcd2}` — DS-Replication-Get-Changes
    - `{1131f6ad-9c07-11d1-f79f-00c04fc2dcd2}` — DS-Replication-Get-Changes-All
    - `{89e95b76-444d-4c62-991a-0facbeda640c}` — DS-Replication-Get-Changes-In-Filtered-Set
    
- **Source IP:** `10.0.0.216` (WIN-HIDUIPTH344)
- **SPL Query:**
```bash
index=windows ComputerName="WIN-HHSGPJM30S2*" EventCode=4662 | rex field=_raw "Account Name:\s+(?<AccountName>[^\n\r]+)" | rex field=_raw "Account Domain:\s+(?<AccountDomain>[^\n\r]+)" | rex field=_raw "Logon ID:\s+(?<LogonID>[^\n\r]+)" | rex field=_raw "Object Type:\s+(?<ObjectType>[^\n\r]+)" | rex field=_raw "Properties:\s+(?<Properties>[^\n\r]+)" | rex field=_raw "Access Mask:\s+(?<AccessMask>[^\n\r]+)" | table _time ComputerName AccountName AccountDomain LogonID ObjectType Properties AccessMask | sort _time
```
# Detección de ataques de fuerza bruta RDP   /   Detecting RDP Brute Force Attacks

### Resumen

```bash
# RDP Brute Force  
  
Idea:  
  
El atacante intenta **muchas contraseñas por RDP**  
hasta encontrar la correcta.  
  
RDP = Remote Desktop Protocol  
Puerto común: 3389  
  
------------------------------------------------  
  
Proceso del ataque:  
  
1. El atacante encuentra un servidor con RDP abierto.  
2. Intenta iniciar sesión muchas veces.  
3. Cada intento usa diferentes contraseñas.  
4. Si una funciona → obtiene acceso remoto al sistema.  
  
------------------------------------------------  
  
Señales de detección (logs):  
  
- Muchos intentos
  
Un ataque de **RDP brute force** puede ocurrir contra:
- un **servidor**
- una **workstation**
- una **máquina de usuario**
- una **máquina expuesta a internet**
```

---
```shell
index="rdp_bruteforce" sourcetype="bro:rdp:json"
| bin _time span=5m
| stats count values(cookie) by _time, id.orig_h, id.resp_h
| where count>30
```

**Query utilizada en laboratorio** (La misma de arriba)
```bash
index="rdp_bruteforce" sourcetype="bro:rdp:json"
| bin _time span=5m
| stats count values(cookie) by _time, id.orig_h, id.resp_h
| where count>30
```
`values(cookie)` : Extrae los **valores únicos del campo `cookie`**.
El `cookie` en logs RDP de Zeek **suele contener el username que el cliente intenta usar**, pero **no es una autenticación confirmada**, solo el valor enviado en el handshake.
# Detección de malware de baliza   /   Detecting Beaconing Malware

### Resumen

```bash
Beaconing Malware  
  
Beacon = comunicación periódica entre un host infectado y un servidor C2 (Command & Control).  
  
Objetivo:  
- recibir comandos  
- enviar información  
- mantener persistencia  
  
Patrón típico:  
El host infectado se conecta al mismo destino a intervalos regulares.  
  
Ejemplo:  
Host → cada 60s → request HTTP → servidor C2  
  
Indicadores de detección:  
- Intervalos de tiempo casi iguales entre conexiones  
- Muchas conexiones pequeñas  
- Comunicación repetida src → dest  
  
Campos útiles en Splunk:  
src / id.orig_h → host infectado  
dest / id.resp_h → posible C2  
dest_port → puerto usado  
_time → calcular intervalos  
method (HTTP) → tipo de request  
  
Técnicas SPL usadas para detectarlo:  
sort _time → ordenar eventos  
streamstats → obtener tiempo del evento anterior  
eval timedelta → calcular intervalo entre conexiones  
eventstats avg() → promedio del intervalo  
where → filtrar intervalos similares  
stats count → contar eventos
```

---

**Significado Query:** *¿hay un host que se conecta a otro host siempre cada X segundos?* **Deteccion de beaconing**
```shell
index="cobaltstrike_beacon" sourcetype="bro:http:json" 
| sort 0 _time
| streamstats current=f last(_time) as prevtime by src, dest, dest_port
| eval timedelta = _time - prevtime
| eventstats avg(timedelta) as avg, count as total by src, dest, dest_port
| eval upper=avg*1.1
| eval lower=avg*0.9
| where timedelta > lower AND timedelta < upper
| stats count, values(avg) as TimeInterval by src, dest, dest_port, total
| eval prcnt = (count/total)*100
| where prcnt > 90 AND total > 10
```
`streamstats last(_time) as prevtime` : Esto guarda **la hora del evento anterior**.
`eval timedelta = _time - prevtime` : Calcula el tiempo entre eventos.
`eventstats avg(timedelta)` : Calcula el **intervalo promedio**.
`upper = avg*1.1` y `lower = avg*0.9` : Esto crea un margen de **±10%**. Se permite que el intervalo sea entre **9 y 11 segundos**.
`where timedelta > lower AND timedelta < upper` : Filtra eventos donde el intervalo es **casi igual al promedio**.
`stats` : Cuenta cuántos eventos cumplen ese patrón.
`eval prcnt` : Calcula qué porcentaje del tráfico sigue ese intervalo.
`where prcnt > 90` : Solo muestra casos donde **más del 90% del tráfico es regular**.

**Cambiar solo:**
```bash
index  
sourcetype  
campos de IP/puerto  
thresholds
```
**No cambiar:**
```bash
lógica de cálculo de tiempo  
(streamstats, timedelta, avg)
```

![300](../Fotos/Pasted%20image%2020260311142836.png)   

![Pasted image 20260316122252](../Fotos/Pasted%20image%2020260316122252.png)

**Query utilizada en laboratorio**
*solo visualiza volumen de tráfico HTTP ES UTIL PORQUE en el beaconing se realiza princeipalmente al menos mediante http*
```bash
index=”cobaltstrike_beacon” sourcetype=”bro:http:json“
| bin _time span=1m
| stats count as event_count by _time
| timechart span=1m count(event_count) as “Beaconing Events”
```
Genera un **gráfico de eventos por minuto** para visualizar actividad de beaconing.

![600](../Fotos/Pasted%20image%2020260311145604.png)

Que es lo mismo que lo de arriba pero sin renombrar el nombre del stat count:
*solo visualiza volumen de tráfico HTTP ES UTIL PORQUE en el beaconing se realiza princeipalmente al menos mediante http*
```bash
index="cobaltstrike_beacon" sourcetype="bro:http:json"
| bin _time span=1m
| stats count by _time
| timechart span=1m count as "Beaconing Events"
```

Beaconing **no es exclusivo de HTTP**.
Protocolos comunes:
- HTTP / HTTPS (más frecuente)
- DNS
- SMB
- ICMP
- TCP personalizado.
# Detección de escaneo de puertos Nmap   /   Detecting Nmap Port Scanning

### Resumen

```bash
Patrón típico del ataque:  
Un mismo host intenta conectarse a MUCHOS puertos  
de un mismo destino en poco tiempo.  
  
Ejemplo:  
192.168.1.10 → 192.168.1.20:21  
192.168.1.10 → 192.168.1.20:22  
192.168.1.10 → 192.168.1.20:80  
192.168.1.10 → 192.168.1.20:443  
192.168.1.10 → 192.168.1.20:445  
  
Indicadores de detección:  
- mismo src  
- mismo dest  
- muchos dest_port diferentes  
- en corto intervalo de tiempo  
  
Campos útiles en Splunk:  
src / id.orig_h → host que escanea  
dest / id.resp_h → host objetivo  
dest_port → puertos escaneados  
proto → protocolo  
_time → ventana temporal  
  
Filtros SPL típicos para detectar escaneo:  
  
count conexiones  
stats count by src dest dest_port  
  
contar puertos distintos  
dc(dest_port)  
  
ventana de tiempo  
bin _time span=1m  
  
detección típica  
where dc(dest_port) > X
```

**Identificar el escaneo de puertos Nmap, usando los registros Splunk y Zeek.**
```shell
index="cobaltstrike_beacon" sourcetype="bro:conn:json" orig_bytes=0 dest_ip IN (192.168.0.0/16, 172.16.0.0/12, 10.0.0.0/8) 
| bin span=5m _time 
| stats dc(dest_port) as num_dest_port by _time, src_ip, dest_ip 
| where num_dest_port >= 3
```
`dc(dest_port)` : distinct **count**. Cuenta **cuántos puertos diferentes intentó conectar el host**. contar valores únicos
`where num_dest_port >= 3` : Solo muestra hosts que intentaron **3 o más puertos distintos**.

**Quey utilizada en el laboratorio:**
```bash
index="cobaltstrike_beacon" sourcetype="bro:conn:json" orig_bytes=0 dest_ip IN (192.168.0.0/16, 172.16.0.0/12, 10.0.0.0/8) dest_port=505  
| bin span=5m _time  
| stats count by _time, src_ip, dest_ip, dest_port
```
`orig_bytes=0` → el cliente **no envía datos**
`dest_ip` → solo **redes privadas**
```bash
192.168.0.0/16  
172.16.0.0/12  
10.0.0.0/8
```

# Detección de ataques de fuerza bruta de Kerberos   /   Detecting Kerberos Brute Force Attacks

### Resumen

```bash
Kerberos Brute Force Attack  
(Detección de ataques de fuerza bruta de Kerberos)  

* Una petición de autenticación que un cliente envía al Domain Controller usando el puerto 88 para obtener un ticket Kerberos

Idea del ataque  
El atacante intenta adivinar la contraseña de una cuenta del dominio  
enviando muchas solicitudes de autenticación Kerberos al KDC.  
  
Cómo funciona  
1. El atacante envía muchas solicitudes AS-REQ al KDC.  
2. Cada intento usa una contraseña diferente.  
3. Si la contraseña es incorrecta → el KDC responde con error.  
4. Si acierta → obtiene un TGT válido.  
  
Objetivo  
Descubrir la contraseña real de una cuenta del dominio.  
  
Indicadores típicos  
- Muchas solicitudes Kerberos en poco tiempo.  
- Muchos errores de autenticación.  
- Varias solicitudes desde una misma IP.  
  
Campos útiles en Splunk (Zeek/Bro)  
- id.orig_h → IP del cliente que intenta autenticarse  
- id.resp_h → IP del KDC (Domain Controller)  
- request_type → tipo de petición Kerberos  
- error_code → errores de autenticación  
  
Idea de filtro en SPL  
Detectar muchas solicitudes Kerberos desde una misma IP.  
  
Ejemplo:  
  
index=kerberos_logs sourcetype="bro:kerberos:json"  
| stats count by id.orig_h  
| where count > 50  
  
Interpretación  
Si una IP genera demasiadas solicitudes Kerberos,  
puede estar intentando adivinar contraseñas mediante fuerza bruta.
```

---
**Identificar los ataques de fuerza bruta de Kerberos, usando troncos Splunk y Zeek.**
```shell
index="kerberos_bruteforce" sourcetype="bro:kerberos:json"
error_msg!=KDC_ERR_PREAUTH_REQUIRED
success="false" request_type=AS
| bin _time span=5m
| stats count dc(client) as "Unique users" values(error_msg) as "Error messages" by _time, id.orig_h, id.resp_h
| where count>30
```
`KDC_ERR_PREAUTH_REQUIRED` : Es un error normal cuando Kerberos pide pre-autenticación. Se excluye porque **no indica ataque**.
`success="false"` : Significa que **la autenticación falló**. `request_type=AS` : AS = **Authentication Service request**. Es cuando el cliente pide un **TGT (Ticket Granting Ticket)** al **KDC**.
`stats count dc(client) as "Unique users"` : - **count** → número de intentos / - **dc(client)** → cantidad de **usuarios distintos atacados**
`where count > 30` : Si hay **más de 30 intentos en 5 minutos** desde una misma IP: → probable **Kerberos brute force o password spraying**.

Un nombre de usuario válido solicitará al servidor que se ajuste o generará un error como , indicando que se requiere preautenticación. Por otro lado, un nombre de usuario inválido aparecerá con un código de error Kerberos en el mensaje AS-REP (Authentication Service Response). Examinando las respuestas a sus mensajes AS-REQ, los adversarios pueden determinar rápidamente qué nombres de usuario son válidos en el sistema objetivo.`return a TGT``KRB5KDC_ERR_PREAUTH_REQUIRED``KRB5KDC_ERR_C_PRINCIPAL_UNKNOWN`*

![Pasted image 20260305104135](../Fotos/Pasted%20image%2020260305104135.png)

**Querys utilizadas en laboratorio:**
```bash
_index="kerberos_bruteforce" sourcetype="bro:kerberos:json" accrescent/windomain.local_
```
o
```bash
_index="kerberos_bruteforce" sourcetype="bro:kerberos:json" client="accrescent/windomain.local_"
```

![Pasted image 20260311180220](../Fotos/Pasted%20image%2020260311180220.png)

→ posible **Kerberos brute force** contra ese usuario. Cuenta del dominio que intenta autenticarse (usuario accrescent del dominio windomain.local)
# Detección del kerberoasting   /   Detecting Kerberoasting

### Resumen

```bash
Idea del ataque  
El atacante solicita tickets Kerberos (TGS) de cuentas de servicio que tienen SPN  
para extraer el hash del ticket y crackearlo offline.  
  
Cómo funciona  
1. El atacante ya tiene acceso a una cuenta del dominio.  
2. Busca cuentas que tengan SPN (servicios como SQL, IIS, etc.).  
3. Solicita al KDC un TGS para ese servicio.  
4. El KDC responde con el TGS.  
5. El TGS contiene un hash del password de la cuenta de servicio.  
6. El atacante guarda ese ticket y lo crackea offline.  
  
Objetivo  
Obtener la contraseña de cuentas de servicio del dominio.  
  
Por qué funciona  
Los tickets TGS están cifrados con la clave de la cuenta de servicio,  
por lo que ese ticket se puede intentar crackear fuera de la red.  
  
Indicadores típicos  
- Muchas solicitudes TGS.  
- Solicitudes TGS para varios SPN diferentes.  
- Un usuario solicitando muchos tickets de servicio.  
  
Campos útiles en Splunk (Zeek/Bro)  
- id.orig_h → IP del cliente  
- id.resp_h → IP del KDC  
- service → servicio solicitado  
- request_type → tipo de petición Kerberos  
- client → usuario que solicita el ticket  
  
Idea de filtro en SPL  
  
index=kerberos_logs sourcetype="bro:kerberos:json"  
| stats count by id.orig_h service  
| where count > 10  
  
Interpretación  
Si una máquina solicita muchos TGS para distintos servicios,  
puede estar intentando recolectar hashes para Kerberoasting.
```

---
*Al poseer solo una cuenta de usuario legítima y su contraseña, un atacante podría recuperar los tickets SPN e intentar romperlos fuera de línea.`Kerberoasting`
Tras examinar numerosos recursos sobre kerberoasting, se ve que se utiliza para el cifrado de tickets en secreto. Aprovecharemos esta base como punto de detección en esta sección.`RC4`*
#### Notas
**Kerberoasting funciona solo con TGS, porque esos tickets se cifran con la contraseña de la cuenta de servicio.**
El atacante solicita **tickets de servicio (TGS)** para cuentas que tienen **SPN configurado**. Ese ticket contiene un **hash cifrado con la contraseña de la cuenta de servicio**, que luego puede **romperse offline**.
##### Cómo funciona (resumen)
1. El atacante tiene acceso a un usuario del dominio (cualquier cuenta válida).
2. Busca cuentas con **SPN (Service Principal Name)**.
3. Solicita un **TGS** al **KDC (Key Distribution Center)**.
4. El **TGS contiene un hash de la contraseña del servicio**.
5. El atacante extrae ese hash y lo **crackea offline** (por ejemplo con **Hashcat**).

**Identificar el kerberoasting, usando troncos Splunk y Zeek.**
```bash
index="sharphound" sourcetype="bro:kerberos:json" request_type=TGS cipher="rc4-hmac" forwardable="true" renewable="true" 
| table _time, id.orig_h, id.resp_h, request_type, cipher, forwardable, renewable, client, service
```
`RC4-HMAC` : Los atacantes suelen pedir RC4 porque es mucho más fácil de crackear offline.

En los logs de **Zeek** (`bro:kerberos:json`) el puerto del servidor aparece normalmente como **`id.resp_p`**.
```bash
index="sharphound" sourcetype="bro:kerberos:json"
request_type=TGS cipher="rc4-hmac"
forwardable="true" renewable="true"
| table _time, id.orig_h, id.resp_h, id.resp_p, request_type, cipher, client, service
```
##### Cómo leerlo
- `id.orig_h` → IP del atacante
- `id.resp_h` → servidor Kerberos (DC)
- `id.resp_p` → **puerto usado**
#### Visto en modulo de academy
**La query detecta:** hosts que generan solo solicitudes TGS sin solicitudes AS **(ESTA QUERY DETECTA MAS GOLDEN TICKET PORQUE AHI YA ESTAS AUTENTICADO)**
```bash
index="golden_ticket_attack" sourcetype="bro:kerberos:json" 
| where client!="-" 
| bin _time span=1m  
| stats values(client), values(request_type) as request_types, dc(request_type) as unique_request_types by _time, id.orig_h, id.resp_h 
| where request_types=="TGS" AND unique_request_types==1
```

![Pasted image 20260311182127](../Fotos/Pasted%20image%2020260311182127.png) 

![Pasted image 20260311182155](../Fotos/Pasted%20image%2020260311182155.png) 

![Pasted image 20260311182248](../Fotos/Pasted%20image%2020260311182248.png)

![Pasted image 20260311182521](../Fotos/Pasted%20image%2020260311182521.png)

*Desglose de la búsqueda*:
- `index="golden_ticket_attack" sourcetype="bro:kerberos:json"`Esta línea especifica la fuente de datos en la que la consulta está buscando. Busca eventos en el índice donde el (formato de datos) es .`golden_ticket_attack``sourcetype``bro:kerberos:json`
- `where client!="-"`: Elimina eventos donde **no hay usuario Kerberos identificado**. Se quedan solo eventos donde: client = usuario real
- `bin _time span=1m`: Agrupa los eventos en **ventanas de 1 minuto**.
- `stats values(client), values(request_type) as request_types, dc(request_type) as unique_request_types by _time, id.orig_h, id.resp_h`Esta línea agrupa por tiempo, IP origen y IP destino
    - `values(client)`: Todos los valores únicos del cliente asociados a los eventos.
    - `values(request_type) as request_types`: Todos los tipos de peticiones únicas asociadas a los eventos.
    - `dc(request_type) as unique_request_types`: El recuento distinto de tipos de solicitudes.
- `where request_types=="TGS" AND unique_request_types==1`: Busca casos donde: solo hay TGS y **no hay AS request previo**.

**Query utilizada en laboratorio**
**Sieve para ver ventanas donde solo hay solicitudes TGS**
```bash
index="golden_ticket_attack" sourcetype="bro:kerberos:json"
| where client!="-"
| bin _time span=1m
| stats values(client), values(request_type) as request_types, dc(request_type) as unique_request_types by _time, id.orig_h, id.orig_p, id.resp_h, id.resp_p
| where request_types=="TGS" AND unique_request_types==1
```
Solo añadiste:
`id.resp_p`
`id.origin_p`

![Pasted image 20260316154545](../Fotos/Pasted%20image%2020260316154545.png)

# Detectando Billetes Dorados   /   Detecting Golden Ticket

### Resumen

```bash
Detección de Golden Ticket / Detecting Golden Ticket  
  
Idea  
El atacante falsifica un TGT usando el hash de la cuenta krbtgt del dominio.  
  
Qué es krbtgt  
- Cuenta especial de Active Directory.  
- El KDC usa su hash para firmar todos los TGT.  
- Si el atacante roba ese hash puede crear TGT falsos.  
  
Cómo funciona el ataque  
1. El atacante obtiene el hash de krbtgt.  
2. Con ese hash crea un TGT falso (Golden Ticket).  
3. El ticket puede incluir cualquier usuario (ej: Administrator).  
4. El ticket se presenta al KDC para pedir TGS.  
5. El KDC confía en el TGT y entrega acceso.  
  
Resultado  
Acceso total al dominio durante mucho tiempo.  
  
Indicadores de detección  
- TGT con duración anormalmente larga.  
- Uso de cuentas inexistentes. (hay que saber las cuenas existentes para determinar esto)
- Autenticaciones Kerberos sin AS-REQ previo.  
  
Campos útiles en Splunk  
- EventCode=4768 (TGT request)  
- EventCode=4769 (TGS request)  
- user  
- service  
- ticket_lifetime  
- src_ip  
  
Idea de filtro en SPL  
Buscar TGT/TGS sospechosos o inconsistentes.  
  
Ejemplo simple  
index=windows EventCode=4769  
| stats count by user, service, src_ip  
| where count > 50

BUSCAMOS ESTE PATRON ANORMAL:
TGS sin AS-REQ previo
TGS-REQ → TGS-REP

PATRON NORMAL:
AS-REQ → AS-REP → TGS-REQ → TGS-REP
```

*Estas 2 querys No detectan Golden Ticket de forma precisa. Solo detectan un comportamiento que podría estar relacionado.*
**Mas arriba tenemos querys mas precisas**

*Al menos estas 2 querys sirven para (**Actividad Kerberos incompleta o anómala**)*

```bash
index="golden_ticket_attack" sourcetype="bro:kerberos:json" 
| where client!="-" 
| bin _time span=1m 
| stats values(client), values(request_type) as request_types, dc(request_type) as unique_request_types by _time, id.orig_h, id.resp_h 
| where request_types=="TGS" AND unique_request_types==1
```
`| where client!="-"` : Elimina eventos donde **no se identificó usuario Kerberos**.
`| where request_types=="TGS" AND unique_request_types==1` : solo existen solicitudes TGS y **no existen solicitudes AS**.
`by _time, id.orig_h, id.resp_h` : agrupa eventos por: tiempo, IP origen. IP destino.

**Query Utilizada en laboratorio**
```bash
index="golden_ticket_attack" sourcetype="bro:kerberos:json"
| where client!="-"
| bin _time span=1m
| stats values(client), values(request_type) as request_types, dc(request_type) as unique_request_types by _time, id.orig_h, id.orig_p, id.resp_h, id.resp_p
| where request_types=="TGS" AND unique_request_types==1
```
Se agrego 
`id_orig_p`
`id_resp_p`

| Campo     | Significado    |
| --------- | -------------- |
| id.orig_h | IP origen      |
| id.orig_p | puerto origen  |
| id.resp_h | IP destino     |
| id.resp_p | puerto destino |
#### Cómo se detecta Golden Ticket de verdad

|EventID|Significado|
|---|---|
|4768|TGT request|
|4769|TGS request|
|4624|logon|
# Detectando el PSExec de Cobalt Strike   /   Detecting Cobalt Strike's PSExec

### Resumen

```bash
Detección de PSExec de Cobalt Strike / Detecting Cobalt Strike PSExec  
  
Idea  
Cobalt Strike usa una técnica tipo PSExec para ejecutar comandos remotamente en otra máquina mediante SMB y servicios remotos.  
  
Qué es PSExec  
PSExec = herramienta de Sysinternals que permite ejecutar comandos en otro host usando SMB y creación de servicios remotos.  
  
Cómo funciona el ataque  
1. El atacante tiene credenciales válidas.  
2. Se conecta por SMB al host objetivo.  
3. Copia un binario o payload al ADMIN$ share.  
4. Crea un servicio remoto en el host.  
5. El servicio ejecuta el payload (Cobalt Strike beacon).  
  
Resultado  
Ejecución remota de comandos con privilegios del servicio (normalmente SYSTEM).  
  
Indicadores de detección  
- Acceso a ADMIN$ o C$ vía SMB.  
- Creación de servicios remotos.  
- Transferencia de ejecutables por SMB.  
- Ejecución remota inmediatamente después del acceso SMB.  
  
Campos útiles en Splunk  
Si usas logs de Zeek / Bro:  
  
sourcetype="bro:smb_files:json"  
Campos útiles:  
- action  
- name  
- path  
- id.orig_h (host atacante)  
- id.resp_h (host víctima)  
  
Acciones importantes  
- SMB::FILE_OPEN  
- SMB::FILE_WRITE  
- SMB::FILE_DELETE  
  
Idea de filtro SPL  
Detectar creación o transferencia de ejecutables por SMB.  
  
Ejemplo  
index=* sourcetype="bro:smb_files:json"  
| search name="*.exe"  
| stats count by id.orig_h, id.resp_h, name  
  
Qué buscar  
- ejecutables copiados por SMB  
- muchas operaciones SMB en poco tiempo  
- acceso a ADMIN$ seguido de ejecución  
  
Concepto clave para tus notas  
PSExec lateral movement = SMB + copiar binario + crear servicio remoto.
```

*El atacante A tiene credenciales admin de la maquina B pero no esta en ese host por lo que mediante smb carga un payload para ejecucion remota de shell*

![Pasted image 20260316195848](../Fotos/Pasted%20image%2020260316195848.png)

---
La versión de Cobalt Strike se utiliza para ejecutar cargas útiles en sistemas remotos, como parte del proceso posterior a la explotación.`psexec`

```bash
index="cobalt_strike_psexec" sourcetype="bro:smb_files:json" action="SMB::FILE_OPEN" name IN ("*.exe", "*.dll", "*.bat") path IN ("*\\c$", "*\\ADMIN$") size>0
```
`action="SMB::FILE_OPEN"` : alguien abrió un archivo vía SMB
`name IN ("*.exe", "*.dll", "*.bat")` : tipo de archivo
`C$` : acceso remoto al disco C. *accesibles solo con privilegios admin*
`ADMIN$` : acceso remoto a la carpeta de Windows entonces abre: `C:\Windows` *accesibles solo con privilegios admin*
`size>0` : el tamaño del archivo es mayor que 0 bytes

![350](../Fotos/Pasted%20image%2020260312130716.png)

**Query utilizada en laboratorio**
```bash
index="change_service_config" sourcetype="bro:dce_rpc:json"
| spath endpoint
| search endpoint=svcctl
| table _time, id.orig_h, id.resp_h, endpoint, operation
```
`| spath endpoint` : Sirve para **extraer campos dentro de JSON**. Aquí extrae: endpoint que indica **qué servicio RPC se está utilizando**. (Tus logs vienen como JSON desde **Zeek**.)
`| search endpoint=svcctl` : `svcctl` significa: Service Control Manager Es el servicio de Windows que permite
- crear servicios
- iniciar servicios
- modificar servicios

![250](../Fotos/Pasted%20image%2020260312134737.png)    

![400](../Fotos/Pasted%20image%2020260305133408.png)    

![Pasted image 20260312141635](../Fotos/Pasted%20image%2020260312141635.png)

# Detección de Zerologon   /   Detecting Zerologon

### Resumen
```bash
Vulnerabilidad  
Fallo en el protocolo Netlogon usado entre máquinas y el Domain Controller.  
  
CVE  
CVE-2020-1472  
  
Idea del ataque  
El atacante abusa de un error criptográfico en Netlogon para autenticarse  
contra el Domain Controller usando valores de autenticación en cero.  

Netlogon = protocolo/servicio de Windows usado para la autenticación  
entre máquinas del dominio y el Domain Controller.  
  
Función principal  
Permitir que un equipo del dominio se autentique y establezca  
una conexión segura con el Domain Controller.  
  
Qué usa  
- Active Directory  
- Kerberos / NTLM  
- Canal seguro llamado Secure Channel
  
Cómo funciona  
1. El atacante envía múltiples solicitudes de autenticación Netlogon.  
2. Usa un challenge y credenciales con valor 0.  
3. Debido al fallo criptográfico, algunas solicitudes son aceptadas.  
4. El atacante se autentica como el Domain Controller.  
5. Puede cambiar la contraseña de la cuenta del DC.  
  
Resultado  
Control total del dominio.  
  
Indicadores de detección  
- Muchas solicitudes de autenticación Netlogon seguidas.  
- Autenticaciones anómalas desde un host hacia el DC.  
- Tráfico repetido hacia el servicio Netlogon.  
  
Campos útiles en Splunk (Zeek/Bro)  
- id.orig_h → host atacante  
- id.resp_h → Domain Controller  
- auth_type  
- success / status  
- _time  
  
Idea de filtro SPL  
Detectar muchos intentos de autenticación Netlogon desde una misma IP.  
  
Ejemplo  
  
index=zerologon sourcetype="bro:dce_rpc:json"  
| stats count by id.orig_h  
| where count > 100  
  
Idea clave para notas  
  
Zerologon = abuso de Netlogon para autenticarse como Domain Controller  
usando valores criptográficos en cero.
```

![Pasted image 20260317004138](../Fotos/Pasted%20image%2020260317004138.png)   

![350](../Fotos/Pasted%20image%2020260312153804.png)

---
**Identificar Zerologon, usando los registros Splunk y Zeek.**
```bash
index="zerologon" endpoint="netlogon" sourcetype="bro:dce_rpc:json" 
| bin _time span=1m 
| where operation == "NetrServerReqChallenge" OR operation == "NetrServerAuthenticate3" OR operation == "NetrServerPasswordSet2" 
| stats count values(operation) as operation_values dc(operation) as unique_operations by _time, id.orig_h, id.resp_h 
| where unique_operations >= 2 AND count>100
```

```bash
1. NetrServerReqChallenge  
2. NetrServerAuthenticate3 (muchas veces)  
3. autenticación exitosa  
4. NetrServerPasswordSet2
```
**Explicacion**

![480](../Fotos/Pasted%20image%2020260312155728.png)   

![Herramienta de análisis de logs que muestra una consulta de búsqueda para eventos de Zerologon. Muestra dos eventos con columnas para tiempo, IP de origen, IP de respuesta, recuento, valores de operación y operaciones únicas.|900](https://cdn.services-k8s.prod.aws.htb.systems/content/modules/233/117.png)  

![Pasted image 20260312162304](../Fotos/Pasted%20image%2020260312162304.png)

`| where unique_operations >= 2 AND count>100` : La detección exige **dos condiciones**. (**1** Debe haber **al menos 2 tipos de llamadas Netlogon**. **2** Debe haber **más de 100 llamadas en 1 minuto**.)

1️⃣ El atacante envía muchas solicitudes **Netlogon authentication** al **Domain Controller**.
2️⃣ Debido a un fallo criptográfico en **AES-CFB8**, el atacante puede usar **vectores de inicialización (IV) en cero**.
3️⃣ Después de muchos intentos, uno **pasa la autenticación sin conocer la contraseña** de la máquina.
4️⃣ El atacante puede entonces **cambiar la contraseña del Domain Controller** y obtener **control total del dominio**.

 La vulnerabilidad puede ser explotada por un atacante para hacerse pasar por cualquier ordenador, incluido el controlador de dominio, y ejecutar llamadas a procedimientos remotos en su nombre. El atacante puede explotar esta debilidad criptográfica intentando autenticarse contra el controlador de dominio usando una clave de sesión compuesta solo por ceros, eludiendo efectivamente el proceso de autenticación. Esto permite al atacante establecer un canal seguro con el controlador de dominio sin conocer la contraseña de la cuenta de la máquina. Una vez establecido este canal, el atacante puede utilizar la función NetrServerPasswordSet2 para cambiar la contraseña de la cuenta del ordenador por cualquier valor, incluida una contraseña en blanco. Esto otorga efectivamente al atacante el control total sobre el controlador de dominio y, por extensión, sobre todo el dominio de Active Directory.

**Query utilizada en el laboratorio** pero no determinante para encontrar la respuesta
```bash
index="zerologon" endpoint="netlogon" sourcetype="bro:dce_rpc:json"
```
# Detección de exfiltración (HTTP)   /   Detecting Exfiltration (HTTP)

### Resumen
```bash
Idea  
Un host comprometido envía datos robados hacia un servidor externo usando HTTP.  
  
Objetivo  
Sacar información de la red sin levantar sospechas.  
  
Cómo funciona  
1. El atacante compromete una máquina.  
2. Recoge datos (credenciales, documentos, DB, etc.).  
3. Los envía a su servidor usando HTTP.  
  
Ejemplo  
Host interno → HTTP POST → servidor del atacante  
  
Indicadores típicos  
- Grandes cantidades de datos saliendo por HTTP  
- Muchas peticiones POST  
- Comunicación repetida con un mismo destino externo  
- Tamaños de respuesta/request anormales  
  
Campos útiles en Splunk (Zeek/Bro HTTP)  
- id.orig_h → host interno  
- id.resp_h → servidor externo  
- method → GET / POST  
- uri  
- request_body_len  
- response_body_len  
- _time  
  
Idea de filtros SPL  
Detectar grandes volúmenes de datos enviados por HTTP.  
  
Ejemplo  
  
index=* sourcetype="bro:http:json"  
| stats sum(request_body_len) as bytes_sent by id.orig_h id.resp_h  
| where bytes_sent > 10000000
```

---
**Identificar la exfiltración HTTP usando registros de Splunk y Zeek.**
*Busca hosts que envían muchos datos por HTTP POST. (exfiltración de datos)*
```bash
index="cobaltstrike_exfiltration_http" sourcetype="bro:http:json" method=POST 
| stats sum(request_body_len) as TotalBytes by src, dest, dest_port 
| eval TotalBytes = TotalBytes/1024/1024
```
`request_body_len` : tamaño del cuerpo HTTP enviado, es decir, cantidad de datos enviados en el POST
`sum(request_body_len)` : Suma todos los bytes enviados.
`| eval TotalBytes = TotalBytes/1024/1024` : Convierte bytes → megabytes. 1 MB = 1024 * 1024 bytes TotalBytes = 13.8 MB

**Query utilizada en laboratorio** (la misma que arriba)
```bash
index="cobaltstrike_exfiltration_http" sourcetype="bro:http:json" method=POST
| stats sum(request_body_len) as TotalBytes by src, dest, dest_port
| eval TotalBytes = TotalBytes/1024/1024
```
# Detección de exfiltración (DNS)   /   Detecting Exfiltration (DNS)

### Resumen
```bash
Idea  
El atacante usa consultas DNS para sacar datos de la red.  
  
Objetivo  
Evadir controles de seguridad usando DNS,  
ya que casi todas las redes permiten tráfico DNS.  
  
Cómo funciona  
1. El malware roba datos del sistema.  
2. Codifica esos datos (base64/hex).  
3. Los inserta dentro del nombre de dominio.  
4. El host hace consultas DNS al dominio del atacante.  
5. El servidor DNS del atacante reconstruye los datos.  
  
Ejemplo  
Host → consulta DNS:  
dGhpcy1pcy1kYXRh.attacker.com  
  
El subdominio contiene los datos robados.  
  
Indicadores típicos  
- Subdominios muy largos  
- Muchas consultas DNS hacia el mismo dominio  
- Alta entropía en los nombres de dominio  
- Gran volumen de queries DNS  
  
Campos útiles en Splunk (Zeek/Bro DNS)  
- id.orig_h → host interno  
- query → dominio consultado  
- query_len → longitud del dominio  
- _time  
  
Idea de filtros SPL  
  
detectar dominios largos  
  
index=* sourcetype="bro:dns:json"  
| eval query_length=len(query)  
| where query_length > 50  
  
detectar muchas consultas al mismo dominio  
  
index=* sourcetype="bro:dns:json"  
| stats count by id.orig_h query  
| where count > 100  
  
Idea clave para notas  
  
DNS exfiltration = datos codificados dentro de consultas DNS  
para sacarlos de la red hacia un servidor controlado por el atacante.
```

---
**Identificar la exfiltración de DNS, usando los registros de Splunk y Zeek.**
```bash
index=dns_exf sourcetype="bro:dns:json"  
| eval len_query=len(query)  
| search len_query>=40 AND query!="*.ip6.arpa*" AND query!="*amazonaws.com*" AND query!="*._googlecast.*" AND query!="_ldap.*"  
| bin _time span=24h  
| stats count(query) as req_by_day by _time, id.orig_h, id.resp_h  
| where req_by_day>60  
| table _time, id.orig_h, id.resp_h, req_by_day
```
`eval len_query=len(query)` :  calcula **cuántos caracteres tiene**. `query` = dominio consultado. ejemplo: a8f72d92b7a1.data.attacker.com
~`search len_query>=40` : Se queda solo con **dominios** **largos (≥40 caracteres)**. Suele indicar esto datos codificados dentro del dominio
Exclusión de trafico normal:
- `query!="*.ip6.arpa*"`
- `query!="*amazonaws.com*"`
- `query!="*._googlecast.*"`
- `query!="_ldap.*"`
`stats count(query) as req_by_day` : req_by_day = número de consultas DNS y las grupa por `_time` → día  / `id.orig_h` → host que consulta  / `id.resp_h` → servidor DNS
~`where req_by_day>60` :  Se queda con los que hicieron más de 60 **consultas** DNS largas en un día

**Query usada en laboratorio:** Se modifico minimamente la query superior 
```bash
index=dns_exf sourcetype="bro:dns:json"
| eval len_query=len(query)
| search len_query>=40 AND query!="*.ip6.arpa*" AND query!="*amazonaws.com*" AND query!="*._googlecast.*" AND query!="_ldap.*"
| stats count by query
| sort -count
```
`stats count by query`
- `stats` → agrega datos.
- `count` → cuenta cuántas veces aparece cada consulta DNS.
- `by query` → agrupa el conteo por **nombre de dominio consultado**.
Resultado: muestra **qué queries DNS se repiten más**.

![Pasted image 20260313230922](../Fotos/Pasted%20image%2020260313230922.png)

# Detección de ransomware   /   Detecting Ransomware

### Resumen 
```bash
Idea  
Malware que cifra archivos del sistema y exige un rescate para recuperarlos. 
  
Objetivo  
Bloquear el acceso a los datos para pedir pago (normalmente en criptomonedas).  
  
Cómo ocurre  
1. El sistema se infecta.  
2. El malware comienza a cifrar archivos.  
3. Se crean nuevas versiones cifradas.  
4. Aparece una nota de rescate.  
  
Comportamiento típico  
- Muchos archivos modificados en poco tiempo  
- Creación de extensiones nuevas (.locked, .encrypted, etc.)  
- Gran actividad de escritura en disco  
- Eliminación de copias de seguridad  
  
Indicadores de detección  
- Gran número de operaciones de archivo  
- Creación masiva de archivos nuevos  
- Cambios de extensión sospechosos  
  
Campos útiles en Splunk (ej. Zeek / SMB logs)  
id.orig_h → host que modifica archivos  
id.resp_h → servidor de archivos  
name → nombre del archivo  
path → ubicación del archivo  
action → tipo de operación (write, create, delete)  
  
Idea de filtro SPL  
  
index=* sourcetype="bro:smb_files:json"  
| stats count by id.orig_h name  
| where count > 100  
  
Idea clave para notas  
  
Ransomware = cifrado masivo de archivos en poco tiempo  
para exigir un rescate.
```

----
#### (Sobreescritura excesiva)
**Identificar ransomware, usando los registros Splunk y Zeek.**
*Actividad masiva de apertura + renombrado de archivos vía SMB*
```bash
index="ransomware_open_rename_sodinokibi" sourcetype="bro:smb_files:json" 
| where action IN ("SMB::FILE_OPEN", "SMB::FILE_RENAME")
| bin _time span=5m
| stats count by _time, source, action
| where count>30 
| stats sum(count) as count values(action) dc(action) as uniq_actions by _time, source
| where uniq_actions==2 AND count>100
```
`where count > 30` : Se queda solo con acciones donde hubo **más de 30 operaciones**. eliminando actividad normal
`sum(count)` : total de operaciones SMB
`values(action)` : lista de acciones observadas
`dc(action)` : cantidad de tipos distintos de acción
`where uniq_actions == 2 AND count > 100` : **1 condicion**: `uniq_actions = 2` Significa que ocurrieron ambas acciones: FILE_OPEN y FILE_RENAME **2 condicion**: Más de **100 operaciones en 5 minutos**.
#### (Cambio excesivo de nombre con la misma extensión)
**Identificar ransomware, usando los registros Splunk y Zeek.**
*Renombrado masivo de archivos con cambio de extensión vía SMB*
```bash
index="ransomware_new_file_extension_ctbl_ocker" sourcetype="bro:smb_files:json" action="SMB::FILE_RENAME" 
| bin _time span=5m 
| rex field="name" "\.(?<new_file_name_extension>[^\.]*$)" 
| rex field="prev_name" "\.(?<old_file_name_extension>[^\.]*$)" 
| stats count by _time, id.orig_h, id.resp_p, name, source, old_file_name_extension, new_file_name_extension, 
| where new_file_name_extension!=old_file_name_extension 
| stats count by _time, id.orig_h, id.resp_p, source, new_file_name_extension 
| where count>20 | sort -count
```

![180](../Fotos/Pasted%20image%2020260313233501.png)

`rex field="name" "\.(?<new_file_name_extension>[^\.]*$)"` : Extrae la extensión del archivo **después del renombrado**. Ejemplo: document.txt.locked
`rex field="prev_name" "\.(?<old_file_name_extension>[^\.]*$)"` : Extrae la extensión **antes del renombrado**.. Ejemplo: document.txt
`stats count by _time id.orig_h id.resp_p name source old_file_name_extension new_file_name_extension` :  Agrupa eventos por: tiempo, host origen, archivo, extensión antigua, extensión nueva.
`where new_file_name_extension != old_file_name_extension` : Se queda solo con eventos donde: la extensión cambió
`stats count by _time id.orig_h id.resp_p source new_file_name_extension` : Ahora cuenta **cuántos archivos se renombraron a la misma extensión**.
`where count > 20` : mas de 20 archivos cambiaron a la misma extensión en 5 minutos.
`sort -count` : Muestra primero **los casos con más actividad**.

![Herramienta de análisis de registros que muestra una consulta de búsqueda para eventos de ransomware con acción SMB::FILE_RENAME. Muestra un evento con columnas para tiempo, IP de origen, IP de respuesta, fuente, extensiones de nombre de archivo antiguas y nuevas, y conteo.|1100](https://cdn.services-k8s.prod.aws.htb.systems/content/modules/233/124.png)

**Query utilizada en laboratorio**
*Actividad sospechosa de borrado masivo de archivos vía SMB*
```bash
index="ransomware_excessive_delete_aleta" sourcetype="bro:smb_files:json"
| where action IN ("SMB::FILE_OPEN", "SMB::FILE_DELETE")
| bin _time span=5m
| stats count by _time, source, action
| where count>30
| stats sum(count) as count values(action) dc(action) as uniq_actions by _time, source
| where uniq_actions==2 AND count>100
```
`where count > 30` :  Se descartan acciones con poca frecuencia
`sum(count)` : Total de operaciones SMB
`values(action)` : Lista de acciones observadas
`distinct count` :  (número de tipos distintos de acción)
`where uniq_actions == 2 AND count > 100` : **Condicion 1** : La alerta aparece cuando hay ambas acciones el File open y File delete **Condicion 2** : Hubo mas de 100 operaciones totales en 5 minutos

![500](../Fotos/Pasted%20image%2020260314094453.png)   

![Pasted image 20260314095150](../Fotos/Pasted%20image%2020260314095150.png)   

![Pasted image 20260314095316](../Fotos/Pasted%20image%2020260314095316.png)

**Nota:** Se pueden encontrar extensiones conocidas relacionadas con ransomware en los recursos a continuación.

- [https://docs.google.com/spreadsheets/d/e/2PACX-1vRCVzG9JCzak3hNqqrVCTQQIzH0ty77BWiLEbDu-q9oxkhAamqnlYgtQ4gF85pF6j6g3GmQxivuvO1U/pubhtml](https://docs.google.com/spreadsheets/d/e/2PACX-1vRCVzG9JCzak3hNqqrVCTQQIzH0ty77BWiLEbDu-q9oxkhAamqnlYgtQ4gF85pF6j6g3GmQxivuvO1U/pubhtml)
- [https://github.com/corelight/detect-ransomware-filenames](https://github.com/corelight/detect-ransomware-filenames)
- [https://fsrm.experiant.ca/](https://fsrm.experiant.ca/)

# Evaluacion de Habilidades

**Query ejercicio 1**
```bash
index="empire" sourcetype="bro:http:json" 
| sort 0 _time
| streamstats current=f last(_time) as prevtime by src, dest, dest_port
| eval timedelta = _time - prevtime
| eventstats avg(timedelta) as avg, count as total by src, dest, dest_port
| eval upper=avg*1.1
| eval lower=avg*0.9
| where timedelta > lower AND timedelta < upper
| stats count, values(avg) as TimeInterval by src, dest, dest_port, total
| eval prcnt = (count/total)*100
| where prcnt > 80 AND total > 10
```

![350](../Fotos/Pasted%20image%2020260314101941.png)

`| where prcnt > 80 AND total > 10`
Significa:
"Mostrar solo comunicaciones donde más del 80% de los intervalos son iguales y hay más de 10 eventos"

**Query ejercicio 2**
```bash
index="printnightmare" sourcetype="bro:dce_rpc:json"
operation="RpcAddPrinterDriverEx"
| table _time id.orig_h id.resp_h operation
```

1. `index="printnightmare"`  
Busca solo en el dataset del laboratorio relacionado con **PrintNightmare**.
2. `sourcetype="bro:dce_rpc:json"`
Eventos de **RPC (Remote Procedure Call)** capturados por Zeek/Bro.
3. `operation="RpcAddPrinterDriverEx"`
Esta es la operación usada en el exploit **PrintNightmare** para instalar drivers maliciosos.
4. `table _time id.orig_h id.resp_h operation`
Muestra:
- `_time` → momento del evento
- `id.orig_h` → **IP del atacante**
- `id.resp_h` → servidor objetivo
- `operation` → función RPC usada

También hubiera servido esta SPL
```bash
index="printnightmare" sourcetype="bro:dce_rpc:json"
```

**Query ejercicio 3**
```bash
index="bloodhound_all_no_kerberos_sign" sourcetype="bro:dce_rpc:json" operation="SamrOpenAlias"
```


