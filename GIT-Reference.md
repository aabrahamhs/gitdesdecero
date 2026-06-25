# GIT: Guia de reference

A continuación, se detalla la guía completa de comandos. Todos los ejemplos asumen que estás ejecutando los comandos en la terminal.

---

## 1. git init

**Explicación general:** Crea un nuevo repositorio Git vacío o reinicializa uno existente. Es el primer comando que ejecutas para empezar a rastrear un proyecto.

* **Ejemplos básicos:**

  ```bash
  # Convierte el directorio actual en un repositorio Git (crea la carpeta oculta .git)
  git init
  ```

  ```bash
  # Crea una nueva carpeta llamada "mi-proyecto" y la inicializa como repositorio Git
  git init mi-proyecto
  ```

  ```bash
  # Inicializa el repositorio especificando que la rama principal se llame "main"
  git init -b main
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Crea un repositorio "desnudo" sin directorio de trabajo (para servidores centrales)
  git init --bare
  ```

  ```bash
  # Inicializa usando un directorio de plantillas con configuraciones predeterminadas
  git init --template=/ruta/a/plantilla
  ```

  ```bash
  # Inicializa el repositorio pero guarda la carpeta .git en una ubicación diferente
  git init --separate-git-dir=/ruta/externa/.git
  ```

  ```bash
  # Inicializa el repositorio de forma silenciosa sin mensajes en consola
  git init --quiet
  ```

  ```bash
  # Inicializa permitiendo que múltiples usuarios del mismo grupo tengan permisos de escritura
  git init --shared=group
  ```

---

## 2. git config

**Explicación general:** Permite obtener y establecer opciones de configuración de repositorio o globales que determinan el comportamiento y aspecto de Git.

* **Ejemplos básicos:**

  ```bash
  # Configura tu nombre de autor para todos los commits en el sistema
  git config --global user.name "Tu Nombre"
  ```

  ```bash
  # Configura tu correo electrónico asociado a los commits globales
  git config --global user.email "tu@email.com"
  ```

  ```bash
  # Muestra todas las configuraciones actuales que aplican a este repositorio
  git config --list
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Configura vim como el editor predeterminado global de git
  git config --global merge.tool vimdiff
  ```
  
  ```bash
  # Configura VS Code como el editor predeterminado para este repositorio específico
  git config --local core.editor "code --wait"
  ```

  ```bash
  # Crea un atajo personalizado para usar "git st" en lugar de "git status"
  git config --global alias.st status
  ```

  ```bash
  # Convierte automáticamente los saltos de línea al formato de Windows al descargar
  git config --global core.autocrlf true
  ```

  ```bash
  # Muestra el valor de una configuración y de qué archivo exacto proviene
  git config --show-origin user.name
  ```

  ```bash
  # Elimina la configuración del nombre de usuario del archivo local/global actual
  git config --unset user.name
  ```

---

## 3. git branch

**Explicación general:** Permite listar, crear o eliminar ramas (branches). Una rama representa una línea independiente de desarrollo.

```text
Antes de crear la rama:
A --- B --- C (HEAD -> main)

Después de `git branch feature`:
A --- B --- C (HEAD -> main, feature)
            ^
       Ambas ramas apuntan al mismo commit.
```

* **Ejemplos básicos:**

  ```bash
  # Lista todas las ramas locales; la actual se marca con un asterisco (*)
  git branch
  ```

  ```bash
  # Crea una nueva rama apuntando al commit actual sin cambiar de rama
  git branch nueva-funcionalidad
  ```

  ```bash
  # Lista todas las ramas locales y también las remotas registradas
  git branch -a
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Elimina una rama de forma segura (Git avisará si tiene cambios sin fusionar)
  git branch -d nombre-rama
  ```

  ```bash
  # Fuerza la eliminación de una rama perdiendo los cambios no fusionados
  git branch -D nombre-rama
  ```

  ```bash
  # Renombra una rama local existente
  git branch -m rama-vieja rama-nueva
  ```

  ```bash
  # Lista única y exclusivamente las ramas del servidor remoto
  git branch -r
  ```

  ```bash
  # Muestra qué ramas ya han sido completamente integradas en la rama actual
  git branch --merged
  ```

---

## 4. git switch

**Explicación general:** Cambia entre ramas. Fue introducido para separar la funcionalidad de cambiar de rama de la de restaurar archivos (que antes hacían un `checkout` confuso).

* **Ejemplos básicos:**

  ```bash
  # Cambia tu espacio de trabajo a la rama "main"
  git switch main
  ```

  ```bash
  # Crea una nueva rama y cambia a ella en un solo paso
  git switch -c nueva-rama
  ```

  ```bash
  # Vuelve a la rama en la que estabas posicionado justo antes
  git switch -
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Crea una rama, pero si ya existe, la resetea forzosamente al commit actual
  git switch -C nombre-rama
  ```

  ```bash
  # Cambia a un commit específico sin acoplarse a ninguna rama (detached HEAD)
  git switch --detach <hash-commit>
  ```

  ```bash
  # Crea una rama completamente huérfana, vacía y sin historial previo
  git switch --orphan nueva-rama-vacia
  ```

  ```bash
  # Crea una rama local configurada para rastrear una rama remota específica
  git switch -c mi-rama origin/mi-rama
  ```

  ```bash
  # Cambia de rama e intenta fusionar los cambios locales no confirmados en ella
  git switch --merge otra-rama
  ```

---

## 5. git checkout

**Explicación general:** Comando clásico para cambiar de ramas, revisar commits antiguos y restaurar archivos modificados.

* **Ejemplos básicos:**

  ```bash
  # Cambia el espacio de trabajo hacia la rama "dev"
  git checkout dev
  ```

  ```bash
  # Crea la rama "feature-1" y salta a ella de inmediato
  git checkout -b feature-1
  ```

  ```bash
  # Descarta todos los cambios locales no guardados en un archivo específico
  git checkout -- archivo.txt
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Coloca el repositorio en un estado temporal para examinar un commit del pasado
  git checkout <hash-commit>
  ```

  ```bash
  # Extrae un archivo tal como estaba en un commit antiguo y lo pone en el disco actual
  git checkout <hash-commit> -- archivo.txt
  ```

  ```bash
  # Permite seleccionar interactivamente qué fragmentos de un archivo restaurar
  git checkout -p <hash-commit> -- archivo.txt
  ```

  ```bash
  # Fuerza la creación de la rama sobrescribiendo su historial si ya existía
  git checkout -B mi-rama
  ```

  ```bash
  # Cambia de rama intentando un merge de tres vías con tus modificaciones locales
  git checkout -m <rama-destino>
  ```

---

## 6. git restore

**Explicación general:** Restaura archivos del directorio de trabajo o del "staging area" (índice). Es la alternativa moderna y recomendada a usar `checkout` para archivos.

* **Ejemplos básicos:**

  ```bash
  # Descarta los cambios locales en el archivo que aún no han pasado por "git add"
  git restore index.html
  ```

  ```bash
  # Descarta las modificaciones locales de todos los archivos del directorio actual
  git restore .
  ```

  ```bash
  # Descarta los cambios locales de todos los archivos con extensión .txt
  git restore *.txt
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Saca un archivo del staging area (deshace el "git add") manteniendo el código intacto
  git restore --staged index.html
  ```

  ```bash
  # Restaura un archivo al estado exacto que tenía hace dos commits atrás
  git restore --source HEAD~2 archivo.js
  ```

  ```bash
  # Permite elegir interactivamente qué líneas descartar o mantener de un archivo
  git restore -p archivo.css
  ```

  ```bash
  # Saca el archivo del staging area y además destruye sus modificaciones del disco duro
  git restore --staged --worktree archivo.txt
  ```

  ```bash
  # Sincroniza un archivo para que quede idéntico al último commit en disco y staging
  git restore --source=HEAD --staged --worktree archivo.txt
  ```

---

## 7. git rm

**Explicación general:** Elimina archivos del directorio de trabajo y del "staging area" al mismo tiempo, preparando la eliminación para el próximo commit.

* **Ejemplos básicos:**

  ```bash
  # Borra el archivo físicamente y lo prepara para el commit de eliminación
  git rm viejo_archivo.txt
  ```

  ```bash
  # Elimina un directorio completo junto con todo su contenido de forma recursiva
  git rm -r carpeta/
  ```

  ```bash
  # Busca y elimina todos los archivos del directorio con extensión .log
  git rm *.log
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Deja de rastrear el archivo en Git pero lo mantiene intacto en tu disco duro
  git rm --cached archivo.txt
  ```

  ```bash
  # Fuerza el borrado de un archivo modificado localmente que no se ha comiteado
  git rm -f archivo.txt
  ```

  ```bash
  # Intenta borrar el archivo pero ignora el error si este no existía en el proyecto
  git rm --ignore-unmatch archivo_dudoso.txt
  ```

  ```bash
  # Simula la eliminación para mostrarte qué archivos se borrarían sin borrarlos realmente
  git rm -n archivo.txt
  ```

  ```bash
  # Desvincula absolutamente todos los archivos locales del índice de Git sin borrarlos del disco
  git rm -r --cached .
  ```

---

## 8. git merge

**Explicación general:** Une dos o más historiales de desarrollo de diferentes ramas en una sola.

**1. Fast-Forward (Avance Rápido):**
```text
Antes:
A --- B (main) --- C --- D (feature)

Comando (en main): git merge feature
Después (Fast-forward):
A --- B --- C --- D (main, feature)
```

**2. Merge Commit (Commit de Fusión):**
```text
Antes:
A --- B --- E --- F (main)
       \
        C --- D (feature)

Comando (en main): git merge feature
Después:
A --- B --- E --- F --- M (main)
       \               /
        C ----- D ----/ (feature)
```

* **Ejemplos básicos:**

  ```bash
  # Une los commits de "feature-branch" dentro de la rama donde estás parado
  git merge feature-branch
  ```

  ```bash
  # Cancela el proceso de fusión actual si surgen conflictos y restaura todo
  git merge --abort
  ```

  ```bash
  # Continúa con el proceso de fusión tras resolver los conflictos y hacer "git add"
  git merge --continue
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Desactiva el fast-forward y obliga a Git a crear un commit de fusión explícito
  git merge --no-ff feature-branch
  ```

  ```bash
  # Agrupa todos los commits de la rama en un solo bloque de cambios listo para comitear
  git merge --squash feature-branch
  ```

  ```bash
  # Resuelve conflictos automáticamente eligiendo siempre la versión de la rama entrante
  git merge -X theirs feature-branch
  ```

  ```bash
  # Cancela la operación si la fusión no se puede completar como un Fast-forward limpio
  git merge --ff-only feature-branch
  ```

  ```bash
  # Ejecuta la fusión definiendo el mensaje del commit directamente desde la consola
  git merge -m "Integra la nueva UI del login" feature-branch
  ```

---

## 9. git reset

**Explicación general:** Mueve la punta de la rama actual (HEAD) a un commit específico del pasado, alterando el historial y opcionalmente el índice y el disco duro.

```text
Commit A (pasado) <-- Commit B (actual)

1. git reset --soft A
Historial: Vuelve al A.
Staging: Mantiene los cambios del B listos para comitear.
Directorio de Trabajo: Intacto.

2. git reset A  (o --mixed, por defecto)
Historial: Vuelve al A.
Staging: Se vacía.
Directorio de Trabajo: Intacto (los archivos conservan las modificaciones).

3. git reset --hard A
Historial: Vuelve al A.
Staging: Se vacía.
Directorio de Trabajo: DESTRUYE los cambios, vuelve al estado exacto de A.
```

* **Ejemplos básicos:**

  ```bash
  # Deshace el último commit devolviendo los cambios al estado modificado en disco
  git reset HEAD~1
  ```

  ```bash
  # Remueve un archivo específico del staging area sin modificar su contenido
  git reset archivo.txt
  ```

  ```bash
  # Vacía por completo el staging area de todos los archivos preparados
  git reset
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # ¡Peligroso! Deshace el último commit borrando permanentemente el código del disco
  git reset --hard HEAD~1
  ```

  ```bash
  # Deshace el commit pero deja el código modificado listo en el staging area
  git reset --soft HEAD~1
  ```

  ```bash
  # Deshace el último commit pero conserva de forma segura tus archivos sin confirmar
  git reset --keep HEAD~1
  ```

  ```bash
  # Sincroniza forzosamente tu rama local para que sea idéntica al main del servidor
  git reset --hard origin/main
  ```

  ```bash
  # Saca bloques específicos de código de un archivo del staging area interactivamente
  git reset -p
  ```

---

## 10. git log

**Explicación general:** Muestra el registro del historial de commits cronológicamente hacia atrás.

* **Ejemplos básicos:**

  ```bash
  # Muestra el historial completo de commits con detalles de autor, fecha y mensaje
  git log
  ```

  ```bash
  # Limita la salida del historial únicamente a los últimos 5 commits
  git log -n 5
  ```

  ```bash
  # Muestra una versión ultracompacta del historial con un commit por línea
  git log --oneline
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Dibuja un gráfico en texto con el árbol de ramificaciones de todo el repositorio
  git log --graph --all --oneline --decorate
  ```

  ```bash
  # Filtra el historial mostrando únicamente los commits de un desarrollador específico
  git log --author="Juan Perez"
  ```

  ```bash
  # Muestra los commits realizados exclusivamente en las últimas dos semanas
  git log --since="2 weeks ago"
  ```

  ```bash
  # Muestra el historial de un archivo indicando los cambios de código exactos (diff)
  git log -p archivo.js
  ```

  ```bash
  # Localiza commits que añadieron o borraron una palabra clave exacta dentro del código
  git log -S "palabra_clave"
  ```

---

## 11. git diff

**Explicación general:** Compara diferencias entre los archivos del directorio de trabajo, el índice y los commits.

* **Ejemplos básicos:**

  ```bash
  # Muestra los cambios locales hechos que aún no se han preparado con "git add"
  git diff
  ```

  ```bash
  # Muestra las diferencias de los archivos que ya están listos en el staging area
  git diff --staged
  ```

  ```bash
  # Muestra las modificaciones actuales hechas exclusivamente en un archivo
  git diff archivo.css
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Compara todas las modificaciones de tu espacio de trabajo contra el último commit
  git diff HEAD
  ```

  ```bash
  # Compara todo el árbol de código entre la punta de la rama A y la rama B
  git diff rama-A..rama-B
  ```

  ```bash
  # Muestra los cambios de código introducidos entre dos puntos exactos del pasado
  git diff <hash-commit-1> <hash-commit-2>
  ```

  ```bash
  # Muestra un resumen estadístico de archivos cambiados e inserciones/borrados
  git diff --stat
  ```

  ```bash
  # Compara los archivos ignorando los cambios de espacios en blanco e indentación
  git diff -w
  ```

---

## 12. git stash

**Explicación general:** Almacena temporalmente los cambios en una pila oculta y limpia el espacio de trabajo, permitiendo recuperarlos más tarde.

```text
(Cambios actuales guardados) -> stash@{0}: WIP on main...
(Cambios guardados antes)    -> stash@{1}: WIP on feature...

Al ejecutar `git stash pop`, se extrae y aplica el `stash@{0}` de la punta.
```

* **Ejemplos básicos:**

  ```bash
  # Guarda tus modificaciones actuales rastreadas en la pila temporal
  git stash
  ```

  ```bash
  # Restaura el último estado guardado en la pila y lo elimina de ella
  git stash pop
  ```

  ```bash
  # Lista todas las capturas temporales de código almacenadas en la pila
  git stash list
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Guarda cambios dándoles un nombre descriptivo e incluyendo archivos nuevos
  git stash push -m "trabajo-incompleto-header" -u
  ```

  ```bash
  # Aplica los cambios guardados en la posición 2 de la pila sin eliminarlos de ella
  git stash apply stash@{2}
  ```

  ```bash
  # Borra permanentemente de la pila el registro en la posición 1
  git stash drop stash@{1}
  ```

  ```bash
  # Elimina por completo todos los stashes de la pila sin excepción
  git stash clear
  ```

  ```bash
  # Crea una nueva rama a partir del stash indicado, aplica el código y lo borra
  git stash branch nueva-rama stash@{0}
  ```

---

## 13. git clean

**Explicación general:** Elimina del directorio de trabajo los archivos que no están bajo el control de versiones (untracked).

* **Ejemplos básicos:**

  ```bash
  # Simulación de limpieza: muestra qué archivos no rastreados se borrarían
  git clean -n
  ```

  ```bash
  # Fuerza el borrado definitivo de los archivos no rastreados del disco
  git clean -f
  ```

  ```bash
  # Borra los archivos no rastreados únicamente dentro de la ruta indicada
  git clean -f <ruta/al/directorio>
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Elimina del directorio los archivos no rastreados junto con las carpetas vacías
  git clean -fd
  ```

  ```bash
  # Elimina única y exclusivamente los archivos listados en el .gitignore
  git clean -fX
  ```

  ```bash
  # Realiza una limpieza masiva eliminando archivos no rastreados e ignorados
  git clean -fx
  ```

  ```bash
  # Inicia un menú interactivo en consola para decidir el destino de cada archivo
  git clean -i
  ```

  ```bash
  # Simula la eliminación combinada de archivos y carpetas no rastreadas
  git clean -n -d
  ```

---

## 14. git tag

**Explicación general:** Crea marcas o puntos de control inmutables en el historial de commits, comúnmente utilizados para identificar versiones de lanzamiento (Releases).

* **Ejemplos básicos:**

  ```bash
  # Despliega en orden alfabético todas las etiquetas existentes en el proyecto
  git tag
  ```

  ```bash
  # Crea una etiqueta ligera que actúa como un puntero directo al commit actual
  git tag v1.0.0
  ```

  ```bash
  # Crea una etiqueta anotada firmada con autor, fecha y mensaje descriptivo
  git tag -a v1.0.0 -m "Versión de lanzamiento inicial"
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Elimina una etiqueta específica del repositorio local
  git tag -d v1.0.0
  ```

  ```bash
  # Envía una etiqueta específica al servidor remoto configurado
  git push origin v1.0.0
  ```

  ```bash
  # Sube de manera masiva todas las etiquetas locales creadas hacia el remoto
  git push origin --tags
  ```

  ```bash
  # Filtra y muestra únicamente los tags que coincidan con el patrón indicado
  git tag -l "v1.8*"
  ```

  ```bash
  # Cambia el entorno al commit exacto marcado por la etiqueta (en modo detached HEAD)
  git checkout v1.0.0
  ```

---

## 15. git show

**Explicación general:** Muestra información detallada y los cambios de código estructurados de cualquier objeto de Git (commits, tags, árboles).

* **Ejemplos básicos:**

  ```bash
  # Muestra los metadatos y las líneas modificadas en el último commit (HEAD)
  git show
  ```

  ```bash
  # Muestra el autor, fecha, mensaje y líneas de código alteradas en ese commit
  git show <hash-commit>
  ```

  ```bash
  # Despliega la información detallada correspondiente al penúltimo commit
  git show HEAD~1
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Visualiza el código completo de un archivo tal y como existía en ese commit
  git show <hash-commit>:src/main.py
  ```

  ```bash
  # Muestra el resumen estadístico del commit sin detallar las líneas de código
  git show --stat <hash-commit>
  ```

  ```bash
  # Lista únicamente los nombres de los archivos afectados por el commit indicado
  git show --name-only <hash-commit>
  ```

  ```bash
  # Muestra los datos de creación del tag junto con el commit al que apunta
  git show v2.0.0
  ```

  ```bash
  # Suprime los diffs de código y muestra exclusivamente el mensaje original del commit
  git show -s --format=%B <hash-commit>
  ```

---

## 16. git reflog

**Explicación general:** Registra localmente el historial completo de los movimientos del puntero HEAD, permitiendo recuperar commits eliminados o deshacer resets destructivos.

* **Ejemplos básicos:**

  ```bash
  # Muestra la lista cronológica de acciones locales (cambios de rama, commits, resets)
  git reflog
  ```

  ```bash
  # Despliega el registro histórico de cambios enfocados solo en la rama "main"
  git reflog show main
  ```

  ```bash
  # Despliega el registro completo de la punta del entorno global actual
  git reflog show HEAD
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Restaura la rama actual exactamente al estado en que estaba hace 4 movimientos
  git reset --hard HEAD@{4}
  ```

  ```bash
  # Elimina una entrada específica del registro del reflog local
  git reflog delete HEAD@{0}
  ```

  ```bash
  # Purga y elimina inmediatamente todo el historial acumulado en el reflog
  git reflog expire --expire=now --all
  ```

  ```bash
  # Muestra los movimientos del reflog formateados visualmente con la estética de git log
  git log -g
  ```

  ```bash
  # Cambia temporalmente la vista al estado del repositorio correspondiente a ese movimiento
  git checkout HEAD@{2}
  ```

---

## 17. git cherry-pick

**Explicación general:** Copia un commit existente de cualquier rama y lo aplica como un nuevo commit en la rama actual.

```text
Antes (estando en main):
A --- B --- C (main)
       \
        D --- E --- F --- G (feature)

Comando: git cherry-pick F

Después:
A --- B --- C --- F' (main)
       \
        D --- E --- F --- G (feature)
```

* **Ejemplos básicos:**

  ```bash
  # Duplica los cambios del commit indicado en la punta de la rama actual
  git cherry-pick <hash-commit>
  ```

  ```bash
  # Aborta la operación si se presentan conflictos complejos y restaura la rama
  git cherry-pick --abort
  ```

  ```bash
  # Concluye el cherry-pick tras solventar conflictos manualmente y agregarlos
  git cherry-pick --continue
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Aplica secuencialmente un rango de commits desde el commit A hasta el B
  git cherry-pick A..B
  ```

  ```bash
  # Aplica secuencialmente un rango de commits incluyendo de forma explícita al commit A
  git cherry-pick A^..B
  ```

  ```bash
  # Vuelca los cambios del commit al área de trabajo sin generar el commit automático
  git cherry-pick -n <hash-commit>
  ```

  ```bash
  # Aplica el cambio y lanza el editor para personalizar el mensaje original del commit
  git cherry-pick -e <hash-commit>
  ```

  ```bash
  # Permite hacer cherry-pick de un commit de merge indicando el número de padre base
  git cherry-pick -m 1 <hash-merge-commit>
  ```

---

## 18. git rebase

**Explicación general:** Vuelve a aplicar los commits de la rama actual sobre la punta de otra rama base, reescribiendo la historia linealmente.

```text
Antes (en feature):
A --- B --- C (main)
       \
        D --- E (feature)

Comando: git rebase main

Después:
A --- B --- C (main)
             \
              D' --- E' (feature)
```

* **Ejemplos básicos:**

  ```bash
  # Desplaza la base de tu rama actual colocándola justo encima del final de main
  git rebase main
  ```

  ```bash
  # Cancela el proceso de rebase por completo devolviendo la rama a su estado previo
  git rebase --abort
  ```

  ```bash
  # Reanuda la aplicación de commits tras resolver un conflicto intermedio
  git rebase --continue
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Abre un menú interactivo para editar, fusionar (squash) o eliminar los últimos 3 commits
  git rebase -i HEAD~3
  ```

  ```bash
  # Mueve commits de una rama secundaria a una nueva base externa
  git rebase --onto nueva-base rama-vieja mi-rama
  ```

  ```bash
  # Permite ejecutar un rebase interactivo completo remontándose al commit inicial del repo
  git rebase -i --root
  ```

  ```bash
  # Omite el commit conflictivo actual si sus cambios ya se encuentran integrados en la base
  git rebase --skip
  ```

  ```bash
  # Ejecuta una validación automatizada (como pruebas) tras aplicar cada commit del rebase
  git rebase -x "npm test" HEAD~3
  ```

---

## 19. git remote

**Explicación general:** Administra el conjunto de repositorios remotos ("enlaces") cuyas ramas realizas el seguimiento.

* **Ejemplos básicos:**

  ```bash
  # Lista los alias remotos configurados junto con sus direcciones de entrada y salida
  git remote -v
  ```

  ```bash
  # Registra un nuevo enlace a un repositorio remoto vinculándolo al alias "origin"
  git remote add origin https://github.com/usuario/repo.git
  ```

  ```bash
  # Muestra un reporte completo de las ramas y estado de sincronización de un remoto
  git remote show origin
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Modifica el alias de identificación de una conexión remota
  git remote rename origin destino
  ```

  ```bash
  # Elimina el vínculo de conexión con el repositorio remoto indicado
  git remote remove origin
  ```

  ```bash
  # Actualiza la dirección URL vinculada a un alias remoto existente
  git remote set-url origin https://nueva-url.com/repo.git
  ```

  ```bash
  # Elimina de forma local las ramas de seguimiento cuyos remotos reales ya no existen
  git remote prune origin
  ```

  ```bash
  # Descarga y actualiza de manera simultánea la información de todos los remotos guardados
  git remote update
  ```

---

## 20. git fetch

**Explicación general:** Descarga los objetos, commits y referencias de un repositorio remoto, pero sin fusionar ni alterar tus ramas locales de trabajo.

* **Ejemplos básicos:**

  ```bash
  # Descarga todas las novedades del repositorio remoto configurado por defecto
  git fetch
  ```

  ```bash
  # Descarga las actualizaciones de ramas del repositorio identificado como "origin"
  git fetch origin
  ```

  ```bash
  # Trae los cambios de todas las fuentes remotas registradas en el proyecto
  git fetch --all
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Descarga de manera exclusiva las novedades pertenecientes a la rama main remota
  git fetch origin main
  ```

  ```bash
  # Descarga las novedades y limpia las referencias a ramas borradas en el servidor
  git fetch -p
  ```

  ```bash
  # Fuerza la descarga e importación de todas las etiquetas del repositorio remoto
  git fetch --tags
  ```

  ```bash
  # Trae una rama remota y la guarda localmente en una rama nueva sin cambiar de entorno
  git fetch origin <rama-remota>:<rama-local>
  ```

  ```bash
  # Muestra un reporte en pantalla de qué se descargaría del servidor sin realizar cambios
  git fetch --dry-run
  ```

---

## 21. git pull

**Explicación general:** Descarga el historial del servidor remoto (`fetch`) e integra inmediatamente esos cambios en la rama local actual (`merge` o `rebase`).

```text
Comando: git pull origin main

1. Ejecuta "fetch":
   (Local)    A --- B
   (Remoto)   A --- B --- C --- D (descargado a 'origin/main')

2. Ejecuta "merge" (Fast-forward):
   (Local)    A --- B --- C --- D (HEAD -> main)
```

* **Ejemplos básicos:**

  ```bash
  # Sincroniza la rama actual descargando e integrando los cambios de su par remota
  git pull
  ```

  ```bash
  # Descarga e integra la rama main del remoto "origin" en tu rama posicionada
  git pull origin main
  ```

  ```bash
  # Descarga las novedades y en lugar de hacer merge, aplica rebase de tus commits locales
  git pull --rebase
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Actualiza la rama local únicamente si se puede realizar por Fast-forward limpio
  git pull --ff-only
  ```

  ```bash
  # Descarga y une cambios deteniendo el flujo antes de generar el commit final de fusión
  git pull --no-commit
  ```

  ```bash
  # Guarda en stash tus cambios locales automáticamente antes del pull y los restaura después
  git pull --autostash
  ```

  ```bash
  # Concentra cronológicamente todos los commits remotos nuevos en un único cambio local
  git pull --squash origin main
  ```

  ```bash
  # Trae y busca actualizar la rama actual recolectando información de todos los remotos
  git pull --all
  ```

---

## 22. git push

**Explicación general:** Envía tus commits locales y objetos asociados a un repositorio remoto para actualizar las ramas del servidor.

* **Ejemplos básicos:**

  ```bash
  # Publica los nuevos commits de tu rama actual a su correspondiente contraparte remota
  git push
  ```

  ```bash
  # Envía los cambios de la rama main local al servidor remoto origin
  git push origin main
  ```

  ```bash
  # Sube la rama y establece un enlace persistente de rastreo para futuros comandos push
  git push -u origin feature
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # ¡Peligroso! Reescribe forzosamente la historia del servidor remoto con la tuya
  git push --force
  ```

  ```bash
  # Sobrescribe de forma segura: solo fuerza si nadie más ha subido cambios remotos
  git push --force-with-lease
  ```

  ```bash
  # Comando para eliminar una rama de forma definitiva en el servidor remoto
  git push origin --delete rama-obsoleta
  ```

  ```bash
  # Envía todas las etiquetas de control locales creadas directamente al servidor
  git push --tags
  ```

  ```bash
  # Envía la rama actual de trabajo al remoto bajo su mismo nombre de origen
  git push origin HEAD
  ```

---

## 23. git blame

**Explicación general:** Muestra el contenido de un archivo anotando cada línea con el hash del commit, autor y fecha del último cambio aplicado.

* **Ejemplos básicos:**

  ```bash
  # Inspecciona el archivo completo para determinar la autoría de cada línea de código
  git blame archivo.js
  ```

  ```bash
  # Limita la inspección de autorías únicamente de la línea 10 a la 20
  git blame -L 10,20 archivo.js
  ```

  ```bash
  # Muestra el correo electrónico de los autores en lugar de sus nombres descriptivos
  git blame -e archivo.js
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Analiza la autoría ignorando commits que solo alteraron espacios o identación
  git blame -w archivo.js
  ```

  ```bash
  # Detecta y asocia el autor original si las líneas de código se movieron de lugar
  git blame -M archivo.js
  ```

  ```bash
  # Detecta líneas de código que fueron copiadas desde otros archivos del proyecto
  git blame -C archivo.js
  ```

  ```bash
  # Evalúa la autoría de las líneas del archivo en un punto exacto del pasado
  git blame <hash-commit> -- archivo.js
  ```

  ```bash
  # Analiza cronológicamente en sentido inverso para rastrear quién borró o editó líneas antiguas
  git blame --reverse <hash-inicio>..HEAD archivo.js
  ```

---

## 24. git bisect

**Explicación general:** Utiliza el algoritmo de búsqueda binaria en el historial de commits para aislar de forma exacta el commit que introdujo un error.

```text
[1]--[2]--[3]--[4]--[5]--[6]--[7]--[8]--[9]--[10]
 ^ (Funcionaba)                               ^ (Falla hoy)

Git salta al commit intermedio (5) para que evalúes si el código falla allí.
Si indicas "good", el error está entre 6 y 10. Se repite hasta hallar al culpable.
```

* **Ejemplos básicos:**

  ```bash
  # Inicia formalmente la sesión de búsqueda binaria para rastrear un bug
  git bisect start
  ```

  ```bash
  # Notifica a Git que el estado del commit actual está roto o presenta el fallo
  git bisect bad
  ```

  ```bash
  # Indica un commit anterior del historial en el que la aplicación funcionaba bien
  git bisect good <hash-commit-antiguo>
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Automatiza la búsqueda ejecutando un script de pruebas tras cada salto de commit
  git bisect run npm test
  ```

  ```bash
  # Finaliza la sesión de depuración y regresa a la rama y commit originales
  git bisect reset
  ```

  ```bash
  # Omite el commit sugerido si este no compila o no se puede testear el bug
  git bisect skip
  ```

  ```bash
  # Despliega el reporte de auditoría con las marcas asignadas en la sesión actual
  git bisect log
  ```

  ```bash
  # Carga y reproduce los pasos de una sesión guardada a partir de un log externo
  git bisect replay logfile.txt
  ```

---

## 25. git grep

**Explicación general:** Realiza búsquedas de expresiones o patrones de texto en los archivos del repositorio de forma ultrarrápida.

* **Ejemplos básicos:**

  ```bash
  # Localiza la coincidencia exacta de la cadena de texto en los archivos del proyecto
  git grep "const apiKey"
  ```

  ```bash
  # Busca el término ignorando la distinción entre mayúsculas y minúsculas
  git grep -i "TODO"
  ```

  ```bash
  # Muestra los resultados de búsqueda indicando el número exacto de línea en el archivo
  git grep -n "function init"
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Devuelve un conteo numérico de cuántas coincidencias existen por cada archivo
  git grep -c "class"
  ```

  ```bash
  # Muestra la línea encontrada incluyendo el nombre de la función contenedora
  git grep -p "console.log"
  ```

  ```bash
  # Formatea la salida agrupando visualmente las líneas debajo del nombre del archivo
  git grep --break --heading "API_KEY"
  ```

  ```bash
  # Rastrea la presencia del texto en los archivos históricos de un commit antiguo
  git grep "texto" <hash-commit-antiguo>
  ```

  ```bash
  # Búsqueda condicional: filtra líneas que incluyan simultáneamente ambos términos
  git grep -e "error" --and -e "database"
  ```

---

## 26. git worktree

**Explicación general:** Permite gestionar múltiples directorios de trabajo vinculados al mismo repositorio local, posibilitando trabajar en ramas distintas de forma paralela en carpetas separadas.

* **Ejemplos básicos:**

  ```bash
  # Lista todas las carpetas de trabajo vinculadas activamente a este repositorio
  git worktree list
  ```

  ```bash
  # Extrae la rama indicada en una nueva carpeta independiente fuera del directorio actual
  git worktree add ../nueva-carpeta mi-rama
  ```

  ```bash
  # Desvincula y elimina de forma segura una carpeta de trabajo complementaria
  git worktree remove ../nueva-carpeta
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Crea una rama nueva y la extrae directamente en un árbol de trabajo asignado
  git worktree add -b <nueva-rama> ../carpeta-test
  ```

  ```bash
  # Limpia los registros de directorios de trabajo que fueron borrados manualmente
  git worktree prune
  ```

  ```bash
  # Bloquea un worktree para evitar que sus registros internos sean eliminados
  git worktree lock ../mi-carpeta
  ```

  ```bash
  # Quita el bloqueo de seguridad previamente asignado a un directorio de trabajo
  git worktree unlock ../mi-carpeta
  ```

  ```bash
  # Traslada un directorio de trabajo a una nueva ubicación actualizando los enlaces
  git worktree move ../carpeta-actual ../carpeta-nueva
  ```

---

## 27. git submodule

**Explicación general:** Permite alojar y gestionar un repositorio de Git externo de manera integrada dentro de una subcarpeta de tu propio proyecto.

* **Ejemplos básicos:**

  ```bash
  # Incorpora un repositorio externo dentro de la ruta local especificada
  git submodule add https://github.com/usuario/libreria.git vendor/libreria
  ```

  ```bash
  # Registra las configuraciones de los submódulos declarados en el .gitmodules
  git submodule init
  ```

  ```bash
  # Sincroniza el contenido del submódulo para que coincida con el commit guardado
  git submodule update
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Inicializa, clona y descarga recursivamente submódulos tras clonar un proyecto nuevo
  git submodule update --init --recursive
  ```

  ```bash
  # Ejecuta de forma secuencial un comando de Git en cada uno de los submódulos activos
  git submodule foreach 'git pull origin main'
  ```

  ```bash
  # Remueve la configuración local de un submódulo limpiando su espacio de trabajo
  git submodule deinit ruta/al/submodulo
  ```

  ```bash
  # Muestra el estado actual y el commit en el que se encuentra cada submódulo
  git submodule status
  ```

  ```bash
  # Actualiza la configuración de las direcciones de los submódulos si cambiaron de URL
  git submodule sync
  ```

---

## 28. git archive

**Explicación general:** Empaqueta y comprime los archivos de un commit o rama omitiendo automáticamente la carpeta de control `.git` y los archivos ignorados.

* **Ejemplos básicos:**

  ```bash
  # Genera un archivo ZIP con el código limpio perteneciente al último commit (HEAD)
  git archive --format=zip HEAD -o mi-proyecto.zip
  ```

  ```bash
  # Genera un empaquetado comprimido TAR de la punta actual de la rama main
  git archive --format=tar main -o codigo-main.tar
  ```

  ```bash
  # Despliega los formatos de empaquetado de compresión disponibles en el entorno
  git archive -l
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Genera una compresión que contiene únicamente el contenido de un directorio específico
  git archive HEAD src/
  ```

  ```bash
  # Genera el paquete resguardando todos los archivos dentro de una carpeta raíz ficticia
  git archive --prefix=mi-app-v1/ HEAD -o release.zip
  ```

  ```bash
  # Descarga un empaquetado de un proyecto remoto sin necesidad de clonarlo previamente
  git archive --remote=git@github.com:usuario/repo.git HEAD -o proy.tar
  ```

  ```bash
  # Empaqueta y comprime los archivos exactos que pertenecen a una etiqueta de versión
  git archive -o release-v2.zip v2.0.0
  ```

  ```bash
  # Genera un empaquetado TAR y lo comprime externamente mediante gzip en consola
  git archive --format=tar HEAD | gzip > proyecto.tar.gz
  ```

---

## 29. git gc

**Explicación general:** Ejecuta la recolección de basura (Garbage Collector), optimizando la base de datos interna mediante compresión y eliminando objetos inalcanzables.

* **Ejemplos básicos:**

  ```bash
  # Ejecuta el proceso de limpieza estándar compactando el almacenamiento interno
  git gc
  ```

  ```bash
  # Evalúa el estado del repositorio y ejecuta la limpieza únicamente si es necesario
  git gc --auto
  ```

  ```bash
  # Fuerza la purga inmediata de objetos huérfanos sin esperar el tiempo de gracia
  git gc --prune=now
  ```

* **Ejemplos intermedios/avanzados:**

  ```bash
  # Realiza una optimización profunda recalculando deltas para reducir espacio al máximo
  git gc --aggressive
  ```

  ```bash
  # Ejecuta el proceso de recolección de basura en segundo plano omitiendo mensajes en consola
  git gc --quiet
  ```

  ```bash
  # Diagnóstico: muestra el recuento de objetos sueltos y el peso del repositorio en disco
  git count-objects -v
  ```

  ```bash
  # Remueve físicamente del almacenamiento interno los objetos inalcanzables del sistema
  git prune
  ```

  ```bash
  # Agrupa los objetos sueltos y los consolida en archivos grandes de pack eficientes
  git repack -a -d
  ```
