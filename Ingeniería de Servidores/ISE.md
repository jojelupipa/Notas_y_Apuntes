---
title: Ingeniería de Servidores						# Título
author: Jesús Sánchez de Lechina Tejada		# Nombre del autor
header-includes:      	 	        	# Incluir paquetes en LaTeX
toc: true                   			# Índice
numbersections: false       			# Numeración de secciones
fontsize: 11pt              			# Tamaño de fuente
geometry: margin=1in        			# Tamaño de los márgenes
---

\newpage
**Presentación**
Información asignatura:

Profesor: Héctor Pomares
Correo: hector@ugr.es

Tutorías: M,X: 12:30-15:30 CITIC, CB-3

Puntuación:

Teoría 60%: Examen de Teoría.                Mínimo 5/10 

Prácticas 40% Defensa/entrega de prácticas.  Mínimo 5/10 


# Tema 1. Introducción a la Ingeniería de Servidores

Un **servidor** es un sistema informático, un conjunto de hardware,
software y humanware que permiten trabajar con información.

## Clasificación de Sistemas Informáticos y Servidores
Estos sistemas se pueden clasificar de diversas maneras, según su
arquitectura, uso general o específico...

Respecto a arquitectura de servicio se pueden clasificar en Sistema
aislado, arquitectura cliente-servidor, de n capas o
Cliente-Cola-Cliente.

### Arquitectura de n capas

Es una arquitectura cliente-servidor que tiene n tipos de nodos, lo
que permite distribuir la carga y aumentar la escalabalidad (capacidad
de crecer para dar soporte a más clientes). Por el contrario supone
más carga en la red, así como una mayor complejidad para programarlo y
administrarlo.

### Arquitectura Cliente-Cola-Cliente

Habilita a todos los clientes para desempeñar tareas semejantes
interactuando cooperativamente para realizar una actividad
distribuida, mientras que el servidor actua como una cola que captura
todas las peticiones y sincroniza el funcionamiento del sistema.

Ejemplos de esto son torrent o skype.

## Requisitos y evaluación de servidores

### **Prestaciones:**
Cuantificar la velocidad con la que se realiza una determinada carga
de trabajo. 

**Tiempo de Respuesta(R):** También conocida como latencia, es el
tiempo total entre que se solicita una tarea al servidor y se finaliza
la misma. Este aumenta con el número de trabajos recibidos.

**Productividad(X):** Llamada también ancho de banda, es la cantidad de
trabajo realizado por el servidor o por un componente del mismo por
unidad de tiempo. Es destacable que la productividad se incrementa con
el número de trabajos recibidos, si no hay trabajos la productividad
es 0. 


**Disponibilidad:** Se regula mediante los períodos en los que el
servidor está activo o inactivo, y cuando está inactivo si ese período
de inactividad es planificado o no planificado.

Esta es mejorable mediante sistemas de alimentación que resistan
caídas de luz o usar configuraciones RAID ("copias de seguridad").
<!--Completar información sobre RAID-->

**Seguridad:** Todo servidor debe de ser seguro ante incursión de
individuos no autorizados, corrupción o alteración de datos y las
posibles interferencias que impidan el acceso a los recursos. Para
ello usa medidas como la autenticación de usuarios, encriptación de
datos, cortaguegos y mecanismos de prevención de ataques DOS.

**Extensibilidad-expansibilidad:**

**Escalabilidad:**

