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
  
