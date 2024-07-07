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
cambio. Primero lo sacamos del :term:`stage`:

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

Pero ahora sí hacemos commit:

.. code-block:: console
   :caption: Añadimos el cambio al stage

   $ git add curso-git-app

.. code-block:: console
   :caption: Confirmamos el cambio

   $ git commit -m "Ups... este commit está mal."

.. code-block:: console
   :caption: Salida del comando de confirmación

   $git commit -m "Ups... este commit está mal."
   [main 6354788] Ups... este commit está mal.
   1 file changed, 1 insertion(+), 1 deletion(-)

Bien, una vez confirmado el cambio, vamos a deshacer el cambio con la
orden ``git revert``:

.. code-block:: console
   :caption: Revertir el cambio

   $ git revert HEAD --no-edit

.. code-block:: console
   :caption: Salida del comando revert

   [main a0d1cb3] Revert "Ups... este commit está mal."
   Date: Fri Jul 5 08:53:28 2024 +0200
   1 file changed, 1 deletion(-)

.. code-block:: console
   :caption: Veamos como ha quedado el *log* después del cambio

   $ git log -2

.. code-block:: console
   :caption: Salida del comando log

   $ git log -2
   commit a0d1cb399a0b1cd6046f6a5d7ed768565ee54d13 (HEAD -> main)
   Author: Fernando Raya <f...@gmail.com>
   Date:   Fri Jul 5 08:53:28 2024 +0200

   Revert "Ups... este commit está mal."

   This reverts commit 6354788c9b952524164084478e1f3ad9a7503fd2.

   commit 6354788c9b952524164084478e1f3ad9a7503fd2
   Author: Fernando Raya <f...@gmail.com>
   Date:   Fri Jul 5 08:51:22 2024 +0200

   Ups... este commit está mal.

Borrar commits de una rama
^^^^^^^^^^^^^^^^^^^^^^^^^^

El anterior apartado revierte un :term:`commit`, pero deja huella en el
historial de cambios. Para hacer que no aparezca en el historial hay
que usar la orden ``git reset``. Volveremos a la versión *v1.1* antes
del commit.

.. code-block:: console
   :caption: Reset del historial al commit con etiqueta v1.1

   $ git reset --hard v1.1

Realizaremos el comando y miraremos como queda el repositorio:

.. code-block:: console
   :caption: Salida del comando reset y muestra del log

   $ git reset --hard v1.1
   HEAD is now at f0b885f Añade README

   $ git log -1
   commit f0b885f0b6aa3e0d18ebcb5bd3df23f33ea3a09f (HEAD -> main, tag: v1.1)
   Author: Fernando Raya <f...@gmail.com>
   Date:   Thu May 23 20:45:17 2024 +0200

   Añade README

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
acabamos de realizar. Tenemos nuestro archivo
``curso-de-git-app.dylan`` de la siguiente manera:

.. code-block:: dylan
   :caption: curso-de-git-app.dylan

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
   // thing we do.
   main(application-name(), application-arguments());

Añadiremos el autor del programa:

.. code-block:: dylan
   :caption: curso-de-git-app.dylan
   :emphasize-lines: 2

   Module: curso-de-git-app
   Author: Fernando Raya

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
   // thing we do.
   main(application-name(), application-arguments());

.. code-block:: console
   :caption: Añadimos el fichero al stage y confirmamos el cambio

   $ git commit -a -m "Añadido el autor del programa"

.. code-block:: console
   :caption: Salida de la confirmación del cambio

   $ git commit -a -m "Añadido el autor del programa"
   [main 910b3d8] Añadido el autor del programa
   1 file changed, 1 insertion(+)

.. tip::

   El parámetro ``-a`` hace un ``git add`` antes de hacer
   :term:`commit` de todos los archivos modificados o borrados (de los
   nuevos no), con lo que nos ahorramos un paso.

.. code-block:: console
   :caption: Nos percatamos que se nos ha olvidado poner el correo electrónico		

   $ git log -1
   commit 910b3d874fb201362bb21e9e09eccac4661718ab (HEAD -> main)
   Author: Fernando Raya <f...@gmail.com>
   Date:   Fri Jul 5 13:52:38 2024 +0200

Volvemos a modificar nuestro archivo:

.. code-block:: dylan
   :caption: curso-de-git-app.dylan
   :emphasize-lines: 3

   Module: curso-de-git-app
   Author: Fernando Raya
   Email: <f...@gmail.com>

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
   // thing we do.
   main(application-name(), application-arguments());

Y en esta ocasión usamos ``commit --amend`` que nos permite modificar
el último estado confirmado, sustituyéndolo por el estado actual:

.. code-block:: console
   :caption: Añadimos el fichero al stage

   $ git add curso-de-git-app.dylan

.. code-block:: console
   :caption: Enmendamos y confirmamos el cambio

   $ git commit --amend -m "Añadido el autor del programa y su email"

.. code-block:: console
   :caption: Salida del comando amend

   $ git commit --amend -m "Añadido el autor del programa y su email"
   [main 1419047] Añadido el autor del programa y su email
   Date: Fri Jul 5 13:52:38 2024 +0200
   1 file changed, 1 insertion(+)

   $ git log -2
   commit 141904775d0029e6a8286887b9d013c7372ba787 (HEAD -> main)
   Author: Fernando Raya <f...@gmail.com>
   Date:   Fri Jul 5 13:52:38 2024 +0200

   Añadido el autor del programa y su email

   commit f0b885f0b6aa3e0d18ebcb5bd3df23f33ea3a09f (tag: v1.1)
   Author: Fernando Raya <f...@gmail.com>
   Date:   Thu May 23 20:45:17 2024 +0200

   Añade README

.. warning::

   Nunca modifiques un :term:`commit` que ya hayas sincronizado con
   otro repositorio o que hayas recibido de él. Estarías alterando la
   historia de cambios y provocarías problemas de sincronización.

Moviendo y borrando archivos
----------------------------

Mover un archivo a otro directorio con git
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Para mover archivos usaremos la orden ``git mv``. Dividiremos los
ficheros fuente en subdirectorios, uno para la librería y otro para la
aplicación:

.. code-block:: console
   :caption: Creamos el directorio source para la librería

   $ mkdir -p source/lib

.. code-block:: console
   :caption: Ahora el directorio para la aplicación

   $ mkdir source/app

.. code-block:: console
   :caption: Movemos los archivos de la librería a ``source/lib``

   $ git mv curso-de-git.{dylan,lid} library.dylan source/lib

.. code-block:: console
   :caption: Mostremos el status

   $ git status

.. code-block:: console
   :caption: Salida del comando status

   $ git status
   On branch main
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           renamed:    curso-de-git.dylan -> source/lib/curso-de-git.dylan
           renamed:    curso-de-git.lid -> source/lib/curso-de-git.lid
           renamed:    library.dylan -> source/lib/library.dylan

.. code-block:: console
   :caption: Movemos los archivos de la aplicación a ``source/app``

   git mv curso-de-git-app.{dylan,lid} curso-de-git-app-library.dylan source/app

.. code-block:: console
   :caption: Mostremos el status

   $ git status
   On branch main
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           renamed:    curso-de-git-app-library.dylan -> source/app/curso-de-git-app-library.dylan
           renamed:    curso-de-git-app.dylan -> source/app/curso-de-git-app.dylan
           renamed:    curso-de-git-app.lid -> source/app/curso-de-git-app.lid
           renamed:    curso-de-git.dylan -> source/lib/curso-de-git.dylan
           renamed:    curso-de-git.lid -> source/lib/curso-de-git.lid
           renamed:    library.dylan -> source/lib/library.dylan

Guardaremos los cambios:

.. code-block:: console
   :caption: Confirmar cambios

   $ git commit -m "Reordenar ficheros en subdirectorios"

.. code-block:: console
  :caption: Salida de la confirmación

  $ git commit -m "Reordenar ficheros en subdirectorios"
  [main b386fd2] Reordenar ficheros en subdirectorios
   6 files changed, 0 insertions(+), 0 deletions(-)
   rename curso-de-git-app-library.dylan => source/app/curso-de-git-app-library.dylan (100%)
   rename curso-de-git-app.dylan => source/app/curso-de-git-app.dylan (100%)
   rename curso-de-git-app.lid => source/app/curso-de-git-app.lid (100%)
   rename curso-de-git.dylan => source/lib/curso-de-git.dylan (100%)
   rename curso-de-git.lid => source/lib/curso-de-git.lid (100%)
   rename library.dylan => source/lib/library.dylan (100%)

Mover y borrar archivos
^^^^^^^^^^^^^^^^^^^^^^^

Podíamos haber hecho el paso anterior con la órden del sistema ``mv``
y el resultado hubiera sido el mismo. Lo siguiente es a modo de
ejemplo y no es necesario que lo ejecutes:

.. code-block:: console

   $ mkdir -p source/{lib,app}
   $ mv curso-de-git.{dylan,lid} library.dylan source/lib
   $ mv curso-de-git-app.{dylan,lid} curso-de-git-app-library.dylan source/app
   $ git add source/lib
   $ git add source/app
   $ git rm curso-de-git.{dylan,lid} library.dylan
   $ git rm curso-de-git-app.{dylan,lid} curso-de-git-app-library.dylan

Y, ahora sí, ya podemos guardar los cambios:

.. code-block:: console

   $ git commit -m "Reordenar ficheros a subdirectorios"
