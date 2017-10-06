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
**Presentación**

*) Viernes a partir de las 9:00 se abren las plazas de los grupos de
prácticas 

*) Empezamos los seminarios en dos semanas (La semana del 25)

*) El sistema de evaluación está en la página dtstc.ugr.es/it


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




  
  
