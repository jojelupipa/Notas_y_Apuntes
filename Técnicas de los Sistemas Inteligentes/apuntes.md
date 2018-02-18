---
title: Técnicas de los Sistemas Inteligentes						# Título
author: Jesús Sánchez de Lechina Tejada		# Nombre del autor
header-includes:      	 	        	# Incluir paquetes en LaTeX
toc: true                   			# Índice
numbersections: false       			# Numeración de secciones
fontsize: 11pt              			# Tamaño de fuente
geometry: margin=1in        			# Tamaño de los márgenes
---
<!--
A.Gonzalez@decsai.ugr.es
 
 Las prácticas empiezan la semana del 19

Notas para prácticas:

Usaremos ROS: Robot Operating System (recomendable usar nuestros
portátiles )

-->
\newpage

# Tema 1. Sistemas Inteligentes y Búsqueda

<!-- Referencia: Nilsson y Russel -->

Un sistema inteligente se puede definir como el producto del
desarrollo de una inteligencia artificial, que es capaz de interactuar
con su entorno de manera "más inteligente" que otros sistemas.

**Búsqueda:**

La búsqueda está basada en el uso de algoritmos (aparte del algoritmo
A* para la selección del mejor nodo), esta tendrá un conjunto de
propiedades y heurísticas que desarrollaremos posteriormente. La
búsqueda a su vez es parte esencial de áreas como la robótica, la
planificación y la satisfacción de restricciones.

## Búsqueda heurística y propiedades

### Heurística y tipos de problemas

Las heurísticas son criterios, métodos o principios para decidir cuál
de entre varias acciones promete ser mejor para alcanzar una
determinada meta.



