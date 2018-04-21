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
