---
title: Informática Gráfica						# Título
author: Jesús Sánchez de Lechina Tejada		# Nombre del autor
header-includes:      	 	        	# Incluir paquetes en LaTeX
toc: true                   			# Índice
numbersections: false       			# Numeración de secciones
fontsize: 11pt              			# Tamaño de fuente
geometry: margin=1in        			# Tamaño de los márgenes
---


\newpage

<!-- 22/09/2017-->
# Introducción
<!--
Profesor: Domingo Martin Perandrés 
Correo: dmartin@ugr.es
-->
Conceptos previos para Informática Gráfica:

* **Operaciones con vectores (2,3 y 4D)**

  -Producto escalar
  
  -Producto vectorial
  
  -Modulo
  
  -Normalización
  
  -Regla de la mano derecha


* **Operaciones con matrices**

  - Producto de vectores por matrices

  -Producto de matrices


* **Trigonometría básica**

* **Ecuación de la recta (explicita, implícita,
  paramétrica)** 

  -Senos y cosenos
  
  -Teorema de Pitágoras
  
  -C. cartesianas $\Leftrightarrow$ C. polares

* **Ecuación de la recta (explicita, implícita, paramétrica)**
* **Ecuación del plano**


Se recomienda usar Linux en cualquier distro y QT para crear
interfaces de usuario (o en su defecto “glut“)

\newpage

En informática gráfica el objetivo es la generación de imágenes en un
ordenador.

Buscaremos siempre métodos de representación para los modelos a
representar. Por ejemplo, un prisma puede ser representado por un
vector de sus vértices (de la STL en nuestro caso).

Una vez hayamos establecido nuestro modelo debemos de utilizar OpenGL
para poder representarlo.

En OpenGL hay ciertas directivas, para dibujar el prisma que pensamos
previamente deberíamos comenzar por dibujar punto a punto 


```c++
glBegin(<GL_POINTS>); // Dibujará todo lo que haya entre begin y end
                      // Hay que indicarle cómo tratará lo que ha de dibujar

glVertex3f(<coordenadas>);

(...)

glEnd()
```

Para representar, por ejemplo un cubo, partiendo de que tenemos una
clase Vertice que permite almacenar las tres coordenadas (x,y,z) de un
punto debemos usar el mismo procedimiento. Redimensionamos un vector
de 8 vértices (los mismos que tiene un cubo), los inicializamos y una
vez procedamos a pintar, con glBegin(GL_Points) usaremos un bucle que
recorra los puntos del vector, accediendo a cada componente a la hora
de dibujar cada punto en cada iteración.

Tenemos que distinguir entre lo que es modelar y dibujar. Por un lado
modelar es la representación de nuestro modelo en una estructura de
datos arbitraria. Pintar, por su parte, es utilizar dicha estructura
para simplemente pintarlo, mostrarlo por pantalla.

Para completar la representación de nuestro cubo deberíamos añadir un
vector con los aristas de este (edges), cada vértice se rellenaría
guardando los índices de los dos puntos que lo definen.

A la hora de dibujarlo, el código diferiría levemente:


Nuestro vector de aristas sería del tipo `Edges[0]={0,1}`.

```c++
glBegin(GL_LINES);

for(int i = 0; i < Edges.size; i++){

    glVertex3f(Vertices[Edges[i].0]);
	glVertex3f(Vertices[Edges[i].1]);
}
gLend();

	
```

Con esto pintaríamos nuestras líneas, que deben pintarse
independientemente de los puntos.

OpenGL usa un formato de color RGB, donde cada color puede tomar un
valor entre 0 y 1

<!-- 29/09/2017 -->

**Ejemplo basado en la práctica 1:**

Dada una jerarquía del tipo

(O3DS)-----(O3D)

Implementaríamos este esquema del siguiente modo:

El archivo cabecera quedaría así:
```c++
#include <vector>
#include <vertex.h>

//Comenzamos con la implementación de la clase O3DS

class O3DS{
	std::vector<vertex> vertices;
	
	public void DrawPoints();
};
```

Y el fuente que implementa la cabecera, así:

```c++
#include <O3DS.h>


O3DS::DrawPoints(){
    glBegin(GL_POINTS);
 
    for(int i = 0; i < vertices.size(); i++){
		glVertex3f(vertices[i].x,vertices[i].y,vertices[i].z);
	}
	
	glEnd();
}

```

<!-- Comprobar si en la implementación del drawpoints anterior influye -->
<!-- la llamada a .size() en el tiempo o si es conveniente almacenar -->
<!-- el tamaño en una variable para tamaños muy grandes del -->
<!-- vector. -->

:)

hola holaaa
bonitos apuntes
:) (la de antes fue Jose Antonio)
