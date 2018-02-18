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
<!--
**Presentación**
Información asignatura:

Profesor: Héctor Pomares
Correo: hector@ugr.es

Tutorías: M,X: 12:30-15:30 CITIC, CB-3

Puntuación:

Teoría 60%: Examen de Teoría.                Mínimo 5/10 

Prácticas 40% Defensa/entrega de prácticas.  Mínimo 5/10 
-->

# Tema 1. Introducción a la Ingeniería de Servidores

Un **servidor** es un sistema informático, un conjunto de hardware,
software y humanware que permiten trabajar con información.

## Clasificación de Sistemas Informáticos y Servidores
Estos sistemas se pueden clasificar de diversas maneras, según su
arquitectura (paralelismo SISD,MISD, SIMD,MIMD), uso general o
específico... 

Respecto a arquitectura de servicio se pueden clasificar en Sistema
aislado, arquitectura cliente-servidor, de n capas o
Cliente-Cola-Cliente.

### Sistema aislado

No interactúa con otros sistemas, tales como los computadores
personales.

### Arquitectura cliente-servidor

Las tareas se reparten entre el cliente, proveedor de servicios y los
demandantes, clientes.

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


### **Disponibilidad:** 

Se regula mediante los períodos en los que el
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

### **Seguridad:** 

Todo servidor debe de ser seguro ante incursión de
individuos no autorizados, corrupción o alteración de datos y las
posibles interferencias que impidan el acceso a los recursos. Para
ello usa medidas como la autenticación de usuarios, encriptación de
datos, cortafuegos y mecanismos de prevención de ataques DOS.

### **Extensibilidad-expansibilidad:** 

Hace referencia a la facilidad que
ofrece el sistema para aumentar sus características o recursos.
Se permite esto mediante el uso de Sistemas Operativos modulares de
código abierto, o del uso de interfaces de E/S estándar para facilitar
la incorporación de más dispositivos al sistema.

### **Escalabilidad:** 

Un servidor es escalable cuando sus prestaciones
pueden aumentar significativamente ante un incremento significativo de
su carga, es decir, de mantener su productividad al incrementar los
clientes.

Esto se puede implementar mediante cloud computing, clusters,
virtualización, distribución de carga, Storage Area Networks,
Programación Paralela... 

<!--21/09/2017-->
### **Mantenimiento:** 

Procurar que el servidor siga funcionando manteniendo las mismas
prestaciones iniciales. 

### **Coste:**

Es necesario que todo diseño se ajuste al presupuesto. Desde el coste
del hardware-software hasta la eficiencia energética.

## Comparación conjunta de prestaciones y coste

<!-- Tal vez este apartado sea más importante que los conceptos de -->
<!-- "cultura general" anteriores (speedup y prestaciones al menos)-->

Estudiaremos el concepto de "rapidez" en los computadores.

* **Tiempos de ejecución:** Dado el tiempo en que tarda un computador
en ejecutar un programa determinado podemos realizar una serie de
comparaciones en base a este.

Cambio relativo de $t_A$ con respecto a $t_B$ viene dado por:
$$\Delta t_{A,B} = \frac{t_A - t_B}{t_B} = \frac{t_A}{t_B}-1$$

**Speed Up:**
$$S_B(A) = \frac{V_A}{V_B} = \frac{t_B}{t_A}$$

Siendo $V_A$ la velocidad entendida como Cómputo_realizado/Tiempo.


Entonces el **cambio relativo** es $\Delta V_{A,B} = S_B(A) -1$

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
coma flotante. Este procesador emplea un 65% del tiempo en coma
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
   
\newpage

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
  
  Dado que la CPU necesita un voltaje muy inferior al resto de la
  placa base existen reguladores de voltaje que permiten bajar la
  tensión a los 0'8V~1'1V para que puedan funcionar correctamente.
  
  Históricamente se ha evolucionado exponencialmente en términos de
  número de transistores, la frecuencia, la cantidad de Watios... pero
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


<!-- 05/10/2017 -->

  
* **Ranuras de expansión:** Permiten la conexión con otras tarjetas de
  circuito impreso.
  
  Buses:
  
  **PCI:** Peripherial Component Interconnect. Son half-duplex. Sus
  frecuencias son muy bajas, para ciclos relativamente lentos.

  **AGP:** Accelerated Graphics Port. Proporciona conexión directa
  entre el adaptador de gráficos y la memoria.
  
  Con el tiempo todos estos protocolos han sido superados por
  PCI-Express
  
  **PCIe:** No es un bus, es una conexión punto a punto. Full-duplex,
  esto lo consigue ya que cada LANE está compuesta por 4 cables. El
  reloj está embebido en los datos. Es hot plug. Su codificación ha
  avanzado tanto que por cada 130b que envía 128b son de datos y solo
  tiene dos extra, lo cual disminuye la sobrecarga frente a los 8b/10b
  de versiones anterior
  
  **Ethernet:** Para redes de Área Local, con conexiones de cable
  trenzado o fibra óptica.
  
  **USB:** Universal Serial Bus (Intel)
  
  **FireWire:** Puerto de serie de altas prestaciones (Apple)
  
  **Thunderbolt:** Combina PCIe y DisplayPOrt para obtener
  almacenamiento externo de altas prestaciones.


* **Almacenamiento Permanente:** Sirven para almacenar información de
  manera “permanente”, a diferencia de la RAM, no es volátil. Existen
  distintos tipos (magnéticos, ópticos, SSD...) y distintos
  protocolos/interfaces: ATA (Advanced Technology Attatchment),
  PATA(Parallel ATA), SATA (Serial ATA), SATAe, SCSI (Small Computer
  System Interface) Ultra-SCSI, SAS (Serial Attached SCSI), USB... 
  
  Entre otras interfaces se encuentra el canal de fibra o la infiniband.
  
  
  
* **Chipset:** Es el conjunto de circuitos integrados que se encargan
  de controlar las comunicaciones entre los diferentes componentes de
  la placa base.

<!-- 19/10/2017 -->

* **Memoria ROM:** También llamada firmware, es un tipo de memoria no
volátil que almacena el código de arranque del ordenador. Identifica
los dispositivos instalados, hacer el Power-on self-test del sistema y
arrancar el SO.

Cada placa suele contentener un conjunto de parámetros definidos por
el usuario que se almacenan mediante una memoria RAM-CMOS alimentada
por una pila (la misma pila que alimenta el reloj en tiempo real).

* **Fuente de alimentación:** Su misión es obtener tensiones continuas
  que necesita un computador. (Entrada AC 200V-50hz / Salidas DC 5,12
  y 3,3V. Potencia de 250W a 1000W)
  
### Centros de Procesamiento de Datos

Son centros donde se concentran los recursos del procesamiento de
datos de una organización.

Se les atribuye un diseño determinado, con la apropiada
infraestructura, disposición y cableado. Con un sistema de alimentación que lo
proteja de fallos en el suministro eléctrico. Además cuentan con un
sistema de ventilación capaz de disipar el calor para mantener las
condiciones óptimas. 

\newpage

# Tema 3

## Concepto de Monitor de Actividad

**Definiciones:**

* **Carga:** Conjunto de tareas que ha de realizar un sistema.

* **Actividad de un sistema:** Conjunto de operaciones que se realizan
  en el sistema como consecuencia de la carga que tiene.
  
La actividad de un sistema puede estar reflejada por otras variables
tales como los procesadores, la memoria, los discos, la red y el
sistema global. 

Podemos estudiar cómo un sistema se adapta a un determinado tipo de
carga usando un **monitor de actividad**. Estos nos permiten medir la
actividad, procesar la información y mostrar los resultados.

Las ventajas de usar un monitor están en la posibilidad de localizar
los cuellos de botella, predecir cargas futuras, tarificar a los
clientes, obtener modelos para realizar estudios (para
Administradores), Conocer las partes críticas (hot spot) de una
aplicación (para programadores) y adaptarse dinámicamente a la carga
(para el Sistema Operativo).

Hay distintos **tipos de monitores**, **por eventos**, cada vez que
ocurre algún determinado evento se realiza el estudio. También están
los **monitores por muestreo**, que recaban información estadística
cada un determinado tiempo de muestreo T.

Los monitores pueden ser tanto Software (programas), Hardware
(dispositivos físicos) como Híbridos.

Una medida, a su vez, viene determinada por su exactitud, precisión,
resolución del monitor/sensor, período de muestreo, tasa máxima de
entrada, anchura de entrada y sobrecarga.

La sobrecarga en un monitor se puede estudiar dados los recursos del
sistema el monitor consume.

$$ Sobrecarga_{recurso} =
\frac{UsoRecurso_{monitor}}{CapacidadRecurso} $$

<!-- 26/ 10/2017 -->
## Monitorización a nivel de sistema

Existen diversas utilidades y registros que nos permiten obtener
información acerca de nuestro ordenador.

Directorio **/proc**: Es una carpeta en RAM utilizada por el núcleo de
Unix que nos permite obtener información sobre los datos del
kernel. Permite el acceso a indormación global del SO, a la de cada
uno de los procesos del sistema y a algunos parámetros del kérnel.

La mayoría de monitores de Unix utilizan este directorio como fuente
de información.

**uptime:** Indica el tiempo que lleva el sistema en marcha y la
“carga media” que soporta. 


**ps** (process status): Información sobre el estado actual de los
procesos del sistema.

**top:** Cada T segundos muestra carga media, procesos, consumo de
memoria... Normalmente se ejecuta en modo interactivo.

**vmstat:** Paginación, swapping, interrupciones, cpu...

**Monitor sar:** Muy utilizado para la detección de cuellos de botella
en sistemas Unix. Almacena tanto información actual como histórica
haciendo uso de */proc* para ello.

Se basa en dos órdenes complementarias: *sadc* para recopilar datos
estadísticos y *sar* para leerlos y traducirlos a formato legible.

Además existen multitud de programas y herramientas como CollectL,
Nagios, Munin, SarCheck... que se utilizan con fines similares.

## Monitorización a nivel de aplicación (profilers)

Observan el comportamiento de una aplicación para optimizar su código,
permiten obtener información sobre un programa, en qué parte del
código pasa la mayor parte del tiempo, cuantas veces se ejecuta cada
línea...

**time:** *(/usr/bin/time)* Recibe como argumento un programa, lo
analiza y devuelve algunas estadísticas de uso, como el tiempo que se
ejecutó en un determinado modo o los cambios de contexto requeridos.

**gprof:** Da información sobre el tiempo de ejecución y el número de
veces que se ejecuta cada función de un programa. Requiere de los
parámetros -pg a la hora de compilarlos en C.

**gcov:** Estudia el número de veces que se ejecuta cada línea del
código del programa. También requiere de parámetros específicos
(-fprofile-arcs y -ftest-coverage.

Además existen otros profilers como Valgrind (binarios ya compilados),
V-Tune o CodeXL (usan contadores hardware).

\newpage

<!-- 02/11/2017 -->

# Tema 4. Análisis Comparativo del Rendimiento

## Índices clásicos de rendimiento

Características de un índice de rendimiento.

**Fiabilidad:** Si un sistema indica que A es mejor que B es porque
siempre el rendimiento de A es mejor que el de B.

**Linealidad:** Si el índice de rendimiento aumenta, el rendimiento
real aumenta en la misma proporción.

**Repetibilidad:** Que el sistema de medida ante las mismas
condiciones devuelve los mismos resultados.

**Consistencia y facilidad de medición:** Este índice debe ser medible
desde cualquier sistema y siempre del mismo modo.

* **Tiempo de ejecución:** Suele considerarse el mejor índice de
  rendimiento, aunque depende del programa o programas que se
  ejecuten.
  
* **MIPS:** Millones de instrucciones por segundo

* **MFLOPS** (Normalizados: operaciones simples, como la suma, resta,
  multiplicación o comparación, otras como la división requieren más
  operaciones)
  
Todos estos índices tienen diversos problemas, pues no se logran
adaptar perfectamente a cualquier situación, pues cada computador
puede tener arquitecturas distintas y pueden no ser representativos.

En su lugar intentamos medir qué ordenador es el más rápido para
distintos tipos de carga. Lo que se conoce como “*Benchmarking*”.

## Benchmarking

Evaluar la carga real que un ordenador tiene que soportar no es
viable, pues esta puede cambiar en tiempo real y no es previsible. En
su lugar se utilizan **modelos de carga**. Aproximaciones de la carga real
que puede recibir un sistema informático.

Para hacer estos modelos es necesario identificar los recursos que más
demande la carga y hacer la recolección y análisis de datos oportuno.

El **benchmarking** es un test de rendimiento que permite someter a la
misma carga distintos sistemas informáticos, esta carga pretende ser
lo suficientemente genérica como para ser representativa.

Existen distintos tipos de benchmark:

* **Microbenchmark:** Es el benchmark para componentes específicos, ya
  sea caché, memoria, procesador... En general es mucho más difícil de
  medir, pues no es sencillo aislar un componente.
  
* **Macrobenchmark:** Benchmark para una aplicación real, mide
  conjuntos de aplicaciones habitualmente utilizadas en un área.
  
Uno de los microbenchmarks más reconocidos es el SPEC, que tiene
algunos índices significativos:

**Base:** Compilación en modo conservador, con reglas estrictas para que
todos usen las mismas opciones de compilación.

**Peak:** Es el rendimiento pico, permitiendo la elección de las
opciones óptimas para cada programa.

<!--09/11/2017 -->
## Análisis de los resultados de un benchmark

El rendimiento es una variable multidimensional, que debería de tener
múltiples índices, sin embargo es más fácil realizar comparaciones con
un único índice. Entonces hay que usar algún tipo de media.

La media aritmética tiende a ser desechada, pues puede dar lugar a
medidas no representativas, por ejemplo si se hiciese la media
aritmética de los tiempos de ejecución, sólo tendría sentido mejorar
los programas benchmark grandes que puedan tener mayores variaciones y
mejoras.

Para intentar solventar esto se puede aplicar una media ponderada,
dándole un peso a cada programa, dándole un peso inversamente
proporcional a los tiempos de ejecución de cada programa, ejecutados
en una máquina de referencia. 

Aun esto resulta ser una medida imprecisa, pues todo depende de la
máquina de referencia escogida.

Alternativa: La media geométrica. Cuanto menos tiempo tenga mayor será
el índice. Una máquina tiene mejores especificaciones que otra si la
média geométrica de la primera es menor que la media geométrica de la
segunda.

¿Por qué interesa la media geométrica?

Se interesa premiar las mejoras sustanciales, no se castigan los
empeoramientos no muy sustanciales.

<!-- 16/11/2017 -->

## Introducción al diseño de experimentos

A la hora de realizar un benchmark puede que influyan muchos factores,
tanto SO, como Memoria RAM, tipo de discos duros...

Terminología:

**Variable respuesta o dependiente:** Índice que usamos para las
compararciones. 

**Factor:** Cada una de las variables de las que depende nuestra
variable respuesta.

**Nivel:** Los valores que puede tomar cada factor

**Repetición:** Número de veces que se repite cada experimiento

**Interacción:** El efecto de un determinado nivel de un factor sobre
la variable de respuesta puede ser diferente para cada nivel de otro
factor. Por ejemplo, el uso de un determinado SO puede implicar un
mayor consumo de RAM.

Tipos de diseños:

**Unifactor:** Una configuraciónd determinada como base y se estudia
un único factor cada vez.

Saber qué es el test ANOVA y para qué sirve es importante de cara al
examen. 

<!-- 23/11/2017 -->
\newpage
# Tema 5. Análisis operacional en servidores.

Modelar el rendimiento de un servidor.

Para comprender nuestro servidor usaremos el modelo de redes basados
en colas, pues estudiaremos cada recurso como únicamente accesible por
un único usuario.

Una red de cola es un conjunto de estaciones de servicio, que estan
compuestas por un recurso y una cola de espera para los trabajos que
intentan acceder a él.

Lo más relevante en este modelo es tanto el procesamiento como la
Entrada/Salida. Un proceso que entra en la CPU espera en la cola de la
CPU hasta su turno, luego, si lo requiere avanza hasta la siguiente
estación de servicio y luego sale, o sale directamente si solo
necesita CPU.

Variables temporales de un trabajo que se usan en una estación de
servicio:

* **Tiempo de espera en la cola (W)**

* **Tiempo de servicio (S):** Tiempo que transcurre desde que se ocupa
  el recurso hasta que se libera.
  
* **Tiempo de respuesta (R):** Suma de los dos tiempos anteriores.

Las estaciones con más de un servidor son capaces de atender a más de
un trabajo en paralelo. Aumentar el número de servidores disminuirá el
tiempo de espera en cola.

* **Tiempo de reflexión(Z):** Tiempo que tarda un usuario antes de
  volver a enviar una petición al servidor.
  
**Redes de colas abiertas:** Los trabajos llegan a la red a través de
una fuente externa que no controlamos, son procesados y salen a través
de uno o más sumideros. El número de trabajos en el servidor puede
variar con el tiempo. Se conoce la tasa de llegada de trabajos al
servidor.

**Redes de colas cerradas:** Presentan un número constante de trabajos
que van recirculando por la red ($N_t$). Y se distinguen en batch o
interactivas si carecen de interacción con usuarios o si la tienen.

**Redes de colas mixtas:** Cuando el modelo no corresponde a ninguno
de los dos anteriores.

### Variables y leyes operacionales

Por conveniencia, al hablar de todas estas variables, nos referiremos
a los valores medios en lugar de a un valor concreto cuando esta
información se omita.

**Variables operacionales básicas:**

* **T (global temporal):** Duración del período de medida sobre el que se extrae el
  modelo.
  
Luego existen otras variables operacionales básicas de una estación de
servicio determinada durante este tiempo T:

* **$A_i$:** Número de trabajos solicitados a la estación i-ésima
  (arrivals). 
  
* **$B_i$:** Tiempo total en el que la estación i-ésima ha estado
  ocupado (busy).
  
* **$C_i$:** Número de trabajos completados por la estación i-ésima
  (completions). 

**Variables operacionales deducidas**:

A partis de estas variables se pueden extraer muchas otras:

* $\lambda _i$ Tasa media de llegada (trabajos/segundo, $A_i$/T).

* $\tau _i$ Tiempo medio entre llegadas (segundos/trabajo 1/$\tau _i$,
  menos utilizada).

* $X_i$ Productividad media (trabajos/segundo, $C_i/T$).

* $S_i$ Tiempo medio de servicio ($B_i/C_i$).

* $W_i$ Tiempo medio de espera en cola. 

* $R_i$ Tiempo medio de respuesta ($W_i + S_i$).

* $U_i$ Porcentaje del tiempo que el dispositivo está ocupado.

* $N_i$ Número medio de trabajos en la estación de servicio (cola más
  recurso)
  
* $Q_i$ Número medio de trabajos en la cola de espera 

* $U_i$ Número medio de trabajos siendo servidos por el dispositivo
  ($B_i/T$)
  
**Variables globales del servidor:**

En lugar de tener el subíndice i tienen el subíndice 0. Se usan sólo
algunas variables de las anteriores, pues algunas no tienen sentido
(por ejemplo el tiempo ocupado).

**Básicas:** $A_0,\ C_0$
**Deducidas:** $\lambda_0,\tau_0, X_0, R_0, N_0$

<!-- 30/11/2017 -->

**Razón de visita y demanda de servicio**

La razón de visita nos informa de la proporción entre el número de
trabajos completados por el servidor y el número de trabajos
completados por una estación de servicio concreta.

$$V_i = \frac{C_i}{C_o}$$

La demanda de servicio, por su parte, es la cantidad de tiempo que,
por término medio, el dispositivo i-ésimo  ha dedicado a cada trabajo
que abandona el servidor.

$$D_i = \frac{B_i}{C_o} = V_i \times S_i $$

**Ejercicio:**
Después de monitorizar el disco duro de un servidor web durante un
periodo de 30 segundos, se sabe que ha estado en funcionamiento un
total de 27 segundos. Asimismo, se han contabilizado durante ese
periodo un total de 74 peticiones de lectura/escritura al disco duro y
un total de 72 peticiones completadas. Se ha estimado que cada
consulta atendida por el servidor web ha requerido una media de 4
accesos de E/S al disco duro. Calcule: 

a) ¿Cual es la tasa media de llegada al disco duro?

T = 30s
$B_{DD}$ = 27s (DD = Disco Duro)
$A_{DD}$ = 74 peticiones
$C_{DD}$ = 72 trabajos completados
$V_{DD} = \frac{C_{DD}}{C_o}$ = 4 (razón de visita)


$\lambda _{DD} = \frac{A_{DD}{T}} = \frac{74pet}{30s} = 2.47pet/s$

b) ¿Cuál es la productividad media del disco duro?

$X_{DD} = \frac{72}{30} = 2.4pet/s$

c) Determínese la utilización del disco duro, su tiempo de servicio y
su demanda de servicio.

Utilización del disco duro $U_{DD} = \frac{27s}{30s} = 0.9$

Tiempo de servicio: $S_{DD} = \frac{B_{DD}}{C_{DD}} = \frac{27s}{72} =
0.375$

Demanda de servicio: $D_{DD} = V_{DD} \cdot S_{DD} = 1.5s$

d)¿Cuál es la productividad media del servidor web?

$X_0 = \frac{C_0}{T} = \frac{18pet}{30s} = 0.6pet/s$

<!-- Faltan leyes operacionales hasta Ley de la Utilización -->

**Ley del flujo forzado:**

La productividad de cada dispositivo siempre es V veces la del
servidor (son proporcionales)

$$V_i = \frac{C_i}{C_o} = \frac{X_i}{X_o}$$


<!-- 7/12/2017 -->

<!-- 14/12/2017-->

## Técnicas de mejora

**Sintonización o ajuste (tuning):** Optimizar el funcionamiento de
las componentes del servidor que existen actualmente. Ejemplos de esto
pueden ser el overclocking, optimizar la configuración de los
dispositivos existentes o del mismo Sistema Operativo.

**Actualización o ampliación (upgrading):** Reemplazar los
dispositivos por otros más rápidos o añadir nuevos dispositivos
extra. Los principales problemas de esta estrategia es su mayor coste
y los problemas de compatibilidad con lo anterior.

## Algoritmos de resolución de modelos de redes de colas

Para resolver el modelo de redes de colas podemos proponer la
siguiente metodología.

Supondremos conocidos el número de estaciones de servicio (K).

En cada estación conoceremos la razón de visita medio de cada estación
($V_i$) y tiempo de servicio medio de cada estación ($S_i$).

Además si la red es abierta conoceremos la tasa de llegada al servidor
($\lambda _0$), y si es cerrada conoceremos el número total de
trabajos en la red ($N_T$) y si es interactiva, el tiempo medio de
reflexión de los usuarios Z.

**Hipótesis del peor caso posible:**

Dada una red de colas abierta supondremos que un trabajo que llega
tiene que esperar a que se procesen los $N_i$ trabajos que de media hay en
la estación, uno comenzando a ser servido y el resto esperando:

$$ W_i = N_i \cdot S_i$$

Luego, para ser procesado tendrá que esperar su propio tiempo de
servicio. Por tanto, el tiempo medio de respuesta vendrá dado por este
peor escenario posible:

$$R_i = W_i + S_i$$

De esto podemos deducir (aplicando que $N_i = \lambda_i \cdot R_I$):

$$R_i = \lambda _i \cdot R_i \cdot S_i + S_i \Rightarrow R_i =
\frac{S_i}{1-\lambda _i \cdot S_i} = \frac{S_i}{1-U_i} =
\frac{S_i}{1-\lambda _0 \cdot D_i}$$
