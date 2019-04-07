# Tema 1: Introducción

Las redes IP no están preparadas para dar soporte a todo tipo de aplicación. Para algunas es necesario unos requisitos mínimos que se conocen como **"Calidad de Servicio"**.

## Requisitos de las aplicaciones en red

Los principales tipos de requisitos que pueden surgir a la hora de diseñar una aplicación multimedia en red son:

* **Fiabilidad en la transferencia de datos:** Hay que decidir si la aplicación es tolerante a pérdidas o no.

* **Ancho de banda:** El tráfico puede ser **elástico**, cuando no necesitan una tasa fija de transmisión como hacer una transferencia de ficheros, o puede ser **inelástico**, cuando necesitan una tasa fija (un flujo continuo) para que funcione debidamente, por ejemplo la radio por internet.

* **Requisitos de tiempo acotado:** Que la respuesta se reciba en tiempos pequeños para poder hacer un buen uso de la aplicación. Por ejemplo los juegos en red.

Las **aplicaciones en tiempo real:** Requieren ancho de banda mínimo, retardo acotado (especialmente en aplicaciones interactivas), pero suelen ser tolerantes a pérdidas según la aplicación (p.ej. en VoIP puedes entender un mensaje si llega un poco entrecortado).

## Limitaciones del servicio Best-effort

Actualmente internet es una red que une millones de dispositivos usando TCP/IP, pero tiene un problema, y es que ofrece un servicio de conmutación de paquetes orientado a datagramas que no ofrece garantías de retardo ni orden de llegada.

Sus principales dificultades para estas aplicaciones son:

* **Ancho de banda compartido no garantizado**

* **Retardo variable:** Debido a las esperas en los routers intermedios

* **Pérdida de paquetes:** Aunque una tasa baja de pérdidas no degrada la calidad del servicio notoriamente, grandes ráfagas de pérdidas pueden afectar a la calidad percibida. (Se consideran pérdidas tanto pérdidas en sí como aquellas que llegan con retardo superior al límite máximo esperado)

TCP/IP utiliza como principales protocolos de transporte TCP y UDP.  **TCP** orientado a conexión, fiable y ordenado, que lleva a cabo un control de congestión y no ofrece garantías en los retardos. **UDP** es no orientado a conexión, no fiable y no ordenado; no realiza ningún control de la congestión, permitiendo una tasa de transmisión fija pero sin asegurar su recepción; tampoco ofrece garantías en los retardos.

## Definición Calidad de Servicio (QoS)

Un conjunto de parámetros objetivos y subjetivos que se le prometen a un usuario para definir el grado de satisfacción esperado.

Existen distintos tipos de medición de la QoS:

* QoS planificada: La planificada por el proveedor

* QoS conseguida: Lo que realmente ha proporcionado el proveedor

* QoS percibida: La que el usuario que puede ser percibida de distinta manera al proveedor

* QoS inferida: La calidad que determina el proveedor basándose en el análisis de la opinión del usuario y la QoS conseguida que se está midiendo.

### Parámetros medibles

* **Tasa de transferencia (trhoughput):** bits por unidad de tiempo. Bitrate/tasa de datos. Este bitrate piede ser constante **CBR** o variable **VBR**. Puede ser menor la percibida por el usuario que la que existe en la red por el overhead en los paquetes. En *VBR* hay que distinguir las tasas promedio **MBR** y las tasas en los períodos de *pico* **PBR**, así como la duración de estos picos y el ratio de ráfaga (MBR/PBR).

* **Retardo extremo a extremo:** Retardo de acceso (tiempo hasta que el medio puede aceptar la transmisión de datos), tiempo de transmisión, tiempo de propagación y tiempo de ida y vuelta RTT

* **Variación de retardo (jitter):** Diferencia entre retardo de paquetes

* **Tasa de error:** Recoge errores por alteración de datos, pérdida de paquetes, duplicación de datos y entrega desordenada. *Medidas:* Tasa bits erróneos (BER), tasa paquetes erróneos (PER), tasa paquetes perdidos (PLR)

También existen otras medidas relativas al soporte de la red y retardos.

## Características del tipo de tráfico

### Tráfico elástico

No se requiere una tasa de transferencia. Puede ser postergable o impostergable (correo electrónico/descarga de una página web).

Requisitos QoS: Tolerable a retardos. Tolera distintas tasas de transferencia. No tolera pérdidas de datos. Esto requiere una garantía de QoS mínima

### Tráfico inelástico

La aplicación tiene una importante carga temporal (p.ej. streaming de vídeo)

Requisitos: Retardo de transferencia controlado, retardo varible controlado, cierta tolerancia a pérdidas determinada por la aplicación.

\newpage

# Tema 2: Mecanismos y arquitecturas de red para la provisión de QoS

## Problemas de la arquitectura actual

La arquitectura best-effort proporciona las funcionalidades mínimas para conseguir la conectividad entre extremos. Intenta entregar los paquetes a su destino en un período de tiempo razonable. Son los nodos extremos los que los que mantienen estados sobre las conexiones y no los intermedios. Del mismo modo no se garantiza una equidad en el tráfico entre distintos extremos. Intenta usar el camino más corto con métricas que no están relacionadas significativamente con la QoS. Además no ofrece ninguna cuota para la latencia y el jitter.

En resumen: no dan una respuesta temporal predecible, no distingue el tipo de tráfico, no acepta peticiones dinámicas de QoS y tiene una capacidad de monitorización limitada.

Para poder proveer una QoS deseada es necesario centrarse en el tratamiento de los encaminadores según el tipo de tráfico y cómo estos aprovechan las características de QoS de sus enlaces. Hay que abordar las tareas de **señalización**, para solicitar extremo a extremo una QoS determinada y **contabilidad del uso de recursos de red**, para autenticar las solicitudes y monitorizar el uso de recursos.

## Mecanismos de provisión de QoS

La calidad de servicio observada de extremo a extremo es la que se obtiene de la concatenación de la QoS ofrecida por cada salto en la ruta del paquete.

En la red IP las pérdidas y retardos se deben principalmente al comportamiento en situaciones de congestión, donde suelen usar colas FIFO por lo general.

Para proteger un tipo de tráfico hay que satisfacer lo siguiente:

* **QoS por salto:** Mínimo elemento controlable de QoS, debe aprovechar las características de sus enlaces.

* **Ingeniería de tráfico y encaminamiento:** Utilizar múltiples caminos alternativos.

* **Señalización:** Protocolos para definir y establecer la QoS de la red.

### Procesamiento de paquetes por salto en un encaminador genérico (sin QoS)

Tiene *interfaces de entrada y salida*, *máquina de reenvío* que envía a la interfaz correspondiente según la IP destino y *máquina de gestión* para descartar paquetes en situaciones de congestión.

### Procesamiento de paquetes por salto en un encaminador con CQS

**CQS:** Clasificador, encolado y planificador (Classifier, queue, schedule)

Además de tener los elementos de un encaminador general se le añaden las fases de:

* **Clasificiación y consulta de la FIB** (Forwarding Information Base) para identificar el paquete y su interfaz de destino

* **Verificación de conformidad y marcado** para actuar en caso de que el paquete no haya llegado en el período marcado. Se comprueba que la clase de tráfico esté cumpliendo con su *perfil de tráfico*. Este define la velocidad de llegada de los paquetes o el máximo que puede enviar en un intervalo de tiempo. Para esto se puede usar la técnica del cubo horadado que se explicará más adelante. El marcado puede ser útil para suavizar las características del tráfico en los extremos de la red, aunque no es la opción más efectiva.

* **Encolado y planificación** para reenviar el paquete según las reglas de compartición de enlace o de conformado de tráfico, o descartarlo según la política de descarte en caso de congestión.

### Elementos para la provisión de QoS

* **Clasificador:** Identifica la clase del paquete, se mantienen N bits en la cabecera y se toma este campo como *clave de clasificación*. Estas claves se comparan con unas *reglas de clasificación* para actuar de acorde a ellas. Alternaticamente se puede usar una clasificación multicampo si se utilizan campos de IP, protocolo o puerto.

* **Colas por clase de tráfico**

* **Planificador**  para decidir el orden de servicio en las colas. Se puede modificar la frecuencia de servicio a una clase de tráfico determinada para realizar el *rate shaping*, acotando así el comportamiento de la clase y evitando ráfagas que producirían descartes en encaminadores posteriores. Algunos ejemplos de planificadores simples son planificadores con **prioridad estricta** o con **Round Robin**(orden y turnos por cola). Otros planificadores pueden ser adaptativos, **RR deficitario** con turno activo mientras queden bytes pendientes para la tasa media esperada o **Encolado equitativo con o sin ponderación**, que recalcula la secuencia de turnos para dar servicio al que menos se acerca a su tasa mínima.

* **Conformación de tráfico (traffic shaping):** Para controlar el servicio a un flujo concreto se limita el ancho que consumirá.

* **Verificación de conformidad (policing):** Se descartan los paquetes cuando llegan demasiados sin ajustarse a los límites acordados.

### Conformación de tráfico

El mecanismo más extendido para regular la tasa con la que se le pueden inyectar paquetes de un tipo determinado a la red es el **cubo horadado/leaky bucket**. Se pueden procesar hasta un número limitado de testigos, uno por cada paquete a introducir. Para que el paquete sea retransmitido hay que eliminar el testigo del cubo. Los testigos se introducen en el cubo a una tasa determinada en función del tiempo.

Esto permite limitar tanto el número máximo de paquetes enviados en un instante (limitado por el número de testigos) y limitar la tasa de retransmisión a un máximo definido por la tasa de generación de testigos.

### Gestión de colas

Mantiene y establece las colas en el encaminador. Puede añadir o sacar paquetes de la cola, descartarlos, monitorizar la cola para reducir el retardo del encolado y poder soportar ráfagas de tráfico.

Se puede reducir la ocupación de las colas mediante algunos mecanismos como la **Señalización en banda**, notifica explícitamente de la congestión marcando sus paquetes. O también descartando los paquetes en la cabecera de la cola.

Se pueden usar algunas medidas explícitas para prevenir la congestión, por ejemplo la detección temprana aleatoria (Random Early Detection) haciendo uso de umbrales para detectarla. De esta técnica han surgido variantes que se basan en la ponderación de distintas clases de tráfico, adaptativas (dinámicas) o por flujo.

### Reordenación de paquetes

Los protocolos en los extremos no suelen manejar bien que los datagramas lleguen desordenados, por tanto se pueden reordenar los paquetes marcados y los conformados. De hecho es una tarea recomendable.

### Encaminamiento con QoS

Para mejorar el encaminamiento se consideran distintas métricas en función de los costes de los enlaces, entonces se elaboran tablas para cada tipo de requisito (retardo, ancho de banda...) y cada paquete es enviado según el tipo de su clase y los requisitos de la misma.

Esto sin embargo tiene un coste adicional, primero el trabajo de mantener tantas tablas y algún tipo de identificador en los paquetes, pero además se sobrecarga la red para mantener estas métricas que pueden ser muy dinámicas.

## Ingeniería de tráfico y control explícito de arquitecturas

Es necesario ser capaz de balancear la carga mediante el establecimiento de rutas alternativas para evitar la congestión. A veces se utilizan rutas que no sean las óptimas para a medio plazo optimizar la infraestructura de la red.

Otras opciones son:

* Encaminamiento desde la fuente

* Tablas de encaminamiento con varios parámetros

* Túneles IP (forzar una ruta concreta)

* MPLS (MultiProtocol Label Switching): Los paquetes son etiquetados y reenviados según unas rutas preconstruidas por etiquetas.

## Modelos de red

Los anteriores esquemas deben implementarse en un modelo de red. Los más representativos actualmente son:

* Servicios Integrados (IntServ)

* Servicios Diferenciados (DiffServ)

* MultiProtocol Label Switching (MPLS)

En todos ellos existen encaminadores fronterizos que aceptan el tráfico de los sistemas finales y centrales, que conectan los nodos del borde y los intermedios.

Los nodos fronterizos llevan a cabo tareas de conformado, marcado y control de admisión. Los nodos centrales dan un trato diferenciado a los paquetes de distintas clases para afrontar períodos de congestión.

### Servicios integrados

Garantiza QoS a aplicaciones individuales. Cada sesión especifica la reserva (Rspec) y la especificación del tráfico a generar (Tspec) antes de comenzar.

Define dos clases de aplicaciones, las de tiempo real y las convencionales.

En este modelo hay tres tipos de elementos de red: Elementos habilitados, sin capacidades y QoS-aware.

#### Clases de servicios básicos IntServ

* **Servicio de calidad garantizada:** Ofrece una cota superior para los retardos en cola, la aplicación informa del carácter de su tráfico y la red le devuelve la latencia garantizada.

* **Servicio de carga controlada:** Se garantiza a la sesión que el porcentaje de datagramas descargado y el retardo en cola será similar al que existiría en una red sin congestión, un alto porcentaje de paquetes serán transmitidos y tendrán un retardo similar.

#### Señalización IntServ: RSVP

Es necesario proveer de algún mecanismo para coordinar los comportamientos de cada camino. La señalización realiza tanto negociación (control de admisión) como configuración para los elementos intermedios en la red.

Las fuentes y los receptores se envían periódicamente mensajes describiendo el tráfico y la calidad esperada.

La descripción del tráfico **SENDER_TSPEC** se caracteriza mediante el uso del cubo horadado de tasa r y capacidad b; tasa de datos pico p; unidad mínima de verificado de conformidad m (si un paquete tiene un tamaño menor se le asigna ese tamaño al usarlo en el cubo); tamaño máximo de paquete M.

RSVP no selecciona ninguna ruta ni encamina con QoS, las reservas se llevan a cabo en la ruta más corta, ya que sin modificar el comportamiento del siguiente salto no se puede forzar un camino diferente y no se adoptan aún protocolos de encaminaminto con QoS entre dominios.

**Requisitos:** Clasificación multicampo; varias colas, una por flujo y al menos otra para el tráfico no conformado; medición del tráfico mediante el uso de cubos horadados; capa de gestión que soporte RSVP

### Servicios Diferenciados

Reduce la complejidad de la arquitectura desplazando las tareas de clasificación, conformado, verificación y marcado en los encaminadores de acceso. Esto permite hacer encaminadores centrales más rápidos y conmenos requisitos de almacenaje de información.

Los nodos fronterizos utilizan la clasificación multicampo para obtener solo unas pocas clases agregadas. DiffServ define comportamientos por salto, bloques funcionales que permiten construir servicios extremo a extremo. Los administradores de red serán los encargados de combinar dichos bloques para ofrecer servicios concretos.

Este tipo de red se llama **dominio DiffServ^**. Los nodos que rodean un dominio se llaman nodos fronterizos DS y pueden ser de entrada(ingreso)/salida(egreso) o interiores. El acondicionamiento del tráfico se realiza en los nodos de ingreso (usando clasificación multicampo, conformado de tráfico y marcado), según el acuerdo de acondicionamiento del tráfico(TCA). Los flujos de cada aplicación se llaman microflujos y varios de ellos compartirán el mismo DSCP (para clasificación).

#### Comportamientos por salto PHB

Especifican el comportamiento de encolado, gestión de colas y planificación, sin especificar la técnica a usar. Los PHB con comportmiento similar se agrupan en grupos de PHB.

#### Clases de servicios DiffServ

Los propuestos actualmente son:

* **PHB expedito (EF):** Para cada clase se especifica la salida mínima a obtener independientemente del tráfico, se comporta como si las colas tuvieran baja ocupación. Requiere conformar el tráfico a la entrada del dominio y configurar el planificador para superar la tasa de llegada del agregado sin que afecte al tráfico no EF. Se usa para aplicaciones con requisitos de bajo retardo, jitter y pérdidas.

* **PHB de envío asegurado (AF):** Define un grupo de PHB, se clasifica según el servicio y la precedencia de descarte. Para no reordenar paquetes hay que utilizar diferentes colas para cada servicio, y los paquetes de un mismo flujo irán a la misma cola.

##### Consideraciones para DiffServ

En el encaminador de ingreso se lleva a cabo el *Acondiciomiento de tráfico*. Necesita una política de verificación  ce conformidad agresiva y sobredimensionar el núcleo de la red. Presenta problemas con otros protocolos como TCP.

IntServ y DiffServ pueden desplegarse de forma cooperativa. IntServ se aplicaría para acceder a la red DiffServ.

## ¿Qué es MPLS?

MultiProtocol Label Switching es una tecnología de reenvío de paquetes que se basa en etiquetas. Se utiliza en redes de área amplia. La etiqueta se inserta entre las cabeceras de nivel de enlace y de interconexión de redes.

Se puede usar para implementar VPN, ingeniería de tráfico, QoS, cualquier transporte sobre MPLS o reinicio rápido de rutas. Reduce la complejidad en el reenvío en los routers.

### Conceptos básicos

* **Clase de equivalencia de reenvío FEC:** Similar a la definición de clase

* **Etiqueta:** Ideentificador FEC

* **MPLS Label Switched Path:** Camino conmutado por etiquetas, unidireccional y compuesto por routers múltiples

* **Routers MPLS:** Tienen nodos de ingreso LER, de tránsito (LSR) y de egreso.

* **Tunel LSP:** La ruta LSP no tiene por qué seguir el mismo camino que el indicado por el algoritmo de encaminamiento correspondiente.

#### Etiqueta MPLS

Tiene una longitud fija de 32 bits donde se usan 20 bits de etiqueta, 3 bits para indicar QoS, 1 bit para indicar si la etiqueta es la última de la lista de etiquetas y 8 para TTL (Time To Live como en IP).

### Señalización MPLS

Para usar un LSP es necesario señalizarlo entre los distintos encaminadores, cada etiqueta es un valor local para cada enlace.

Hay dos protocolos de encaminamiento principales:

* **Protocolo de distribución de etiquetas, LDP** que suele usarse en MPLS VPN

* **Protocolo de reserva de recursos con ingeniería de tráfico, RSVP-TE**

En algunas redes puede necesitarse usarse ambas.

### Reencaminamiento rápido

Mejora la convergencia en caso de fallo en un camino. Para ello se precalculan varios caminos de respaldo por si falla un enlace en lugar de calcularlo cuando se produce el problema como pasa en IP. Este nuevo camino puede activarse en milisegundos, mucho más rápido que en IP.
