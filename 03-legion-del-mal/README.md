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

## Git branch

Las ramas en Git permiten trabajar en líneas de desarrollo independientes sin afectar el código principal. Cada rama puede tener su propio historial de commits y luego integrarse con la rama principal mediante un merge o rebase.

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
