.. _`git-basico`:

Aspectos básicos de Git
=======================

Instalación
-----------

GNU/Linux
^^^^^^^^^

Si quieres instalar Git en Linux a través de un instalador binario, en
general puedes hacerlo a través de la herramienta básica de gestión de
paquetes que trae tu distribución.

.. code-block:: console
   :caption: Red Hat, Fedora, Alma Linux

   $ sudo dnf install git-all

.. code-block:: console
   :caption: Debian, Ubuntu

   $ sudo apt install git-all

.. code-block:: console
   :caption: Opensuse

   $ sudo zypper in git

Windows
^^^^^^^

Accede a https://git-scm.com/download/win y descarga la versión más
apropiada.

MacOS
^^^^^

En MacOS se recomienda tener instalada la herramienta `Brew
<https://brew.sh>`_. Después, ejecutar:

.. code-block:: console

   $ brew install git

Configuración
-------------

Tu identidad
^^^^^^^^^^^^

Lo primero que deberías hacer cuando instalas Git es establecer tu
nombre de usuario y dirección de correo electrónico. Esto es
importante porque las **confirmaciones de cambios** (*commits*) en Git
usan esta información, y es introducida de manera inmutable en los
commits que envías:

.. code-block:: console

   $ git config --global user.name "John Doe"
   $ git config --global user.email johndoe@example.com

.. note::

   También se recomienda configurar el siguiente parámetro:

   .. code-block:: console

      $ git config --global push.default simple

   Esta opción hace que se sincronicen solo los cambios de la rama
   local con la rama remota que se llame igual. Todavía no hemos visto
   nada de esto, lo pones y ya está.

Bash completion
^^^^^^^^^^^^^^^

*Bash completion* es una utilidad que permite a bash completar órdenes
y parámetros. Por defecto suele venir desactivada en Ubuntu y es
necesario modificar el archivo ``$HOME/.bashrc`` para poder
activarla. Simplemente hay que descomentar las líneas que lo activan.
