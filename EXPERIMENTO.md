# Experimento con Gitignore: Configuración Global y Local

Este documento detalla las pruebas realizadas para comprender el funcionamiento de las reglas de exclusión en Git, tanto a nivel global del sistema como a nivel local del repositorio.

## 1. Configuración y Prueba de `.gitignore_global`

Primero, configuramos Git para que utilice un archivo de exclusión global ubicado en el directorio del usuario:
\`\`\`bash
git config --global core.excludesfile ~/.gitignore_global
\`\`\`

Dentro de este archivo, añadimos patrones genéricos de archivos que no queremos rastrear en ningún proyecto, como extensiones de compilación o archivos del sistema (`*.o`, `*.log`, `*.zip`, `.DS_Store`).

**Prueba:**
Creamos archivos simulando estos formatos (`prueba.o`, `documento.log`, `archivo.zip` y `carpeta/.DS_Store`). 
Al ejecutar \`git status\`, la terminal devolvió el mensaje "no hay nada para confirmar". Esto demuestra que Git ignoró estos archivos automáticamente en todo el sistema sin necesidad de configurar el repositorio local. 

*(Ver imagen de referencia: Captura desde 2026-09-28 13-11-00.png)*

## 2. Configuración y Prueba de `.gitignore` Local

A continuación, creamos un archivo `.gitignore` en la raíz del repositorio local para establecer reglas específicas del proyecto:
- `dir1/*` (Ignora todo en dir1)
- `!dir1/info.txt` (Excepción: NO ignorar info.txt)
- `dir2/*.txt` (Ignora archivos .txt en dir2)
- `dir3/**/*.txt` (Ignora archivos .txt en dir3 y sus subcarpetas)
- `*.o` (Ignora archivos .o localmente)

**Prueba:**
Creamos la estructura de directorios y archivos correspondientes para poner a prueba los patrones. Al ejecutar \`git status\`, el resultado mostró únicamente como "Archivos sin seguimiento":
1. `.gitignore` (El archivo de reglas en sí).
2. `dir1/` (Aparece porque la regla `!` salvó al archivo `info.txt` de ser ignorado, mientras que `ignorado.tmp` fue omitido).
3. `dir2/` (Aparece porque contiene el archivo `otros.py`, el cual no es un `.txt` y por tanto no fue bloqueado).

Todo el contenido de `dir3` fue ignorado correctamente gracias al patrón `**/*.txt`.

*(Ver imagen de referencia: Captura desde 2026-09-28 13-13-28.png)*

## 3. Conclusiones y Diferencias

A través de este experimento, hemos comprobado las diferencias clave entre ambos métodos:

* **Ignorar de forma Global (`.gitignore_global`):** Se aplica a nivel de usuario en el sistema operativo. Es ideal para excluir archivos generados por el propio sistema (como `.DS_Store` en macOS) o por herramientas y editores de código locales. Al ser global, previene que estos archivos basura se suban por accidente a cualquier repositorio de la máquina.
* **Ignorar de forma Local (`.gitignore`):** Se aplica únicamente al repositorio donde se encuentra alojado. Este archivo se debe confirmar (commit) y subir al repositorio remoto, de manera que todos los desarrolladores que colaboren en el proyecto compartan las mismas reglas de exclusión (por ejemplo, carpetas de dependencias como `node_modules`, entornos virtuales o archivos compilados específicos del lenguaje del proyecto).
