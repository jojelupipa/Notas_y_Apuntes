---
title:						# Título
author: Jesús Sánchez de Lechina Tejada		# Nombre del autor
header-includes:      	 	        	# Incluir paquetes en LaTeX
	- \usepackage[spanish]{babel}
toc: true                   			# Índice
numbersections: false       			# Numeración de secciones
fontsize: 11pt              			# Tamaño de fuente
geometry: margin=1in        			# Tamaño de los márgenes
---


\newpage

<!--

Francisco Velasco Anguita: fvelasco@ugr.es

-->

# Tema 1. Introducción a los Sistemas Gráficos

Un sistema gráfico es un tipo de sistema informático (conjunto de
reglas o principios orientados a almacenar y procesar información
cuyos componentes son el hardware y el software) cuyo papel
fundamental es la creación de información gráfica.

## Disciplinas:

* **Modelado:** La geometría de los objetos a representar

* **Síntesis de imágenes (rendering):** La capacidad de realismo de
  una representación.
  
* **Animación:** Realizar una representación adecuada que permita de
  manera realista la animación, el movimiento, de nuestros modelos.
  
* **Realidad Virtual:** Dar una alternativa "virtual" a procesos que
  en la vida real serían demasiado costosos.
  
* **Interacción:** Facilitar la interacción, el contacto, entre el
  usuario y el programa
  
* **Visualización:** Permitir una representación de la información
  para hacerla fácilmente comprensible, para poder realizar un
  análisis más fácilmente.
  
\newpage

# Tema 2. Desarrollo de un Sistema Gráfico

## Concepto de Modelo

Un modelo es una representación de las características más relevantes
que definen a una entidad. Dicha entidad puede ser real o imaginaria,
pero nos permite trabajar con el modelo en lugar de hacerlo con la
entidad, lo cual puede suponernos un ahorro en tiempo y medios.

El modelo tiene unas etapas de creación y posterior edición con el fin
de acercarse lo más posible a la entidad a representar. Este modelo
nos permitirá una correcta visualización de manera que podamos obtener
información al respecto de la manera más fiable posible.


## Desarrollo de un sistema gráfico

La etapa de **creación** puede ser procedural, mediante carga
de archivos o interactiva, en la de **edición** podemos distinguir
cómo variará en el objeto en función del tiempo (*animación*), en
función de sus relaciones con otros objetos (*colisiones*) y con el
usuario (*pick y procesamiento de eventos*).


Entre los distintos tipos de modelos que existen podemos encontrar
grafos de escena, mallas de polígonos, volúmenes (representación de
datos en un array tridimensional), escenarios...

<!-- 06/03/2018 -->

El desarrollo de un sistema gráfico inicialmente se compone de tres
etapas previas: **Extracción de requisitos no funcionales**,
**generación de modelos3D (modelado/digitalización)** y **extracción
de requisitos funcionales**. Una vez recopilada toda esta información
pasamos a la gestión de escena y tras esto se encuentra el proceso de
visualización. 

## Gráficos en web

Desarrollar sistemas gráficos para web es una tarea complicada, pues
presenta los inconvenientes de que todo algoritmo que diseñemos se
ejecutará en hardware con recuros desconocidos y variados. En esto la
visualización 3D en web deja ver sus problemas particulares:

Puesto que todo se realizará generalmente con una arquitectura
cliente/servidor, con terminales distanciadas entre sí y cada una con
sus propiedades de conectividad características hay que tener en
cuenta que todo modelo que queramos transferir puede tener un tamaño
muy grande en relación a la capacidad de transferencia de
información. Esto imposibilitaría llevar a cabo sistemas en tiempo
real o simplemente añadir un molesto retraso en los tiempos de
descarga.

Adicionalmente la visualización 3D requiere una cantidad de recursos
considerable, y no todos los computadores pueden lidiar con ello.

Los navegadores web, por su parte, tienen restricciones. Tanto de
memoria como de acceso al sistema de archivos ni a las bibliotecas del
sistema. A esto hay que añadirle no sólo las restricciones de cada
navegador web sino también la diversidad de sistemas operativos.

### Tecnologías usadas

Recientemente se han utilizado tecnologías como **VRML**, estándar no
aceptado por todos lo navegadores y que requiere plugins para su
funcionamiento, y **Applets Java**, que también está limitado a 64MB y
250 000 polígonos (insuficiente para la mayoría de sistemas hoy en
día).

En contrapartida, gracias a la decadencia de IE y Safari han surgido
otros estándares impulsados por **HTML5** o **WebGL** (que da acceso a
las capacidades OpenGL del cliente).

WebGL, si bien es un sistema multiplataforma, capaz de convivir con
otro html y gratis, por sí mismo requiere programar a bajo nivel, y no
dispone de primitivas geométricas, matrices de transformación,
materiales ni fuentes de iluminación. Todo hay que diseñarlo e
implementarlo. Esto, por su parte, proporciona mucha flexibilidad y
eficiencia al usar buffer objects y shaders.

Por estos motivos es una buena alternativa utilizar una capa de
abstracción sobre WebGL. La más conocida y la usada en esta asignatura
es **Three.js**, open source y mantenida, orientada a objetos y con
soporte para interacción y formatos de archivos 3D.

\newpage

# Tema 3. Grafos de Escena

Un sistema gráfico se organiza mediante un modelo jerárquico en el
cual podemos observar qué objetos hay en la escena y cómo se
relacionan entre sí. Esto se implementa mediante una estructura de
datos apropiada, en este caso, el grafo.

Un grafo de escena es un grafo dirigido acíclico que tiene diversos
tipos de nodos:

* **Geometría:** Objetos geométricos, fondo de la escena...

* **Nodos que influyen en otros elementos:** Tales como luces,
  materiales o comportamientos.
  
* **Nodos gestión:** Vista, grupos o transformaciones.

Este grafo (árbol) tiene una única raiz. Sus hojas pueden representar
figuras, luces, vistas o comportamientos.

El grafo está orientado a objetos, pues cada rama puede ser una
componente perfectamente reusable. Este diseño permite la adaptación
dinámica a cambios y separar representación y visualización.

## Representación de Figuras

Una figura es la unión de su geometría, las propiedades intrínsecas de
la figura, y su aspecto, la manera en que es visualizada.

En la representación **B-Rep** (boundary representation/representación
de fronteras) una figura se representa por la frontera
que la delimita, y para esta frontera se utiliza una una malla de
polígonos. La representación es exacta para figuras poliédricas y
aproximada para curvas.

Para representar una figura es necesario almacenar su **geometría**,
las coordenadas de los vértices que la representan, y su
**topología**. Adicionalmente, para mejorar la representación se puede
guardar su **vector normal** para calcular la iluminación y sus
**coordenadas de textura** para aplicar posteriormente texturas.

La geometría se puede almacenar de manera indexada o no indexada,
según si sus vértices se almacenan independientemente de sus caras y
no se repiten o si se almacenan los vértices y las caras
conjuntamente, guardando en sentido antihorario el trío de vértices
que define cada cara.

```c++
// Indexada
Vertices = {A, B, C, D ... }
Caras = {1,0,2, 0,1,3, 0,2,3 ... } // En sentido antihorario, en tríos

// No indexada
Vertices_caras = {B,A,C, A,B,D, A,C,D ... } // En sentido antihorario también 
```

La indexada ahorra mucho espacio de almacenamiento, permite
modificar vértices rápidamente y facilita operaciones de
pertenecencia.

La no indexada, por su parte, permite una visualización más rápida y
permite tener un mismo vértice con varias normales y coordenadas de
textura según la cara desde la que se use.

*Las ventajas de la indexada son las desventajas de la no indexada y
viceversa.* 

### Creación de geometría

La geometría se puede generar **Manualmente**, en el código;
**proceduralmente**, dados unos parámetros (generar alguna figura 
por revolución o utilizar three.js); **utilizando primitivas básicas
y operando con ellas** para generar figuras más complejas
(transformaciones como extrusión, barrido o revolución; y operaciones
booleanas como unión, intersección y diferencia); mediante la
**interacción**; **cargando de un archivo**, por ejemplo los .obj,
para los cuales hay que importar los materiales y el modelo.

La clase **Mesh** conforma un tipo especial de nodo, pues además de
geometría y material, incluye una transformación.

Es necesario recordar los tipos de transformaciones geométricas que
hay: **Traslación, escalado uniforme y rotación**

Las combinaciones de traslaciones con rotaciones o escalados **no son
conmutativas**. 

En three.js, en la clase mesh se pueden configurar mediante los
atributos *position*, *rotation* y *scale*, los cuales se realizan
respecto al eje local de cada objeto, transmitiendo todas las
transformaciones a sus descendientes. Se realizan en este orden:
Escalado, rotaciones *z*, *y*, *x* y finalmente las traslaciones. Esto hay
que tenerlo en cuenta a la hora de definir la geometría que queremos
en nuestra figura. En caso de requerir otro orden para nuestro diseño
habrá que hacer uso de nodos intermedios auxiliares (del tipo Object3D).

Las tranformaciones se realizan bien al recorrer el grafo para
visualizar un frame en cada nodo mesh o si se modifican los atributos
de transformación entre frames.

Otro tipo de transformaciones se pueden efectuar utilizando
`applyMatrix(matriz4)` la cual permite, introduciendo en `matriz4` la
matriz correspondiente, aplicar un escalado, rotación o traslado de
manera permanente a la geometría (no al nodo Mesh), por lo cual se
puede modificar su posición respecto a su eje local, pero esta
transformación nunca la heredarán sus hijos.

Three.js nos permite conformar la jerarquía añadiendo y eliminando
nodos de esta mediante los métodos *add* y *remove*.

## Animación

La animación consiste en que un conjunto de cambios en una figura
generen la sensación de cambio o movimiento (para percibir movimiento
se requieren al menos 24 imágenes por segundo). 

Hay diversos modos de implementar las animaciones. Bien con una
**animación procedural**, modificando parámetros en función del
tiempo, mediante **escenas clave**, designando un conjunto de escenas
clave e interpolando manual o automáticamente (tween, p.ej.) los
valores de los atributos de los elementos de la escena y configurando
los cambios en función del tiempo, o también se puede **animar
mediante caminos**, definiendo una trayectoria que el objeto seguirá,
esto se puede hacer con splines.

## Interacción

### Picking
La interacción puede ser tanto con la aplicación, usando una GUI, como
con la escena, usando *picking*, edición o movimientos de cámara.

*La GUI usada en esta asignatura es 
[dat.gui](https://github.com/dataarts/dat.gui).*

El picking se implementa definiendo el comportamiento de la aplicación
cuando suceden algunos eventos en el ratón, tales como apretar o
soltar algún botón de éste, moverlo, utilizar la rueda, o mediante el
uso de funciones. Además, este comportamiento se puede modificar si
tenemos en cuenta que alguna tecla modificadora (ctrl, alt o shift)
está pulsada.

Se pueden utilizar **Estados de la aplicación**: variables globales
que indican qué se está haciendo para determinar cómo proceder en
determinadas funciones.

Para implementar el picking es necesario primero saber en qué pixel se
pulsó, posteriormente hay que pasar un rayo que vaya desde la cámara
hasta el infinito y pase por dicho pixel, así obtenemos los objetos
que hayan intersectado con ese rayo. Estos objetos se guardan en un
vector cuyo primer elemento fue el primer elemento con el que
intersectó, y, por tanto, el deseado.

Otra característica interesante en el picking es el **feedback**:
hacer saber al usuario qué objeto se ha seleccionado. Esto puede
hacerse de muchos modos, como por ejemplo cambiando la opacidad de un
objeto o cambiando su aspecto de cualquier modo (p.e.: modo alambre).

### Colisiones

Por lo general es necesario saber cuándo dos objetos están
colisionando. Esto se realiza en dos fases: Una **fase gruesa** para
descartar rápidamente elementos que no colisionan, usando *indexación
espacial* (se utilizan estructuras de descomposición espacial, octrees
o KD-trees) o *cajas y esferas englobantes*. Y una **fase fina**,
donde se detecta con mayor exactitud si dos elementos están
colisionando.

### Física

Un motor de física permite dotar de “realismo físico” a los elementos
que conforman la escena. En contrapartida, estos requieren múcho cálculo.

\newpage

# Tema 4. Visualización

El renderizado involucraba todo el proceso de generación de imágenes,
en el cual influenciaba el paso de un modelo 3D a una visualización 2D
(una pantalla) en la cual afectan factores como la proyección o la
distorsión de la perspectica, así como la fotometría, luz reflejada
por los objetos de la escena.

## Cámaras

La cámara y sus **vistas** es uno de los principales puntos de la
visualización. Proporciona la capacidad de captar la atención a
voluntad (**encuadre**) y se puede animar a voluntad (siempre evitando
el *motion sickness*).

Una cámara está compuesta por diversos elementos: Existe el plano de
proyección, que corresponde con lo que nosotros vemos en nuestra
pantalla, un par de planos delanteros y traseros, para restringir lo
que vamos a visualizar y un punto de proyección hacia el cual se
proyectan todos los puntos de la escena (esto nos permite cambiar “lo
cerca que miramos”). Tiene un par de vectores, VNP
(normal al plano) que define “a dónde está mirando” y otro VUp (view
up) que define la disposición de la cámara, que permite por ejemplo,
visualizar en horizontal o vertical. Estos vectores junto a una
posición, definen con exactitud a una cámara.

Además, se puede distinguir entre proyecciones ortográficas y en
perspectiva según si los puntos se proyectan de manera perpendicular o
con una deformación que imita a la perspectiva en la realidad.

## Luces

Las luces son usadas para poder percibir los objetos en las escenas,
así como también pueden producir un ambiente. Una luz básica se
define por su posición, color e intensidad. Este es el concepto de
**luz puntual** pero existen otros tipos. La **luz focal**
adicionalmente tiene un ángulo de apertura que reduce el área que se
ilumina. La **luz direccional**, por su parte, es una luz focal cuyo
punto se encuentra en el infinito. Esto implica que los rayos
incidentes son paralelos entre sí. La **luz ambiental** es otro tipo
de luz el cual ilumina a toda la escena por igual y simula la luz
indirecta, permite añadir una dominante de color.

### Materiales

El material de los objetos influye en la manera que se refleja la luz
que se proyecta sobre estos. Todo material tiene una componente
**especular** y otra **difusa**, así como otra componente
**ambiental**.

La especular depende del ángulo que forman el rayo de luz reflejado y
el observador. La difusa sin embargo depende del ángulo que forman la
normal y la posición de la luz. La luz ambiental es inherente al
material, una luz que emite continuamente independientemente del resto
de luces.

Three.js implementa una clase Material que usa shaders para llevar a
cabo el proceso de iluminación. 

