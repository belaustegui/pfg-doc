#Backend de un portal de datos e información sobre la Tierra

Este Trabajo Fin de Grado fue realizado por
[Cristian Álvarez Belaustegui](http://belaustegui.github.io/about/) y dirigido
por José Emilio Labra Gayo dentro del grupo de investigación
[Web Semántica Oviedo (WESO)](http://www.weso.es/).  La lectura del proyecto
tuvo lugar el día 23 de Julio de 2014 en la
[Escuela de Ingeniería Informática de Oviedo](http://www.ingenieriainformatica.uniovi.es/).

##Resumen

El presente proyecto tiene como objetivo la creación de la infraestructura de
soporte  para un portal de datos abiertos.  Concretamente el portal será el
nuevo [Land Portal](landportal.weso.es), propiedad del IFAD (Fondo Internacional para
el Desarrollo Agrícola) y perteneciente a la ONU (Organización de las Naciones
Unidas).  Este Proyecto Fin de Grado forma parte del proyecto *Rebuilding
IFAD's LandPortal - RFQ/2013/016/SC* desarrollado por el grupo de investigación
[Web Semántica Oviedo (WESO)](http://www.weso.es/) y la empresa
[SB Consulting](http://www.sbc4d.com/).

El sistema resultante está formado por dos secciones principales:

- Por un lado una sección orientada a los datos (LandBook), que permite
la publicación y consulta de catálogos de datos en varios formatos
como JSON, XML o RDF.  Los catálogos de datos almacenados en el
sistema son utilizados, entre otros, por unas visualizaciones que los
presentan de forma gráfica y atractiva con el objetivo de facilitar su
interpretación por parte de los usuarios.

- Por otro lado una sección social (LandDebate) que permite la creación de
noticias, eventos y debates con los que fomentar la participación e
implicación de los usuarios.  Esta sección también alberga el blog
del nuevo Land Portal.

Una parte importante del sistema es su soporte a la internacionalización en todas
sus secciones.  La internacionalización es completa e incluye tanto las interfaces
de usuarios como el propio modelo de datos.  Esta característica permite que el
nuevo Land Portal sea utilizado por multitud de personas y organizaciones
pertenecientes a diversos países.

El sistema está desarrollado utilizando varias tecnologías diferentes: varios lenguajes
de programacion (PHP, Python JavaScript), un gestor de contenidos o CMS (Drupal),
un catálogo de datos (CKAN), una base de datos relacional (MySQL), una base de
datos no relacional (Virtuoso) y un framework de desarrollo web (Flask).

Para la realización de este proyecto se han utilizado los conocimientos adquiridos
durante los estudios del Grado en Ingeniería Informática del Software.
