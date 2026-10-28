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
