# Git Desde Cero

> commit 3 desde feature/TICKET-002-cambiosFile002

## ¿Qué es Git?
Git es un sistema de control de versiones distribuido que te permite rastrear los cambios en tu código fuente de manera estructurada y segura.

## Los 4 Espacios de Trabajo en Git (Workspaces)
Para entender cómo funciona Git, es indispensable conocer las "estaciones" o espacios por los que transita tu código:

* **Working Directory (Directorio de Trabajo):** Es tu carpeta actual en la computadora. Aquí creas, editas o eliminas archivos. Git ve estos cambios pero aún no los está rastreando oficialmente.
* **Staging Area (Área de Preparación / Index):** Es una sala de espera. Aquí "subes" los archivos específicos que quieres incluir en tu próximo guardado.
* **Local Repository (Repositorio Local):** Es el historial permanente de tu proyecto en tu computadora. Cuando haces un "commit", los archivos del Staging Area se guardan aquí con una etiqueta de tiempo y autor.
* **Remote Repository (Repositorio Remoto):** Es un servidor externo donde compartes tu historial local con otros desarrolladores y mantienes un respaldo en la nube.

### Diagrama de Flujo de los Espacios de Trabajo
```text
+---------------+         +--------------+         +--------------+         +---------------+
|               |         |              |         |              |         |               |
|  Working Dir. |         | Staging Area |         |  Local Repo  |         |  Remote Repo  |
|               |         |              |         |              |         |               |
+---------------+         +--------------+         +--------------+         +---------------+
        |                         |                        |                        |
        |       git add           |                        |                        |
        |------------------------>|                        |                        |
        |                         |       git commit       |                        |
        |                         |----------------------->|                        |
        |                         |                        |        git push        |
        |                         |                        |----------------------->|
        |                         |                        |                        |
        |                         |                        |        git pull        |
        |<--------------------------------------------------------------------------|
```

## Comandos Básicos de Inicio

A continuación, los comandos que te permitirán mover tus archivos a través de estos espacios en tu día a día:

```bash
# Configura tu nombre de usuario para que quede registrado en el historial
git config --global user.name "Tu Nombre"

# Configura tu correo electrónico de desarrollador
git config --global user.email "tu@email.com"

# Inicializa un nuevo repositorio de Git en tu directorio actual (Crea el Local Repo)
git init

# Muestra el estado actual de tus archivos y en qué espacio se encuentran
git status

# Mueve un archivo específico desde el Working Directory al Staging Area
git add nombre_del_archivo.txt

# Mueve TODOS los archivos modificados o nuevos al Staging Area
git add .

# Guarda permanentemente los cambios del Staging Area en el Local Repo
git commit -m "Mensaje descriptivo sobre los cambios realizados"

# Conecta tu repositorio local con un repositorio remoto
git remote add origin https:#url-del-repositorio-remoto.git

# Sube los commits guardados en tu Local Repo hacia el Remote Repo
git push -u origin main

# Descarga los últimos cambios del Remote Repo y los integra en tu Working Directory
git pull origin main
```

## Ramas en Git (Branches)

Una rama en Git representa una línea independiente de desarrollo. Conceptualmente, una rama es simplemente un puntero móvil que apunta a uno de los commits en tu historial.

Permiten trabajar en nuevas características, corrección de errores o experimentos sin afectar el código estable de la rama principal (generalmente llamada main o master).


En este estado, la rama principal (main) se queda detenida en un punto del historial, mientras que la rama secundaria (feature) avanza con sus propios cambios independientes.

```text
(C1) <--- (C2) <--- (C3)  [rama: main]
            \
             <--- (F1) <--- (F2)  [rama: feature-login]
```

## Fusión de Ramas (Merge)

El proceso de fusión toma las líneas de desarrollo independientes de dos ramas diferentes y las une en una sola historia unificada.

Existen dos tipos principales de procesos de fusión:

1. **Fast-Forward (Avance rápido):** Ocurre si la rama principal no ha recibido ningún commit nuevo desde que se bifurcó la rama secundaria. Git simplemente desliza el puntero de la rama principal hacia adelante hasta alcanzar el último commit de la rama secundaria sin crear un historial complejo.

```text
Antes del merge:
(C1) <--- (C2)  [rama: main]
            \
             <--- (F1) <--- (F2)  [rama: feature]

Después del merge:
(C1) <--- (C2) <--- (F1) <--- (F2)  [rama: main] [rama: feature]
```

2. **Three-Way Merge (Fusión de 3 vías):** Ocurre si ambas ramas han avanzado con nuevos commits de forma paralela y simultánea. Git realiza un análisis basado en tres puntos claves (el ancestro común de ambas ramas y las dos puntas de las ramas actuales) y genera automáticamente un nuevo commit especial llamado "commit de fusión" (merge commit) para entrelazar ambas historias.

```text
Antes del merge:
(C1) <--- (C2) <--- (C3)  [rama: main]
            \
             <--- (F1) <--- (F2)  [rama: feature]

Después del merge:
(C1) <--- (C2) <--- (C3) <--------- (M1)  [rama: main] (Commit de Fusión)
            \                       /
             <--- (F1) <--- (F2) <--
```

## Comandos Básicos para Ramas y Fusión

A continuación, los comandos esenciales que te permitirán operar con tus espacios y flujos de trabajo ramificados:

```bash
# Lista todas las ramas locales de tu repositorio actual
git branch

# Crea una nueva rama llamada "feature-nueva" sin moverte de la rama actual
git branch feature-nueva

# Cambia tu espacio de trabajo activo a la rama especificada
git checkout feature-nueva

# Comando alternativo moderno para cambiar de rama de forma segura
git switch feature-nueva

# Crea una nueva rama y te posiciona en ella inmediatamente en un solo paso
git checkout -b feature-nueva

# Fusiona la rama especificada dentro de tu rama actual (ej. debes estar en main para absorber los cambios)
git merge feature-nueva

# Elimina de forma segura la rama secundaria una vez que sus cambios ya fueron integrados con éxito
git branch -d feature-nueva
```
