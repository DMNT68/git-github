# GitHub

GitHub es una plataforma para alojar repositorios Git en la nube y colaborar en proyectos. Git es la herramienta que controla las versiones del código; GitHub agrega funciones para compartir repositorios, revisar cambios y coordinar el trabajo de un equipo.

## Conceptos principales

- **Repositorio:** contiene los archivos del proyecto y su historial de cambios.
- **Remoto:** copia del repositorio alojada en GitHub. Por convención, suele llamarse `origin`.
- **Rama:** línea de trabajo independiente. Permite desarrollar una funcionalidad o corregir un error sin modificar directamente la rama principal.
- **Commit:** registro de un conjunto de cambios con un mensaje que explica qué se hizo.
- **Pull request (PR):** propuesta para integrar los cambios de una rama en otra. Permite revisar el código y conversar antes de incorporarlo.
- **Issue:** tarea, error o idea registrada para organizar el trabajo.

## Crear una cuenta de GitHub

Para colaborar en GitHub necesitas una cuenta personal:

1. Entra en [github.com](https://github.com/) y selecciona **Sign up**.
2. Escribe tu correo electrónico y crea una contraseña segura.
3. Elige un nombre de usuario disponible. Este nombre será visible en tu perfil y puede aparecer en las direcciones de tus repositorios.
4. Completa la verificación solicitada y confirma tu correo con el código que GitHub te envíe.
5. Sigue los pasos de configuración inicial y, si quieres, agrega una foto y otros datos a tu perfil.

Guarda tus credenciales de forma segura y no las compartas. Puedes activar la autenticación en dos pasos desde la configuración de seguridad de tu cuenta para protegerla mejor.

## Crear un repositorio en GitHub

1. Inicia sesión en GitHub y selecciona **New repository** (también puedes usar el botón **+** y elegir **New repository**).
2. Escribe un nombre descriptivo para el repositorio y, si quieres, agrega una descripción.
3. Elige si será **Public** (visible para cualquiera) o **Private** (visible solo para ti y las personas que autorices).
4. Si empiezas un proyecto nuevo, puedes marcar **Add a README file**. También puedes elegir un `.gitignore` y una licencia según las necesidades del proyecto.
5. Selecciona **Create repository**.

GitHub mostrará la dirección del repositorio. Para descargarlo a tu computadora y comenzar a trabajar, copia la URL HTTPS y ejecuta:

```bash
git clone https://github.com/usuario/nombre-del-repositorio.git
cd nombre-del-repositorio
```

Si ya tienes un proyecto en una carpeta local y quieres publicarlo en un repositorio nuevo, créalo en GitHub sin agregar un README, `.gitignore` ni licencia para evitar conflictos con los archivos locales. Luego, desde la carpeta del proyecto, publica su rama principal:

```bash
git init -b main
git add .
git commit -m "Agrega proyecto inicial"
git remote add origin https://github.com/usuario/nombre-del-repositorio.git
git push -u origin main
```

Si el proyecto ya es un repositorio Git, no ejecutes `git init`; agrega el remoto y publica la rama correspondiente. Revisa `git status` antes de preparar los archivos y no incluyas credenciales ni datos sensibles.

## Flujo de trabajo con GitHub

Un flujo común consiste en actualizar el proyecto, trabajar en una rama propia y proponer los cambios mediante un pull request.

### 1. Obtener el repositorio

Si el repositorio ya existe en GitHub, clónalo para crear una copia local:

```bash
git clone https://github.com/usuario/proyecto.git
cd proyecto
```

Si vas a contribuir a un proyecto de otra persona y no tienes permiso para escribir en él, primero crea un **fork** desde GitHub y clona tu fork.

### 2. Crear una rama de trabajo

Actualiza la rama principal y crea una rama para tu tarea:

```bash
git switch main
git pull origin main
git switch -c feature/nombre-de-la-tarea
```

Usa nombres descriptivos para las ramas, por ejemplo `feature/formulario-contacto` o `fix/error-validacion`.

### 3. Realizar y guardar los cambios

Edita los archivos y revisa qué cambió antes de preparar el commit:

```bash
git status
git diff
git add archivo-modificado
git commit -m "Agrega formulario de contacto"
```

Puedes usar `git add .` para preparar todos los cambios del directorio, pero revisa primero `git status` para no incluir archivos que no quieras compartir.

### 4. Publicar la rama en GitHub

`git push` envía los commits de una rama local al repositorio remoto, por ejemplo, a GitHub. No publica cambios que todavía no estén guardados en un commit. Antes de usarlo, puedes revisar el estado del repositorio:

```bash
git status
```

La primera vez que publiques una rama, usa `-u` para asociarla con la rama remota:

```bash
git push -u origin feature/nombre-de-la-tarea
```

Reemplaza `feature/nombre-de-la-tarea` por el nombre de tu rama. La opción `-u` configura el seguimiento entre la rama local y la remota; después de crear otros commits en esa rama, basta con ejecutar:

```bash
git push
```

Si Git rechaza el envío porque el remoto tiene cambios que no están en tu copia local, actualiza tu rama con `git pull` y resuelve cualquier conflicto antes de volver a intentarlo. Evita `git push --force` si no sabes cómo afecta los cambios remotos, ya que puede sobrescribir commits de otras personas.

### 5. Abrir un pull request

En GitHub, abre un pull request desde tu rama hacia `main`. Describe el propósito de los cambios y, si aplica, enlaza el issue relacionado. El equipo puede revisar el código, dejar comentarios y solicitar ajustes.

Si necesitas hacer correcciones, realiza nuevos commits en la misma rama y publícalos:

```bash
git add archivo-modificado
git commit -m "Ajusta validacion del formulario"
git push
```

Los nuevos cambios aparecerán en el pull request existente.

### 6. Integrar los cambios y actualizar el repositorio local

Cuando el pull request haya sido aprobado y se integre, actualiza tu copia local:

```bash
git switch main
git pull origin main
```

Luego puedes iniciar otra tarea desde una rama actualizada.

## Actualizar cambios con `git pull`

`git pull` descarga los cambios de un repositorio remoto y los integra en la rama local en la que estás. En el caso habitual, combina dos operaciones: `git fetch` obtiene los commits remotos y `git merge` los integra. Según la configuración de Git, la integración también puede hacerse mediante rebase.

Para actualizar la rama `main` desde el remoto `origin`, cámbiate primero a ella y ejecuta:

```bash
git switch main
git pull origin main
```

La forma general del comando es `git pull <remoto> <rama>`. Por ejemplo, `git pull origin main` obtiene los cambios de la rama `main` de `origin` y los integra en la rama local actual; por eso conviene comprobar en qué rama estás con `git status` antes de ejecutarlo.

Si Git informa que hay conflictos, abre los archivos señalados, decide cómo combinar los cambios y guarda el resultado. Después, marca los archivos como resueltos y completa la integración:

```bash
git add archivo-con-conflictos
git commit
```

Si la integración se hizo mediante rebase, continúa con `git rebase --continue` en lugar de `git commit`. Revisa `git status` para ver qué operación está en curso y qué pasos faltan.

## Tags y releases en GitHub

Un **tag** (etiqueta) identifica un commit específico, normalmente para marcar una versión publicada, como `v1.0.0`. A diferencia de una rama, que avanza con nuevos commits, un tag suele permanecer asociado a la misma versión. Un **GitHub Release** es una publicación creada en GitHub a partir de un tag; puede incluir notas de versión y archivos descargables.

### Crear y publicar un tag

Cuando los cambios de una versión estén listos y confirmados en la rama adecuada, crea un tag anotado. La anotación guarda información como el mensaje, el autor y la fecha:

```bash
# Crear un tag para el commit actual
git tag -a v1.0.0 -m "Versión 1.0.0"

# Revisar los tags locales
git tag

# Publicar el tag en GitHub
git push origin v1.0.0
```

Para marcar otro commit, puedes indicar su identificador en el comando `git tag`. Para publicar todos los tags locales que aún no estén en el remoto, usa `git push origin --tags`.

### Crear un Release

1. Abre el repositorio en GitHub y entra en **Releases**.
2. Selecciona **Draft a new release**.
3. Elige el tag que acabas de publicar, o crea uno nuevo desde esa pantalla.
4. Escribe un título y notas que describan las novedades, correcciones y cambios importantes.
5. Si corresponde, adjunta archivos de la versión y selecciona **Publish release**.

Usa una convención consistente para nombrar versiones. Por ejemplo, `v1.2.3` suele representar versión mayor, menor y de corrección, respectivamente. Publicar un Release no reemplaza el tag: el Release se construye a partir de él.

## Recomendaciones

- Mantén cada rama enfocada en una sola tarea.
- Haz commits pequeños con mensajes claros y en imperativo, como `Agrega validacion al formulario`.
- Revisa los cambios con `git diff` antes de confirmar y compartir.
- No subas contraseñas, tokens ni otros datos sensibles al repositorio.
- Lee las instrucciones del proyecto y ejecuta sus pruebas antes de abrir un pull request.
