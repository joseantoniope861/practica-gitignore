# Experimento con Gitignore

## Patrones probados
- **Globales (`~/.gitignore_global`)**: Probé `*.o`, `*.log`, `*.zip` y `.DS_Store`. Al hacer `git status`, Git los ignoró completamente sin necesidad de configurar nada en el proyecto local.

- **Locales (`.gitignore`)**:

  - `dir1/*` ignoró la carpeta entera, pero la regla `!dir1/info.txt` funcionó correctamente para excluir ese archivo específico del ignore.
  - `dir2/*.txt` ignoró los archivos de texto, pero dejó pasar los de otras extensiones como `.py`.
  - `dir3/**/*.txt` aplicó la regla a todos los niveles de subcarpetas dentro de dir3.

## Diferencias entre ignorar local y globalmente
- **Global**: Afecta a todos los repositorios Git en mi usuario/ordenador. Es útil para archivos de mi sistema operativo o de mi editor de código que no quiero subir por accidente a ningún proyecto.
- **Local**: Solo afecta al proyecto actual. El archivo `.gitignore` se sube al repositorio para que todos los colaboradores del equipo tengan las mismas reglas sobre qué archivos (como dependencias, compilados o temporales del proyecto) se deben ignorar.
