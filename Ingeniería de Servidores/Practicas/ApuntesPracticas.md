---
title: Prácticas Ingeniería de Servidores						# Título
author: Jesús Sánchez de Lechina Tejada		# Nombre del autor
header-includes:      	 	        	# Incluir paquetes en LaTeX
toc: true                   			# Índice
numbersections: false       			# Numeración de secciones
fontsize: 11pt              			# Tamaño de fuente
geometry: margin=1in        			# Tamaño de los márgenes
---
<!-- Info Asignatura: 
Profesor de prácticas: Jorge
Correo: jorgesg@ugr.es
-->

# Práctica 1

## Conceptos previos
**RAID:** permite que diversos discos duros se comporten como uno solo.

Hay distintos tipos de RAID:
**Por software:** Divide los datos y escribe en cada disco duro. Esto supone una carga en el CPU.

* **RAID0:** Necesita al menos dos discos duros. Se divide los datos
  en dos partes y se escribe en cada disco duro, si se pierde la
  información en uno se pierde toda la información. 

* **RAID1:** Se duplica la información en el segundo disco, cual copia
  de seguridad, para evitar la pérdida de la información si uno de los
  discos falla 

* **RAID5:** Necesitamos al menos tres discos duros, dividimos los
  datos en dos partes, guardamos cada mitad en cada uno de los dos
  primeros discos duros y con el tercero usamos bits de paridad para
  mantener la seguridad que nos proporciona RAID1. 


**Por hardware:** Retira la carga de la CPU y se encarga directamente
de esto el hardware, suponiendo un coste económico mayor. 

**LVM:** Es un administrador de volúmenes lógicos que permite crear
particiones lógicas en Linux (Logical Volume Manager). 

**Tipos de virtualización:**
Por **software:** Un sistema operativo anfitrión (Host) da lugar al
uso de diversos sistemas operativos mediante el uso de una máquina
virtual. 

Por **hardware:** Es la virtualización de ordenadores como plataformas
hardware, abstrae el funcionamimento de una máquina para emularla. 

Dockers, contenedores, es una tecnología reciente que permite la
compartición de recursos entre máquinas virtuales y anfitrión. 

**MD:** Es una utilidad (dispositivo) de Linux para gestionar y
monitorizar los RAID. 

**FDE:** Full Disk Encryption, hace ilegibles los datos del disco duro
para aquellos que no tengan permisos. Full Disk Encryption, en
concreto indica que todo en el disco salvo el gestor de arranque está
cifrado. 

## Instalación de Ubuntu Server

Para instalar Ubuntu Server queremos un RAID1, con LVM y cifrado.

Tendremos tres particiones para home, boot y swap.

LVM: Permite crear abstracciones y redimensionar fácilmente las
particiones del disco duro. Permite unir distintos volúmenes físicos
en grupos de volúmenes que permite un alto nivel de abstracción para
gestionar el espacio en disco. 

Consta de 4 capas, Almacenamiento real, volúmenes físicos, grupos de
volúmenes y volúmenes lógicos. 

MBR/GPT son sistemas de particiones, MBR sólo podía trabajar con 4
particiones físicas y con un límite de 2'2TB. 

### Preparando la máquina virtual

1. Creamos la máquina con todo por defecto, Linux de 64 bits. Se puede
   bajar la memoria RAM hasta 512/1024MB. 

2. Creamos un nuevo disco duro virtual. Usando VDI para el tipo de
   disco duro. 

3. A la hora de elegir el tamaño podemos elegir el reservado dinámico
   o estático, en este caso usaremos el dinámico para evitar consumir
   los recursos de nuestro ordenador. 

4. Le otrogaremos 10 GB, que es un tamaño máximo que se podrá reservar
   dinámicamente (En caso haber seleccionado dinámico en el paso
   anterior). 

5. Procedemos a montar la iso en la configuración de la máquina
   virtual con sus dos discos duros correspondientes, preparando en
   almacenamiento el contador SATA. El disco lo montaremos
   seleccionando la imagen correspondiente en la unidad óptica en el
   apartado "Almacenamiento". En el controlador SATA añadiremos un
   nuevo disco duro de exactamente las mismas propiedades para dar
   soporte a nuestro RAID1. 

Ahora iniciaremos nuestra máquina virtual y realizaremos las
configuraciones previas de idioma, ubicación y disposición de
teclado.  

6. Pondremos el nombre de nuestra máquina y nuestro usuario. Como
   contraseña usaremos "**practicas,ISE**". Y posteriormente no
   permitiremos cifrar los discos, de este paso nos encargaremos
   después nosotros mismos. 

7. Usaremos un método de particionado manual.

8. Aquí tendremos los dos discos duros (sda y sdb), sda --> Nueva
   tabla de particionado, idem para sdb 
   
9. Seleccionaremos configuración por software, aceptaremos los cambios
en las tablas de particiones y crearemos un dispositivo "MD" y
usaremos RAID1. 

<!-- Posible pregunta de examen, qué es un MD--> 
10. Indicaremos que usaremos dos discos duros activos(2 y 0 usaremos),
    y seleccionamos (con espacio) los dos discos duros. Finalmente
    confirmamos los cambios. 

Aquí se deberían de apreciar los diversos discos creados, un solo RAID con el tamaño total.

<!--Insertar foto img/P1_raid_md.JPG-->

Diferenciamos aquí lo que es FDE, pues no queremos cifrar la carpeta personal. <!-- Posible pregunta de examen-->

11. Procedemos a configurar los volúmenes lógicos (en la configuración
    de LVM). Nombre del grupo de volúmenes: "Servidor", y ahí metemos
    nuestro RAID (/dev/md0). 

12. Creamos cuatro volúmenes lógicos: Dentro del grupo de volúmenes
    "Servidor" indicamos el nombre del volumen "Arranque", que
    recibirá 200MB. Posteriormente crearemos el volumen "Hogar", de
    800MB. Tercer volumen "raiz", que se otorga lo que le quede en el
    grupo de volúmenes menos 2048MB, al swap se le da el doble de la
    RAM disponible (2GB). 

Entonces aparecerán los cuatro grupos de volúmenes lógicos.

<!--Insertar img/P1_logic_volumes.JPG-->

A continuación deberemos proceder al cifrado de volúmenes

### Cifrado

Cremos una nueva encriptación, pero sólo cifraremos los dispositivos
hogar, swap y raíz. (La swap se encripta por seguridad, pues se podría
hacer un volcado de datos en la swap y se obtendrían los datos sin
cifrar). 

Utilizaremos la configuración de la partición por defecto. Le daremos
a terminar y ponemos nuestra contraseña de cifrado, la misma anterior
de "practicas,ISE" (La contraseña de cada disco cifrado tendrá que ser
introducida dos veces). 

En nuestra tabla en este momento aparecerán los volúmenes cifrados.

<!-- Insertar img/P1_encrypted.JPG-->

En este momento ya tenemos las particiones cifradas que nos interesan
y procederemos a configurar los puntos de montaje. 

### Puntos de montaje

Seleccionamos cada partición para usarla con la extensión
correspondiente, ext4 para todas salvo para swap, /boot se montará en
esta primera partición de arranque. 

Posteriormente configuraremos el cifrado para /home, con ext4. 

Después el RAID cifrado, utilizarmos con ext4 y punto de montaje raíz "/"

Por último el área de intercambio, swap, lo usaremos como área de intercambio, sin indicar punto de montaje.

<!--Insertar img/P1_mount.JPG-->

Aquí finalizamos el proceso de particionado y escribimos los cambios
en el disco duro. En un momento nos solicitará información sobre el
proxy que dejaremos vacío. E ignoraremos las actualizaciones
automáticas. 

Los programas a instalar los dejaremos por defecto. Aceptaremos la
instalación del boot en el disco duro. 

sudo grub-install /dev/sdb para instalar el grub en el sdb (nuestro
segundo disco duro) 

Con esto se debería de concluir la instalación de Ubuntu Server, tras
reiniciar deberíamos de meter tres veces la contraseña 

Con lsblk podemos observar que tendría que ser coherente la
información de las particiones en los dos discos duros. 

## Instalación de CentOS

<!-- El enlace de la descarga lo podemos encontrar en SWAD-->

En CentOS tendremos un único disco duro, con una partición física y
otra mapeada.

La preparación de la máquina es el mismo que para ubuntu, por defecto
todas las opciones iniciales.

Del mismo modo añadimos en el controlador de la unidad óptica la
imagen e iniciamos la **instalación por defecto**.

1. Le damos a instalar CentOS. Tras unos instantes de preparación
   pasará a la selección de idioma. (En nuestro caso, inglés,
   indicando el *layout* español) 
   
2. En la siguiente pantalla en sistema lo dejaremos por defecto, solo
   tendremos un único disco y pasaríamos al siguiente paso.
   
3. Crearemos el *Root*, estableciendo la contraseña **practicas,ISE**,
   la misma que usábamos anteriormente. Y tras esto esperaremos a la
   finalización de la instalación.
   
4. Podemos observar, al finalizar la instalación, los discos que hay
   con `lsblk`, y obtener más información con `mount | grep
   sda`. Otros comandos de interés, com `df`, con la opción “-h”
   muestran todos los volúmenes lógicos, su punto de montaje y el
   espacio que ocupan. `lvmdskscan` muestra más información sobre los
   volúmenes lógicos, así como `lvdisplay ` muestra más información
   al respecto. 
   
   
   <!-- Posibles preguntas de examen pueden ser comprender estos -->
   <!-- comandos y sus opciones y saber para qué los usamos. -->
   
5. Una vez hecho esto apagamos la máquina y en las opciones de
   configuración-almacenamiento añadimos un nuevo disco duro por
   defecto.
   
6. Creamos entonces nuestro volúmen físico con `pvcreate /dev/sdb`

7. Procederemos pues a crear nuestro grupo de volúmenes. El comando
   `vgdisplay` nos proporciona información sobre estos
   grupos. Crearemos el grupo “cl” con el comando <!--Buscar comando-->
   
8. `vgextend cl /dev/sdb` Extendemos el grupo que hemos creado
   previamente para añadir el último volumen físico creado.
   
9. `lvcreate -L 4G -n newvar cl` nos permite crear un nuevo volumen
   lógico de 4 GB. Podemos usar `lvdisplay` para obtener más
   información sobre estos volúmenes.
   
10. `mkfs -t ext4 /dev/cl/newvar` nos permite crear un sistema de
    archivos sobre el volumen lógico que habíamos creado en el paso
    anterior.
	
11. Para probar si esto ha funcionado procederemos a montar el volumen
    que hemos creado con `mkdir /mnt/vartmp` y luego con `mount
    /dev/cl/newvar /mnt/vartmp` y posteriormente comprobar que podemos
    camiar a ese directorio y la información que se nos proporciona es
    correcta. Con `df -h observaremos` el punto de montaje y más
    información al respecto.
	
12. `systemctl isolate runlevel1` cambia el modo, así si usamos
	`systemctl status` veremos que estamos en modo mantenimiento. Así
	ya podremos hacer la copia del contenido de /var `cp
	--preserve=context(<o all en su defecto>) -r /var/* /mnt/vartemp`
	(o en lugar de la opción `--preserve=<opcion>` podemos usar `-a`.

	<!-- systemctl isolate runlevel1.target -> modo monousuario
	 en la documentación de redhat consultar esto
	 
	 access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/...
	 
	 Como pregunta de examen se puede preguntar para qué sirve esto
	 -->
	 
	 
13. Una vez hecha la copia en el nuevo volumen lógico es
    correspondiente que el punto de montaje temporal sea desmontado
    con `umount /mnt/vartmp/`
	
14. Con `mount /dev/cl/newvar /var` observamos que montamos nuestro
    volumen lógico en var, pero esto desaparecerá tras un reinicio así
    que desmontamos con umount y buscamos la manera de hacerlo
    permanente.
	
15. En /etc modificamos el archivo `/etc/fstab` conforme a la sintaxis
    del fichero. Con blkid observamos el id de cada disco. Aquí habría
    que **hacer una copia de seguridad de fstab** por si se corrompiera
    el fichero. Una vez nos hemos quedado con el id podemos añadirlo a
    fstab con `blkid | grep newvar >> /etc/fstab`. La línea tiene que
    quedar tal que `<UUID> /var (punto de montaje) ext4  default 0
    0`. Este archivo podemos editarlo con vi. <!--Suerte si no se ha
    usado nunca vi-->
	
	
16. Montamos todo con `mount -a` y comprobamos con `df -h` que todo
    esté correcto.
