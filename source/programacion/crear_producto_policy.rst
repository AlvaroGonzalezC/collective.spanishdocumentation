.. -*- coding: utf-8 -*-

.. _producto_policy:

========================================
Creación de un producto de configuración
========================================

.. sidebar:: Sobre este artículo

    :Autor(es): Carlos de la Guardia, Leonardo J. Caballero G.
    :Correo(s): carlos.delaguardia@gmail.com, leonardocaballero@gmail.com
    :Compatible con: Plone 3, Plone 4
    :Fecha: 31 de Diciembre de 2013

En esta articulo busca explicar como crear paquetes de configuración general de 
un sitio representando las reglas generales de manejo de sitios para Plone 3.

Introducción
============

Se detallan los pasos para crear un producto de configuración y se describe
cada uno de los directorios y archivos importantes generados.

Un producto de configuración (policy product) incluye toda la configuración
general del sitio. Representa las reglas generales de manejo de sitios Plone
de una organización y puede incluir:

* Configuraciones del sitio y propiedades de navegación.

* Productos propios y de terceros que deben instalarse automáticamente con el
  sitio.

* Configuraciones de viewlets.

* Estructura inicial de contenido del sitio.

* Pasos adicionales a la instalación del producto, como creación de cuentas de
  usuarios y contenido personalizado.

* Portlets utilizados en el sitio.

* Flujo de trabajos generales de la organización.

Producto de configuración
=========================

El primer paso para la creación del producto se hace utilizando el esqueleto
de paquete para Plone proporcionado por :command:`paster`:

.. tip::
    Debe tener instalado el paquete :ref:`ZopeSkel <skel_plone>` para poder 
    usar el comando :command:`paster`.

.. code-block:: sh

    $ paster create -t plone cliente1.policy
    Selected and implied templates:
       ZopeSkel#basic_namespace A project with a namespace package
       ZopeSkel#plone                    A Plone project
    Variables:
       egg:     cliente1.policy
       package: cliente1policy
       project: cliente1.policy

A continuación, :command:`paster` realiza algunas preguntas para personalizar 
la generación del paquete. La primera es si deseamos contestar todas las
preguntas (all) o solo algunas (easy). Contestemos `all`.

Después nos pregunta el los nombres del paquete Namespace (primera parte del
nombre pasado al template) y el nombre del paquete (segunda parte). Como los
valores por omisión son los mismos que pasamos arriba, basta presiona la
tecla `enter`.

.. code-block:: sh

    Enter namespace_package (Namespace package (like plone)) ['plone']: cliente1
    Enter package (The package contained namespace package (like example)) ['example']: policy

.. tip::
    #. el espacio de nombres se usa para poder agrupar varios paquetes bajo un mismo nombre
    #. el nombre del paquete en sí
    
Siempre debe ser True para funcionar en Zope 2.

.. code-block:: sh

    Enter zope2product (Are you creating a Zope 2 Product?) [False]: True
    
La versión del paquete se utiliza en el :menuselection:`Configuración del sitio --> Productos adicionales` 
para mostrar al usuario la versión instalada del producto.

.. code-block:: sh

    Enter version (Version) ['1.0']: 0.1

Después, se pide una corta descripción del paquete; este y los datos que siguen son para los 
metadatos del proyecto en el :term:`PyPI`:.

.. tip::
    los metadatos del paquete es para definir un perfil de registro para subir el paquete a un 
    repositorio como el :term:`Python Package Index`.

.. code-block:: sh

    Enter description (One-line description of the package) ['']: Plone site policy for Cliente1 website
    Enter long_description (Multi-line description (in reST)) ['']: a Plone site policy package for Cliente1 website
    Enter author (Author name) ['Plone Foundation']: Leonardo J. Caballero G.
    Enter author_email (Author email) ['plone-developers@lists.sourceforge.net']:
    Enter keywords (Space-separated keywords/tags) ['']: plone policy package cliente1 website
    Enter url (URL of homepage) ['http://svn.plone.org/svn/collective/cliente1.policy']:
    Enter license_name (License name) ['GPL']:
    
Finalmente, esta ultima pregunta siempre ocupara el valor por defecto, debe ser ``False`` 
para funcionar bien en Zope 2.

.. code-block:: sh

    Enter zip_safe (True/False: if the package can be distributed as a .zip file) [False]:
    Creating template basic_namespace
    ...
    Running /usr/bin/python2.4 setup.py egg_info

Este comando genera un directorio de distribución donde se encuentra la
información y código para distribuir el paquete resultante como :term:`Egg`. 
Dentro de ese directorio se encuentra un sub-directorio con el espacio de nombres 
general (en este ejemplo sería 'cliente1') y dentro de ese último el verdadero 
directorio del producto para Zope (en este cliente1, 'policy').

Dentro del directorio del producto se encuentran los dos archivos
imprescindibles para crear un producto para Zope 2, junto con un esqueleto de
módulo para ``tests``:

* :file:`__init__.py`, incluye un método llamado 'initialize' para que Zope reconozca
  el paquete como producto.

* :file:`configure.zcml`, el archivo de configuración con XML, que permite al producto
  utilizar código basado en Zope 3.

* :file:`tests.py`, esqueleto de módulo para tests.

Una vez generado el producto, debemos agregar un directorio para almacenar la
configuración de :ref:`Generic Setup <perfiles_genericsetup>`:

.. code-block:: sh

    $ cd cliente1.policy/cliente1/policy
    $ mkdir profiles
    $ mkdir profiles/default

Después registramos ese directorio como perfil, dentro del archivo
:file:`configure.zcml`:

.. code-block:: xml

    <genericsetup:registerProfile
         name="default"
         title="Cliente1 site policy"
         directory="profiles/default"
         description="Turn a Plone site into the Cliente1 site."
         provides="Products.GenericSetup.interfaces.EXTENSION"
         />

Ahora ya es posible agregar dentro del directorio del perfil toda la
configuración deseada. La manera recomendada de generar los archivos xml
necesarios para ello, es crear un sitio nuevo de Plone y a continuación
modificar toda la configuración que se quiere incluir en el producto. Una vez
hecho esto, se debe exportar la configuración modificada desde la herramienta
de portal_setup, la cual se puede acceder a esta desde la raíz del portal desde la
administración de Zope (ZMI):

Al seleccionar los pasos deseados y presionar el botón de **export selected
steps**, se obtiene un archivo comprimido que contiene la configuración
expresada en XML para todos los pasos seleccionados. Este archivo debe
descomprimirse en el directorio del perfil creado en el paso anterior:

.. code-block:: sh

    $ cd profiles/default
    $ tar xzf setuptool_20080630134421.tar.gz

Como ejecutar código Python en import steps
===========================================

Finalmente, en algunas ocasiones hay pasos que queremos realizar al momento de
la instalación de un producto de configuración que no son manejables con
:ref:`Generic Setup <perfiles_genericsetup>`. En esos casos, existe un mecanismo para ejecutar código Python
en el momento que se instala un perfil. Se crea un archivo :file:`setuphandlers.py` en
la raíz del producto, con el siguiente código:

.. code-block:: python

    from Products.CMFCore.utils import getToolByName

    def setupVarious(context):
        if context.readDataFile('cliente1.policy_various.txt') is None:
            return
    site = context.getSite()
    # aquí va el código especial

El método setupVarious es donde se coloca el código especial para la
instalación, que puede hacer cualquier cosa que se necesite dentro del portal.
Para prevenir la ejecución de este código durante la instalación de otros
productos, se agrega un archivo de texto vacío, llamado 
:file:`cliente1.policy_various.txt`, dentro de :file:`profiles/setup` y se 
verifica su existencia dentro de este método.

Para enlazar este código con los pasos de importación, existe un paso especial
en :ref:`Generic Setup <perfiles_genericsetup>`, llamado import_steps. Para activarlo, 
debemos agregar el siguiente código dentro del archivo :file:`import_steps.xml`, 
dentro del directorio :file:`profiles/default`:

.. code-block:: xml

    <?xml version="1.0"?>
    <import-steps>
       <import-step id="cliente1.policy.various"
                    version="20080625-01"
                    handler="cliente1.policy.setuphandlers.setupVarious"
                    title="Cliente1 Policy: miscellaneous import steps">
         <dependency step="plone-content" />
         Various import steps that are not handled by GS import/export
         handlers.
       </import-step>
    </import-steps>

Lo único que puede variar dependiendo de lo que necesitemos hacer, es la
parte donde se listan los steps de dependencia, marcados por la etiqueta
dependency en el XML. En el atributo step de esa etiqueta se debe colocar el
nombre del paso que necesitamos sea ejecutado antes que nuestro código. Se
pueden agregar varias etiquetas dependency con distintos pasos para el caso de'
que nuestro código dependa de varios pasos.


Referencia
==========

- `Pasos para crear un producto de configuración`_ desde la comunidad Plone México.

.. _Pasos para crear un producto de configuración: http://www.plone.mx/docs/policy.html
