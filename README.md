# Taller entregable 1: Git, GitHub y GitHub Actions

Automatización introductoria que imprime un saludo personalizado, la fecha y hora,
y la información del sistema operativo del runner mediante un script de Python.

## Integrantes

**Nombre completo:** Quinnie Villarreal, Diego Sanchez

**Usuario de GitHub y propietario del repositorio:** [quinnie9](https://github.com/quinnie9).

## Entregable

- [Workflow](.github/workflows/mi_workflow.yml): configura los eventos y ejecuta el job `imprimir_info`.
- [Script Python](mi_script.py): imprime el autor, el sistema operativo y la fecha y hora.

## Respuestas

### 1. ¿Qué eventos configuraste para disparar tu workflow y por qué?

Configuramos `push`, limitado a la rama `main`, para ejecutar la automatización cada vez que suba cambios a la rama principal. También configuramos `workflow_dispatch`
para poder ejecutarla manualmente desde la pestaña Actions sin crear un commit.
Son dos eventos independientes dentro de `on`.

### 2. ¿Qué sistema operativo utiliza el runner y qué relación tiene con el shell?

`ubuntu-latest` utiliza Ubuntu, una distribución de Linux. El job se ejecuta en
una máquina virtual alojada por GitHub, independiente de la computadora que usamos; por eso funciona aunque yo use Windows o macOS. El runner proporciona el entorno donde se ejecutan los pasos y el shell interpreta los comandos de los pasos `run`.

En el paso del saludo configuramos explícitamente `shell: bash`. Bash interpreta
`echo` y la sustitución `$(date)`, que ejecuta `date` para obtener la fecha y hora del runner. El script Python también consulta el sistema y la hora del runner, por lo que en Actions debe mostrar Linux.

### 3. Tres acciones preconstruidas no mencionadas en el material

| Acción | ¿Qué hace y para qué sirve? |
| --- | --- |
| [actions/setup-node](https://github.com/actions/setup-node) | Configura una versión de Node.js y la deja disponible para los pasos posteriores. Sirve para ejecutar herramientas, construir aplicaciones y realizar pruebas de proyectos JavaScript. |
| [actions/cache](https://github.com/actions/cache) | Guarda y restaura dependencias y otros archivos mediante claves de caché. Sirve para reducir descargas y acelerar ejecuciones posteriores. |
| [docker/build-push-action](https://github.com/docker/build-push-action) | Construye imágenes de contenedores con Docker Buildx y permite publicarlas en un registro. Sirve para empaquetar una aplicación y distribuir una imagen reproducible de ella. |

### 4. ¿Qué es un secreto y cómo lo usarías?

Un secreto es un valor sensible, como un token o una contraseña, almacenado en
la configuración de GitHub Actions para que no quede escrito en los archivos
del repositorio. Se usa para autenticar tareas que necesitan acceder a servicios.

Lo creariamos en **Settings > Secrets and variables > Actions** y lo referenciariamos mediante una expresión como `${{ secrets.API_TOKEN }}`. Por ejemplo, podriamos pasarlo a un programa mediante una variable de entorno:

```yaml
env:
  API_TOKEN: ${{ secrets.API_TOKEN }}
```

El programa leería esa variable para autenticarse, sin imprimir su contenido en
los logs. Este taller no necesita secretos personalizados: el permiso `contents: read` del `GITHUB_TOKEN` automático es suficiente para descargar el
repositorio.

### 5. Reflexión individual sobre GitHub Actions

Como propuesta para nuestro flujo de trabajo diario, automatizariamos estas dos tareas:

- **Ejecutar pruebas al subir cambios o abrir una pull request.** Esto permitiría
  detectar errores antes de integrar el código y evitaría depender de recordar
  ejecutar las pruebas manualmente.
- **Revisar el formato y la calidad del código.** Automatizar estas revisiones
  ayudaría a mantener un estilo consistente y detectar problemas comunes,
  dejando más tiempo para revisar la lógica del programa.

Este taller es el primer paso para comprender los eventos, jobs y pasos que
permitirían implementar esas automatizaciones. El workflow actual imprime
información; esas pruebas y revisiones serían mejoras futuras.

