.. -*- coding: utf-8 -*-

.. _instancia_zope_debug:

==============================================
Creando instancia Zope adicional de depuración
==============================================

:Autor(es): Leonardo J. Caballero G.
:Correo(s): leonardocaballero@gmail.com
:Compatible con: Plone 3, Plone 4
:Fecha: 28 de Julio de 2013

Es posible que desee mantener su ``buildout.cfg`` para producción 
y sincronizar la configuración de desarrollo de forma automática 
como sea posible.

Una buena idea es utilizar ``buildout.cfg`` misma en cada entorno. 
Si con cosas condicionales, como poner el modo de depuración activo, 
como es requerido, usted puede ampliar las secciones buildout, que a 
su vez crear **Instancias Zope adicionales** con la siguiente configuración:

.. code-block:: cfg

  [instance]
  recipe = plone.recipe.zope2instance
  zope2-location = ${zope2:location}
  user = admin:x
  http-address = 8080
  debug-mode = off
  verbose-security = off

Y crea su instancia Debug Zope, con la siguiente configuración:

.. code-block:: cfg

  # Crear un script lanzado el cual iniciará una 
  # instancia Zope en modo debug
  [debug-instance]
  # Extiende la instancia principal de producción
  <= instance

  # Aquí sobre escribes configuraciones especifica 
  # para hacer la instancia que se ejecute en modo debug
  debug-mode = on
  verbose-security = on
  event-log-level = DEBUG

Y ahora usted puede iniciar si instancia de **desarrollo** Zope como: 

.. code-block:: sh

  ./bin/debug-instance fg

Y su instancia principal de Zope permanece en modo de producción: 

.. code-block:: sh

  ./bin/instance start

.. note::

    Usando siempre el modo ``fg`` de Zope para el modo depuración, 
    pero no registra en el nivel de log.

Referencias
===========

-   `Plone Hosting`_

.. _Plone Hosting: http://collective-docs.readthedocs.org/en/latest/hosting/
