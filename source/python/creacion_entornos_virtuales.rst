.. -*- coding: utf-8 -*-

.. _creacion_entornos_virtuales:

=====================================
Creación de entornos virtuales Python
=====================================

.. sidebar:: Sobre este artículo

    :Autor(es): Leonardo J. Caballero G.
    :Correo(s): leonardocaballero@gmail.com
    :Compatible con: Python 2.4 o versiones superiores
    :Fecha: 31 de Diciembre de 2013

.. _que_es_virtualenv:

¿Qué es virtualenv?
===================

`virtualenv`_, es una herramienta para crear entornos virtuales (aislados) en Python.


.. _por_que_virtualenv:

¿Por que crear entornos virtuales en Python?
============================================

Si usted está en un sistema Linux, BSD, Cygwin, u otros similares a Unix como
sistema operativo, pero no tienen acceso al usuario root, puede crear su
propio entorno virtual (instalación) Python, que utiliza su propia biblioteca de
directorios y algunos enlaces simbólicos hacia todo el directorio de instalación 
del Python de su sistema.

En el caso más simple, su instalación virtual de Python que viven bajo el
directorio `home` del usuario ~/. Utilice la opción ``--help`` para obtener la
lista completa de las opciones disponibles la herramienta :command:`virtualenv`.

Cuando haya terminado la creación del entorno virtual, tendrá un ejecutable
de python local al usuario que lo creó (por ejemplo :command:`~/bin/python`) 
que está vinculado a la instalación del Python de su sistema :command:`/usr/bin/python` 
y hereda todas sus librerías actuales, pero además le permite añadir nuevas librerías 
tanto como usted lo desee. 

Sólo tiene que utilizar este nuevo Python en lugar de la instalación Python 
de su sistema, y puede modificarlo a su gusto sin dañar nada del Python de su 
sistema operativo. De igual forma usted debe seguir usando las instrucciones de 
instalación estándar para instalar setuptools y EasyInstall o Distribute y pip, 
desde su nueva instalación (:command:`~/bin/python`) Python en lugar del Python 
de su sistema :command:`/usr/bin/python`.


Entornos virtuales de Python locales al usuario
-----------------------------------------------

Para evitar usar la instalación base del Python de tu sistema, que
previamente tiene instalada, se recomienda instalar un entorno de virtual de
Python local al usuario, algunos casos de usos para virtualenv, se describe a
continuación:

-   No es necesarios permisos de administración para instalar librerías y
    aplicaciones Python, ya que estas se hace locales en al directorio del
    usuario.

-   Mayor comodidad de trabajar con versiones de librerías y aplicaciones
    más actuales las que maneja tu sistema.

.. _instalacion_virtualenv:

Modos de Instalación
====================

Para instalar paquete :command:`virtualenv` en su sistema puede instalarlo con 
:ref:`Setuptools <que_es_setuptools>`, :ref:`Distribute <que_es_distribute>` 
para :term:`paquete Egg` o por sistema paquete Debian


Instalación con paquete Debian 
------------------------------

Para instalar :command:`virtualenv` en distribuciones basadas en Debian GNU/Linux 
como paquete ``Debian``, debe instalar los requisitos previos con el siguiente 
comando: 

.. code-block:: sh

    # aptitude install libc6-dev python-dev python-virtualenv

.. note::

  A veces es mejor instalar la versión más reciente del paquete :command:`virtualenv`
  desde el repositorio :term:`PyPI`, debido que siempre la versión de Debian no esta 
  actualizada con respecto a la versión publicada en el repositorio :term:`PyPI`. 


Instalación con Setuptools
--------------------------

Para instalar :command:`virtualenv` en distribuciones basadas en Debian GNU/Linux 
con :ref:`Setuptools <que_es_setuptools>`, debe instalar los requisitos previos 
con el siguiente comando: 

.. code-block:: sh

    # aptitude install libc6-dev python-dev python-setuptools

Luego debe instalar la versión más reciente del paquete :command:`virtualenv`
desde el repositorio :term:`PyPI`, entonces debe instalar con el siguiente comando: 

.. code-block:: sh

    # easy_install virtualenv


Instalación con Distribute
--------------------------

Para instalar :command:`virtualenv` en distribuciones basadas en Debian GNU/Linux 
con :ref:`Distribute <que_es_distribute>`, debe instalar los requisitos previos 
con el siguiente comando: 

.. code-block:: sh

    # aptitude install libc6-dev python-dev python-distribute python-pip

Luego debe instalar la versión más reciente del paquete :command:`virtualenv`
desde el repositorio :term:`PyPI`, entonces debe instalar con el siguiente comando: 

.. code-block:: sh

    # pip install virtualenv


.. _creando_virtualenv:

Creando entornos virtuales de Python locales al usuario
=======================================================

Preparando la estructura de directorios de los Virtualenv en usuario local,
es una buena practica organizativa más no es un estándar por defecto en la
comunidad Python para esto muestro una forma de trabajo y se realizan
ejecutando los siguientes comando:

.. code-block:: sh

    $ cd $HOME ; mkdir ./virtualenv ; cd virtualenv


Crear entorno virtual del Python 2.7 de tu sistema al directorio
:file:`~/virtualenv` del usuario, ejecutando el siguiente comando: 

.. code-block:: sh

    $ virtualenv --python=/usr/bin/python2.7 python2.7

Usar distribute en virtualenv
-----------------------------

Opcionalmente puede usar :ref:`distribute <que_es_distribute>` en ``virtualenv`` para esto debe
ejecutar el siguiente comando: 

.. code-block:: sh
 
    $ virtualenv --distribute --python=/usr/bin/python2.7 python2.7

.. note::

  Este paso de creación del entorno virtual solo se realiza cada ves que 
  necesite crear un entorno virtual nuevo para sus proyectos Python.


.. _activar_virtualenv:

Activar el entorno virtual
==========================

Activar el entorno virtual creado previamente, ejecutando el siguiente
comando: 

.. code-block:: sh

    $ source ./python2.7/bin/activate

Hasta este momento tiene activada el entorno virtual usted puede verificar
esto debido a que su shell de comando inicia con el siguiente prefijo
**(python2.7)**, entiendo que este prefijo es el nombre de entorno virtual que
usted acaba de activar.

Aquí ya puede usar herramientas como :ref:`easy_install <easyinstall_setuptools>` 
o :ref:`pip <que_es_pip>` para instalar :term:`paquetes Egg`....

.. note::

  Cada ves que necesite trabajar dentro del entorno virtual necesita 
  activar este mismo.



Desactivar el entorno virtual
=============================

Cuando termine de usar el entorno virtual puede desactivarlo de la siguiente
forma: 

.. code-block:: sh

    (python2.7)$ deactivate

De esta forma ya puedes realizar operaciones de shell fuera del entorno virtual.

.. note::

  Cada ves que necesite salirse del entorno virtual necesita desactivar este mismo.


Aprovechamiento
===============

El trabajar con la herramienta le permite tener varios entornos aislados de
pruebas tanto en la misma versión de Python 2.7 como en diversas versiones
Python, como por ejemplo Python 2.4 y Python 2.7, entre otras más ventajas.


Referencias
===========

- `Creating a "Virtual" Python`_.
- `Virtualenv, a Virtual Python Environment builder`_.
- :ref:`Distribute y pip <distribute_pip>`.

.. _virtualenv: http://pypi.python.org/pypi/virtualenv/
.. _Creating a "Virtual" Python: http://peak.telecommunity.com/DevCenter/EasyInstall#creating-a-virtual-python
.. _Virtualenv, a Virtual Python Environment builder: http://pypi.python.org/pypi/virtualenv
