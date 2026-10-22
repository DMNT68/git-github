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