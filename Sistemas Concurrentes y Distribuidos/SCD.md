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

<!-- Faltan cosas de las diapositivas desde el principio hasta la -->
<!-- página 21 (Grafos de sincronización) -->

Un **Grafo de sincronización** es un Grafo Dirigido Acíclico donde
cada nodo representa una secuencia de sentencias del programa. Este
restringe las sentencias que pueden ser ejecutadas o que dependen de
la finalización de otras anteriores. Es decir, demuestran el
*paralelismo potencial* del programa.

## Modelo Abstracto de concurrencia

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


