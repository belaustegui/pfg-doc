Rendimiento del Receiver
========================


Este documento explica las soluciones que se tomaron para mejorar el rendimiento
del *Receiver* y las partes que lo conforman: tanto el generador de SQL como el
Parser de XML.

Introducción
------------
Cuando los datasets empezaron a crecer en tamaño también empezaron a aparecer
los problemas de rendimiento y los fallos de memoria.  Fue en ese momento cuando
se hizo evidente la necesidad de optimizar el rendimieento (sobre todo el
consumo de memoria).

El primer paso fue realizar un *profiling* del funcionamiento del *Receiver*
[memory_profiler](https://pypi.python.org/pypi/memory_profiler) y ver así los
puntos en los que se podría realizar una optimización.  
Surgieron aquí dos puntos de optimización principales: la inserción de
observaciones y el parser.


Optimizaciones aplicadas
------------------------
En esta sección se enumerarán las optimizaciones que se aplicaron en el *Receiver*.

* **Utilizar un nuevo módulo de parseo.** Python incorpora una implementación del
parser *ElementTree* en lenguaje C. Esta implementación es mucho más rápida y
eficiente en memoria. Ambas implementaciones tienen la misma interfaz, por lo que
pueden intercambiarse fácilmente.
* **Instanciar un único parser.** Cada vez que se instancia el parser se crea
una representación en memoria en forma de árbol con el contenido del fichero XML,
esta representación ocupa unos 300mb en memoria por un fichero de 20mb. En
diferentes partes del código estaba creando hasta 4 instancias simultáneas, todas
ellas se sustituyeron por una única instancia compartida.
* **Usar generadores cuando sea posible.** En lugar de devolver listas de objetos,
que están ocupando mucho espacio en memoria, se pasó a utilizar generadores, que
van devolviendo cada elemento sólo cuando se le pide.
* **Reestructurar la arquitectura.** La capa *Helpers* fue creada originalmente
con el propósito de aislar las consultas al API.  Posteriormente decidí que las
consultas al API eran una mala decisión y que, puesto que ya existe una sesión
abierta, es mucho más sencillo consultar directamente la base de datos. Eliminar
esta capa permitió aumentar el rendimiento de forma notable, puesto que permitió
recorrer una vez menos las listas de objetos.
* **Inserciones parciales en la base de datos.** Originalmente estaba haciendo las
inserciones de objetos en la base de datos de forma conjunta (todos los
indicadores, todas las observaciones, etc).  El problema de esto es que la conexión
con la base de datos se rompe cuando el volúmen de inserciones es muy grande.  La
solución consistió en realizar inserciones de un máximo de 10000 elementos y,
posteriormente, eliminarlos de la sesión para que puedan ser recogidos por el
recolector de basura.
* **Crear índice inverso de Slices y Observaciones.** En el XML-Schema de los
ficheros que le llegan al *Receiver* está especificado que son las Slices quienes
contienen una lista con los IDs de las Observaciones asociadas.  Lo que hice
fue construir un índice inverso cuyas claves son los IDs de las Observaciones y
cuyos valores son los ID del Slice correspondiente. Esto nos ahorra una gran
consulta a la base de datos y nos permite devolver las observaciones con el ID
de su Slice asociado ya establecido.


Optimizaciones no aplicadas
---------------------------
En esta sección se enumeran las optimizaciones que no se aplicaron al *Receiver*
y se da una explicación de por qué no se implementaron.

* **Utilizar *iterparse*.** *Iterparse* utiliza una forma de parsear los XML
similar a SAX. Esta forma de parsear consiste en utilizar eventos en los que se
llama a diferentes funciones, en lugar de construir directamente todo el árbol de
objetos en memoria.  Esta optimización no se aplicó porque obligaría a cambiar
bastante el código del *Parser*. Además el resto de optimizaciones ya consiguieron
un rendimiento suficientemente bueno.
