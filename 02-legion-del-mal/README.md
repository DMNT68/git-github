# Comandos de Git — Sección 02

Apuntes de los comandos de Git vistos en esta sección. El primero que vimos es `git diff`.

## `git diff`

Muestra las diferencias entre los cambios que tienes en tu directorio de trabajo y la última versión preparada para el próximo commit. Sirve para revisar qué modificaste antes de agregar los archivos al área de preparación.

```bash
git diff
```

En la salida:

- Las líneas que empiezan con `-` se eliminaron o corresponden a la versión anterior.
- Las líneas que empiezan con `+` se agregaron o corresponden a la versión nueva.
- El contexto alrededor de esas líneas ayuda a ubicar el cambio.

### Flujo básico

```bash
# Modifica un archivo y revisa sus cambios
git diff

# Prepara el archivo para el próximo commit
git add README.md

# Crea un commit con los cambios preparados
git commit -m "docs: actualizar apuntes"
```

`git diff` muestra los cambios que todavía no preparaste con `git add`. Después de agregar un archivo, puedes revisar lo preparado con:

```bash
git diff --staged
```

### Revisar cambios preparados: `git diff --staged`

La opción `--staged` muestra las diferencias entre los cambios agregados al área de preparación con `git add` y el último commit. Así puedes comprobar qué se incluirá en el próximo commit.

```bash
git add README.md
git diff --staged
```

La opción correcta es `--staged`, no `--stage`. También puedes usar `git diff --cached`, que es un sinónimo.

## Corregir el último commit: `git commit --amend`

`--amend` es una opción de `git commit` (no un comando independiente). Permite reemplazar el último commit, por ejemplo, para corregir su mensaje:

```bash
git commit --amend -m "docs: corregir mensaje"
```

También puedes preparar cambios con `git add` y agregarlos al último commit:

```bash
git add README.md
git commit --amend --no-edit
```

`--no-edit` conserva el mensaje del commit anterior. Al modificar un commit, Git crea un nuevo identificador para él. Evita enmendar commits que ya compartiste con otras personas, porque cambiar el historial puede causarles inconvenientes.

## Deshacer el último commit conservando los cambios: `git reset --soft HEAD^`

Este comando mueve la referencia de `HEAD` al commit anterior, pero conserva los cambios del commit deshecho en el área de preparación. No elimina el trabajo realizado en los archivos.

```bash
git reset --soft HEAD^
```

Después puedes revisar lo que quedó preparado con `git diff --staged` y crear otro commit, por ejemplo, con un mensaje corregido. `HEAD^` se refiere al primer padre del commit actual; en un historial lineal equivale a `HEAD~1`.

Este comando modifica el historial local. Si el commit ya fue compartido con otras personas, coordina antes de reescribirlo.

## Consultar movimientos de `HEAD`: `git reflog`

`git reflog` muestra un registro local de los movimientos recientes de las referencias, como `HEAD`. Puede ayudar a localizar un commit al que ya no se llega desde una rama después de acciones como `git reset`.

```bash
git reflog
```

Las entradas muestran un identificador abreviado, la referencia y la acción realizada. Para inspeccionar un estado anterior del reflog, puedes usar una referencia como:

```bash
git show HEAD@{1}
```

El reflog es local y sus entradas pueden caducar o eliminarse; no sustituye una copia de seguridad. Antes de restablecer una rama a una entrada del reflog, confirma que elegiste el commit correcto y considera crear una rama de respaldo.

## Mover o renombrar archivos: `git mv`

`git mv` mueve un archivo o cambia su nombre y prepara el cambio para el próximo commit. Su sintaxis es:

```bash
git mv <origen> <destino>
```

Por ejemplo, para renombrar un archivo:

```bash
git mv planes/salvar-mundo.md planes/destruir-mundo.md
```

Luego revisa el cambio preparado con `git status` o `git diff --staged` y confirma el cambio con `git commit`. También podrías mover el archivo manualmente y ejecutar `git add` para preparar el cambio.