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

<!--14/09/2017-->
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

RAID (redundant array of independent disks) es un sistema de
almacenamiento que combina el uso de múltiples discos de
almacenamiento en una única unidad lógica para implementar la
redundancia de datos, esto es la existencia de datos adicionales a los
originales que permiten la corrección de errores en datos almacenados
o transmitidos.

**Seguridad:** Todo servidor debe de ser seguro ante incursión de
individuos no autorizados, corrupción o alteración de datos y las
posibles interferencias que impidan el acceso a los recursos. Para
ello usa medidas como la autenticación de usuarios, encriptación de
datos, cortaguegos y mecanismos de prevención de ataques DOS.

**Extensibilidad-expansibilidad:** Hace referencia a la facilidad que
ofrece el sistema para aumentar sus características o recursos.
Se permite esto mediante el uso de Sistemas Operativos modulares de
código abierto, o del uso de interfaces de E/S estándar para facilitar
la incorporación de más dispositivos al sistema.

**Escalabilidad:** Un servidor es escalable cuando sus prestaciones
pueden aumentar significativamente ante un incremento significativo de
su carga, es decir, de mantener su productividad al incrementar los
clientes.
Esto se puede implementar mediante cloud computing, clusters,
virtualización, distribución de carga, Storage Area Networks,
Programación Paralela... 

<!--21/09/2017-->
**Mantenimiento:** Procurar que el servidor siga funcionando
manteniendo las mismas prestaciones iniciales.

**Coste:** Es necesario que todo diseño se ajuste al
presupuesto. Desde el coste del hardware-software hasta la eficiencia
energética.

## Comparación conjunta de prestaciones y coste

Estudiaremos el concepto de "rapidez" en los computadores.

* **Tiempos de ejecución:** Dado el tiempo en que tarda un computador
en ejecutar un programa determinado podemos realizar una serie de
comparaciones en base a este.

Cambio relativo de $t_A$ con respecto a $t_B$ viene dado por:
$$\Delta t_{A,B} = \frac{t_A - t_B}{t_B} = \frac{t_A}{t_B}-1$$

**Speed Up:**
$$S_B(A) = \frac{V_A}{V_B} = \frac{t_B}{t_A}$$

Siendo $V_A$ la velocidad entendida como Cómputo_realizado/Tiempo.


Entoncces el **cambio relativo** es $\Delta V_{A,B} = S_B(A) -1$

Una vez conocidas las prestaciones nos interesa conocer el coste de
estos computadores para poder obtener la máxima relación
Prestaciones/Coste. 

Definimos la **Prestación** como la inversa del tiempo de respuesta.

De este modo esta relación se calcula del siguiente modo:

$$\frac{Prestaciones_A}{Coste_A} = \frac{\frac{1}{t_A}}{Coste_A}$$ 

Obtenida esta relación podemos dividir las relaciones de cada
computador para poder esclarecer cuánto mejor es una computadora con
respecto a la otra.

## Límites en la mejora del tiempo de respuesta. Ley de Amdahl

Al intentar mejorar las prestaciones de un computador existe una
limitación en las cuales nos debemos fijar para saber qué prestaciones
hay que optimizar para obtener una mayor repercusión en nuestros
tiempos de respuesta.

Cuando intentamos reducir el tiempo de ejecución de un programa nos
centramos en un determinado recurso, que es utilizado una porción del
tiempo de ejecución. Si mejoramos dicho recurso sólo optimizaremos esa
fracción del tiempo, el resto del programa que no use ese recurso no
podrá ser reducida con esa mejora.

La **ley de Amdahl** se basa en este principio.

$$S=\frac{1}{1-f+\frac{f}{k}}$$

Donde S es la ganancia en velocidad (speed-up), $f$ la fracción de
tiempo mejorable y $k$ la mejora del componente.

Además podemos calcular la ganancia máxima cuando la mejora tienda a
valores infinitos.

$$S_{max}= \lim_{k\to \infty} \frac{1}{1-f}$$

**Generalización de la ley de Amdahl**

$$S = \frac{1}{(1-\sum \limits _{i=1}^nf_i)+ \sum \limits _{i=1}^n \frac{f_i}{k_i}}$$

\newpage

## Ejercicios

**\underline{Ejercicio 1.3}**


E/S:              10s
Op. ComaFlotante: 60s
Op.Enteras:       30s

Tiempo total: 100s

Tras la mejora tendríamos:

E/S:              10s
Op. ComaFlotante: 20s
Op.Enteras:       15s

Tiempo total 45s

$$S=\frac{v_m}{v_o}=\frac{t_o}{t_m} = \frac{100}{45}=2.22$$

**\underline{Ejercicio 1.13}**

Tiempo total del programa: 70s

85% CPU --> 59.5s
15% E/S --> 10.5s

Un procesador que cueste el doble, ¿cuántas veces más rápido ha de ser
para rentabilizarlo?

Siendo x el coste del primer ordenador calculamos:

Prestaciones1/Coste1 = (1/70)/x
Prestaciones2/Coste2 = (1/T2)/2x

Siendo T2 la suma del tiempo de E/S invariable más el tiempo de CPU de
este computador.

Es decir que la relación del segundo ordenador debe ser mayor que la
del primero.

10'5+59'5/k $\leq$ 70/2 $\Rightarrow$ k $\geq$ 2'43


**\underline{Ejercicio 1.11}**

Un nuevo procesador mejora 15 veces la ejecución de las operaciones en
cooma flotante. Este procesador emplea un 65% del tiempo en coma
flotante.

1. En el procesador original, calcular la fracción de tiempo que usaba
   la coma flotante.
   
   Tiempo actual: $T_m$
   Tiempo del recurso mejorado: $0.65\cdot T_m$
   Tiempo no mejorable: $0.35\cdot T_m$
   
   Esto implica que el tiempo anterior $T_o$ se compone de el tiempo
   no mejorable y un tiempo 15 veces más lento que la mejora
   actual. Es decir:
   
   $T_o = T_m\cdot 0.35 + 15\cdot T_m \cdot 0.65$
   
   Entonces anteriormente usaba una fracción equivalente a
   $\frac{T_m\cdot 15\cdot 0.65}{T_o}$
   
   En este momento podemos estudiar la ganancia, expresada en función
   del tiempo de mejora:
   
   $$S = \frac{0.35\cdot T_m + 0.65\cdot T_m\cdot 15}{T_m}=10.1$$
   $$F = 0.965$$
   
<!-- 28/09/2017 -->

# Tema 2

## Placa Base

**Placa Base:** Una placa base es la tarjeta de circuito impreso
(PCB). En ella se conectan los principales componentes del computador
mediante elementos conductores. Hoy en día se hacen placas multicapa
para poder dar soporte a todas las conexiones. Todas las placas base
tienen una lista de características consultables llamada *Datasheet*.

### Elementos de la placa base

* **Zócalos de la CPU (Sockets):** Facilitan la conexión entre el
  microprocesador y la placa base de manera que puede ser remplazado
  sin necesidad de soldaduras. Algunos suelen tener una pequeña
  palanca que reduzca el riesgo de romper o doblar cualquier
  patilla. La CPU suele tener un ventilador propio, pues es el
  elemento que más calor genera, y por tanto necesita ser
  disipado. Para esto se suele usar la pasta térmica que es mejor
  conductor del calor que el propio aire. Incorporan su propia memoria
  caché, más rápida y pequeña (Static RAM, SRAM).
  
  Dado que la CPU necesita un voltaje mucho inferior al resto de la
  placa base existen reguladores de voltaje que permiten bajar la
  tensión a los 0'8V~1'1V para que puedan funcionar correctamente.
  
  Históricamente se ha evolucionado exponencialmente en términos de
  número de transistores, la frecuencia, la cantidad de Vatios... pero
  la capacidad de procesamiento monohebra se quedó atrás en el momento
  en el que se llegó a la *“Thermal Wall”*, no se podía generar toda
  la potencia que se quisiese sin generar un calor de manera que no
  fuese viable. Desde ese momento se desarrolló la tecnología
  multi-núcleo.
  
  Los tres grandes distribuidores de microprocesadores son Intel, AMD
  e IBM. Intel tiene su gama de servidores *“Xeon”*, que le da la
  capacidad de coexistir con otros microprocesadores en la misma placa
  base. AMD tiene por su parte a *“Opteron”*. La familia de servidores
  de IBM es *“POWER”*.
  
* **Zócalos para Memoria DRAM (Dynamic RAM):** Son más lentos que la
  SRAM, y son módulos de memoria principal de lectura/escritura, son
  volátiles y necesitan refresco, es decir, un controlador tiene que
  reescribir los datos sobre sí misma cada pocos milisegundos, pues
  esta memoria es volátil. 
  
  Los conectores están agrupados en canales de memoria a los que la
  CPU puede acceder en paralelo, pudiendo conectarse a varios módulos
  de memoria en cada canal.
  
  La mayor velocidad que podemos obtener lo haremos con la que más
  memoria nos permita tener a nuestro alcance. En las tecnologías de
  memoria actuales las DDR4 son las más recientes y veloces
  <!--Dato pendiente de confirmar-->
  
  






