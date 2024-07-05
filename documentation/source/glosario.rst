Glosario
========

.. glossary::

   **Repositorio**
     El repositorio es el lugar en el que se almacenan los datos
     actualizados e históricos de cambios.

   *Repository*
     Ver :term:`Repositorio`

   **Revisión**
     Una revisión es una versión determinada de la información que se
     gestiona. Hay sistemas que identifican las revisiones con un
     contador (Ej. subversion). Hay otros sistemas que identifican las
     revisiones mediante un código de detección de modificaciones
     (Ej. git usa SHA1).

   **Etiqueta**
     Los tags permiten identificar de forma fácil revisiones
     importantes en el proyecto. Por ejemplo se suelen usar tags para
     identificar el contenido de las versiones publicadas del
     proyecto.

   *Tag*
     Ver :term:`Etiqueta`
      
   **Rama**
     Un conjunto de archivos puede ser ramificado o bifurcado en un
     punto en el tiempo de manera que, a partir de ese momento, dos
     copias de esos archivos se pueden desarrollar a velocidades
     diferentes o en formas diferentes de forma independiente el uno
     del otro.

   *Branch*
     Ver :term:`Rama`

   **Cambio**
     Un cambio (o diff, o delta) representa una modificación
     específica de un documento bajo el control de versiones. La
     granularidad de la modificación que es considerada como un
     cambio varía entre los sistemas de control de versiones.

   *Change*
      Ver :term:`Cambio`
      
   **Desplegar**
      Es crear una copia de trabajo local desde el repositorio. Un
      usuario puede especificar una revisión en concreto u obtener la
      última. El término :term:`checkout` también se puede utilizar
      como un sustantivo para describir la copia de trabajo.

   *Checkin*
      Ver :term:`desplegar`
      
   *Checkout*
      Ver :term:`desplegar`
       
   **Confirmar**
      Confirmar es escribir o mezclar los cambios realizados en la
      copia de trabajo del repositorio. Los términos :term:`commit` y
      :term:`checkin` también se pueden utilizar como sustantivos para
      describir la nueva revisión que se crea como resultado de
      confirmar.

   *Commit*
      Ver :term:`Confirmar`
    
   **Conflicto**
      Un conflicto se produce cuando diferentes partes realizan
      cambios en el mismo documento, y el sistema es incapaz de
      conciliar los cambios. Un usuario debe resolver el conflicto
      mediante la integración de los cambios, o mediante la selección
      de un cambio en favor del otro.

   *Conflict*
      Ver :term:`Conflicto`

   **Cabeza**
      También a veces se llama tip (punta) y se refiere a la última
      confirmación, ya sea en el tronco (:term:`trunk`) o en una rama
      :term:`branch`. El tronco y cada rama tienen su propia cabeza,
      aunque :term:`HEAD` se utiliza a veces libremente para referirse al
      :term:`tronco`.

   *Head*
      Ver :term:`Cabeza`
       
   **Tronco**
      La única línea de desarrollo que no es una rama (a veces
      también llamada línea base, línea principal o master).

   *Trunk*
      Ver :term:`Tronco`
       
   **Fusionar**
      Una fusión o integración es una operación en la que se aplican
      dos tipos de cambios en un archivo o conjunto de
      archivos. Algunos escenarios de ejemplo son los siguientes:

      - Un usuario, trabajando en un conjunto de archivos, actualiza
        o sincroniza su copia de trabajo con los cambios realizados y
        confirmados, por otros usuarios, en el repositorio.

      - Un usuario intenta confirmar archivos que han sido actualizado
        por otros usuarios desde el último despliegue
        (:term:`checkout`), y el software de control de versiones
        integra automáticamente los archivos (por lo general, después
        de preguntarle al usuario si se debe proceder con la
        integración automática, y en algunos casos sólo se hace si la
        fusión puede ser clara y razonablemente resuelta).
 
      - Un conjunto de archivos se bifurca, un problema que existía
        antes de la ramificación se trabaja en una nueva rama, y la
        solución se combina luego en la otra rama.

      - Se crea una rama, el código de los archivos es independiente
        editado, y la rama actualizada se incorpora más tarde en un
        único tronco unificado.

   *Merge*
      Ver :term:`Fusionar`

   **Integrar**
      Ver :term:`Fusionar`

   **Mezclar**
      Ver :term:`Fusionar`

   *Stage*
      Staged o preparado significa que has marcado un archivo
      modificado en su versión actual para que vaya en tu próxima
      confirmación.

   *staging*
      Acción de pasar un archivo al :term:`stage`.
