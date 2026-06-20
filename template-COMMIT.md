# GUIA PARA ESTRUCTURAR UN BUEN COMMIT EN GIT

Un commit no es un simple "guardado". Es la unidad básica de la historia de tu
proyecto. Un buen commit permite hacer blame, revert y cherry-pick de forma
quirúrgica, y facilita la revision de codigo en Pull Requests.

## 1. ANATOMIA DE UN COMMIT PERFECTO

Un commit de calidad se divide en 3 partes obligatorias y 1 opcional:

    <tipo>[alcance opcional]: <asunto>

    [cuerpo opcional pero altamente recomendado]

    [pie opcional - para issues o breaking changes]

**IMPORTANTE:** Debe haber una linea en blanco entre el asunto y el cuerpo,
y entre el cuerpo y el pie.

## 2. EL ASUNTO (SUBJECT) - Maximo 50 caracteres

Es el "titular" del commit. Debe responder a: "Si aplico este commit,
¿que pasara con el codigo?"

* Usa imperativo presente: Como si le dieras una orden a Git.
    * CORRECTO -> "Add login validation"
    * CORRECTO -> "Fix memory leak in cache"
    * INCORRECTO -> "Added login" (pasado)
    * INCORRECTO -> "I fixed the bug" (primera persona)
* Sin punto final.
* Primera letra mayuscula (excepto si usas tipos en minuscula como "feat:").
* Maximo 50 caracteres (si pasas de 50, estas mezclando demasiadas cosas).

## 3. EL TIPO (TYPE) - BASADO EN CONVENTIONAL COMMITS

Usa estos prefijos para estandarizar y permitir generacion automatica
de CHANGELOG.md:

* **feat:** Una nueva funcionalidad para el usuario (feature).
* **fix:** Correccion de un bug.
* **docs:** Cambios exclusivos en documentacion (README, comentarios).
* **style:** Cambios esteticos (espacios, indentacion, comillas, lint).
                No afecta la logica.
* **refactor:** Reescritura de codigo sin cambiar su comportamiento externo.
* **perf:** Mejora de rendimiento (es un tipo especial de refactor).
* **test:** Anadir o corregir pruebas unitarias / de integracion.
* **chore:** Tareas de mantenimiento (actualizar dependencias, configuraciones).
* **ci:** Cambios en integracion continua (GitHub Actions, Jenkins, etc.).

**EJEMPLO PRACTICO:**
    feat(auth): add OAuth2 login button

## 4. EL CUERPO (BODY) - Maximo 72 caracteres por linea

Aqui explicas el ¿QUE? y sobre todo el ¿POR QUE?. El ¿COMO? ya se ve en el
git diff, asi que no lo repitas.

* Contexto: Explica el problema que resuelve este commit.
* Alternativas: Si probaste otra solucion y no funciono, mencionlo brevemente.
* Limitaciones: Si tiene efectos secundarios conocidos, documentalos.

## 5. EL PIE (FOOTER)

Usalo para:

* Cerrar issues: "Closes #123" (cierra el ticket 123 al hacer merge).
* Breaking Changes: Si esta actualizacion rompe versiones anteriores,
  escribelo con "BREAKING CHANGE:" al inicio del pie.

## 6. EJEMPLO PRACTICO: BIEN vs. MAL

**EJEMPLO MALO (Commit monolitico y vago):**
    git commit -m "cambios varios"
    > No dice que cambio, ni por que. Es inutil en un git blame.

**EJEMPLO BUENO (Siguiendo todas las reglas):**
    git commit -m "refactor(api): simplify error handling middleware

    Previously, each route duplicated try/catch blocks.
    Centralize error logic into a single middleware to reduce
    boilerplate and improve maintainability.

    Closes #42"

## 7. LA REGLA DE ORO: COMMITS ATOMICOS

Un commit debe contener UNA SOLA COSA logica.

**EJEMPLO CORRECTO (Bien separado):**
    Commit 1 -> feat: add email field to user schema
    Commit 2 -> test: add unit tests for email validation

**EJEMPLO INCORRECTO (Mal):**
    Un solo commit que cambia 20 archivos, arregla 3 bugs, anade una libreria
    y cambia el CSS.

**CONSEJO:** Si en el asunto tienes que usar la palabra "y" (ej: "Add login and fix
footer"), es senal de que debe ser VARIOS commits.

## 8. COMO APLICARLO EN TU FLUJO DIARIO

1. **Staging selectivo:**
   No uses "git add ." a ciegas. Usa "git add -p" para anadir fragmentos
   especificos de archivos y lograr commits atomicos.

2. **Configura una plantilla para ayudar a tu equipo:**
   Crea un archivo llamado ".gitmessage.txt" en la raiz de tu proyecto con
   este contenido:

       ------------------------------------------------------------------------------
       # <tipo>(<alcance>): <asunto> (max 50 chars)
       #
       # CUERPO (max 72 chars por linea):
       # - Explica QUE se hizo y POR QUE.
       # - No expliques COMO (eso esta en el diff).
       #
       # PIE:
       # Closes #numero
       # BREAKING CHANGE: descripcion
       ------------------------------------------------------------------------------

   Luego asignalo global o localmente con:
   `git config commit.template .gitmessage.txt`

## 9. BONUS: ¿Y SI YA SUBI UN COMMIT MALO?

* Si NO lo has subido al remoto (git push):
    Usa "git commit --amend" para corregir el mensaje o anadir archivos
    olvidados en el ultimo commit.

* Si YA lo subiste al remoto:
    Usa "git commit --fixup <hash>" y luego "git rebase -i --autosquash"
    para limpiar la historia de forma segura (tema avanzado para otra leccion).
