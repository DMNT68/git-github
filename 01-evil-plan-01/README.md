# Fundamentos

En esta sección daremos nuestros primeros pasos con Git. Aprenderemos para qué sirve un sistema de control de versiones, cuáles son sus comandos básicos y cómo utilizarlo para registrar y consultar la evolución de un proyecto.

## Temas de la sección

### 1. ¿Qué es y para qué sirve Git?

Git es un sistema de control de versiones distribuido. Permite guardar la evolución de los archivos de un proyecto, revisar qué cambió, recuperar versiones anteriores y trabajar de forma organizada sin perder el historial.

### 2. Primeros comandos

Conoceremos comandos esenciales como `git status`, `git add`, `git commit`, `git diff` y `git log`. Estos comandos permiten consultar el estado del proyecto, preparar cambios, crear registros y revisar las diferencias realizadas.

### 3. Crear un repositorio

Aprenderemos a convertir una carpeta en un repositorio utilizando `git init` y a reconocer la carpeta `.git`, donde Git almacena la información del historial y la configuración del proyecto.

### 4. Flujo de trabajo con Git

Practicaremos el ciclo habitual de trabajo: modificar archivos, revisar los cambios, agregarlos al área de preparación y crear un commit con un mensaje claro que describa lo realizado.

### 5. Historial de cambios (Git Log)

Usaremos `git log` para consultar los commits del proyecto, conocer su autor y fecha, leer sus mensajes y entender cómo ha evolucionado el código.

### 6. Alias para comandos

Crearemos alias para acortar comandos frecuentes y personalizar la experiencia de trabajo con Git. Por ejemplo, podremos configurar un alias para consultar el historial de forma más resumida.

### 7. Práctica: Proyecto Omega

Aplicaremos lo aprendido en un proyecto práctico. Realizaremos cambios pequeños, los registraremos con commits y consultaremos el historial para reforzar el flujo básico de trabajo con Git.

## Comandos de práctica

Este repositorio puede utilizarse para practicar el flujo normal mientras haces cambios pequeños, por ejemplo, modificar un texto de la interfaz o ajustar un estilo:

```bash
git status
git diff
git add README.md
git commit -m "docs: describe the project"
git log --oneline
```

Después de cada cambio, revisa `git status` y `git diff` antes de crear un commit para comprobar qué se incluirá.
