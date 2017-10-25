---
title: Sistemas Concurrente y Distribuidos						# Título
author: Jesús Sánchez de Lechina Tejada		# Nombre del autor
header-includes:      	 	        	# Incluir paquetes en LaTeX
toc: true                   			# Índice
numbersections: false       			# Numeración de secciones
fontsize: 11pt              			# Tamaño de fuente
geometry: margin=1in        			# Tamaño de los márgenes
---

\newpage

# Tema 1. Introducción


## Conceptos básicos

* **Programa secuencial:** Conjunto de datos con las respectivas
instrucciones secuenciales sobre estos.

* **Programa concurrente:** Conjunto de programas secuenciales que se
pueden ejecutar en paralelo.

* **Proceso:** Ejecución de un programa secuencial.

* **Concurrencia:** Describe el potencial para la ejecución paralela.

* **Programación concurrente:** Conjunto de notaciones y técnicas
usadas para expresar y resolver problemas de sincronización y
comunicación.

* **Programación paralela:** Aprovecha la capacidad de procesamiento
paralelo del hardware para acelerar la resolución de problemas.

* **Programación distribuida:** Su principal objetivo es hacer que
varios componentes software ubicados en diferentes ordenadores
trabajen juntos.

* **Programación de tiempo real:** Se centra en la programación de
sistemas que requieren respuestas de tiempo estrictas.


La programación concurrente mejora la eficiencia y la calidad de los
programas. En sistemas monoprocesador evita esperas ociosas y el uso
por parte de varios usuarios. En sistemas multiprocesador permite
distribuir las tareas para acelerar el tiempo de ejecución.

La concurrencia variará en tanto a la arquitectura del computador
usado, pues en monoprocesadores usaremos un paralelismo virtual que
simplemente nos permitirá gestionar mejor el tiempo, mientras que en
otras arquitecturas, tanto en multiprocesadores como en sistemas
distribuidos se permite un parelelismo real, pues cada unidad de
procesamiento puede gestionar una tarea distinta, manteniendo una
conexión entre todas ellas según sea necesario.

## Modelo Abstracto de concurrencia

Los conjuntos de instrucciones que ejecuta un computador puede ser
atómica o no atómica.

Decimos que es **atómica** cuando se tiene que ejecutar de principio a
fin sin verse afectada por otros procesos del programa.

Decimos que es **no atómica** en caso contrario.

Las sentencias atómicas pueden ejecutarse en orden distinto, a esto se
le llama **interfoliación**. A veces un programa requiere que unas
sentencias tengan una interfoliación determinada, para evitar fallos y
asegurarse el correcto funcionamiento del programa.


<!--20/09/2017-->
Un **Grafo de sincronización** es un Grafo Dirigido Acíclico donde
cada nodo representa una secuencia de sentencias del programa. Este
restringe las sentencias que pueden ser ejecutadas o que dependen de
la finalización de otras anteriores. Es decir, demuestran el
*paralelismo potencial* del programa.

### Definición estática de procesos
El número de procesos no cambian entre ejecuciones.

Notación en pseudocódigo:
```
var ... {vars.compartidas} // Variables compartidas a todos los procesos

process NomUno;           // Cada proceso se asocia con su id y su código
var ... {vars.locales}    // mediante la palabra clave “process”
begin
	... {codigo}
end

process NomDos;
var ... {vars.locales}
begin
	... {codigo}
end

... {otros procesos}

```


#### Definición estática de vectores de procesos

Se pueden usar definiciones estáticas de grupos de procesos similares
que solo se diferencia en el valor de una constante (vectores de
procesos).

Pseudocódigo:
```
var ... {vars.compartidas}

process NomP[ind : a...b];
var ... {vars.locales}
begin
	... {codigo}
	... { (ind vale a, a+1, ... , b) }
end

... {otros procesos}

```

### Creación de procesos no estructurada con fork-join

* **fork:** Sentencia que especifica que la rutina nombrada puede
  comenzar su ejecución, al mismo tiempo que comienza la sentencia
  siguiente
  
* **join:** Sentencia que espera a la terminación de la rutina
  nombrada, antes de comenzar la sentencia siguiente.
  
Pseudocódigo:
```

procedure P1;
begin
	A;
	fork P2;
	B
	join P2;
	C;
end

procedure P2;
begin
	D;
end

```

### Creación de procesos estructurada con cobegin-coend

Las sentencias en un bloque delimitado por cobegin-coend comienzan su
ejecución todas ellas a la vez. El “*coend*” se espera a que terminen
todas las demás sentencias.

Pseudocódigo:
```
begin
	A;
	cobegin
		B; C; D;
	coend
	E;
end
```

## Exclusión Mutua y Sincronización

En un programa pueden haber conjuntos de procesos que no son
independientes entre sí, y esto implica que algunas de las formas de
combinar las secuencias no son válidas.

Cuando esto ocurre se dice que hay una **condición de
sincronización**. Un caso particular es la *exclusión mutua*,
secuencias finitas de instrucciones que deben de ejecutarse de
principio a fin por un único proceso, sin que otro las esté ejecutando
a la vez.


Los conjuntos de instrucciones que deben de ejecutarse de este modo se
denomina **sección crítica**.

Ejemplo:

```c++
x = 0;
cobegin
x = x+1;
x = x-1;
coend
```

Si imprimiésemos aquí x, al estar ejecutándose todas sus sentencias
de manera concurrente, las instrucciones elementales de carga, suma y
escritura de cada sentencia, al acceder a la misma variable, se pueden
superponer estas, pudiendo obtener resultados indeseados e
inesperados.

Para solucionar esto indicamos la exclusión mutua con los signos `<` y
`>`: “`<sentencia>`”

Ejemplo de esto con el mismo código anterior sería:

```c++
x = 0;
cobegin
<x = x+1>;
<x = x-1>;
coend
```

## Propiedades de los sistemas concurrentes

Hay dos tipos de propiedades que son ciertos para todas las posibles
secuencias de interfoliación:

### Seguridad(Safety)

Condiciones que deben cumplirse siempre para evitar situaciones
perjudiciales. Se realizan de manera estática y suelen restringir las
interfoliaciones indeseadas.

Ejemplos:

* **Exclusión mutua**

* **Deadlock-freedom:** Nadie esperará a sucesos que nunca sucederán

* **Productor-Consumidor:** El consumidor debe consumir todos los datos
  producidos por el productor en el orden de producción.
  
### Vivacidad(Liveness)

Son propiedades que deben cumplirse “eventualmente”. Estas se realizan
de manera estática, lo que las hace más difícil de comprobar.

Ejemplos:

* **Starvation-freedom:** Un proceso no puede ser indefinidamente
  propuesto, en algún momento deberá avanzar.
  
* **Fairness:** Un proceso que desee avanzar deberá hacerlo con
  justicia relativa con respecto a los demás. Más ligado a la
  implementación, existen varios grados de esta.
  
  

