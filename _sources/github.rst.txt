.. _`_github`:

Github
======

`Github <https://github.com>`__ es lo que se denomina una forja, un
repositorio de proyectos que usan Git como sistema de control de
versiones. Es la forja más popular, ya que alberga millones de
repositorios. Debe su popularidad a sus funcionalidades sociales,
principalmente dos: la posibilidad de hacer forks de otros proyectos y
la posibilidad de cooperar aportando código para arreglar errores o
mejorar el código. Si bien, no es que fuera una novedad, sí lo es lo
fácil que resulta hacerlo. A raíz de este proyecto han surgido otros
como `Gitlab <https://about.gitlab.com>`__, pero *Github* sigue siendo
el más popular y el que tiene mejores y mayores
características. algunas de estas son:

-  Un wiki para documentar el proyecto, que usa MarkDown como lenguaje
   de marca.

-  Un portal web para cada proyecto.

-  Funcionalidades de redes sociales como followers.

-  Gráficos estadísticos.

-  Revisión de código y comentarios.

-  Sistemas de seguimiento de incidencias.

Lo primero es entrar en el portal (https://github.com/) para crearnos
una cuenta si no la tenemos aún.

.. _`_tu_clave_públicaprivada`:

Tu clave pública/privada
------------------------

Muchos servidores Git utilizan la autentificación a través de claves
públicas SSH. Y, para ello, cada usuario del sistema ha de generarse
una, si es que no la tiene ya. El proceso para hacerlo es similar en
casi cualquier sistema operativo. Ante todo, asegurarte que no tengas ya
una clave. (comprueba que el directorio ``$HOME/usuario/.ssh`` no tiene
un archivo ``id_dsa.pub`` o ``id_rsa.pub``).

Para crear una nueva clave usamos la siguiente orden:

.. code-block:: console

   $ ssh-keygen -t ed25519 -C "tu_email@example.com"

.. note::

   Si estas usando un sistemas antiguo (*legacy*) que no soporte el
   algoritmo Ed25519 usa:

   .. code-block:: console

      $ ssh-keygen -t rsa -C "tu_email@example.com"

.. warning::

   Tu clave te identifica contra los repositorios remotos, asegúrate de
   no compartir la clave privada con nadie. Por defecto la clave se crea
   como *solo lectura*.

.. _`_configuración`:

Configuración
-------------

Vamos a aprovechar para añadir la clave RSA que generamos antes, para
poder acceder desde git a los repositorios. Para ellos nos vamos al menú
de configuración de usuario (*Settings*)

.. figure:: images/github-topbar.png
   :alt: Barra principal de menú

   Barra principal de menú

Nos vamos al menú *SSH and GPG Keys* y añadimos una nueva clave. En
*Title* indicamos una descripción que nos ayude a saber de dónde procede
la clave y en key volcamos el contenido del archivo
``~/.ssh/id_ed25519.pub``. Y guardamos la clave.

.. figure:: images/github-sshkeys.png
   :alt: Clave SSH

   Clave SSH

Con esto ya tendriamos todo nuestro entorno para poder empezar a
trabajar desde nuestro equipo.

.. _`_clientes_gráficos_para_github`:

Clientes gráficos para GitHub
-----------------------------

Además, para Github existe un cliente propio, `Github
desktop <https://desktop.github.com>`__ o `Github
CLI <https://cli.github.com>`__

De todas maneras, estos clientes solo tienen el fin de facilitar el uso
de Github, pero no son necesarios para usarlo. Es perfectamente válido
usar el cliente de consola de Git o cualquier otro cliente genérico para
Git. Uno de los más usados actualmente es
`GitKraken <https://www.gitkraken.com/>`__.

.. _`_crear_un_repositorio`:

Crear un repositorio
--------------------

Vamos a crear un repositorio donde guardar nuestro proyecto. Para ello
pulsamos el signo *+* que hay en la barra superior y seleccionamos
*New repository*.

Ahora tenemos que designar un nombre para nuestro repositorio, por
ejemplo: *taller-de-git*.

.. figure:: images/github-newrepo.png
   :alt: Nuevo repositorio

   Nuevo repositorio

Nada más crear el repositorio nos saldrá una pantalla con instrucciones
precisas de como proceder a continuación.

Básicamente podemos partir de tres situaciones:

1. Todavía no hemos creado ningún repositorio en nuestro equipo.

2. Ya tenemos un repositorio creado y queremos sincronizarlo con Github.

3. Queremos importar un repositorio de otro sistema de control de
   versiones distinto.

.. figure:: images/github-quicksetup.png
   :alt: Quick setup

   Quick setup

.. _`_clonar_un_repositorio`:

Clonar un repositorio
---------------------

Una vez que tengamos el repositorio en Github, eventualmente vamos a
querer descargarlo en otro de nuestros ordenadores para poder trabajar
en él. Esta acción se denomina **clonar** y para ello usaremos la orden
``git clone``.

.. figure:: images/github-clone-or-download.png
   :alt: Clonar o descargar

   Clonar o descargar

En la página principal de nuestro proyecto podemos ver un botón que
indica ``Code`. Si lo pulsamos aparece *clone* o *download*. Podemos
elegir entre clonar con *ssh* o *https*. Recordad que si estáis en
otro equipo y queréis seguir utilizando ssh deberéis generar otra para
de claves privada/pública como hicimos anteriormente e instalarla en
nuestro perfil de Github.

Para clonar nuestro repositorio y poder trabajar con él todo lo que
debemos hacer es lo siguiente:

.. code-block:: console

   $ git clone git@github.com:fraya/curso-de-git.git
   $ cd curso-de-git

.. _`_ramas_remotas`:

Ramas remotas
-------------

Si ahora vemos el estado de nuestro proyecto veremos algo similar a
esto:

.. code-block:: console
   :caption: Listado del historial

   $ git hist --all
   * [2024-07-05] [b386fd2] | Reordenar ficheros en subdirectorios {{Fernando Raya}}  (HEAD -> main, origin/main)
   * [2024-07-05] [1419047] | Añadido el autor del programa y su email {{Fernando Raya}} 
   * [2024-05-23] [f0b885f] | Añade README {{Fernando Raya}}  (tag: v1.1)
   * [2024-05-23] [88a170e] | Añadir comentario {{Fernando Raya}}  (tag: v1)
   * [2024-05-23] [dfb648b] | Añadir parámetro por defecto {{Fernando Raya}}
   * [2024-05-23] [7645f86] | Parametrizar el mensaje de saludo {{Fernando Raya}} 
   * [2024-05-22] [6bf8f65] | Revision inicial {{Fernando Raya}}

Aparece que hay una nueva rama llamada ``origin/main``. Esta rama
indica el estado de sincronización de nuestro repositorio con un
repositorio remoto llamado *origin*, en este caso el de *Github*.

.. note::

   Por norma se llama automáticamente *origin* al primer repositorio con
   el que sincronizamos nuestro repositorio.

Podemos ver la configuración de este repositorio remoto con la orden
``git remote``:

.. code-block:: console
   :caption: git remote show origin

   $ git remote show origin
   * remote origin
    Fetch URL: git@github.com:fraya/curso-de-git.git
    Push  URL: git@github.com:fraya/curso-de-git.git
    HEAD branch: main
    Remote branch:
      main tracked
    Local branch configured for 'git pull':
      main merges with remote main
    Local ref configured for 'git push':
      main pushes to main (up to date)

De la respuesta tenemos que fijarnos en las líneas que indican *fetch* y
*push* puesto que son las acciones de sincronización de nuestro
repositorio con el remoto. Mientras que *fetch* se encarga de traer los
cambios desde el repositorio remoto al nuestro, *push* los envía.

.. _`_enviando_actualizaciones`:

Enviando actualizaciones
------------------------

Vamos a añadir una licencia a nuestra aplicación. Creamos un fichero
LICENSE con el siguiente contenido:

.. code-block:: console

   MIT License

   Copyright (c) [year] [fullname]

   Permission is hereby granted, free of charge, to any person obtaining a copy
   of this software and associated documentation files (the "Software"), to deal
   in the Software without restriction, including without limitation the rights
   to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
   copies of the Software, and to permit persons to whom the Software is
   furnished to do so, subject to the following conditions:

   The above copyright notice and this permission notice shall be included in all
   copies or substantial portions of the Software.

   THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
   IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
   FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
   AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
   LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
   OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
   SOFTWARE.

Y añadidos y confirmamos los cambios:

.. code-block:: console
   :caption: Añadimos la licencia

   $ git add LICENSE

.. code-block:: console
   :caption: Confirmamos la licencia

   $ git commit -m "Añadida licencia"

.. code-block:: console
   :caption: Salida de la confirmación

   $ git commit -m "Añadida licencia"
   [main 70ef551] Añadida licencia
   1 file changed, 21 insertions(+)
   create mode 100644 LICENSE

.. code-block:: console
   :caption: Listado del historial
   :emphasize-lines: 2, 3

   $ git hist --all
   * [2024-07-08] [70ef551] | Añadida licencia {{Fernando Raya}}  (HEAD -> main)
   * [2024-07-05] [b386fd2] | Reordenar ficheros en subdirectorios {{Fernando Raya}}  (origin/main)
   * [2024-07-05] [1419047] | Añadido el autor del programa y su email {{Fernando Raya}} 
   * [2024-05-23] [f0b885f] | Añade README {{Fernando Raya}}  (tag: v1.1)
   * [2024-05-23] [88a170e] | Añadir comentario {{Fernando Raya}}  (tag: v1)
   * [2024-05-23] [dfb648b] | Añadir parámetro por defecto {{Fernando Raya}} 
   * [2024-05-23] [7645f86] | Parametrizar el mensaje de saludo {{Fernando Raya}} 
   * [2024-05-22] [6bf8f65] | Revision inicial {{Fernando Raya}}

Viendo la historia podemos ver como nuestro master no está en el mismo
punto que ``origin/main``. Si vamos a la web de *Github* veremos que
``LICENSE`` no aparece aún. Así que vamos a enviar los cambios con la
primera de las acciones que vimos ``git push``:

.. code-block:: console
  :caption: git push

   $ git push -u origin main

.. code-block:: console
   :caption: Salida del comando *push*

   $ git push -u origin main
   Enumerating objects: 4, done.
   Counting objects: 100% (4/4), done.
   Delta compression using up to 4 threads
   Compressing objects: 100% (3/3), done.
   Writing objects: 100% (3/3), 902 bytes | 902.00 KiB/s, done.
   Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
   remote: Resolving deltas: 100% (1/1), completed with 1 local object.
   To github.com:fraya/curso-de-git.git
      b386fd2..70ef551  main -> main
   branch 'main' set up to track 'origin/main'.

.. note::

   La orden ``git push`` necesita dos parámetros para funcionar: el
   repositorio y la rama destino. Así que realmente lo que teníamos que
   haber escrito es:

   ::

      $ git push origin main

   Para ahorrar tiempo escribiendo *git* nos deja vincular nuestra rama
   local con una rama remota, de tal manera que no tengamos que estar
   siempre indicándolo. Eso es posible con el parámetro
   ``--set-upstream`` o ``-u`` en forma abreviada.

   .. code-block:: console

      $ git push -u origin main

   Si repasas las órdenes que te indicó Github que ejecutaras verás que
   el parámetro ``-u`` estaba presente y por eso no ha sido necesario
   indicar ningún parámetro al hacer push.

.. _`_recibiendo_actualizaciones`:

Recibiendo actualizaciones
--------------------------

Si trabajamos con más personas, o trabajamos desde dos ordenadores
distintos, nos encontraremos con que nuestro repositorio local es más
antiguo que el remoto. Necesitamos descargar los cambios para poder
incorporarlos a nuestro directorio de trabajo.

Para la prueba, Github nos permite editar archivos directamente desde la
web. Pulsamos sobre el archivo ``README.md``. En la vista del archivo,
veremos que aparece el icono de un lápiz. Esto nos permite editar el
archivo.

.. figure:: images/github-edit.png
   :alt: Editar archivo

   Editar archivo

.. note::

   Los archivos con extensión ``.md`` están en un formato denominado
   *MarkDown*. Se trata de un lenguaje de marca que nos permite escribir
   texto enriquecido de manera muy sencilla.

Modificamos el archivo como queramos, por ejemplo, añadiendo el tipo
de licencia:

.. code-block:: console
   :caption: Fichero ``README.md``

   # Curso de Git

   Estamos aprendiendo Git y Github

   ## Licencia

   Ver fichero [LICENSE](./LICENSE)

.. figure:: images/github-changes.png
   :alt: Confirmar cambios

   Confirmar cambios

El cambio quedará incorporado al repositorio de Github, pero no al
nuestro. Necesitamos traer la información desde el servidor remoto. La
orden asociada es ``git fetch``:

.. code-block:: console
   :caption: Traemos la información del servidor remoto

   $ git fetch

.. code-block:: console
   :caption: Salida del comando *fetch*

   $ git fetch
   remote: Enumerating objects: 5, done.
   remote: Counting objects: 100% (5/5), done.
   remote: Compressing objects: 100% (3/3), done.
   remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
   Unpacking objects: 100% (3/3), 1014 bytes | 507.00 KiB/s, done.
   From github.com:fraya/curso-de-git
      70ef551..257432b  main       -> origin/main

.. code-block:: console
   :caption: Listar historial despúes del cambio remoto
   :emphasize-lines: 2, 3

   $ git hist --all
   * [2024-07-08] [257432b] | Update README.md {{Fernando Raya}}  (origin/main)
   * [2024-07-08] [70ef551] | Añadida licencia {{Fernando Raya}}  (HEAD -> main)
   * [2024-07-05] [b386fd2] | Reordenar ficheros en subdirectorios {{Fernando Raya}} 
   * [2024-07-05] [1419047] | Añadido el autor del programa y su email {{Fernando Raya}} 
   * [2024-05-23] [f0b885f] | Añade README {{Fernando Raya}}  (tag: v1.1)
   * [2024-05-23] [88a170e] | Añadir comentario {{Fernando Raya}}  (tag: v1)
   * [2024-05-23] [dfb648b] | Añadir parámetro por defecto {{Fernando Raya}}
   * [2024-05-23] [7645f86] | Parametrizar el mensaje de saludo {{Fernando Raya}} 
   * [2024-05-22] [6bf8f65] | Revision inicial {{Fernando Raya}}

Ahora vemos el caso contrario, tenemos que ``origin/main`` está por
delante que ``HEAD`` y que la rama ``main`` local.

Ahora necesitamos incorporar los cambios de la rama remota en la
local.  La forma de hacerlo lo vimos en el capítulo anterior mezclar
ramas usando ``git merge`` o ``git rebase``.

Habitualmente se usa ``git merge``:

.. code-block:: console
   :caption: Mezclamos el repositorio

   $ git merge origin/main

.. code-block:: console
   :caption: Salida del comando *merge*

   $ git merge origin/main
   Updating 70ef551..257432b
   Fast-forward
    README.md | 4 ++++
    1 file changed, 4 insertions(+)

.. code-block::
   :caption: Salida del historial tras *merge*
   :emphasize-lines: 2

   $ git hist --all
   * [2024-07-08] [257432b] | Update README.md {{Fernando Raya}}  (HEAD -> main, origin/main)
   * [2024-07-08] [70ef551] | Añadida licencia {{Fernando Raya}}
   * [2024-07-05] [b386fd2] | Reordenar ficheros en subdirectorios {{Fernando Raya}} 
   * [2024-07-05] [1419047] | Añadido el autor del programa y su email {{Fernando Raya}} 
   * [2024-05-23] [f0b885f] | Añade README {{Fernando Raya}}  (tag: v1.1)
   * [2024-05-23] [88a170e] | Añadir comentario {{Fernando Raya}}  (tag: v1)
   * [2024-05-23] [dfb648b] | Añadir parámetro por defecto {{Fernando Raya}}
   * [2024-05-23] [7645f86] | Parametrizar el mensaje de saludo {{Fernando Raya}} 
   * [2024-05-22] [6bf8f65] | Revision inicial {{Fernando Raya}}

.. note::

   Como las operaciones de traer cambios (``git fetch``) y de mezclar ramas
   (``git merge`` o ``git rebase``) están muy asociadas, *git* nos ofrece
   una posibilidad para ahorrar pasos que es la orden ``git pull`` que
   realiza las dos acciones simultáneamente.

Para probar, vamos a editar de nuevo el archivo README.md y añadimos
algo más:

.. figure:: images/github-pull-change.png
   :caption: Cambiamos otra vez README.md

.. figure:: images/github-pull-change-commit.png
   :caption: Confirmar el cambio en README.md

Y ahora probamos a actualizar con ``git pull``:

.. code-block:: console

   $ git pull

.. code-block:: console
   :caption: Salida del comando ``git pull``

   $ git pull
   remote: Enumerating objects: 5, done.
   remote: Counting objects: 100% (5/5), done.
   remote: Compressing objects: 100% (3/3), done.
   remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
   Unpacking objects: 100% (3/3), 1.05 KiB | 537.00 KiB/s, done.
   From github.com:fraya/curso-de-git
      257432b..ae4f94d  main       -> origin/main
   Updating 257432b..ae4f94d
   Fast-forward
    README.md | 4 ++++
    1 file changed, 4 insertions(+)


Vemos que los cambios se han incorporado y que las ramas remota y local
de *main* están sincronizadas.

.. _`_problemas_de_sincronización`:

Problemas de sincronización
---------------------------

.. _`_no_puedo_hacer_push`:

No puedo hacer push
~~~~~~~~~~~~~~~~~~~

A veces, al intentar subir cambios nos podemos realizarlo y
encontramos un mensaje como este:

.. code-block:: console

   $ git push
   To github.com:fraya/curso-de-git.git
    ! [rejected]        main -> main (non-fast-forward)
   error: failed to push some refs to 'github.com:fraya/curso-de-git.git'
   hint: Updates were rejected because the tip of your current branch is behind
   hint: its remote counterpart. Integrate the remote changes (e.g.
   hint: 'git pull ...') before pushing again.
   hint: See the 'Note about fast-forwards' in 'git push --help' for details.

La causa es que hemos modificado el repositorio local y el repositorio
remoto también se ha actualizado pero nosotros aún no hemos recibido
esos cambios. Es decir, ambos repositorios se han actualizado y el
remoto tiene preferencia. Hay un conflicto en ciernes y se debe
resolver localmente antes de continuar.

Vamos a provocar una situación donde podamos ver esto en acción. Vamos a
modificar el archivo ``README.md`` tanto en local como en remoto a
través del interfaz web.

En la web vamos a cambiar el fichero ``README.md``. Cambiaremos el
título y añadiremos la versión para que aparezca de la siguiente manera.

.. code-block:: console
   :caption: Fichero ``README.md`` en GitHub

   # Curso de GIT
   v0.1

Haz un :term:`commit` en Github.

En local vamos a cambiar la versión para que aparezca de la siguiente
manera.

.. code-block:: console
   :caption: Fichero ``README.md`` en local

   # Curso de GIT
   v0.1.0
   
Realiza un :term:`commit` para guardar el cambio en local.

.. code-block:: console
   :caption: Confirmar cambio a ``README.md`` en local

   $ git commit -am "Add version v0.1.0"

.. code-block:: console
   :caption: Salida del comando ``git commit``

   $ git commit -am "Add version v0.1.0"
   [main 1e8c0b7] Añadido el mes al README
   1 file changed, 1 insertion(+), 1 deletion(-)

Al intentar ``git pull`` en local, encontramos un conflicto.  La forma
de proceder en este caso, es hacer un ``git fetch`` y un ``git
rebase``. Si hay conflictos deberán resolverse. Cuando esté todo
solucionado ya podremos hacer ``git push``.

.. note::

   Por defecto ``git pull`` lo que hace es un ``git merge``, si queremos
   hacer ``git rebase`` deberemos especificarlos con el parámetro ``-r``:

   .. code-block:: console

       $ git pull --rebase

Vamos a hacer el :term:`pull` con rebase y ver qué sucede.

.. code-block:: console
   :caption: Comando para descargar los cambios y fusionar cambiando de base

   $ git pull --rebase

.. code-block:: console
   :caption: Salida del comando ``git pull --rebase``

   $ git pull --rebase
   Auto-merging README.md
   CONFLICT (content): Merge conflict in README.md
   error: could not apply 84cc97b... Add version v0.1.0
   hint: Resolve all conflicts manually, mark them as resolved with
   hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
   hint: You can instead skip this commit: run "git rebase --skip".
   hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
   Could not apply 84cc97b... Add version v0.1.0

Evidentemente hay un conflicto porque hemos tocado el mismo archivo. Se
deja como ejercicio resolverlo.

Si editamos el fichero ``README.md`` local tenemos:

.. code-block:: console
   :caption: Fichero ``README.md`` en local
   :emphasize-lines: 2-6

   # Curso de Git
   <<<<<<< HEAD
   v0.1
   =======
   v0.1.1
   >>>>>>> 84cc97b (Add version v0.1.1)

   Estamos aprendiendo Git y Github

   ## Colaboradores

   - Fernando Raya <f...@gmail.com>

   ## Licencia

   Ver fichero [LICENSE](./LICENSE)

En las líneas resaltadas podemos ver la diferencia que hay entre la
versión remota con la versión local marcada por los caracteres ``>>>``.

El contenido final del fichero podría ser el siguiente:

.. code-block:: console
   :caption: Fichero ``README.md``
	     
   # Curso de Git
   v0.1.1

   Estamos aprendiendo Git y Github

   ## Colaboradores

   - Fernando Raya <f...@gmail.com>

   ## Licencia

   Ver fichero [LICENSE](./LICENSE)

   
A continuación confirmamos los cambios y los enviamos al servidor

.. code-block:: console
		
   $ git add README.md
   $ git rebase --continue
   $ git push

.. warning::

   ¿Por qué hemos hecho rebase en *main* si a lo largo del curso hemos
   dicho que no se debe cambiar la línea principal?

   Básicamente hemos dicho que lo que no debemos hacer es modificar la
   línea temporal **compartida**. En este caso nuestros cambios en
   *main* solo estaban en nuestro repositorio, porque al fallar el
   envío nadie más ha visto nuestras actualizaciones. Al hacer
   *rebase* estamos deshaciendo nuestros cambios, bajarnos la última
   actualización compartida de *main* y volviéndolos a aplicar. Con
   lo que realmente la historia compartida no se ha modificado.

Este es un problema que debemos evitar en la medida de lo posible. La
menor cantidad de gente posible debe tener acceso de escritura en
*main* y las actualizaciones de dicha rama deben hacerse a través de
ramas secundarias y haciendo :term:`merge` en *main* como hemos visto
en el capítulo de ramas.

.. _`_no_puedo_hacer_pull`:

No puedo hacer pull
~~~~~~~~~~~~~~~~~~~

Al intentar descargar cambios nos podemos encontrar un mensaje como
este:

.. code-block:: console

   $ git pull
   error: Cannot pull with rebase: You have unstaged changes.

O como este:

.. code-block:: console

   $ git pull
   error: Cannot pull with rebase: Your index contains uncommitted changes.

Básicamente lo que ocurre es que tenemos cambios sin confirmar en
nuestro espacio de trabajo. Una opción es confirmar (*commit*) y
entonces proceder como el caso anterior.

Pero puede ocurrir que aún estemos trabajando todavía y no nos
interese confirmar los cambios, solo queremos sincronizar y seguir
trabajando.  Para casos como estos *git* ofrece una pila para guardar
cambios temporalmente. Esta pila se llama *stash* y nos permite
restaurar el espacio de trabajo al último commit.

De nuevo vamos a modificar nuestro proyecto para ver esta situación en
acción.

.. container:: informalexample

   En remoto borra el año de la fecha y en local borra el mes. Pero esta
   vez **no hagas commit en local**. El archivo solo debe quedar
   modificado.

La forma de proceder es la siguiente:

::

   $ git stash save # Guardamos los cambios en la pila
   $ git pull # Sincronizamos con el repositorio remoto, -r para hacer rebase puede ser requerido
   $ git stash pop # Sacamos los cambios de la pila

.. note::

   Como ocurre habitualmente, git nos proporciona una forma de hacer
   todos estos pasos de una sola vez. Para ello tenemos que ejecutar lo
   siguiente:

   .. code-block:: console

      $ git pull --autostash

   En general no es mala idea ejecutar lo siguiente si somos
   conscientes, además, de que tenemos varios cambios sin sincronizar:

   .. code-block:: console

      $ git pull --autostash --rebase

Podría darse el caso de que al sacar los cambios de la pila hubiera
algún conflicto. En ese caso actuamos como con el caso de *merge* o
*rebase*.

De nuevo este tipo de problemas no deben suceder si nos acostumbramos
a trabajar en ramas.
