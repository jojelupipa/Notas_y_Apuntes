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
- Capa Física: Ordenador-punto de acceso

- Capa de Enlace: Permite la comunicación entre dos nodos directamente
  conectados.
  
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

- Capa de Aplicación
