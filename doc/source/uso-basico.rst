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
    :caption: Rama master tras commit ``81f67ea``
    :align: center

    digraph G {
	    rankdir="RL";
	    splines=line;

	    c0

	    {
	        rank=same;
		node [
		    style=filled,
		    color=red,
		    fillcolor=red,
		    shape=rectangle,
		    fontname=monospace,
		    fontcolor=white
		]
		c0 -> "81f67ea" [dir=back]
		master -> c0
	    }
    }
    

Primer cambio
-------------

Vamos a compilar el programa y ejecutar las pruebas:

.. code-block:: console

   $ dylan build --all

Saldrá mucho texto y unas advertencias que no tiene importancia. Una
vez terminado saldrá algo parecido a esto:

.. code-block:: console

   ...
   Opened project curso-de-git (/home/fraya/Dylan/curso-de-git/curso-de-git.lid)
   Build of 'curso-de-git' completed
   [========================================] Building targets: dll within /hom...

Los programas ejecutables se encuentran en ``_build/bin``. Ejecutemos
las pruebas:

.. code-block:: console

   $ _build/bin/curso-de-git-test-suite
   Suite curso-de-git-test-suite:
   Test test-greeting:
   Test test-greeting PASSED in 0.000048s and 8KiB
   Test test-$greeting:
   Test test-$greeting PASSED in 0.000032s and 2KiB
   Suite curso-de-git-test-suite PASSED in 0.000080s and 10KiB
   Ran 2 assertions
   Ran 2 tests
   PASSED in 0.000080 seconds

Ejecutemos ahora el programa:

.. code-block:: console

   $ _build/bin/curso-de-git-app
   Hello world!

Nos gustaría que el mensaje fuera distinto de *Hello world!* y que
dijese *¡Hola mundo!* u *¡Hola mamá!*. Vamos a cambiar el código para
que al pasarle un parámetro nos devuelva un *Hola* personalizado.

Primero voy a cambiar las pruebas con lo que quiero que salga:

.. code-block:: dylan
   :caption: tests/curso-de-git-test-suite.dylan
   :emphasize-lines: 4, 8
   :linenos:

   Module: curso-de-git-test-suite

   define test test-$greeting ()
     assert-equal("Hola", $greeting);
   end test;

   define test test-greeting ()
     assert-equal("¡Hola mundo!", greeting("mundo"));
   end test;

   // Use `_build/bin/curso-de-git-test-suite --help` to see options.
   run-test-application()

En la línea 4, quiero asegurarme que la constante ``$greeting`` que
contenía *Hello World!* se vaya a cambiar por *Hola*.

En la línea 8, quiero que cuando le pase el parámetro *mundo* a la
función ``greeting`` me devuelva *¡Hola mundo!*.

Realizo los cambios, compilo con ``dylan build -a`` o ``dylan build
--all``.

.. code-block:: console
   :emphasize-lines: 10, 11, 13

   $ dylan build -a
   Open Dylan 2023.1

   Opened project curso-de-git-app (/home/fraya/Dylan/curso-de-git/curso-de-git-app.lid)
   Build of 'curso-de-git-app' completed
   [====================================    ] Building targets: exe within /hom...Open Dylan 2023.1

   Opened project curso-de-git-test-suite (/home/fraya/Dylan/curso-de-git/tests/curso-de-git-test-suite.lid)

   /home/fraya/Dylan/curso-de-git/tests/curso-de-git-test-suite.dylan:8.33-50:
   Serious warning - Too many arguments in call to method greeting () => (s :: <string>) - 1 supplied, 0 expected.
                                      -----------------
        assert-equal("¡Hola mundo!", greeting("mundo"));
                                      -----------------
   Build of 'curso-de-git-test-suite' completed
   [=================================       ] Building targets: exe within /hom...Build of "curso-de-git-test-suite" failed with exit status 3.


El compilador se pone serio y nos avisa con un *Serious warning* de
que tenemos demasiados argumentos para llamar al método ``greeting``,
que esperaba 0 y le hemos pasado 1. En la siguiente línea nos muestra
donde ha visto el código sospechoso.

Como un día que vas a cenar y se presenta alguien sin avisar, el
método ``greeting`` tiene un invitado que no esperaba. ¿Qué pasará si
ejecutamos las pruebas?

.. code-block:: console
   :emphasize-lines: 3, 9
   :linenos:

   $ _build/bin/curso-de-git-test-suite
   Suite curso-de-git-test-suite:
     Test test-$greeting:
       FAILED: "Hola" = $greeting
         want: "Hola"
         got:  "Hello world!"
         detail: sizes differ (4 and 12); element 1 is the first mismatch
     Test test-$greeting FAILED in 0.000245s and 23KiB
     Test test-greeting:
       CRASHED: "\<C2>\<A1>Hola mundo!" = greeting("mundo")
         Error evaluating assert-equal expressions: Attempted to call {<simple-method>: ??? () => (<string>)} with 1 arguments
     Test test-greeting FAILED in 0.000230s and 25KiB
   Suite curso-de-git-test-suite FAILED in 0.000475s and 47KiB
     FAILED: curso-de-git-test-suite
     FAILED: test-$greeting
     FAILED: "Hola" = $greeting
        want: "Hola"
        got:  "Hello world!"
        detail: sizes differ (4 and 12); element 1 is the first mismatch
     FAILED: test-greeting
     CRASHED: "\<C2>\<A1>Hola mundo!" = greeting("mundo")
        Error evaluating assert-equal expressions: Attempted to call {<simple-method>: ??? () => (<string>)} with 1 arguments

   Ran 2 assertions
   Ran 2 tests: 2 failed
   FAILED in 0.000475 seconds

En la línea 3 vemos que la primera prueba ha fallado, queríamos
(*want*) que saliera *Hola*, pero tuvimos (*got*) *Hello world!*.

En la línea 9 nuestro segunda prueba también ha fallado, esta vez con
el programa roto (*crashed*). El programa ha preferido suicidarse
antes que ejecutar algo que podría haber estropeado el ordenador.

.. note::

   Cuando un programa hace un *crash*, se mata el mismo y suele
   escribir un volcado de la memoria en el momento de la ejecución
   para que el programador haga la autopsia y pueda saber que
   pasó. Este volcado se conoce como *core dump*. El volcado del
   fallo y el lugar donde se guarda dependen de tu sistema
   operativo. ¡Ya tienes deberes, averígualo!.

Vamos a arreglarlo, primero cambiaremos la constante ``$greeting``
para que contenga *Hola* en lugar de *Hello world!*.

.. code-block:: dylan
   :caption: curso-de-git.dylan
   :emphasize-lines: 4

   Module: curso-de-git-impl

   // Internal
   define constant $greeting = "Hola";

   // Exported
   define function greeting () => (s :: <string>)
     $greeting
   end function;

Grabamos, compilamos (``dylan build -a``) y ejecutamos las pruebas
(``_build/bin/curso-de-git-test-suite```).

.. code-block:: dylan
   :emphasize-lines: 3

   Suite curso-de-git-test-suite:
     Test test-$greeting:
     Test test-$greeting PASSED in 0.000050s and 8KiB
     Test test-greeting:
       CRASHED: "\<C2>\<A1>Hola mundo!" = greeting("mundo")
         Error evaluating assert-equal expressions: Attempted to call {<simple-method>: ??? () => (<string>)} with 1 arguments
     Test test-greeting FAILED in 0.000266s and 30KiB
   Suite curso-de-git-test-suite FAILED in 0.000316s and 38KiB
    FAILED: curso-de-git-test-suite
    FAILED: test-greeting
       CRASHED: "\<C2>\<A1>Hola mundo!" = greeting("mundo")
         Error evaluating assert-equal expressions: Attempted to call {<simple-method>: ??? () => (<string>)} with 1 arguments

   Ran 2 assertions
   Ran 2 tests: 1 failed
   FAILED in 0.000316 seconds

Bueno, la primera prueba ya ha pasado con éxito, pero aún nos queda
arreglar la segunda. Esta será más complicada.

.. code-block:: dylan
   :caption: curso-de-git.dylan
   :emphasize-lines: 7, 8
   :linenos:

   Module: curso-de-git-impl

   // Internal
   define constant $greeting = "Hola";

   // Exported
   define function greeting (mensaje) => (s :: <string>)
     concatenate("¡", $greeting, " ", mensaje, "!")
   end function;

Primero en la línea 7 pasamos a ``greeting`` el parámetro ``mensaje``,
que pasa de no esperar a ninguno ``()`` a esperar uno ``(mensaje)``.

En la línea 8 unimos todas las cadenas para conseguir el *¡Hola
<mensaje>!*.

Grabamos y compilamos el código:

.. code-block:: console
   :emphasize-lines: 6

   $ dylan build -a
   Open Dylan 2023.1

   Opened project curso-de-git-app (/home/fraya/Dylan/curso-de-git/curso-de-git-app.lid)

   /home/fraya/Dylan/curso-de-git/curso-de-git-app.dylan:5.22-32:
   Serious warning - Too few arguments in call to method
   greeting (mensaje :: <object>) => (s :: <string>) -
   0 supplied, 1 expected.
                           ----------
        format-out("%s\n", greeting());
                           ----------
   Build of 'curso-de-git-app' completed
   [====================================    ] Building targets: exe within /hom

Claro, hemos arreglado las pruebas pero no el programa principal, que
sigue esperando una función ``greeting`` sin parámetros.

.. code-block:: dylan
   :caption: curso-de-git-app.dylan
   :emphasize-lines: 5

   Module: curso-de-git-app

   define function main
       (name :: <string>, arguments :: <vector>)
     format-out("%s\n", greeting(arguments[0]));
     exit-application(0);
   end function;

   // Calling our main function (which could have any name) should be the last
   // thing we do
   main(application-name(), application-arguments());

Le pasamos a ``greeting`` el primer argumento que le pase el usuario
al programa ejecutable (``arguments[0]``).

Grabamos, compilamos (ya no se muestran errores) y probamos.

.. code-block:: console
   :emphasize-lines: 4, 6, 10

   $ _build/bin/curso-de-git-test-suite
   Suite curso-de-git-test-suite:
    Test test-greeting:
    Test test-greeting PASSED in 0.000066s and 8KiB
    Test test-$greeting:
    Test test-$greeting PASSED in 0.000009s and 2KiB
   Suite curso-de-git-test-suite PASSED in 0.000075s and 10KiB
   Ran 2 assertions
   Ran 2 tests
   PASSED in 0.000075 seconds

¡Conseguido! Ahora probamos el programa:

.. code-block:: console

   $ _build/bin/curso-de-git-app mundo
   ¡Hola mundo!
   $ _build/bin/curso-de-git-app Luisa
   ¡Hola Luisa!

Dan ganas de saludar a todo el mundo. Tranquila. Vamos a ver el
repositorio.

.. code-block:: console
   :emphasize-lines: 6, 7, 8, 12

   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
	   modified:   curso-de-git-app.dylan
	   modified:   curso-de-git.dylan
	   modified:   tests/curso-de-git-test-suite.dylan

    Untracked files:
      (use "git add <file>..." to include in what will be committed)
	   _build/

    no changes added to commit (use "git add" and/or "git commit -a")

Por un lado vemos que tenemos ficheros modificados (*modified*) que
deberíamos añadir al *stage*. Por otro que el compilador ha creado el
directorio ``_build/`` y que no tiene seguimiento.

Añadimos a nuestro fichero ``.gitignore`` la línea:

.. code-block:: console
   :emphasize-lines: 5

   # directorios de desarrollo

   /_packages/
   /registry/
   /_build/

   # ficheros de copia de seguridad

   *~
   *.bak

Una vez quitado este directorio que no necesitamos, añadimos todos los
cambios.

.. code-block:: console

   $ git add .
   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
        modified:   .gitignore
	modified:   curso-de-git-app.dylan
	modified:   curso-de-git.dylan
	modified:   tests/curso-de-git-test-suite.dylan

Confirmamos los cambios

.. code-block:: console

   $ git commit -m "Parametrizar el mensaje de saludo"
   [master 73c695b] Parametrizar el mensaje de saludo
   4 files changed, 8 insertions(+), 7 deletions(-)

Ahora nuestro repositorio tiene este aspecto:

 .. graphviz::
    :caption: Rama master tras commit ``73c695b``
    :align: center

    digraph G {
	    rankdir="RL";
	    splines=line;

	    c1 -> c0

	    {
	        rank=same;
		node [
		    style=filled,
		    color=red,
		    fillcolor=red,
		    shape=rectangle,
		    fontname=monospace,
		    fontcolor=white
		]

		c1 -> "73c695b" [dir=back]
		master -> c1
	    }
    }

Diferencias entre *workdir* y *staging*
---------------------------------------

Aún hay un problema si llamamos a nuestro programa sin argumentos de
entrada.

.. code-block:: console
   :emphasize-lines: 2

   $ _build/bin/curso-de-git-app
   ELEMENT outside of range: 0
   Backtrace:
     invoke-debugger:internal:dylan##1 + 0x29
     default-handler:dylan:dylan##1 + 0x12
     default-last-handler:common-dylan-internals:common-dylan##0 + 0x2f1
     error:dylan:dylan##0 + 0x27
     0x7f2fe2051a15
     0x7f2fe2062017
     0x7f2fe23e728f
     0x5571c9bf1159
     0x7f2fe1a7f14a
     __libc_start_main + 0x8b
     0x5571c9bf1075

El programa falla porque no hay un elemento 0 en los
argumentos. Modifiquemos la aplicación.

.. code-block:: dylan
   :caption: curso-de-git-app.dylan
   :emphasize-lines: 5-9

   Module: curso-de-git-app

   define function main
       (name :: <string>, arguments :: <vector>)
     let mensaje = if (arguments.size < 1)
                     "mundo"
		   else
		     arguments[0]
		   end;
     format-out("%s\n", greeting(mensaje));
     exit-application(0);
   end function;

   // Calling our main function (which could have any name) should be the last
   // thing we do
   main(application-name(), application-arguments());

Esta vez añadimos los cambios a la fase de *staging*, pero sin
confirmarlos (*commit*).

.. code-block:: console

   $ git add curso-de-git-app.dylan

Volvemos a modificar el programa para indicar con un comentario lo que
hemos hecho:

.. code-block:: dylan
   :caption: curso-de-git-app.dylan
   :emphasize-lines: 5

   Module: curso-de-git-app

   define function main
       (name :: <string>, arguments :: <vector>)
     // Si no hay argumentos poner mensaje por defecto
     let mensaje = if (arguments.size < 1)
                     "mundo"
		   else
		     arguments[0]
		   end;
     format-out("%s\n", greeting(mensaje));
     exit-application(0);
   end function;

   // Calling our main function (which could have any name) should be the last
   // thing we do
   main(application-name(), application-arguments());

Veamos el estado del repositorio:

.. code-block:: console
   :caption: Estado del repositorio con un fichero en *stage* y otro en *workdir*
   :emphasize-lines: 5, 10

   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           modified:   curso-de-git-app.dylan

   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   curso-de-git-app.dylan

Podemos ver como aparecen el archivo ``curso-de-git-app.dylan`` dos
veces. El primero está preparado para ser confirmado y está almacenado
en la zona de *staging*. El segundo indica que el archivo
``curso-de-git-app.dylan`` está modificado otra vez en la zona de
trabajo (*workdir*).

.. warning::

    Si volvieramos a hacer un ``git add curso-de-git-app.dylan``
    sobreescribiríamos los cambios previos que había en la zona de
    *staging*.

Almacenamos los cambios por separado:

.. code-block:: console
   :caption: Añadir el parámetro por defecto
   :emphasize-lines: 1, 5, 14, 15

   $ git commit -m "Se añade un parámetro por defecto"
   [master 47e9e6f] Se añade un parámetro por defecto
   1 file changed, 5 insertions(+)

   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   curso-de-git-app.dylan

   no changes added to commit (use "git add" and/or "git commit -a")

   $ git add .
   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           modified:   curso-de-git-app.dylan


.. graphviz::
   :caption: Rama master tras commit ``47e9e6f``
   :align: center

   digraph G {
	    rankdir="RL";
	    splines=line;

	    c2 -> c1 -> c0

	    {
	    rank=same;
		node [
		    style=filled,
		    color=red,
		    fillcolor=red,
		    shape=rectangle,
		    fontname=monospace,
		    fontcolor=white
		]

		c2 -> "47e9e6f" [dir=back]
		master -> c2
	    }
   }

.. code-block:: console
   :caption: Añadir el comentario

   $ git commit -m "Añade comentario para parámetro por defecto"
   [master 2ee18ff] Añade comentario para parámetro por defecto
   1 file changed, 1 insertion(+)

.. graphviz::
   :caption: Rama master tras commit ``2ee18ff``
   :align: center

   digraph G {
	    rankdir="RL";
	    splines=line;

	    c3 -> c2 -> c1 -> c0

	    {
	      rank=same;
	      node [
		    style=filled,
		    color=red,
		    fillcolor=red,
		    shape=rectangle,
		    fontname=monospace,
		    fontcolor=white
	      ]

		c3 -> "2ee18ff" [dir=back]
		master -> c3
	    }
   }
