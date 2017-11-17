---
title: Informática Gráfica						# Título
author: Jesús Sánchez de Lechina Tejada		# Nombre del autor
header-includes: # Incluir paquetes en LaTeX
	- \usepackage{dsfont}
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
vector de sus vértices (vector de la STL en nuestro caso).

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
glEnd();


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

<!-- 06/10/2017 -->

Para realizar un giro de ángulo $\alpha$ sobre el eje y las
coordenadas de x,z cambian del siguiente modo:

$$x=R\cdot cos(\alpha)$$
$$z=R\cdot sen(\alpha)$$

La coordenada y, se mantendría constante al hacer el giro sobre ese
mismo eje.

El ángulo se calculará en un bucle que dividirá 2$\pi$ en N partes.

Cuando se dibuje la figura de revolución resultante hay que tener en
cuenta que, si bien las tapas forman triángulos, las demás caras
forman cuadriláteros, por lo cual debemos de utilizar triángulos para
representarlas. Para ello dispondremos todos los vértices de la figura
en una matriz que nos sirva para unir todos los puntos de manera que
formen triángulos. Esto lo haremos direccionando dicha matriz de tal
modo que cada vértice que se representa se una con sus elementos
adyacentes y con los elementos adyacentes que corresponderían al
trazado de una diagonal.

De este modo, al modelar los triángulos los pares y los impares (Para
cada cuadrilátero hay una cara superior y otra inferior) se forman
siguiendo una regla distinta para cada uno de ellos.

Tpar= P(i+1,j),P(i,j), P(i,j+1)
Timpar= P(i,j+1),P(i+1,j+1),P(i+1,j)

Los triángulos superiores de la fila superior de la matriz, así como
los inferiores de la última fila no se pintarán, pues son triángulos
"degenerados", con dos puntos que son iguales.


<!-- 13/10/2017 -->

# **Práctica 3. Jerarquía de Objetos**

Para la tercera práctica añadiremos un nuevo objeto al resultado de la
práctica anterior, usaremos un barrido lineal para generar nuevos
objetos. Dada una base generaría una base superior y las
correspondientes caras que conformarán.

La **translación** de un objeto se hará haciendo uso de la suma, esta
función transformará cada vértice del objeto, aplicando la misma
transformación sobre cada uno de ellos.

La **escala** por su parte hará uso del producto. Sólo nos interesa
obtener un cambio en el tamaño de la figura, sin importar un
hipotético cambio de centro del objeto. De modo que cada coordenada
de cada vértice se multiplicaría por la misma constante de la
escala. Se hacen con respecto al origen.

Cada coordenada tendrá un factor multiplicativo según nos interese. Si
multiplicamos por algún número negativo estaríamos cambiando el
vértice de cuadrante.

La otra transformación más común es la **rotación**, todo punto deberá
rotar respecto a un eje determinado. Esto se conseguirá manteniendo la
coordenada del eje respecto al que gira y posteriormente multiplicar las
otras dos coordenadas por el seno y el coseno del ángulo a rotar y
operar con el resultado según proceda.
<!-- La fórmula concreta está en los apuntes -->

<!-- 20/10/2017 -->

Para trabajar estas transformaciones haríamos uso de matrices para las
operaciones, y los puntos serían representados por vectores. Rotación
y escala se podían expresar mediante matrices cuadradas de
orden 3. Pero para la translación tendríamos que hacer uso de
coordenadas homogéneas, aumentar la dimensión mediante una aplicación
de $\mathds{R} ^3$ en $\mathds{R} ^4$ (x,y,z)
$\rightarrow$(xw,yw,zw,w).

Su inversa sería (x,y,z,w) $\rightarrow$ (x/w,y/w,z/w,1).

Estas transformaciones openGL las implementa usando una pila que
modificaremos con glMatrixMode(GL_MODELVIEW), ahí usaremos las
operaciones de transformación que nos ofrece openGL. Hemos de tener en
cuenta la peculiaridad de que, las transformaciones que vayamos
almacenando en esta pila se realizarán en el orden inverso al
introducido, es decir, que si queremos rotar un punto y después
trasladarlo primero tendremos que introducir el glTranslate(x,y,z) y
posteriormente introduciremos el glRotate(angulo,x,y,z) (x,y,z definen
el eje de rotación)

<!-- Falta lo del día 27, por lo general era explicar cómo hacer bien -->
<!-- las transformaciones (el orden adecuado) y cómo hacer un brazo robot -->
<!-- "jerarquizado" de una manera que no era la definitiva que -->
<!-- funcionaría  -->

<!--03/11/2017 -->

La jerarquía era: Con el brazo 2 se crea el 1 y con el brazo 1 se crea
la base.

El brazo 1 podía girar respecto al eje Y y respecto al eje Z. El
segundo brazo sólo podía moverse sobre el eje Z.

Estas transformaciones que mueven los objetos alteran el modelo final.

En el grafo se pueden indicar las conexiones de cada objeto del grafo,
es decir, ver a partir de qué objeto se obtiene el elemento del grafo
y las transformaciones en la pila necesarias para formarlo, además se
puede indicar cuál es el resultado final.

Lo correcto es empezar a modelar "desde lo más simple hasta lo más
complejo". Por ejemplo a la hora de modelar el robot, se empezaría por
"los dedos" del robot.

Por tanto primero generaríamos un único modelo que aparece dos veces,
a partir de un cubo escalaríamos para adaptarlo a la medida adecuada y
luego habría que transladarlo, por lo cual en el código
introduciríamos en la pila el glTranslate y luego el glScale,
realizando push antes y pop después de realizar las transformaciones.

```
   +--+
   |  |
   +--+
```

Entonces pasaríamos a construir la Mano. Esta mano estaría compuesta
por un Dedo, como el que acabamos de hacer, y un cubo que haría de
muñeca.

```
  +--+	 +--+
  |  |	 |	|
  +--+---+--+
  |			|
  +---------+
```

Para crear esta mano habría que usar la pila después de realizar las
transformaciones de cada figura.

```
Push (guardamos la identidad)
glTranslate
glScale
cubo.Draw
Pop (recuperamos la identidad)
Push (guardamos la identidad)
glTranslate
glDraw
Pop (recuperamos la identidad)
Push(guardamos la identidad)
glTranslate
cubo.Draw
Pop (recuperamos la identidad)
```

Entendemos esto como funciones que llaman a otras funciones e intentan
recuperar su contexto, como si cada vez que dibujasemos una figura
quisieramos acumular los cambios que hemos hecho hasta el momento.

Si metiesemos la matriz identidad perderíamos las transformaciones que
se hiciesen en los siguientes niveles con respecto a la posición de
la base de la mano, así se quedarían los dedos en una posición que no
les corresponde. En su lugar hacemos uso de las pilas.



Ahora queremos construir el "antebrazo", llamado Brazo1. Pues a partir
de la Mano que acabamos de crear tenemos que primero realizar las
transformaciones sobre el nuevo cubo para adaptarlo a las medidas
proporcionadas.

```
  +--+	 +--+
  |  |	 |	|
  +--+---+--+
  |			|
  +--+---+--+
	 |	 |
	 |	 |
	 o---o
```

El código se parecería a esto:
```
Push (guardamos la identidad)
glTranslate
glScale
cubo.Draw 
Pop (recuperamos la identidad)
Push
glTranslate
mano.draw
Pop
```

La creación del brazo y la base, lo que nos queda, sería semejante a
lo que hemos hecho hasta ahora.


Lo siguiente sería implementar el movimiento de los elementos,
empezando por los dedos, que se "abrirían y cerrarían".
<!-- 10/11/2017 -->

En Qt, si una clase requiere de eventos deberá incluir Q_Object

```c++
Class miboton{
Q_OBJECT // Usa el macro definido

public slots:
	void click(bool); // Así se definiría un slot
};
```


```c++
QPushButton *Button1= new QPushButton;
miboton *Boton1= new miboton



connect(Button1,SIGNAL(clicked(bool),boton1,SLOT(click(bool));
```

Para gestionar eventos usaremos un timer y las señales y slots.

Es aconsejable, independientemente de si usamos Qt o Glut, implementar
una interfaz gráfica de usuario con botones que realicen las tareas
que realizan ahora mismo las teclas. Además esta interfaz debería de
ser regulada con los widgets para ajustar su layout.


# **Práctica 4. Cámara e iluminación**
Una cámara que toma una instantánea está definida por la posición de
la cámara, un vector que defina la orientación de la cámara (a dónde
mira), y otro vector que define la disposición de la cámara, view-up (la
rotación que difiere al grabar en horizontal o vertical, por ejemplo).

<!--En los apuntes podemos encontrar el nombre que tienen el punto de -->
<!--referencia, el view-up y el lugar al que se está mirando -->

Estamos usando dos sistemas de coordenadas, el sistema de coordenadas
de mundo, común para todos los objetos y el sistema de coordenadas
local, relativo a cada objeto.


En el caso 3D para ajustar un nuevo sistema de coordenadas tendremos
que trabajar con productos vectoriales a partir del sistema de
coordenadas de mundo. Haríamos el producto vectorial del vector que va
desde el origen del SC de mundo hasta el nuevo con el vector
view-up. Esto nos proporcionaría un vector perpendicular a ambos, pero
el view up del SC local puede no ser ortogonal a los otros dos, por lo
tanto realizaríamos otro producto vectorial entre el primer vector y
el que acabamos de calcular. Así obtendríamos el view-up del SC local.

<!--17/11/2017 -->
**Ejercicio:**
Dado un VRP=(1,0,1), VPN=(1,0,1) y un VUP=(0,1,0), se pide calcular la
transformación de la cámara.

Sólo habría que hacer una translación de -VRP  más luego una rotación
de ángulo -45 grados. Este último ángulo lo obtenemos haciendo uso de
trigonometría, descomponiendo mediante giros y analizando estos firos
como triángulos rectángulos.

En los exámenes se suele pedir calcular las transformaciones de vista.


**Proyecciones:** El mecanismo que usan las cámaras para reflejar un
modelo 3D se llama proyección. En una proyección se involucran el
punto a proyectar, la superficie de proyección (la más simple, por
ejemplo, es el plano), las líneas de proyección que salen del punto a
proyectar (centro de proyección), cada una de estas intersecta con el
plano de proyección en un punto.

La diferencia de la perspectiva frente a la proyección paralela es que
el centro de proyección en la paralela está en el infinito, esto es,
las rectas de proyección serían paralelas(las rectas convergen en el
infinito). 

Esta proyección en la realidad no existe, pero se utiliza
"matemáticamente" pues estas proyecciones no producen ninguna "deformación".

Cuando tenemos un punto delante del centro de proyección y otro detrás
se produce una distorsión topológica.

En openGL se usa glFrustum(xmin,xmax,ymin,ymax,PD,PT) y
glOrtho(<mismos_parámetros>) para la proyección perspectiva-paralela.
PD sería el plano delantero y PT el plano trasero, que elimina todo lo
que esté por delante (PD) o por detrás de ellos (PT)
respectivamente. Además, al estar en una pantalla de ordenador debemos
limitar la altura y la anchura de la pantalla/ventana, pues más allá
no se puede ver.


