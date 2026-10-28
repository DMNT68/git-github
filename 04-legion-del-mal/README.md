# Git Stash y Rebase

## Guardar cambios temporalmente con `git stash`

`git stash` guarda temporalmente cambios locales que todavía no quieres incluir en un commit y deja limpio el directorio de trabajo. Es útil, por ejemplo, cuando necesitas cambiar de rama para atender otra tarea. De forma predeterminada incluye los cambios en archivos rastreados; para incluir también archivos nuevos sin seguimiento, usa `-u`.

```bash
# Guardar los cambios rastreados con una descripción
git stash push -m "Trabajo en progreso"

# Guardar también archivos nuevos sin seguimiento
git stash push -u -m "Trabajo en progreso"
```

### Consultar y recuperar cambios guardados

Los stashes se guardan en una lista. Puedes verlos y aplicar uno sin eliminarlo:

```bash
# Listar los stashes
git stash list

# Aplicar el stash más reciente y conservarlo en la lista
git stash apply

# Aplicar un stash específico
git stash apply stash@{1}
```

Si quieres aplicar el stash más reciente y quitarlo de la lista al mismo tiempo, usa `pop`:

```bash
git stash pop
```

`apply` conserva el stash para poder volver a aplicarlo; `pop` lo elimina de la lista si se aplica correctamente. En ambos casos pueden surgir conflictos si los archivos cambiaron desde que guardaste el stash. Resuélvelos como cualquier otro conflicto y revisa el resultado antes de continuar.

### Auto-merge y conflictos al aplicar un stash

Al ejecutar `git stash apply` o `git stash pop`, Git intenta combinar automáticamente los cambios guardados con el estado actual de la rama. Si los cambios afectan partes distintas de un archivo, normalmente puede integrarlos sin intervención. Si ambos conjuntos de cambios modificaron la misma parte, Git no puede decidir qué versión conservar y deja ese archivo en conflicto.

Comprueba qué archivos necesitan atención:

```bash
git status
```

Abre cada archivo en conflicto y revisa las secciones delimitadas por marcas como `<<<<<<<`, `=======` y `>>>>>>>`. Edita el contenido para conservar o combinar los cambios necesarios y elimina las marcas. Luego, indica a Git que resolviste el archivo:

```bash
git add archivo-resuelto
```

Repite el proceso con cada archivo en conflicto y revisa el resultado. No hay un comando `git stash --continue`: una vez resueltos los conflictos, los cambios ya están en el directorio de trabajo y puedes continuar trabajando o crear un commit si corresponde.

Si `git stash pop` encuentra conflictos, Git no elimina ese stash de la lista. Después de resolverlos y verificar que ya no necesitas la copia guardada, puedes eliminarla explícitamente con `git stash drop stash@{0}`. Si aplicaste con `git stash apply`, el stash permanece en la lista tanto si hubo conflictos como si no.

### Eliminar stashes

Cuando ya no necesites un stash, puedes eliminar uno específico o limpiar la lista completa:

```bash
# Eliminar un stash específico
git stash drop stash@{1}

# Eliminar todos los stashes
git stash clear
```

`git stash clear` elimina todos los stashes y no ofrece una opción sencilla para recuperarlos, así que úsalo solo si estás seguro. Antes de borrar un stash, puedes inspeccionar su contenido con `git stash show -p stash@{0}`.

## Reorganizar commits con `git rebase`

`git rebase` vuelve a aplicar los commits de una rama encima de otra. Esto puede dejar el historial lineal, como si el trabajo de la rama hubiera empezado desde el commit más reciente de la rama base. A diferencia de `git merge`, normalmente no crea un commit de unión; los commits reaplicados reciben nuevos identificadores.

Por ejemplo, para actualizar una rama de trabajo con los cambios recientes de `main`:

```bash
git switch feature/login
git fetch origin
git rebase origin/main
```

Git toma los commits propios de `feature/login` y los reaplica sobre `origin/main`. Antes de iniciar el rebase, guarda o confirma los cambios locales pendientes para evitar que interfieran con la operación.

### Resolver conflictos durante un rebase

Si Git no puede aplicar uno de los commits automáticamente, pausa el rebase y muestra los archivos en conflicto. Consulta el estado, edita cada archivo para resolver las marcas de conflicto y prepara los archivos resueltos:

```bash
git status
# Edita y resuelve los archivos
git add archivo-resuelto
git rebase --continue
```

Repite estos pasos si aparecen conflictos en otros commits. Para cancelar el rebase y volver al estado previo a iniciarlo:

```bash
git rebase --abort
```

### Rebase interactivo

El modo interactivo permite revisar y reorganizar los commits todavía no integrados en la rama base:

```bash
git rebase -i origin/main
```

Git abre una lista de commits, normalmente del más antiguo al más reciente. Al comienzo de cada línea aparece una acción que determina qué hacer con ese commit:

- `pick`: conserva y aplica el commit sin cambios. Es la acción predeterminada.
- `reword`: conserva los cambios del commit, pero permite editar su mensaje.
- `edit`: pausa el rebase al llegar a ese commit para modificarlo, por ejemplo, con `git commit --amend`. Después, continúa con `git rebase --continue`.
- `squash`: combina los cambios del commit con el commit anterior y permite editar el mensaje combinado.
- `fixup`: combina los cambios con el commit anterior, pero descarta el mensaje de este commit y conserva el mensaje del anterior.
- `drop`: elimina el commit de la nueva historia.

Por ejemplo, una lista podría quedar así:

```text
pick   a1b2c3d Agrega estructura inicial
reword b2c3d4e Corrige mensaje de commit
edit   c3d4e5f Ajusta validación
squash d4e5f6a Completa validación
fixup  e5f6a7b Corrige detalle menor
drop   f6a7b8c Elimina cambio experimental
```

En este ejemplo, `squash` y `fixup` se combinan con el commit aplicado inmediatamente antes. Guarda y cierra el editor para que Git ejecute el plan; si el rebase se pausa por un `edit` o por conflictos, realiza los cambios necesarios y continúa con `git rebase --continue`.

### Rebase o merge

- Usa **rebase** para actualizar una rama propia y mantener un historial lineal.
- Usa **merge** cuando quieras integrar ramas conservando explícitamente la unión y el historial de ambas.

El rebase reescribe el historial porque crea nuevos commits. Evita hacerlo sobre commits que ya compartiste en una rama de trabajo compartida: otras personas podrían basar su trabajo en los identificadores anteriores.
