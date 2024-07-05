Uso avanzado de Git
===================

Deshacer cambios
----------------

Deshaciendo cambios antes de la fase de staging
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Volvemos a la rama máster y vamos a modificar el comentario que
pusimos:

.. code-block:: console
		
   $ git checkout main
   Ya en 'main'

Modificamos ``curso-git-app.dylan`` de la siguiente manera:

.. code-block:: dylan
   :caption: ``curso-git-app.dylan``
   :emphasize-lines: 6

   Module: curso-git-app

   define function main
       (name :: <string>, arguments :: <vector>)
       // Si no hay argumentos poner mensaje por defecto
       // TODO: cambiar "mundo" por constante $greeting
       let mensaje = if (arguments.size < 1)
		       "mundo"
		     else
		       arguments[0]
		     end;
       format-out("%s\n", greeting(mensaje));
       exit-application(0);
    end function;

    // Calling our main function (which could have any name) should be the last
    // thing we do.
    main(application-name(), application-arguments());

Y comprobamos:

.. code-block:: console

   $ git status
     En la rama main
     Cambios no rastreados para el commit:
       (usa "git add <archivo>..." para actualizar lo que será confirmado)
       (usa "git restore <archivo>..." para descartar los cambios en el directorio de trabajo)
             modificados:     curso-git-app.dylan

   sin cambios agregados al commit (usa "git add" y/o "git commit -a")

El mismo Git nos indica que debemos hacer para añadir los cambios o
para deshacerlos:

.. code-block:: console

   $ git restore curso-git-app.dylan

Comprobamos que todo está igual que antes de añadir el comentario:

.. code-block:: console

   $ git status
   En la rama main
   nada para hacer commit, el árbol de trabajo está limpio
   $ cat curso-git-app.dylan


Deshaciendo cambios antes del commit
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Vamos a hacer lo mismo que la vez anterior, añadiendo el comentario,
pero esta vez sí añadiremos el cambio al :term:`staging` (sin hacer
:term:`commit`).

Así que volvemos a modificar ``curso-git-app.dylan`` igual que la
anterior ocasión:

.. code-block:: dylan
   :caption: ``curso-git-app.dylan``
   :emphasize-lines: 6

   Module: curso-git-app

   define function main
       (name :: <string>, arguments :: <vector>)
     // Si no hay argumentos poner mensaje por defecto
     // TODO: cambiar "mundo" por constante $greeting
     let mensaje = if (arguments.size < 1)
                     "mundo"
		   else
		     arguments[0]
		   end;
     format-out("%s\n", greeting(mensaje));
     exit-application(0);
   end function;

   // Calling our main function (which could have any name) should be the last
   // thing we do.
   main(application-name(), application-arguments());

Y lo añadimos al :term:`staging`

.. code-block:: console

   $ git add curso-git-app.dylan

.. code-block:: console

   $ git status
   En la rama main
   Cambios a ser confirmados:
     (usa "git restore --staged <archivo>..." para sacar del área de stage)
	   modificados:     curso-git-app.dylan

De nuevo, Git nos indica qué debemos hacer para deshacer el
cambio. Primero lo sacamos del :temp:`stage`:

.. code-block:: console

   $ git restore --staged curso-git-app.dylan

Después restauramos la copia de trabajo:

.. code-block:: console

   $ git restore curso-git-app.dylan

Y ya tenemos nuestro repositorio limpio otra vez. Como vemos hay que
hacerlo en dos pasos: uno para borrar los datos del :term:`staging` y
otro para restaurar la copia de trabajo.

Deshaciendo commits no deseados
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Si a pesar de todo hemos hecho un commit y nos hemos equivocado,
podemos deshacerlo con la orden ``git revert``.
Modificamos otra vez el archivo como antes:

.. code-block:: dylan


Pero ahora sí hacemos commit:

.. code-block:: console

   $ git add hola.php
   $ git commit -m "Ups... este commit está mal."
   main 5a5d067] Ups... este commit está mal
   1 file changed, 1 insertion(+), 1 deletion(-)

Bien, una vez confirmado el cambio, vamos a deshacer el cambio con la
orden ``git revert``:

.. code-block:: console

   $ git revert HEAD --no-edit
   [main 817407b] Revert "Ups... este commit está mal"
   1 file changed, 1 insertion(+), 1 deletion(-)
   $ git hist
   * 817407b 2013-06-16 | Revert "Ups... este commit está mal" (HEAD, main) [Sergio Gómez]
   * 5a5d067 2013-06-16 \| Ups... este commit está mal [Sergio Gómez]
   * fd4da94 2013-06-16 \| Se añade un comentario al cambio del valor por defecto (tag: v1) [Sergio Gómez]
   * 3283e0d 2013-06-16 \| Se añade un parámetro por defecto (tag: v1-beta) [Sergio Gómez]
   * efc252e 2013-06-16 \| Parametrización del programa [Sergio Gómez]
   * e19f2c1 2013-06-16 \| Creación del proyecto [Sergio Gómez]

.. graphviz::

   digraph G {
   }


Borrar commits de una rama
^^^^^^^^^^^^^^^^^^^^^^^^^^

El anterior apartado revierte un commit, pero deja huella en el
historial de cambios. Para hacer que no aparezca hay que usar la orden
``git reset``.

.. code-block:: console

   $ git reset --hard v1 H
   HEAD is now at fd4da94 Se añade un comentario al cambio de valor por defecto
   $ git hist
   * fd4da94 2013-06-16 | Se añade un comentario al cambio del valor por defecto (HEAD, tag: v1, main) [Sergio Góme
   * 3283e0d 2013-06-16 | Se añade un parámetro por defecto (tag: v1-beta) [Sergio Gómez]
   * efc252e 2013-06-16 | Parametrización del programa [Sergio Gómez]
   * e19f2c1 2013-06-16 | Creación del proyecto [Sergio Gómez]

.. graphviz::

   digraph G {
   }
   
El resto de cambios no se han borrado (aún), simplemente no están
accesibles porque git no sabe como referenciarlos. Si sabemos su hash
podemos acceder aún a ellos. Pasado un tiempo, eventualmente Git tiene
un recolector de basura que los borrará. Se puede evitar etiquetando
el estado final.

.. warning::

    La orden ``reset`` es una operación delicada. Debe evitarse si no
    se sabe bien lo que se está haciendo, sobre todo cuando se trabaja
    en repositorios compartidos, porque podríamos alterar la historia
    de cambios lo cual puede provocar problemas de sincronización.

Modificar un commit
^^^^^^^^^^^^^^^^^^^

Esto se usa cuando hemos olvidado añadir un cambio a un commit que
acabamos de realizar. Tenemos nuestro archivo \_hola.php\_ de la
siguiente manera:

.. code-block:: dylan

Y lo confirmamos:

.. code-block:: console

   $ git commit -a -m "Añadido el autor del programa"
   [main cf405c1] Añadido el autor del programa
   1 file changed, 1 insertion(+)

.. graphviz::

   digraph G {
   }

.. tip::

   El parámetro ``-a`` hace un ``git add`` antes de hacer
   :term:`commit` de todos los archivos modificados o borrados (de los
   nuevos no), con lo que nos ahorramos un paso.

Ahora nos percatamos que se nos ha olvidado poner el correo
electrónico. Así que volvemos a modificar nuestro archivo:

.. code-block:: dylan


Y en esta ocasión usamos ``commit --amend`` que nos permite modificar
el último estado confirmado, sustituyéndolo por el estado actual:

.. code-block:: console

   $ git add hola.php
   $ git commit --amend -m "Añadido el autor del programa y su email"
   [main 96a39df] Añadido el autor del programa y su email
   1 file changed, 1 insertion(+)
   $ git hist

.. graphviz::

   digraph G {
   }
   
.. warning::
   
   Nunca modifiques un :term:`commit` que ya hayas sincronizado con
   otro repositorio o que hayas recibido de él. Estarías alterando la
   historia de cambios y provocarías problemas de sincronización.

Moviendo y borrando archivos
----------------------------

Mover un archivo a otro directorio con git
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Para mover archivos usaremos la orden ``git mv``:

.. code-block:: console
		
   $ mkdir lib
   $ git mv hola.php lib
   $ git status
   # On branch main
   # Changes to be committed:
   # (use "git reset HEAD ..." to unstage)
   #
   # renamed: hola.php -> lib/hola.php
   #

Mover y borrar archivos
^^^^^^^^^^^^^^^^^^^^^^^

Podíamos haber hecho el paso anterior con la órden del sistema ``mv``
y el resultado hubiera sido el mismo. Lo siguiente es a modo de
ejemplo y no es necesario que lo ejecutes:

.. code-block:: console
		
   $ mkdir lib
   $ mv hola.php lib
   $ git add lib/hola.php
   $ git rm hola.php

Y, ahora sí, ya podemos guardar los cambios:

.. code-block:: console
		
   $ git commit -m "Movido hola.php a lib."
   [main 8c2a509] Movido hola.php a lib.
   1 file changed, 0 insertions(+), 0 deletions(-)
   rename hola.php => lib/hola.php (100%)

.. graphviz::

   digraph G {
   }

