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

*) Viernes a partir de las 9:00 se abren las plazas de los grupos de
prácticas 

*) Empezamos los seminarios en dos semanas (La semana del 25)

*) El sistema de evaluación está en la página dtstc.ugr.es/it
-->

# Tema 1.

Para enviar un mensaje en una red se utilizan diversas capas para
transformar el mensaje hasta un nivel transferible mediante
telecomunicaciones (paquetes de bits), el receptor deberá de usar las
mismas capas y protocolos para poder obtener el mensaje original.

Hoy en día se utiliza un control distribuido de la red.

## Capas y funcionalidades 

### **Modelo OSI**
Grupo 1 de capas:
- Capa Física: Conexión ordenador-punto de acceso

- Capa de Enlace: Permite la comunicación entre dos nodos directamente
  conectados. Delimita la trama y controla errores y flujo.
  
Grupo 2:
- Capa de Red: Nos ofrece la capacidad de enrutamiento que permite
  llevar a los paquetes desde un origen a un destino. Controla además
  la congestión, evitando el envío de paquetes a nodos congestionados

Abstrae toda la comunicación para que únicamente se preocupe del nodo
inicial y nodo destino como si estuvieran conectados directamente.

Grupo 3:


- Capa de Transporte: Control de errores y control de flujo

- Capa de Sesión: Controla el "turno de palabra", decide quién y en
  qué momento envía información.

- Capa de Presentación: Permite una traducción desde el "idioma de la
  red" al "idioma de la aplicación"
  
- Capa de Aplicación: Son las aplicaciones finales, que tienen su
  propio protocolo (http, por ejemplo)
  
  
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

<!--Revisar estos conceptos-->
El tiempo de transmisión es lo que tardan los bits en emitirse.
$$T_t = \frac{P(b)}{V_t (bps)}$$

El tiempo de propagación es el tiempo que el primer bit tarda hasta
llegar a su destino. Se mide en metros por segundo.
$$T_p = \frac{D(m)}{V_p(m/s)}$$

La fibra óptica aumenta el ancho de banda para poder transmitir más
bits a la vez, y por tanto aumenta la velocidad de las transmisiones.

<!--Falta completar Tipos de servicios-->

**Servicios:**
- Orientado a conexión (OC).
- No orientado a conexion (NOC).
- Confirmado (fiable).
- No confirmado (no fiable).

## Internet: Arquitectura y Direccionamiento

Existe una jerarquía en el sistema por el cual nos conectamos a
internet:

* Intranets: Wifi, ethernet

* Redes de acceso del ISP (internet service provider): Cable
  telefónico (xDLS,ISDN), cable (coax), HFC(Hybrid fiber coax),
  FTTH(fiber-to-the-home)

* Redes troncales ATM, SDH, SONET, etc) de grandes operadores de
  telecomunicaciones.
  
  
**Tipos de Tier de ISP:**

* Tier 1: Los grandes proovedores hacen acuerdos de peering para no
  cobrarse los servicios mutuamente, sino que solo gestionan el
  tráfico entre ellos. Sólo tienen acuerdos de peering.

* Tier 2: Tienen tanto acuerdos de peering como pagos a otros ISP más
  grandes que puedan gestionar su tráfico.

* Tier 3: Pagan por los acuerdos de tránsito. No pueden permitirse
 negociar para hacer peering.
 
 <!-- Falta todo lo del día 27/09/2017
	 -Diseños de protocolos
	 
	 (Hasta la quinta página de las diapositivas)
 -->

# Tema 2. Servicios y Protocolos de Aplicación en Internet
  

<!-- 4/09/2017-->
Una aplicación debe de tener unos requisitos, entre estos se encuentran:

* **Pérdidas de datos:** Algunas aplicaciones pueden tolerar las
  pérdidas de datos, tales como streamings de audio/vídeo, pero otras
  deben de asegurar la fiabilidad de la trasferencia (transferencia de
  archivos).
  
* **Requisitos temporales:** También pueden necesitar un pequeño
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
  
Estos al ser usuarios del protocoolo IP (Capa de red) no garantizan el
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

<!--Falta completar lo último de esta clase-->


<!-- 11/10/2017 -->

### Resolución distribuida

El ordenador original no resuelve todo el nombre del dominio. Este
conecta con un servidor que será el encargado de conectar
iterativamente con el resto de servidores. 

De esta manera nos conectaríamos con los servidores “.”, los de
dominio (Top-Level Domain, TDL), servidores Locales y servidores
Autorizados y Zona.


## La navegación Web

Una página web es un fichero (HTML) formado por objetos dicheros HTML,
imágenes, applets y demás tipos de archivo. Cada objeto se direcciona
con una URL. Que tiene su **puerto bien definido**, el 80.

El protocolo HTTP sigue un modelo cliente-servidor. El cliente es el
que pide, recibe y muestra objetos web mediante el browser. El
servidor por su parte es el que envía los objetos web en respuesta a
peticiones. 

### Características HTTP

**TCP al puerto 80:** Inicio de conexión TCP, envío HTTP, cierre de
conexión TCP.

**HTTP es “stateless” $\rightarrow$ Cookies:** El servidor no mantiene
la informacióń sobre las peticiones de los clientes. 

La conexión puede ser persistente o no persistente. En el primer caso
se pueden enviar múltiples objetos sobre una única conexión TCP entre
cliente y servidor, mientras que la no persistente crea nueva conexión
para cada objeto a enviar.

El persistente tiene un tiempo de transmisión total menor que el no
persistente. Pero el no persistente permite gestionar mejor los
recursos del servidor, pues no tiene que mantener el socket abierto
durante toda la conexión.

Hay dos tipos de mensajes HTTP: request y reponse. La petición de un
elemento y su concesión.


**Caché:** Cuando se solicitan numerosas veces algo a un servidor es
conveniente configurar un proxy intermedio que almacene dicha
solicitud. De ese modo quien solicite ese mismo servicio no
involucrará al servidor original sino al proxy, este sólo le envía una
petición al servidor original para saber si es necesario actualizar la
caché o no.

## Correo Electrónico

### Proceso de envío de correo electrónico
En el correo electrónico intervienen dos clientes que enviarán y
recibirán el correo cuando ellos decidan. El procedimiento es el
siguiente:

El usuario de origen utiliza su user agent para mandar el correo a su
servidor de correo, se envía mediante SMTP o HTTP. Una vez hecho esto,
el servidor del que envía el mensaje crea una conexión TCP con el
servidor de correo del usuario destinatario y lo envía. El servidor de
destino almacena el mensaje en la bandeja de entrada del usuario
destino. El destinatario usará su agente de usuario arbitrariamente
para leer el mensaje utilizando POP3, IMAP o HTTP.

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
