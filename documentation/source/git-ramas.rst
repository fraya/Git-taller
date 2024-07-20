Ramas
=====

Administración de ramas
-----------------------

Crear una nueva rama
^^^^^^^^^^^^^^^^^^^^

Cuando vamos a trabajar en una nueva funcionalidad, es conveniente
hacerlo en una nueva rama, para no modificar la rama principal y dejarla
inestable. Aunque la orden para manejar ramas es ``git branch`` podemos
usar también ``git checkout``.

Vamos a crear una nueva rama:

.. code-block:: console

   git branch borrar-constante


.. note::

   Si usamos `git branch` sin ningún argumento, nos devolverá la lista de ramas
   disponibles.

La orden anterior no devuelve ningún resultado y tampoco nos cambia de
rama, para eso debemos usar *checkout*:

.. code-block:: console

   $ git checkout borrar-constante
   Switched to branch 'borrar-contante'

.. note::

   Hay una forma más rapida de hacer ambas acciones en un solo
   paso. Con el parámetro ``-b`` de ``git checkout`` podemos cambiarnos a
   una rama que, si no existe, se crea instantáneamente.

   .. code-block:: console

       $ git checkout -b borrar-constante
       Switched to a new branch 'borrar-constante'

Modificaciones en la rama secundaria
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Para realizar la prueba de modificaremos el fichero
``curso-de-git.dylan``. El saludo por defecto seguirá siendo *Hola*
pero podremos modificarlo pasando el parámetro opcional *saludo*.

.. code-block:: dylan
   :caption: Fichero ``curso-de-git.dylan``

   Module: curso-de-git-impl

   // Internal
   define constant $greeting = "Hola";

   // Exported
   define function greeting (mensaje, #key saludo = $greeting) => (s :: <string>)
     concatenate("¡", $greeting, " ", mensaje, "!")
   end function;

Añadiremos una nueva prueba.

.. code-block:: dylan
   :caption: Fichero ``curso-de-git-test-suite.dylan``
   :highlight-lines: 10-12

   Module: curso-de-git-test-suite

   define test test-$greeting ()
     assert-equal("Hola", $greeting);
   end test;

   define test test-greeting ()
     assert-equal("¡Hola mundo!", greeting("mundo"));
   end test;

   define test test-greeting-saludos ()
     assert-equal("¡Saludos mundo!", greeting("mundo", saludo: "Saludos"));
   end test;

   // Use `_build/bin/curso-de-git-test-suite --help` to see options.
   run-test-application()

Podríamos confirmar los cambios todos de golpe, pero lo haremos de uno
en uno, con su comentario.

.. code-block:: console
   :caption: Añadir el fichero ``curso-de-git.dylan`` modificado a Git

   $ git add source/lib/curso-de-git.dylan

.. code-block:: console
   :caption: Confirmar cambios

   $ git commit -m "Saludo como parámetro"

.. code-block:: console
   :caption: Salida de la confirmación

   $ git commit -m "Saludo como parámetro"
   [borrar-constante d209b81] Saludo como parámetro
   1 file changed, 2 insertions(+), 2 deletions(-)

.. code-block:: console
   :caption: Añadimos el fichero de pruebas a Git

   $ git add tests/curso-de-git-test-suite.dylan

.. code-block:: console
   :caption: Confirmamos el cambio

   $ git commit -m "Añadir prueba para saludo como parámetro"

.. code-block:: console
   :caption: Salida de la confirmación

   $ git commit -m "Añadir prueba para saludo como parámetro"
   [borrar-constante 9bd66c3] Añadir prueba para saludo como parámetro
    1 file changed, 4 insertions(+)

Y ahora con la orden ``git checkout`` podemos movernos entre ramas:

.. code-block:: console
   :caption: Cambiar a la rama ``main``

   $ git checkout main

.. code-block:: console
   :caption: Cambiar a rama ``borrar-constante``

   $ git checkout borrar-constante

Modificaciones en la rama main
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Podemos volver y añadiremos el archivo ``CHANGELOG`` en la rama
principal para anotar los cambios en las versiones:

.. code-block:: console

   $ git checkout main
   Switched to branch 'main'
   Your branch is up to date with 'origin/main'.

.. code-block:: markdown
   :caption: Fichero ``CHANGELOG``

   # Changelog

   Todos los cambios notables del proyecto serán documentados en este
   fichero.

   ## [Unreleased]

   ### Añadido

   - v0.1.0 Cambiar saludo por parámetro opcional

Y lo añadimos a nuestro repositorio en la rama (*main*) en la que
estamos:

.. code-block:: console
   :caption: Añadimos el fichero a Git

   $ git add CHANGELOG

.. code-block:: console
   :caption: Confirmamos los cambios

   $ git commit -m "Añadido CHANGELOG"

.. code-block:: console
   :caption: Salida del comando de confirmación

   $ git commit -m "Añadido CHANGELOG"
   [main 658f895] Añadido CHANGELOG
   1 file changed, 10 insertions(+)
   create mode 100644 CHANGELOG

.. code-block:: console
   :emphasize-lines: 3,4
   :caption: Historial mostrando la bifurcación

   $ git hist --all
   * [2024-07-19] [658f895] | Añadido CHANGELOG {{Fernando Raya}}  (HEAD -> main)
   | * [2024-07-19] [9bd66c3] | Añadir prueba para saludo como parámetro {{Fernando Raya}}  (borrar-constante)
   | * [2024-07-19] [d209b81] | Saludo como parámetro {{Fernando Raya}}
   |/
   * [2024-07-12] [b9d1749] | doc: Ampliar descripción {{Fernando Raya}}  (origin/main, origin/HEAD)
   * [2024-07-12] [b15078e] | doc: Update to v.0.1.2 {{Fernando Raya}}

Mezclar ramas
~~~~~~~~~~~~~

Podemos incorporar los cambios de una rama a otra con la orden ``git
merge``.

.. code-block:: console
   :caption: Cambiamos la rama a ``borrar-constante``

   $ git checkout borrar-constante
   Switched to branch 'borrar-constante'

.. code-block:: console
   :caption: Al mezclar las ramas, se abrirá el editor de texto con un mensaje.		

   $ git merge main

.. code-block:: console
   :caption: Salida de Git tras la fusión de ramas

   $ git merge main
   Merge made by the 'ort' strategy.
   CHANGELOG | 10 ++++++++++
   1 file changed, 10 insertions(+)
   create mode 100644 CHANGELOG

.. code-block:: console
   :caption: Historial tras la mezcla de ramas

   $ git hist --all
   *   [2024-07-20] [b42d86a] | Merge branch 'main' into borrar-constante {{Fernando Raya}}  (HEAD -> borrar-constante)
   |\
   | * [2024-07-19] [658f895] | Añadido CHANGELOG {{Fernando Raya}}  (main)
   * | [2024-07-19] [9bd66c3] | Añadir prueba para saludo como parámetro {{Fernando Raya}} 
   * | [2024-07-19] [d209b81] | Saludo como parámetro {{Fernando Raya}}
   |/
   * [2024-07-12] [b9d1749] | doc: Ampliar descripción {{Fernando Raya}}  (origin/main, origin/HEAD)

.. code-block:: console
   :caption: En la rama ``borrar-constante`` no hay ningún cambio por confirmar		

   $ git status
   On branch borrar-constante
   nothing to commit, working tree clean

En mi ordenador, la rama ``main`` tiene un cambio no subido al
repositorio remoto:

.. code-block:: console
   :caption: Estado de la rama ``main``

   $ git status
   On branch main
   Your branch is ahead of 'origin/main' by 1 commit.
     (use "git push" to publish your local commits)

.. code-block:: console
   :caption: Confirmaremos los cambios

   $ git push
   Enumerating objects: 4, done.
   Counting objects: 100% (4/4), done.
   Delta compression using up to 4 threads
   Compressing objects: 100% (3/3), done.
   Writing objects: 100% (3/3), 423 bytes | 423.00 KiB/s, done.
   Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
   remote: Resolving deltas: 100% (1/1), completed with 1 local object.
   To github.com:fraya/curso-de-git.git
      b9d1749..658f895  main -> main

De esa forma se puede trabajar en una rama secundaria incorporando los
cambios de la rama principal o de otra rama.

Resolver conflictos
~~~~~~~~~~~~~~~~~~~

Un conflicto es cuando se produce una fusión que Git no es capaz de
resolver. Vamos a modificar la rama main para crear uno con la rama
hola.

::

   $ git checkout main
   Switched to branch 'main'

Modificamos nuestro archivo *hola.php* de nuevo:

.. code:: php

   <?php
   // Autor: Sergio Gómez <sergio@uco.es>
   print "Introduce tu nombre:";
   $nombre = trim(fgets(STDIN));
   @print "Hola, {$nombre}\n";

Y guardamos los cambios:

::

   $ git add lib/hola.php
   $ git commit -m "Programa interactivo"
   [main 9c85275] Programa interactivo
    1 file changed, 2 insertions(+), 2 deletions(-)
   $ git hist --all
   *   9c6ac06 2013-06-16 | Merge commit 'c3e65d0' into hola (hola) [Sergio Gómez]
   |\
   * | 9862f33 2013-06-16 | hola usa la clase HolaMundo [Sergio Gómez]
   * | 6932156 2013-06-16 | Añadida la clase HolaMundo [Sergio Gómez]
   | | * 9c85275 2013-06-16 | Programa interactivo (HEAD, main) [Sergio Gómez]
   | |/
   | * c3e65d0 2013-06-16 | Añadido README.md [Sergio Gómez]
   |/
   * 81c6e93 2013-06-16 | Movido hola.php a lib [Sergio Gómez]
   * 96a39df 2013-06-16 | Añadido el autor del programa y su email [Sergio Gómez]
   * fd4da94 2013-06-16 | Se añade un comentario al cambio del valor por defecto (tag: v1) [Sergio Gómez]
   * 3283e0d 2013-06-16 | Se añade un parámetro por defecto (tag: v1-beta) [Sergio Gómez]
   * efc252e 2013-06-16 | Parametrización del programa [Sergio Gómez]
   * e19f2c1 2013-06-16 | Creación del proyecto [Sergio Gómez]

.. code:: mermaid

   gitGraph:
      commit id: "e19f2c1"
      commit id: "efc252e"
      commit id: "3283e0d" tag: "v1-beta"
      commit id: "fd4da94" tag: "v1"
      commit id: "96a39df"
      commit id: "8c2a509"
      branch hola
      commit id: "6932156"
      commit id: "9862f33"
      checkout main
      commit id: "c3e65d0"
      checkout hola
      merge main id: "9c6ac06"
      checkout main
      commit id: "9c85275"

Volvemos a la rama hola y fusionamos:

::

   $ git checkout hola
   Switched to branch 'hola'
   $ git merge main
   Auto-merging lib/hola.php
   CONFLICT (content): Merge conflict in lib/hola.php
   Automatic merge failed; fix conflicts and then commit the result.

Si editamos nuestro archivo ``lib/hola.php`` obtendremos algo similar a
esto:

.. code:: php

   <?php
   // Autor: Sergio Gómez <sergio@uco.es>
   <<<<<<< HEAD
   // El nombre por defecto es Mundo
   require('HolaMundo.php');

   $nombre = isset($argv[1]) ? $argv[1] : "Mundo";
   print new HolaMundo($nombre);
   =======
   print "Introduce tu nombre:";
   $nombre = trim(fgets(STDIN));
   @print "Hola, {$nombre}\n";
   >>>>>>> main

La primera parte marca el código que estaba en la rama donde
trabajábamos (HEAD) y la parte final el código de donde fusionábamos.
Resolvemos el conflicto, dejando el archivo como sigue:

.. code:: php

   <?php
   // Autor: Sergio Gómez <sergio@uco.es>
   require('HolaMundo.php');

   print "Introduce tu nombre:";
   $nombre = trim(fgets(STDIN));
   print new HolaMundo($nombre);

Y resolvemos el conflicto confirmando los cambios:

::

   $ git add lib/hola.php
   $ git commit -m "Solucionado el conflicto al fusionar con la rama main"
   [hola a36af04] Solucionado el conflicto al fusionar con la rama main

.. code:: mermaid

   gitGraph:
      commit id: "e19f2c1"
      commit id: "efc252e"
      commit id: "3283e0d" tag: "v1-beta"
      commit id: "fd4da94" tag: "v1"
      commit id: "96a39df"
      commit id: "8c2a509"
      branch hola
      commit id: "6932156"
      commit id: "9862f33"
      checkout main
      commit id: "c3e65d0"
      checkout hola
      merge main id: "9c6ac06"
      checkout main
      commit id: "9c85275"
      checkout hola
      merge main id: "a36af04"

Rebasing vs Merging
~~~~~~~~~~~~~~~~~~~

Rebasing es otra técnica para fusionar distinta a merge y usa la orden
``git rebase``. Vamos a dejar nuestro proyecto como estaba antes del
fusionado. Para ello necesitamos anotar el hash anterior al de la acción
de *merge*. El que tiene la anotación *“hola usa la clase HolaMundo”*.

Para ello podemos usar la orden ``git reset`` que nos permite mover HEAD
donde queramos.

::

   $ git checkout hola
   Switched to branch 'hola'
   $ git hist
   *   a36af04 2013-06-16 | Solucionado el conflicto al fusionar con la rama main (HEAD, hola) [Sergio Gómez]
   |\
   | * 9c85275 2013-06-16 | Programa interactivo (main) [Sergio Gómez]
   * |   9c6ac06 2013-06-16 | Merge commit 'c3e65d0' into hola [Sergio Gómez]
   |\ \
   | |/
   | * c3e65d0 2013-06-16 | Añadido README.md [Sergio Gómez]
   * | 9862f33 2013-06-16 | hola usa la clase HolaMundo [Sergio Gómez]
   * | 6932156 2013-06-16 | Añadida la clase HolaMundo [Sergio Gómez]
   |/
   * 81c6e93 2013-06-16 | Movido hola.php a lib [Sergio Gómez]
   * 96a39df 2013-06-16 | Añadido el autor del programa y su email [Sergio Gómez]
   * fd4da94 2013-06-16 | Se añade un comentario al cambio del valor por defecto (tag: v1) [Sergio Gómez]
   * 3283e0d 2013-06-16 | Se añade un parámetro por defecto (tag: v1-beta) [Sergio Gómez]
   * efc252e 2013-06-16 | Parametrización del programa [Sergio Gómez]
   * e19f2c1 2013-06-16 | Creación del proyecto [Sergio Gómez]
   $ git reset --hard 9862f33
   HEAD is now at 9862f33 hola usa la clase HolaMundo

Y nuestro estado será:

::

   $ git hist --all
   * 9862f33 2013-06-16 | hola usa la clase HolaMundo (HEAD, hola) [Sergio Gómez]
   * 6932156 2013-06-16 | Añadida la clase HolaMundo [Sergio Gómez]
   | * 9c85275 2013-06-16 | Programa interactivo (main) [Sergio Gómez]
   | * c3e65d0 2013-06-16 | Añadido README.md [Sergio Gómez]
   |/
   * 81c6e93 2013-06-16 | Movido hola.php a lib [Sergio Gómez]
   * 96a39df 2013-06-16 | Añadido el autor del programa y su email [Sergio Gómez]
   * fd4da94 2013-06-16 | Se añade un comentario al cambio del valor por defecto (tag: v1) [Sergio Gómez]
   * 3283e0d 2013-06-16 | Se añade un parámetro por defecto (tag: v1-beta) [Sergio Gómez]
   * efc252e 2013-06-16 | Parametrización del programa [Sergio Gómez]
   * e19f2c1 2013-06-16 | Creación del proyecto [Sergio Gómez]

.. code:: mermaid

   gitGraph:
      commit id: "e19f2c1"
      commit id: "efc252e"
      commit id: "3283e0d" tag: "v1-beta"
      commit id: "fd4da94" tag: "v1"
      commit id: "96a39df"
      commit id: "8c2a509"
      branch hola
      commit id: "6932156"
      commit id: "9862f33"
      checkout main
      commit id: "c3e65d0"
      commit id: "9c85275"

Hemos desecho todos los *merge* y nuestro árbol está *“limpio”*. Vamos a
probar ahora a hacer un rebase. Continuamos en la rama ``hola`` y
ejecutamos lo siguiente:

::

   $ git rebase main
   First, rewinding head to replay your work on top of it...
   Applying: Añadida la clase HolaMundo
   Applying: hola usa la clase HolaMundo
   Using index info to reconstruct a base tree...
   M   lib/hola.php
   Falling back to patching base and 3-way merge...
   Auto-merging lib/hola.php
   CONFLICT (content): Merge conflict in lib/hola.php
   error: Failed to merge in the changes.
   Patch failed at 0002 hola usa la clase HolaMundo
   The copy of the patch that failed is found in: .git/rebase-apply/patch

   When you have resolved this problem, run "git rebase --continue".
   If you prefer to skip this patch, run "git rebase --skip" instead.
   To check out the original branch and stop rebasing, run "git rebase --abort".

El conflicto, por supuesto, se sigue dando. Resolvemos guardando el
archivo ``hola.php`` como en los casos anteriores:

.. code:: php

   <?php
   // Autor: Sergio Gómez <sergio@uco.es>
   require('HolaMundo.php');

   print "Introduce tu nombre:";
   $nombre = trim(fgets(STDIN));
   print new HolaMundo($nombre);

Añadimos los cambios en *staging* y en esta ocasión, y tal como nos
indicaba en el mensaje anterior, no tenemos que hacer ``git commit``
sino continuar con el *rebase*:

::

   $ git add lib/hola.php
   $ git status
   rebase in progress; onto 269eaca
   You are currently rebasing branch 'hola' on '269eaca'.
     (all conflicts fixed: run "git rebase --continue")

   Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)

       modified:   lib/hola.php
   $ git rebase --continue
   Applying: hola usa la clase HolaMundo

Y ahora vemos que nuestro árbol tiene un aspecto distinto, mucho más
limpio:

::

   $ git hist --all
   * 9862f33 2013-06-16 | hola usa la clase HolaMundo (HEAD -> hola) [Sergio Gómez]
   * 6932156 2013-06-16 | Añadida la clase HolaMundo [Sergio Gómez]
   * 9c85275 2013-06-16 | Programa interactivo (main) [Sergio Gómez]
   * c3e65d0 2013-06-16 | Añadido README.md [Sergio Gómez]
   * 81c6e93 2013-06-16 | Movido hola.php a lib [Sergio Gómez]
   * 96a39df 2013-06-16 | Añadido el autor del programa y su email [Sergio Gómez]
   * fd4da94 2013-06-16 | Se añade un comentario al cambio del valor por defecto (tag: v1) [Sergio Gómez]
   * 3283e0d 2013-06-16 | Se añade un parámetro por defecto (tag: v1-beta) [Sergio Gómez]
   * efc252e 2013-06-16 | Parametrización del programa [Sergio Gómez]
   * e19f2c1 2013-06-16 | Creación del proyecto [Sergio Gómez]

.. code:: mermaid

   gitGraph:
      commit id: "e19f2c1"
      commit id: "efc252e"
      commit id: "3283e0d" tag: "v1-beta"
      commit id: "fd4da94" tag: "v1"
      commit id: "96a39df"
      commit id: "8c2a509"
      commit id: "c3e65d0"
      commit id: "9c85275"
      branch hola
      commit id: "6932156"
      commit id: "9862f33"

Lo que hace rebase es volver a aplicar todos los cambios a la rama
máster, desde su nodo más reciente. Eso significa que se modifica el
orden o la historia de creación de los cambios. Por eso rebase no debe
usarse si el orden es importante o si la rama es compartida.

## Mezclando con la rama main

Ya hemos terminado de implementar los cambios en nuestra rama secundaria
y es hora de llevar los cambios a la rama principal. Usamos
``git merge`` para hacer una fusión normal:

::

   $ git checkout main
   Switched to branch 'main'
   $ git merge hola
   Updating c3e65d0..491f1d2
   Fast-forward
    lib/HolaMundo.php | 16 ++++++++++++++++
    lib/hola.php      |  4 +++-
    2 files changed, 19 insertions(+), 1 deletion(-)
    create mode 100644 lib/HolaMundo.php
    $ git hist --all
    * 9862f33 2013-06-16 | hola usa la clase HolaMundo (HEAD -> main, hola) [Sergio Gómez]
    * 6932156 2013-06-16 | Añadida la clase HolaMundo [Sergio Gómez]
    * 9c85275 2013-06-16 | Programa interactivo [Sergio Gómez]
    * c3e65d0 2013-06-16 | Añadido README.md [Sergio Gómez]
    * 81c6e93 2013-06-16 | Movido hola.php a lib [Sergio Gómez]
    * 96a39df 2013-06-16 | Añadido el autor del programa y su email [Sergio Gómez]
    * fd4da94 2013-06-16 | Se añade un comentario al cambio del valor por defecto (tag: v1) [Sergio Gómez]
    * 3283e0d 2013-06-16 | Se añade un parámetro por defecto (tag: v1-beta) [Sergio Gómez]
    * efc252e 2013-06-16 | Parametrización del programa [Sergio Gómez]
    * e19f2c1 2013-06-16 | Creación del proyecto [Sergio Gómez]

Vemos que indica que el tipo de fusión es *fast-forward*. Este tipo de
fusión tiene el problema que no deja rastro de la fusión, por eso suele
ser recomendable usar el parámetro ``--no-ff`` para que quede constancia
siempre de que se ha fusionado una rama con otra.

Vamos a volver a probar ahora sin hacer *fast-forward*. Reseteamos
*main* al estado *“Programa interactivo”*.

::

   $ git reset --hard 9c85275
   $ git hist --all
   * 9862f33 2013-06-16 | hola usa la clase HolaMundo (HEAD -> hola) [Sergio Gómez]
   * 6932156 2013-06-16 | Añadida la clase HolaMundo [Sergio Gómez]
   * 9c85275 2013-06-16 | Programa interactivo (main) [Sergio Gómez]
   * c3e65d0 2013-06-16 | Añadido README.md [Sergio Gómez]
   * 81c6e93 2013-06-16 | Movido hola.php a lib [Sergio Gómez]
   * 96a39df 2013-06-16 | Añadido el autor del programa y su email [Sergio Gómez]
   * fd4da94 2013-06-16 | Se añade un comentario al cambio del valor por defecto (tag: v1) [Sergio Gómez]
   * 3283e0d 2013-06-16 | Se añade un parámetro por defecto (tag: v1-beta) [Sergio Gómez]
   * efc252e 2013-06-16 | Parametrización del programa [Sergio Gómez]
   * e19f2c1 2013-06-16 | Creación del proyecto [Sergio Gómez]

Vemos que estamos como en el final de la sección anterior, así que ahora
mezclamos:

::

   $ git merge -m "Aplicando los cambios de la rama hola" --no-ff hola
   Merge made by the 'recursive' strategy.
    lib/HolaMundo.php | 16 ++++++++++++++++
    lib/hola.php      |  4 +++-
    2 files changed, 19 insertions(+), 1 deletion(-)
    create mode 100644 lib/HolaMundo.php
   $ git hist --all
   *   2eab8ca 2013-06-16 | Aplicando los cambios de la rama hola (HEAD -> main) [Sergio Gomez]
   *\
   | * 9862f33 2013-06-16 | hola usa la clase HolaMundo (hola) [Sergio Gómez]
   | * 6932156 2013-06-16 | Añadida la clase HolaMundo [Sergio Gómez]
   |/
   * 9c85275 2013-06-16 | Programa interactivo (main) [Sergio Gómez]
   * c3e65d0 2013-06-16 | Añadido README.md [Sergio Gómez]
   * 81c6e93 2013-06-16 | Movido hola.php a lib [Sergio Gómez]
   * 96a39df 2013-06-16 | Añadido el autor del programa y su email [Sergio Gómez]
   * fd4da94 2013-06-16 | Se añade un comentario al cambio del valor por defecto (tag: v1) [Sergio Gómez]
   * 3283e0d 2013-06-16 | Se añade un parámetro por defecto (tag: v1-beta) [Sergio Gómez]
   * efc252e 2013-06-16 | Parametrización del programa [Sergio Gómez]
   * e19f2c1 2013-06-16 | Creación del proyecto [Sergio Gómez]

.. code:: mermaid

   gitGraph:
      commit id: "e19f2c1"
      commit id: "efc252e"
      commit id: "3283e0d" tag: "v1-beta"
      commit id: "fd4da94" tag: "v1"
      commit id: "96a39df"
      commit id: "8c2a509"
      commit id: "c3e65d0"
      commit id: "9c85275"
      branch hola
      commit id: "6932156"
      commit id: "9862f33"
      checkout main
      merge hola id: "2eab8ca"

En la siguiente imagen se puede ver la diferencia:

.. figure:: images/gitlab-merge-ff.png
   :alt: Diferencias entre tipos de fusión

   Diferencias entre tipos de fusión
