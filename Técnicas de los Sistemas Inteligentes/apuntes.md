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

Existen problemas, como el de las ocho reinas, que pueden ser
planteados de diversas maneras, bien como un problema de búsqueda en
el que sucesivamente se coloquen reinas de manera que nos aproximemos
a la solución o bien como la colocación aleatoria de las 8 reinas en
el tablero y comprobar si la solución es correcta, comprobar qué
candidatas están fallando, recolocarla y repetir este proceso hasta
conseguirlo. Este es el concepto de heurística. En la heurística de
búsqueda tendremos que establecer una función para valorar la mejor
opción, en este caso cogeríamos la opción que más casillas libres deje
a las siguientes reinas.

### Búsqueda primero el mejor

__El algoritmo A*__ es el método de búsqueda por el mejor nodo más
conocido. Este consiste en la exploración de caminos descartando los
que no son interesantes por su costo. Se utiliza una función de
evaluación la cual se obtiene como la suma de otras dos funciones, la
función coste actual y la función coste estimado del nodo al objetivo.

<!-- Completar tras investigar acerca de la búsqueda y el algoritmo -->
<!-- estrella -->

* **Modelos más generales:** 

  **Algoritmos de agendas**. Una agenda es una
  lista de tareas, y una tarea tiene un objeto a desempeñar, una
  justificación (por qué se está realizando la tarea) y una valoración
  heurística. Se diferencia del A estrella en que cuando dos nodos
  generan el mismo hijo se guarda el enlace al mejor padre, mientras
  que este es más general. <!--El porqué no lo tengo claro--> 
  
* **Modelos con poda:** 

**Búsqueda dirigida**. Es un algoritmo A estrella donde se produce una
poda a priori, es decir que cuando se generan los nodos, solo se
generan un número reducido de nodos.

**Algoritmo A estrella con memoria acotada**. Es un algoritmo con poda
a posteriori (hay varios tipos), básicamente es el establecimiento de
un límite para una determinada estructura, por ejemplo a *ABIERTOS* se
le limita el tamaño, sacando a los nodos menos favorables cuando la
memoria se llene.







