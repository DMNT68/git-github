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

La primera vez, usa `-u` para asociar la rama local con la remota:

```bash
git push -u origin feature/nombre-de-la-tarea
```

Después de crear más commits en esa rama, basta con ejecutar `git push`.

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

## Recomendaciones

- Mantén cada rama enfocada en una sola tarea.
- Haz commits pequeños con mensajes claros y en imperativo, como `Agrega validacion al formulario`.
- Revisa los cambios con `git diff` antes de confirmar y compartir.
- No subas contraseñas, tokens ni otros datos sensibles al repositorio.
- Lee las instrucciones del proyecto y ejecuta sus pruebas antes de abrir un pull request.
