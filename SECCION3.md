# 3. FLUJOS DE TRABAJO BASICO EN GIT

## 13. Flujo de trabajo básico con un repositorio remoto

Hasta ahora, todo tu trabajo vivía encerrado en tu computadora (Repositorio Local). Esta clase introduce el concepto de Repositorio Remoto (como GitHub, GitLab o Bitbucket). Un repositorio remoto es un servidor en internet que funciona como el punto de encuentro central donde tú y tu equipo sincronizan su código.

Para lograr esta comunicación entre tu máquina y el servidor, el flujo de trabajo tradicional (add y commit) se expande con nuevos comandos de red: clone (para descargar), push (para subir) y pull (para actualizar).

Para entender cómo viaja la información entre tu computadora y el servidor remoto, debes conocer los comandos de red fundamentales:

**1. El inicio colaborativo: git clone**

Ya no usas git init para empezar desde cero.

Cuando te unes a un proyecto existente (o clonas uno propio desde GitHub), usas git clone [URL_del_repositorio].

Esto hace dos cosas: descarga todos los archivos físicos a tu carpeta de trabajo y descarga toda la base de datos histórica (.git) a tu computadora. Tienes una copia exacta e independiente del proyecto.

**2. Subir tu trabajo: git push**

El ciclo de trabajo local se mantiene intacto: modificas archivos -> git add (Staging) -> git commit (Repositorio Local).

Pero tus commits siguen estando solo en tu disco duro. Para que tu equipo los vea, debes "empujarlos" al servidor en internet usando git push. Esto sincroniza tu historial local con el de la nube.

**3. Actualizar tu computadora: git pull vs. (fetch + merge)**

Si tu equipo estuvo trabajando y subió código nuevo al servidor, tu computadora se quedará desactualizada. Tienes que traer esos cambios.

La forma manual:

    - git fetch: Descarga silenciosamente las actualizaciones del servidor remoto y las guarda en tu base de datos local de Git, pero no toca los archivos que tienes abiertos en tu editor.
    - git merge: Toma esa información que descargaste y ahora sí, la fusiona visiblemente con los archivos de tu carpeta de trabajo.

La forma práctica (git pull):

    Es el atajo definitivo. Hacer git pull ejecuta automáticamente un fetch y un merge al mismo tiempo. Trae las novedades de internet y actualiza tus archivos inmediatamente para que puedas seguir trabajando con la última versión disponible.

## 2. Introducción a las ramas o branches de Git

Esta lección es tu primera inmersión profunda en el trabajo con ramas (branches) en Git, un concepto vital para no romper el código principal de un proyecto.

A través de un ejemplo práctico con un blog (HTML/CSS), el video demuestra cómo la rama master actúa como tu línea de tiempo oficial. Luego, te enseña a crear una rama secundaria llamada cabecera para diseñar una nueva característica de forma aislada. Lo más impactante es ver cómo, usando el comando git checkout, puedes saltar entre la rama master y la rama cabecera, viendo cómo tus archivos en el editor de código cambian físicamente ante tus ojos, apareciendo y desapareciendo el código nuevo dependiendo de la línea de tiempo en la que estés parado.

**1. Preparando el terreno (El atajo para el Commit)**

Antes de crear ramas, la clase te enseña un truco para ahorrar tiempo.

Normalmente haces git add . y luego git commit -m "mensaje".

El atajo: Puedes usar git commit -am "mensaje". La -a le dice a Git que agregue automáticamente todos los archivos que ya estaban siendo rastreados (que ya habías agregado alguna vez en el pasado) y los guarde de una vez. (Ojo: no funciona con archivos completamente nuevos).

**2. Creando un universo paralelo (git branch)**

Para crear una nueva línea de tiempo, usas git branch [nombre_de_la_rama]. (En el video: git branch cabecera).

Al hacerlo, no pasa nada visualmente. Lo que Git hizo por debajo fue sacarle una "fotografía" exacta al código en ese preciso segundo y crear un camino alternativo.

**3. Saltando a la nueva rama (git checkout)**

    - Crear la rama no significa que ya estés dentro de ella. Tu HEAD (el puntero de Git que dice "estás trabajando aquí") sigue en master.

    - Para entrar a tu nueva rama, usas git checkout cabecera.

    - A partir de este momento, todo lo que edites, guardes o borres se quedará encerrado únicamente en la historia de la rama cabecera.

**4. La magia visual del Checkout**

El momento más importante del video ocurre cuando agregas el código div id="cabecera" estando en la rama cabecera, haces un commit, y luego ejecutas git checkout master para volver al inicio.

¡El código que acabas de escribir desaparece de tu editor!

Esto no es un error. Al volver a master, Git reescribe tus archivos para que se vean exactamente como estaban en esa línea de tiempo, protegiendo tu versión oficial de los "experimentos" que estabas haciendo en la otra rama. Si vuelves a hacer git checkout cabecera, tu código nuevo reaparecerá mágicamente.

## 3. Fusión de ramas con Git merge

El video demuestra un escenario clásico de trabajo en equipo (incluso si estás trabajando solo). En la rama cabecera, se creó el diseño visual del blog. Simultáneamente, en la rama master, se cambió el texto y la fuente del artículo. El objetivo es combinar ambos trabajos. El proceso se realiza a través de git merge, que de forma automática e inteligente analiza ambas líneas de tiempo, junta las piezas de código que no chocan entre sí y crea una nueva versión oficial que contiene el trabajo de ambas ramas.

Fusionar ramas puede dar miedo al principio, pero siguiendo estos pasos la lógica es bastante clara:

**1. La regla de oro del Merge: "Párate donde quieres recibir"**

Este es el error más común en Git. Si quieres que tu código principal (master) reciba las mejoras que hiciste en cabecera, no debes hacer el merge estando en cabecera.

Primero, debes cambiarte a la rama que va a "absorber" los cambios: git checkout master.

El comando para fusionar se lee así: "Estando en master, quiero fusionar a mí la rama X".

**2. Ejecutando la fusión (git merge)**

Una vez en master, ejecutas el comando git merge cabecera.

Git analizará la historia de master y la historia de cabecera.

Como es un nuevo punto en la historia (un nuevo commit que junta a los dos anteriores), Git abrirá automáticamente el editor Vim (que aprendiste a usar clases atrás) para que le des un mensaje a esta fusión. Puedes dejar el mensaje que Git sugiere por defecto presionando Escape y luego Shift + Z + Z para guardar y salir.

**3. ¿Qué pasa por debajo del capó? (El Automerge)**

    - Git es increíblemente inteligente. Si en master editaste la línea 15 y en cabecera editaste la línea 2, Git dice: "No hay problema, nadie se pisó los talones". Mezclará ambos archivos automáticamente.
    - Verás un mensaje en la consola que dice Auto-merging...
    - El resultado final en tu código será un archivo que tiene tanto tu nueva cabecera azul como tus nuevos párrafos.

**4. El nuevo ciclo**

Una vez hecho el merge, la rama cabecera sigue existiendo en el pasado de tu proyecto, pero ya no la necesitas para esto; todo su valor ya fue inyectado a master.

A partir de aquí, tu master es la versión más moderna y completa. Puedes seguir trabajando y haciendo commits normales en master (como el ejemplo donde se volvió a cambiar la fuente a Arial).

## 4. Solución de conflictos al hacer un merge

Esta lección desmitifica el mayor miedo de los principiantes en Git: los conflictos. Un conflicto ocurre cuando Git intenta fusionar dos ramas, pero se da cuenta de que ambas modificaron exactamente la misma línea de código de formas distintas.

El video simula esta situación: en la rama master se cambió el color a azul, y en la rama cabecera se cambió a rojo. Al intentar hacer merge, Git se rinde y pide ayuda humana. La clase muestra cómo leer los marcadores de conflicto que Git inyecta en tu código (<<<<<<< HEAD y >>>>>>> cabecera) y cómo herramientas modernas como Visual Studio Code te facilitan la vida al permitirte elegir qué versión conservar con un solo clic.

**Análisis y Explicación a Detalle: Resolviendo Conflictos**

Enfrentarse a un conflicto es un proceso rutinario. Aquí tienes el paso a paso de lo que ocurre y cómo actuar:

**1. La aparición del conflicto**

Haces el comando de fusión (git merge cabecera estando en master). Si ambas ramas modificaron la misma línea, Git aborta la operación.

    - Tu terminal te avisará claramente: CONFLICT (content): Merge conflict in estilos.css.

    - Si escribes git status, verás que el estado de tu proyecto es MERGING. Estás atascado en el limbo hasta que resuelvas el problema.

**2. Entendiendo los marcadores de conflicto**

Si abres el archivo en un editor básico, verás que Git ensució tu código con marcadores visuales para mostrarte dónde está el problema:

```css
<<<<<<< HEAD
color: blue;
=======
color: red;
>>>>>>> cabecera
```

    - <<<<<<< HEAD: Indica el inicio del bloque de código. Te muestra qué hay en la rama actual donde estás parado (master).

    - =======: Es la línea divisoria.

    - >>>>>>> cabecera: Indica el fin del bloque y te muestra qué intentaba inyectar la rama que estás trayendo.

**3. La resolución manual**

Resolver un conflicto simplemente significa borrar las líneas que no quieres y borrar los marcadores de Git.

Borras <<<<<<< HEAD, ======= y >>>>>>> cabecera.

Decides si te quedas con color: blue;, con color: red;, o si escribes algo completamente nuevo (como color: #333;).

¡Guarda el archivo! (Como se vio en el video, olvidar guardar es un error común).

El Superpoder de Visual Studio Code:

Si usas un editor de código moderno como VS Code, no tienes que borrar nada a mano. El editor detecta los marcadores de Git y te muestra botones flotantes encima del código conflictivo:

    - Accept Current Change: Deja el código como estaba en master.
    - Accept Incoming Change: Acepta el código nuevo de la rama cabecera.
    - Accept Both Changes: Mantiene ambos códigos uno debajo del otro (útil si quieres combinarlos manualmente).

**4. Finalizando el Merge**

Una vez que hayas limpiado el código y guardado el archivo, le dices a Git que el problema está solucionado haciendo un nuevo guardado oficial:

    - Ejecutas git add . (para confirmar que el archivo conflictivo ya está arreglado).
    - Ejecutas git commit -m "Conflicto solucionado".
    - ¡Listo! El estado de MERGING desaparece y la fusión se completa con éxito.
