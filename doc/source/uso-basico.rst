Uso básico de Git
=================

Crear un proyecto
-----------------

Crearemos un proyecto sencillo con `Opendylan
<https://opendylan.org>`_ para practicar Git:

.. code-block:: console

   $ dylan new application curso-de-git

La herramienta ``dylan`` nos ayudará a crear una estructura de
directorios para el proyecto:

.. code-block:: console

   $ tree curso-de-git

La salida es *parecida* a lo siguiente (lo he simplificado):

.. code-block:: console

   curso-de-git/
   ├── curso-de-git-app.dylan
   ├── curso-de-git-app-library.dylan
   ├── curso-de-git-app.lid
   ├── curso-de-git.dylan
   ├── curso-de-git.lid
   ├── dylan-package.json
   ├── library.dylan
   ├── _packages
   │   ├── command-line-parser
   │   │   ├── 3.1.1
   │   │   └── current -> /home/fraya/Dylan/curso-de-git/_packages/command-line-parser/3.1.1
   │   ├── json
   │   │   ├── 1.0.0
   │   │   └── current -> /home/fraya/Dylan/curso-de-git/_packages/json/1.0.0
   │   ├── strings
   │   │   ├── 1.1.0
   │   │   └── current -> /home/fraya/Dylan/curso-de-git/_packages/strings/1.1.0
   │   └── testworks
   │       ├── 3.2.0
   │       └── current -> /home/fraya/Dylan/curso-de-git/_packages/testworks/3.2.0
   ├── registry
   │   └── x86_64-linux
   └── tests
	   ├── curso-de-git-test-suite.dylan
	   ├── curso-de-git-test-suite.lid
	   └── library.dylan

Entraremos en el directorio ```curso-de-git``

.. code-block:: console

   $ cd curso-de-git

Crear el repositorio
--------------------

Para crear un nuevo repositorio se usa la orden ``git init``.

.. code-block:: console
   :caption: Inicialización del repositorio

   $ git init
   hint: Using 'master' as the name for the initial branch. This default branch name
   hint: is subject to change. To configure the initial branch name to use in all
   hint: of your new repositories, which will suppress this warning, call:
   hint:
   hint: 	git config --global init.defaultBranch <name>
   hint:
   hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
   hint: 'development'. The just-created branch can be renamed via this command:
   hint:
   hint: 	git branch -m <name>
   Initialized empty Git repository in /home/fraya/Dylan/curso-de-git/.git/
   
Esto creará un directorio ``.git`` para almacenar nuestro repositorio
local.

.. note::

   Tradicionalmente a la rama inicial de un proyecto se la llamaba
   *master*, pero esta palabra en inglés tiene connotaciones
   esclavistas ya que también significa *amo*, en la relación
   amo/esclavo.

   Las nuevas tendencias en USA pretenden cambiar las palabras sin
   cambiar sus actos, pensando que al desaparecer la palabra "amo"
   desaparecen los esclavos. Por ello *git* nos da una pista por si
   queremos cambiar la rama inicial y poner *main* de nombre.

   Podemos ser politicamente correctos y cambiarlo para todos los
   nuevos repositorios, evitando este mensaje de advertencia

   .. code-block:: console

      git config --global init.defaultBranch main

   siempre que seamos conscientes de que sigue habiendo esclavitud.

Añadir la aplicación
--------------------

Ahora almacenaríamos todos los archivos creados en el repositorio para
trabajar, pero antes miremos lo que tenemos (líneas resaltadas):

.. code-block:: console
   :caption: Estado de nuestro repositorio
   :emphasize-lines: 3, 5, 7, 20

   $ git status

   On branch master

   No commits yet

   Untracked files:
     (use "git add <file>..." to include in what will be committed)
	   _packages/
	   curso-de-git-app-library.dylan
	   curso-de-git-app.dylan
	   curso-de-git-app.lid
	   curso-de-git.dylan
	   curso-de-git.lid
	   dylan-package.json
	   library.dylan
	   registry/
	   tests/

   nothing added to commit but untracked files present (use "git add" to track)

Estamos en la rama (*branch*) *master* y no tenemos ninguna
confirmación (*commits*) aún. Tenemos una lista de ficheros sin
seguimiento (*untracked*) y podemos añadirlos usando ``git add``.

.. warning::

   No añadas los ficheros todavía y sigue leyendo.

.gitignore
^^^^^^^^^^

Sin embargo dentro de los ficheros *untracked* hay ficheros que no
necesitamos en nuestro repositorio.

.. code-block:: console
   :caption: Directorios que no queremos incluir en el repositorio
   :emphasize-lines: 9, 17
      
   $ git status

   On branch master

   No commits yet

   Untracked files:
     (use "git add <file>..." to include in what will be committed)
	   _packages/
	   curso-de-git-app-library.dylan
	   curso-de-git-app.dylan
	   curso-de-git-app.lid
	   curso-de-git.dylan
	   curso-de-git.lid
	   dylan-package.json
	   library.dylan
	   registry/
	   tests/

Los directorios ``_packages`` y ``registry`` son creados por la
herramienta ``dylan``. Estos datos pueden ser creados de nuevo usando
el comando ``dylan update`` en este directorio cuando lo
necesitemos. Por lo tanto **no deben estar dentro de nuestro
repositorio**. Para evitar que estos ficheros entren dentro por error,
crearemos un fichero llamado ``.gitignore`` con una lista de ficheros
y directorios que no queremos que git incluya ni que aparezcan en los
comandos.

Pondremos el siguiente contenido en ``.gitignore``

.. code-block::
   :caption: .gitignore

   # directorios de desarrollo
   
   /_packages/
   /registry/

   # ficheros de copia de seguridad

   *~
   *.bak

Después de grabar el fichero ```.gitignore`` volvemos a ver el estado
del repositorio:

.. code-block:: console
   :caption: Estado del repositorio con ``.gitignore``

   $ git status
   On branch master

   No commits yet

   Untracked files:
     (use "git add <file>..." to include in what will be committed)
	   .gitignore
	   curso-de-git-app-library.dylan
	   curso-de-git-app.dylan
	   curso-de-git-app.lid
	   curso-de-git.dylan
	   curso-de-git.lid
	   dylan-package.json
	   library.dylan
	   tests/

   nothing added to commit but untracked files present (use "git add" to track)
   
Vemos que los directorios ``_packages`` y ``registry`` ya no aparecen
entre los ficheros sin seguimiento. De la misma manera desaparecerían
cualquier fichero terminado en ``~`` (copia de seguridad en Unix) o
``.bak`` (copia de seguridad en Windows) **en cualquier
subdirectorio** (ya que no empieza la línea por ``/`` en
``.gitignore``).

Ahora sí, añadimos la aplicación
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Ya estamos preparados para añadir la aplicación. Podemos añadir los
ficheros uno a uno, ``git add .gitignore`` por ejemplo o añadirlos
todos a la vez, ``git add .``

.. code-block:: console
   :caption: Añadimos todos los ficheros al área de preparación o *staged*


   $ git add .

Volvemos a ver el estado del repositorio:

.. code-block:: console
   :caption: Estado tras añadir los ficheros
   :emphasize-lines: 6, 7

   $ git status
   On branch master

   No commits yet

   Changes to be committed:
     (use "git rm --cached <file>..." to unstage)
	   new file:   .gitignore
	   new file:   curso-de-git-app-library.dylan
	   new file:   curso-de-git-app.dylan
	   new file:   curso-de-git-app.lid
	   new file:   curso-de-git.dylan
	   new file:   curso-de-git.lid
	   new file:   dylan-package.json
	   new file:   library.dylan
	   new file:   tests/curso-de-git-test-suite.dylan
	   new file:   tests/curso-de-git-test-suite.lid
	   new file:   tests/library.dylan

Nos muestra que tenemos una lista de cambios en seguimiento preparados
para ser confirmados (*committed*). También nos muestra que podemos
sacarlos del área de seguimento (*unstage*) mediante el comando ``git
rm --cached <file>...``.

Primera confirmación
^^^^^^^^^^^^^^^^^^^^

No se trata de tu primera comunión ni de la confirmación de la Iglesia
sino de aceptar los ficheros en seguimiento y guardarlos en el
repositorio. Sin más dilación:

.. code-block:: console

   $ git commit -m "Revisión inicial"

Confirmamos con *commit* y escribimos un mensaje de confirmación
(``-m``) en la misma línea (más adelante veremos como hacerlo más
correcto).

La salida del comando es la siguiente:

.. code-block:: console
   :caption: Salida de la orden de confirmación
   :emphasize-lines: 2

   $ git commit -m "Revisión inicial"
   [master (root-commit) 81f67ea] Revisión inicial
   11 files changed, 122 insertions(+)
   create mode 100644 .gitignore
   create mode 100644 curso-de-git-app-library.dylan
   create mode 100644 curso-de-git-app.dylan
   create mode 100644 curso-de-git-app.lid
   create mode 100644 curso-de-git.dylan
   create mode 100644 curso-de-git.lid
   create mode 100644 dylan-package.json
   create mode 100644 library.dylan
   create mode 100644 tests/curso-de-git-test-suite.dylan
   create mode 100644 tests/curso-de-git-test-suite.lid
   create mode 100644 tests/library.dylan

Nuestro repositorio tiene el siguiente aspecto:

 .. graphviz::
    :caption: Rama master
    :align: center

    digraph G {
	    rankdir="LR";
	    pad=0.5;
	    nodesep=0.6;
	    ranksep=0.5;
	    forcelabels=true;
	    
	    node [
	        width=0.12,
		height=0.12,
		fixedsize=true,
		shape=circle,
		style=filled,
		color="#909090",
		fontcolor="deepskyblue",
		font="Arial bold",
		fontsize="14pt"
	    ];
	    
	    edge [
	        arrowhead=none,
		color="#909090",
		penwidth=3
	    ];

	    node [group="master"];
	    s1   [label="master\n\n\n", width=0.33, height=-.33, shape=box];
	    1    [label="81f67ea\n\n\n"];
	    e1   [label="", width=0.33, height=-.33, shape=box];

	    s1 -> 1;
            1  -> e1 [color="#b0b0b0", style=dashed];
    }
