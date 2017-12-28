---
title: Fundamentos de Redes					# Título
author: Jesús Sánchez de Lechina Tejada		# Nombre del autor
header-includes:      	 	        	# Incluir paquetes en LaTeX
toc: true                   			# Índice
numbersections: false       			# Numeración de secciones
fontsize: 11pt              			# Tamaño de fuente
geometry: margin=1in        			# Tamaño de los márgenes
---

\newpage

<!--13/09/2017-->
<!--
**Presentación**
*) El sistema de evaluación está en la página dtstc.ugr.es/it
-->

# Tema 1.

Internet es considerada la red más grande creada por el hombre. Pero
esta requiere un conjunto de reglas, protocolos y un software y
hardware específico para funcionar.

Para enviar un mensaje en una red se utilizan diversas capas para
transformar el mensaje hasta un nivel transferible mediante
telecomunicaciones (paquetes de bits), el receptor deberá de usar las
mismas capas y protocolos para poder obtener el mensaje
original. Esta información es troceada en segmentos, "*paquetes*", que
incluyen una cabecera con información acerca del propio envío.

Hoy en día se utiliza un control distribuido de la red, aunque la
tendencia actual es SDN (Software Defined Network), redes que buscan
simplificar la complejidad de la red separando el plano de control
(software) frente al plano de datos (hardware).

## Capas y funcionalidades 

### **Modelo OSI**
Grupo 1 de capas:
- Capa Física: Conexión ordenador-punto de acceso. Mueven los bits
  dentro de la trama de un nodo al siguiente.

- Capa de Enlace: Permite la comunicación entre dos nodos directamente
  conectados. Delimita la trama (nombre que se le da a los paquetes de
  la capa de enlace) y controla errores y flujo. 
  
Grupo 2:
- Capa de Red: Nos ofrece la capacidad de enrutamiento que permite
  llevar a los paquetes desde un origen a un destino. Controla además
  la congestión, evitando el envío de paquetes a nodos
  congestionados. Incluye el protocolo IP, que es único y hace que se
  haga referencia a esta capa como la capa IP.

Abstrae toda la comunicación para que únicamente se preocupe del nodo
inicial y nodo destino como si estuvieran conectados directamente.

Grupo 3:


- Capa de Transporte: Transporta los mensajes entre los extremos
  finales de la comunicación. Existen dos protocolos de transporte,
  UDP y TCP. Proporciona control de errores y control de flujo.

- Capa de Sesión: Controla el "turno de palabra" y las tareas de
  sincronización, es decir, decide quién y en qué momento envía
  información.

- Capa de Presentación: Permite una traducción desde el "idioma de la
  red" al "idioma de la aplicación", que se puedan entender los datos
  intercambiados. Incluye cifrado y compresión.
  
- Capa de Aplicación: Son las aplicaciones finales, que tienen su
  propio protocolo (http, ftp, por ejemplo). A los paquetes que
  transmite se les conoce como "*mensajes*".
  
  
### **Modelo TCP/IP**

IP es el protocolo más usado en internet, y en el que se centrará la
asignatura.


<!--20/09/2017-->

## Terminología y servicios:

Comunicación **virtual** (horizontal) vs **real**(vertical).

La comunicación virtual es cómo la capa N-ésima de la primera entidad
que se comunica envía un mensaje a la N-ésima capa de la segunda
entidad receptora. Mientras que la real refleja como cada capa se
comunica con su capa inmediatamente inferior hasta que llega a la capa
física que transmite la información y se comunica con la capa
inmediatamente superior hasta llegar a la capa original.

**Retardos en la comunicación:**

El **tiempo de transmisión** es lo que tardan los bits del paquete en
emitirse. Depende de la velocidad de transmisión y el número de bits a
enviar.

$$T_t = \frac{P(b)}{V_t (bps)}$$

El **tiempo de propagación** es el tiempo que el primer bit tarda hasta
llegar a su destino. La velocidad de propagación depende del medio
físico del enlace y se mide en metros por segundo. (Distancia a
recorrer entre velocidad de propagación

$$T_p = \frac{D(m)}{V_p(m/s)}$$

La fibra óptica aumenta el ancho de banda para poder transmitir más
bits a la vez, y por tanto aumenta la velocidad de las transmisiones.

Además de estos tiempos, cuando un nodo recibe un paquete y lo procesa
para reenviarlo se producen **retardos de procesamiento**, cuando el
nodo recibe dicho paquete y procesa su información, y **retardos de
cola**, cuando el mensaje espera en una cola a que todos los mensajes
que llegaron antes que él sean enviados.


**Servicios:**
- **Orientado a conexión (OC)**. Tales como TCP, su objetivo es evitar
  las pérdidas. Se abre una conexión, haciendo que cliente y servidor
  intercambien información de control antes de que se produzca el
  intercambio a nivel de aplicación. Se usa en aplicaciones donde se
  quieran evitar las pérdidas de información.
  
- **No orientado a conexion (NOC)**. Tales como UDP,  proporciona unos
  servicios básicos, sin establecer una conexión previa, por lo que no
  garantiza la correcta entrega del mensaje. Se usa en aplicaciones de
  tiempo real.

- **Confirmado (fiable)**. Pueden garantizar la entrega sin errores y
  en el orden correcto.
  
- **No confirmado (no fiable)**. No garantizan la entrega correcta del
  mensaje.
  

## Internet: Arquitectura y Direccionamiento

Existe una jerarquía en el sistema por el cual nos conectamos a
internet:

* Intranets: Wifi, ethernet

* Redes de acceso del ISP (Internet Service Provider): Cable
  telefónico (xDLS,ISDN), cable (coax), HFC(Hybrid Fiber Coax),
  FTTH(Fiber-To-The-Home)

* Redes troncales (ATM, SDH, SONET, etc) de grandes operadores de
  telecomunicaciones.
  
  
**Tipos de Tier de ISP:**

* Tier 1: Los grandes proovedores hacen acuerdos de peering para no
  cobrarse los servicios mutuamente, sino que solo gestionan el
  tráfico entre ellos. Sólo tienen acuerdos de peering. Se conocen
  como redes troncales y tienen cobertura internacional.

* Tier 2: Tienen tanto acuerdos de peering como pagos a otros ISP más
  grandes que puedan gestionar su tráfico. Tienen cobertura
  regional/nacional.

* Tier 3: Pagan por los acuerdos de tránsito. No pueden permitirse
 negociar para hacer peering. Necesitan conectarse a internet a través
 de un ISP de tier 2.
 
 Para conocer cómo se dirigen las conexiones y cómo se identifican los
 destinatarios se usan diversos elementos. La dirección IP, en la capa
 de red, identifica a los hosts. Para los humanos es más sencillo usar
 los nombres de dominio, los cuales traducen las direcciones del
 lenguaje humano a su correspondiente dirección IP.
 
 <!-- 27/09/2017 -->

\newpage

# Tema 2. Servicios y Protocolos de Aplicación en Internet

## Introducción a las aplicaciones web

Todas las aplicaciones finales que usamos se basan en otros
protocolos. Los protocolos que use la aplicación estarán basados en
otros de transporte como UDP y TCP y estos a su vez en protocolos de
internet como IP, que son los que finalmente tienen acceso a la red.

Generalmente se comportan de acuerdo a una estructura
cliente-servidor. En este existe un host que siempre está activo, el
servidor, con IP permanente y pública. Y este presta un servicio a las
solicitudos de muchos otros hosts, que son sus clientes, con IP que
puede ser dinámica y privada y no se comunican entre sí. Estos, por su
parte, pueden estar activos de manera permanente o intermitente.

Los servidores se agrupan en granjas o clústeres, también denominados
centros de datos, que permiten dar soporte a todas las peticiones de
sus clientes sin ser desbordados.

### **Sockets**

La comunicación entre diversos hosts se realiza mediante el uso de
sockets por parte de los procesos que se encuentran tanto en servidor
como en el cliente.

El proceso cliente es el que inicia la comunicación, mientras que el
proceso servidor es el que espera a ser contactado. Es por esto por lo
que necesita tener una IP permanente y pública. Además, para recibir
mensajes, un proceso debe tener un identificador, compuesto por una IP
y un puerto.

**Retardo en cola:**

Para estimar los retardos en cola se usa teoría de colas. El retardo
en cola es:

$$ R = \frac{\lambda (T_s)^2}{1-\lambda T_s} $$

Donde $T_s$ es el tiempo de servicio y $\lambda$ es el ratio de
llegada de solicitudes.

Esta expresión se puede usar para calcular el retardo en cola en un
router.

### Protocolo de aplicación

Una pregunta que nos podemos hacer es qué define a un protocolo. Y
estos están definidos por el tipo de servicio que sea, el tipo de
mensaje que emita, su sintaxis, semántica y sus reglas.

**Tipos:**

* Protocolos de dominio público: Definidos en RFCs (Request For
  Comments). Son tales como HTTP, SMTP, FTP, IP...
  
* Protocolos propietarios. Tales como skype.

* In-band vs out-of-band: Cuando la gestión se realiza por la misma
  vía que la comunicación frente a cuando se usa una vía paralela.
  
* Stateful vs stateless: Cuando la aplicación guarda información sobre
  todo lo que ocurrió desde el inicio de la misma frente a cuando no
  la guarda.
  
* Persistentes vs no persistentes: Cuando el protocolo hace uso de
  conexiones persistentes frente a cuando no.
  
Normalmente los protocolos son flexibles, tendiendo a tener una
cabecera fija y una serie de "trozos" que pueden ser opcionales u
obligatorios. Estos trozos, a su vez, pueden incluir alguna cabecera
específica más una serie de datos en forma de parámetros que pueden
ser fijos o de longitud variable, con formato TLV (Un campo para el
tipo de parámetro, otro para la longitud y otro para el valor del
parámetro. Es destacable que *todos los parámetros comienzan en
múltiplos de 4 bytes*, pudiendo requerir relleno.

<!-- 4/09/2017-->
Una aplicación debe de tener unos requisitos, entre estos se encuentran:

* **Pérdidas de datos:** Algunas aplicaciones pueden tolerar las
  pérdidas de datos, tales como streamings de audio/vídeo, pero otras
  deben de asegurar la fiabilidad de la transferencia (transferencia de
  archivos).
  
* **Requisitos temporales:** También pueden necesitar el mínimo 
  retraso (delay) para ser efectivos. Un streaming también tiene el
  requisito de que este retraso no sea excesivo, o en los videojuegos
  es necesaria esa sincronización para evitar el lag.
  
* **Rendimiento(Throughput):** Algunas apps también necesitan un ritmo
  determinado de envío de datos.
  
* **Seguridad:** La encriptación, autenticación y no repudio (no
  puedes negar ser el remitente de un envío de datos) son
  factores importantes en las aplicaciones.
  

## Protocolos de transporte

En la capa de transporte existen diversos protocolos:

* **Servicio TCP:** Está orientado a conexión (establecer una conexión
  entre los dos involucrados previo al envío), este transporte es
  fiable ante pérdidas, con control de flujo y de congestión.
  
* **Servicio UDP:** No está orientado a conexión, es decir, no se
  comprueba que ambos estén preparados para realizar la
  comunicación. A su vez carece de todas las propiedades que acabamos
  de destacar sobre TCP.
  
Estos al ser usuarios del protocolo IP (Capa de red) no garantizan el
retardo acotado, las fluctuaciones acotadas, el mínimo throughput y la
seguridad requerida.

## Servicios de Nombres de Dominio (DNS)

<!-- Polling, explicado haciendo uso de un peluche de angry birds -->

Es un servicio (implementado en un servidor) que se encarga de
traducir los nombres a direcciones IP.

Tiene una estructura jerárquica en dominios:

ParteLocal.dominioNivelN.(...).dominioNivel1

Donde Nivel1 es el dominio genérico.

La ICANN se encarga de delegar los nombres y números asignados.

Cuando un host quiere solicitar una determinada página a un servidor
web introduce el nombre de dicho servidor web. El navegador extrae el
nombre del host a partir del URL y lo pasa a la aplicación DNS. El
cliente DNS envía una consulta con dicho nombre al servidor DNS, tras
procesarlo, el servidor le responde con una dirección IP
correspondiente, mediante el cual ya puede iniciar la conexión.

<!-- 11/10/2017 -->

### Resolución distribuida

El ordenador original no resuelve todo el nombre del dominio. Este
conecta con un servidor que será el encargado de conectar
iterativamente con el resto de servidores. 

De esta manera nos conectaríamos con los servidores “.”, los de
dominio (Top-Level Domain, TDL), servidores Locales y servidores
Autorizados y Zona.

Un host solicitaría la dirección de una URL (www.una.direccion.com) a su
servidor local. Este envía la petición a un servidor raíz, el cual
toma el sufijo (.com) y le responde al DNS local una lista de
direcciones responsables de dicho dominio (los responsables del
sufijo .com). Estos responsables son servidores TLD (top level
domain) y a continuación se les envía una petición a estos. El
servidor TLD examina el sufijo (direccion.com) y responde con la
dirección del servidor DNS autorizado que puede dar la dirección del
URL inicial. Después el servidor local consulta a dicho servidor DNS
autorizado y este le responde con la dirección IP de la URL inicial
(www.una.direccion.com).

**Gestión de la base de datos DNS:**

Como hemos comprobado la base de datos está distribuida. Esto implica
que cada zona debe tener al menos un servidor de autoridad, y en cada
zona habrá servidores primarios y secundarios, que bien tendran la
copia master de la base de datos o bien la obtendrán a partir de los
primarios. Existen sólo 13 servidores raíz. Existe un servicio de
caché que permite agilizar las consultas.

Los servidores pueden dar una **respuesta con autoridad**, si tiene
autoridad sobre la zona en la que se encuentra el nombre solicitado y
devuelve la dirección IP. Una **respuesta sin autoridad**, cuando no
tienen autoridad pero tienen la respuesta en caché. O bien puede no
conocer la respuesta, lo que implicaría una consulta a un servidor superior.

## La navegación Web

El protocolo de transferencia de hipertexto, **HTTP**, es el protocolo
de la capa de aplicación de la web y se encuentra en el corazón de la
Web. Este se implementa en dos programas, un cliente y un servidor.

Una página web es un fichero (HTML) formado por objetos, que pueden
ser ficheros HTML, imágenes, applets y demás tipos de archivos. Cada
objeto se direcciona con una URL. La mayoría de las páginas web tienen
un archivo base  HTML donde se referencian los objetos que están
contenidos en esa web. Tiene su **puerto bien definido**, el 80. 

El protocolo HTTP sigue un modelo cliente-servidor. El cliente es el
que pide, recibe y muestra objetos web mediante el browser. El
servidor por su parte es el que envía los objetos web en respuesta a
peticiones. 

### Características HTTP

**TCP al puerto 80:** Inicio de conexión TCP, envío HTTP, cierre de
conexión TCP.

**HTTP es “stateless” $\rightarrow$ Cookies:** El servidor no mantiene
la informacióń sobre las peticiones de los clientes. Esto puede
implicar, por ejemplo, que cuando recibe dos peticiones idénticas del
mismo cliente devuelve el objeto solicitado en lugar de devolver
ningún tipo de error o mensaje informativo.

La conexión puede ser persistente o no persistente. En el primer caso
se pueden enviar múltiples objetos sobre una única conexión TCP entre
cliente y servidor, mientras que la no persistente crea nueva conexión
para cada objeto a enviar.

El persistente tiene un tiempo de transmisión total menor que el no
persistente. Pero el no persistente permite gestionar mejor los
recursos del servidor, pues no tiene que mantener el socket abierto
durante toda la conexión, a cambio, al tener que establecer una
conexión por objeto reduce su velocidad.

Hay dos tipos de mensajes HTTP: request y reponse. La petición de un
elemento y su concesión. Cada uno de ellos tiene un formato
específico, donde se indica la información concreta que se desea
solicitar, o, en caso de ser desarrolladores de la página, mensajes de
gestión. (GET,POST,HEAD,PUT,DELETE)
Las respuestas se asemejan a las peticiones en cuanto a la
indicación de un estado y cabecera, pero adicionalmente poseen el
cuerpo de la entidad, donde se encuentra el objeto solicitado. La
línea de estado indica un código de respuesta (200 OK, 301 moved
permanently, 400 bad request, 505 HTTP version not supported).


**Caché:** Cuando se solicitan numerosas veces algo a un servidor es
conveniente configurar un proxy intermedio que almacene dicha
solicitud. De ese modo quien solicite ese mismo servicio no
involucrará al servidor original sino al proxy, este sólo le envía una
petición al servidor original para saber si es necesario actualizar la
caché o no. Si no dispone de esta solicitud tendrá que inevitablemente
solicitar al servidor original.

## Correo Electrónico

### Proceso de envío de correo electrónico
En el correo electrónico intervienen dos clientes que enviarán y
recibirán el correo cuando ellos decidan. El procedimiento es el
siguiente:

El usuario de origen utiliza su user agent para mandar el correo a su
servidor de correo, se envía mediante SMTP o HTTP. Una vez hecho esto,
el servidor del que envía el mensaje crea una conexión TCP con el
servidor de correo del usuario destinatario y lo envía, este envío sí
que se produce con SMTP. El servidor de destino almacena el mensaje en
la bandeja de entrada del usuario destino. El destinatario usará su
agente de usuario arbitrariamente para leer el mensaje utilizando
POP3 (Post Office Protocol), IMAP (Internet Mail Access Protocol) o
HTTP. 

(La diferencia entre POP3 e IMAP es que POP3 no puede ser usado por
usuarios "nómadas" que quieran acceder desde distintos hosts a su
correo, pues una vez recibidos los archivos, estos sólo permanecen en
el receptor. IMAP permite mantener una carpeta bandeja de entrada en
un servidor IMAP, donde puede gestionar ahí su correo.

## Protocolos Seguros

Hay unos aspectos destacables de la seguridad en internet:

* **Confidencialidad:** Sólo quien esté autorizado a acceder a la
  información puede hacerlo.
  
* **Responsabilidad:** Autenticación, confirmar que los agentes de
  comunicación son quien dicen ser. No repudio: No se pueda negar la
  autoría de una acción y que la acción se ha hecho. Control de
  accesos: Garantía de identidad para el acceso.
  
* **Integridad:** La información no debe ser manipulada.

* **Disponibilidad:** El acceso a los servicios debe estar regulado
  para evitar, por ejemplo, ataques Denial Of Service.
  
También hay un conjunto de mecanismos de seguridad:

* **Cifrado Simétrico:** Utilizamos la misma clave para
  cifrar/descifrar (DES,3DES,AES,RC4).
  
* **Cifrado Asimétrico:** Una clave distinta para encriptar y para
  desencriptar (Diffie & Hellman, RSA).
  
  Se pueden combinar simétrico y asimétrico, le envío al receptor la
  clave del cifrado simétrico cifrada con su clave pública, él la
  desencriptará con su privada y así compartiremos la clave simétrica
  para aumentar la velocidad de las comunicaciones.
  
* **Message Authentication Code:** Hay que impedir que se modifiquen
  los archivos de envío durante el mismo. El mensaje se pasa por un
  hash con una clave que no se puede modificar sin tener la misma
  clave.  (MD5, SHA-1...)
  
* **Firma Digital:** Se pasa por un hash el mensaje, se cifra con la
  clave privada y cualquier receptor puede descifrar la firma y
  comparar el resultado con el hash del mensaje recibido, si coinciden
  se garantizaría la autoría.

* **Certificado:** Para asegurar que las claves pertenecen al
  remitente existen terceros que lo garantizan. Estos terceros son
  confiados y aceptados “públicamente”.

<!-- 8/11/2017 -->

Diversos **ejemplos** de seguridad criptográfica en distintas capas son:

* **Capa de aplicación:** PGP (Pretty Good Privacy), SSH (Secure Shell).

* **Capa de sesión** (entre aplicación y transporte): SSL (Secure
  Socket Layer) ,TSL (Transport Secure Layer).

* **Capa de Red:** IPSec(VPN)

También existen elementos de seguridad Perimetral y Gestión de
riesgos:

* **Firewall:** Sistema que limita los accesos a los elementos de
  nuestro ordenador. También UTMs se están implementando últimamente,
  son una extensión de firewalls.
  
* **Sistemas de detección de intrusiones** IDS. En red NIDS, en host
  HIDS. Analizan el tráfico y detectan posibles ataques y anomalías.
  
* **Antivirus, evaluación de vulnerabilidades, seguridad en
  Aplicaciones**, filtrado web/anti-spam.
  
* **Advanced Thread Detection:** Listas donde se comparte información
  de seguridad. Así se permite enterarse cuanto antes de problemas de
  seguridad.
  
* **SEMs, SIEMs:** Realiza el conjunto de las funcionalidades
  anteriores.
  

## Aplicaciones multimedia

Son de audio y vídeo. Se procura mejorar la calidad del servicio.

Las aplicaciones de flujo almacenado no dan problemas de delay, sin
embargo el throughput es más exigente. Sin embargo el delay en
emisores en directo no pueden tener delay, su velocidad de transmisión
sigue siendo igual de importante.

Todas estas requieren un alto ancho de banda. Son tolerantes a
pérdidas de datos. Tienen un Delay y un Jitter(variabilidad del delay)
acotado. Hacen uso además de multicast (Ejemplo YouTube en las diapositivas).


## Aplicaciones para la interconectividad de redes locales

* **DHCP:** Nos permite configurar dinámicamente direcciones IP

* **DynDNS, No-IP:** Servicios en la red privda con IP pública
  variable, configuración de acceso necesaria
  
* **UPnP:** "Pervasive adhoc com". Comunicación dispositivo <-> NAT

# Tema 3.
<!-- Falta el comienzo del tema 3 -->

<!-- 15/11/2017 -->
## Protocolo Transmission Control Protocol (TCP)

Es un servicio orientado a conexión, con una entrega ordenada
garantizada. Es además full-duplex-

Tiene un sistema de control de flujo de detección y recuperación de
errores. Esto se implementa mediante temporizadores que esperan la
confirmación de la recepción. Si se agota el tiempo se reenvía el
paquete.

Utiliza “piggybacking”, es decir, en un paquete que va en un sentido
de la comunicación se añade información sobre el otro sentido de la
comunicación. Así aprovechan los paquetes de datos para incluir
información de control.


Tiene tanto “timeouts” como ventanas adaptables.

Además es fiable en el control de congestión y flujo.

La información a enviar por TCP se divide en segmentos TCP. 
Cada uno de esos segmentos contiene información de origen y
destino. Además contiene información relativa a sí misma y a la
posición que ocupa esta información respecto al total. Incluye la
longitud de la cabecera del paquete, junto a un espacio reservado para
usos concretos, flags, bytes acerca del control de flujo, comprobación
(checksum) y un puntero a datos que hay que incluir de manera urgente,
por ejemplo datos de control para subir a la capa de aplicación. (Un
ejemplo de esto es un servicio streaming al que se le solicita cambiar
la compresión del vídeo debe de procesarse antes que el resto de los
datos sobre el vídeo que se deben de procesar “más tranquilamente”)

**Control de la conexión:**

El intercambio de información tiene tres fases (three-way handshake).

1. Establecimiento de la conexión (sincronizar el número de secuencia
   y reservar recursos).
   
2. Intercambio de datos (full-duplex).

3. Cierre de la conexión (liberar recursos).


Se abre activamente el cliente y pasivamente el servidor. En esto se
ven involucrados diversos bits y campos de control.

En primer lugar el cliente solicita la conexión. Luego el servidor
recibe la petición y envía la confirmación al cliente, una vez hecho
este el servidor espera una respuesta del cliente para que confirme la
recepción de este paquete. Entonces el cliente le confirma al servidor
que sabe que este está listo para la comunicación. Finalmente comienza
la comunicación.

Para cerrar la conexión se realiza un proceso similar.

**Otros detalles:**
Los campos del control de conexión tienen 32 bits, osea $2^{32}$ valores.

La inicialización se inicia por el ISN, que es elegido por el
Los campos del control de conexión tienen 32 bits, osea $2^{32}$ valores. 
El sistema lo elige, y el estándar sugiere utilizar un contador entero
incrementado en uno por cada 4 microsegundos. Esto protege de
coincidencias, pero no de sabotajes.

El incremento se realiza según los bytes de carga útil (payload). Los
flags SYN y FIN incrementan en 1 el número de secuencia.

**Ejercicio:**
Se desea transferir con protocolo TCP un archivo de L bytes usando un
MSS(maximun segment size) de 536.

a) ¿Cuál es el valor máximo de L tal que los números de secuencia de
TCP no se agoten?

b) Considerando una velocidad de transmisión de 155 Mbps y un total de
66 bytes para las cabeceras de las capas de transporte, red y enlace
de datos, e ignorando las limitaciones debidas al control de flujo y
congestión, calcule el tiempo qeu se tarda en transmitir el archivo en
A.


a) Un segmento tiene $2^{32}$ valores distintos, pero luego tiene dos
bits del SYN y del FIN. En total el valor máximo de L para que no
desborde es $2^{32}-2$.

b) Dados los bytes de las cabeceras buscamos obtener el total y luego
dividimos por la velocidad de transmisión.

Dividimos L entre el tamaño del segmento para saber cuántos segmentos
tenemos. Cogemos el entero inmediatamente superior de
$\frac{2^{32}-2}{536}$ para calcular el número de segmentos. Luego
calculamos el número de bytes así: N_segmentos * 66 + L

Conocidos el número de bytes a transmitir simplemente tenemos que
dividir por la velocidad $\frac{(N_{segmentos}\cdot 66 + L)\cdot 8}{155\cdot 10^6bps}$


<!--22/11/2017 -->

**Control de errores y de flujo**:

El control de flujo regula ámbitos como la velocidad de transmisión,
mientras que el de errores comprueba si se transmiten correctamente
los paquetes.

Después del three-way handshake comienza la conexión. Un paquete (con
su retardo de transmisión al ser enviado) se transmite al receptor
(que lo recibe en un tiempo de propagación. El receptor le responde
con un paquete minúsculo que sólo tendría un breve retardo de
propagación (paquete con las cabeceras).

Por tanto el tiempo total sería igual a dos veces el retardo de
propagación, más el tiempo de tranmisión del paquete inicial, más el
tiempo de transmisión del paquete de respuesta (tiempo despreciable),
más el tiempo de recepción de este último (también despreciable).

En la práctica se realizan envíos en ventanas de varios paquetes como
este. Es una **ventana deslizante** pues cada vez que recibe la
confirmación de la recepción de un paquete permite enviar uno nuevo,
manejando así un número fijo de paquetes. 

El control de errores estudia el checksum de los paquetes, si el
receptor deshecha un paquete, el emisor, tras acabar el temporizador
del paquete sin haber recibido una confirmación, reenvía el paquete.

**ACK**: Siguiente byte que está esperando el receptor.

Existen una serie de reglas para cuando se produzcan diversos errores.
<!-- En las diapositivas aparecen -->

Los timeouts del control de errores varían dinámicamente. Esto se
realiza mediante una estimación de la situación en la red. 

En primer lugar el timeout tiene que ser mayor que el tiempo de ida y
vuelta (RTT). Al menos dos veces el tiempo de transmisión. Luego hay
que controlas si es demasiado corto, pues produciría timeouts
prematuros, o demasiado largo, pues generaría grandes esperas
innecesarias.

$$x_t = x_{t-1}\alpha + y_t(1-\alpha) ; \alpha \in [0,1)$$

<!-- 29/11/2017 -->

**Control de flujo:** 
Procedimiento para evitar que el emisor sature al receptor.

Es un esquema crediticio el receptor avisa al emisor de lo que puede
aceptar.

Se utiliza el campo ventana (WINDOW) en el segmento TCP para
establecer la ventana ofertada.

El receptor responde al emisor con el número de bytes que tiene libre
en su ventana, si este le responde con 0 es que no puede recibir más
paquetes. El emisor tiene que esperar a recibir un nuevo paquete con
el mismo ack pero con un valor de window mayor que cero.

**Control de congestión:**

En el emisor se usa una ventana y un umbral, inicialmente VCongestion
= MaximumSegmentSize, y Umbral es un valor arbitrario inicializado por
el emisor y ambos son regulados cuando se produce algún timeout.

Si VCongestion < Umbral, por cada ACK recibido (crece
exponencialmente, por cada ack que se reciba, si llegan dos, de la
ventana se liberan dos y se aumenta en dos más) 

<!--13/12/2017  FALTA LO DE ESTE DÍA-->

# Tema 4. Redes Conmutadas e Internet

<!-- 20/12/2018 -->

## Conmutación

La conmutación explica cómo se realiza el transporte de la
comunicación en los nodos intermedios. 

Cuando iniciamos una comunicación en una llamada telefónica tendremos
un ancho de banda, un recurso, reservado para nosotros, sin que pueda
ser utilizado por nadie más. (Conmutación de circuitos)

Este es el concepto de recurso dedicado.

Al hacer la conmutación de paquetes, como un ordenador con múltiples
procesos que se conectan a internet, se permite la división en
paquetes de manera que los distintos procesos pueden hacer uso de
internet de manera concurrente.

Este, por su parte, es el concepto de recurso compartido. Su
desventaja es que produce un mayor delay.

## Asociación con Capa de enlace: El protocolo ARP.

ARP permite traducir direcciones que tienen sentido en internet a
direcciones que tienen sentido sobre la tecnología que estemos usando.

**Direcciones MAC:** Son direcciones usadas en redes wifi y ethernet,
que, tras la redirección IP se envía a la MAC del siguiente nodo. Su
formato consta de seis pares de cifras hexadecimales. Son únicas,
asignadas en lotes de $2^{24}$ por IEEE.

Su dirección de broadcast es la FF-FF-FF-FF-FF-FF

## El protocolo ICMP

Es un protocolo de control, se usa para situaciones de error, tales
como que una dirección esté caída. Se envía hacia el origen, es decir,
si el paquete recibe algún error en su transmisión se encarga de que
sea devuelto un mensaje de error al transmisor original.




