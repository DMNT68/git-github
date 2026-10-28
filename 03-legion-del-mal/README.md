# Ramas, uniones, conflictos y tags — Sección 03

## Merge fast-forward

Un merge fast-forward ocurre cuando la rama base no ha recibido nuevos commits desde que se creó la rama de trabajo. En ese caso, Git puede mover el puntero de la rama principal directamente hasta el último commit de la rama secundaria, sin crear un commit de merge adicional.

Esto pasa cuando el historial es lineal:

```bash
git checkout main
git merge feature/nueva-funcionalidad
```

Si `feature/nueva-funcionalidad` se originó desde `main` y `main` no tuvo cambios nuevos, el resultado es simplemente:

```bash
main: A --- B --- C --- D --- E
```

Es decir, `main` avanza hasta donde está `feature`, sin generar una unión con un commit especial.

### Ejemplo visual

```bash
git checkout main
# main: A --- B --- C
# feature: A --- B --- C --- D --- E

git merge feature
# main pasa a ser A --- B --- C --- D --- E
```

En este caso no se crea un commit de merge; solo se hace un avance del puntero.

### Cuándo ocurre

- Cuando la rama objetivo no ha cambiado desde la separación.
- Cuando el historial es limpio y lineal.
- Cuando se integra una rama de trabajo corta o de desarrollo.

### Comandos relacionados

- `git merge --ff-only nombre-rama`: permite el merge solo si puede hacerse como fast-forward; si no, falla.
- `git merge --no-ff nombre-rama`: fuerza la creación de un commit de merge aunque el avance fuese lineal.

Esto ayuda a mantener un historial simple y legible cuando todas las ramas tienen una estructura lineal.

## Merge

El comando `git merge` se usa para integrar los cambios de una rama en otra. Cuando una rama de trabajo ya terminó su desarrollo, se puede combinar su historial con la rama principal para llevar esos avances al proyecto principal.

Por ejemplo:

```bash
git checkout main
git merge feature/nueva-funcion
```

Esto toma los commits realizados en `feature/nueva-funcion` y los incorpora en `main`. Si ambas ramas han avanzado por separado, Git puede crear un commit de merge o resolver conflictos si existen cambios en el mismo archivo.

### Cuándo usar merge

- Cuando se quiere conservar el historial de trabajo de una rama.
- Cuando se termina una funcionalidad y se desea integrarla a la rama principal.
- Cuando varias ramas deben combinarse sin perder su evolución.

### Importante

Antes de hacer un merge, es recomendable revisar el estado de ambas ramas y asegurarse de que no haya cambios pendientes que puedan causar conflictos.

### Merge con conflictos

Un conflicto de merge ocurre cuando Git no puede combinar automáticamente los cambios de dos ramas. Suele suceder cuando ambas modifican la misma parte de un archivo. Git pausa la unión y señala los archivos que necesitan una revisión manual; los cambios no se pierden.

Por ejemplo, después de iniciar el merge:

```bash
git checkout main
git merge feature/nueva-funcion
```

Git puede mostrar un mensaje indicando que hay conflictos. Para identificarlos, consulta el estado:

```bash
git status
```

Abre cada archivo marcado como conflictivo y busca las marcas que Git agrega, como `<<<<<<< HEAD`, `=======` y `>>>>>>> feature/nueva-funcion`. El bloque en conflicto tendrá una estructura similar a esta:

```text
[marcador de inicio: HEAD]
Contenido de la rama actual (main)
[separador]
Contenido de la rama que se está integrando
[marcador de fin: feature/nueva-funcion]
```

`HEAD` representa la rama en la que estás, mientras que la sección inferior corresponde a la rama que intentas unir. Edita el archivo para conservar una de las versiones o combinar ambas según corresponda. Después, elimina todas las marcas y guarda el resultado.

Cuando hayas resuelto y revisado los archivos, márcalos como resueltos y termina la unión:

```bash
git add archivo-resuelto
git merge --continue
```

También puedes usar `git add .` para marcar todos los archivos resueltos, pero revisa antes que no incluya cambios ajenos al merge. Si quieres cancelar la operación y volver al estado anterior, ejecuta:

```bash
git merge --abort
```

Resolver un conflicto no consiste simplemente en elegir una rama: hay que comprobar que el contenido final conserva los cambios necesarios y funciona como se espera.

## Git branch

Las ramas en Git permiten trabajar en líneas de desarrollo independientes sin afectar el código principal. Cada rama puede tener su propio historial de commits y luego integrarse con la rama principal mediante un merge o rebase.

### Importante: no trabajar sobre la rama `main`

La rama `main` suele representar la versión estable y oficial del proyecto. Por esa razón, no se recomienda hacer cambios directamente sobre ella mientras se está desarrollando una nueva funcionalidad o corrigiendo un problema. Si se trabaja en `main`, se corre el riesgo de mezclar código incompleto, romper la versión estable del repositorio o dificultar la colaboración entre varias personas.

En lugar de eso, se crea una rama de trabajo, por ejemplo `feature/registro` o `fix/login`, y allí se realizan los cambios. Cuando todo está validado, se integra esa rama en `main` mediante un merge o un pull request. Este flujo ayuda a mantener un historial limpio, facilita la revisión del código y evita afectar el trabajo de otros desarrolladores.

### Comandos básicos

```bash
git branch
```

Lista todas las ramas del repositorio actual y marca la rama activa con un asterisco.

```bash
git branch nombre-rama
```

Crea una nueva rama a partir del estado actual.

```bash
git checkout nombre-rama
```

Cambia a una rama existente.

```bash
git checkout -b nombre-rama
```

Crea una rama nueva y cambia directamente a ella.

```bash
git branch -d nombre-rama
```

Elimina una rama si ya fue fusionada. Si se quiere forzar su eliminación aunque tenga cambios no integrados, se usa `-D`.

```bash
git branch -m nombre-nuevo
```

Renombra la rama actual.

### Ejemplo práctico

```bash
git branch
# * main
#   feature/login

git checkout -b feature/registro
# Ahora estás en la rama feature/registro
```

Esto permite desarrollar una característica sin tocar la rama principal mientras otros cambios siguen sucediendo en `main`.

### Flujo habitual

```bash
git checkout main
git pull origin main
git checkout -b feature/nueva-funcion
# trabajo...
git add .
git commit -m "Agrega nueva funcionalidad"
git checkout main
git merge feature/nueva-funcion
```

La rama principal queda actualizada con los cambios de la rama de trabajo. Cuando el desarrollo termina, esa rama puede fusionarse o eliminarse según el flujo del proyecto.

### Diferencia con `git switch`

`git checkout nombre-rama` también permite cambiar de rama, pero combina esa función con otras operaciones, como restaurar archivos. `git switch` ofrece una forma más específica y clara de cambiar de rama.

```bash
git switch nombre-rama
```

En resumen, `git branch` permite crear y consultar ramas; `git switch` permite cambiar entre ellas.

## Git switch

El comando `git switch` sirve para cambiar la rama activa del repositorio. Al cambiar de rama, Git actualiza el directorio de trabajo para mostrar los archivos correspondientes a esa rama.

### Cambiar a una rama existente

```bash
git switch nombre-rama
```

Por ejemplo:

```bash
git switch main
```

### Crear una rama y cambiar a ella

Usa la opción `-c` para crear una rama nueva y activarla en un solo paso:

```bash
git switch -c feature/nueva-funcion
```

La rama se crea desde el commit de la rama actual. Para crearla desde otra rama existente, indica el punto de partida:

```bash
git switch -c feature/nueva-funcion main
```

### Volver a la rama anterior

La opción `-` cambia a la rama activa anterior:

```bash
git switch -
```

### Cambiar a un commit sin activar una rama

Para revisar un commit específico sin cambiar una rama, se puede usar `--detach`:

```bash
git switch --detach <commit>
```

Esto deja el repositorio en estado *detached HEAD*. Los commits creados en ese estado no pertenecen a una rama; si quieres conservarlos, crea una rama antes de continuar:

```bash
git switch -c nombre-rama
```

### Cambios sin guardar

Si hay cambios locales que entrarían en conflicto con los archivos de la rama de destino, Git no permitirá el cambio para evitar perderlos. Guarda los cambios con un commit o guárdalos temporalmente con `git stash` antes de cambiar de rama.

### Flujo de trabajo

```bash
git switch main
git pull origin main
git switch -c feature/nueva-funcion
# realiza los cambios
git add .
git commit -m "Agrega nueva funcionalidad"
git switch main
git merge feature/nueva-funcion
```

`git switch` se enfoca en cambiar de rama y `git branch` en administrarlas. Ambos comandos se complementan para trabajar con ramas de forma clara.
