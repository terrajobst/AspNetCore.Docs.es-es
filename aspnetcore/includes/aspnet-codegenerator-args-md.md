<!-- Options common to Razor Pages and Controller -->
| Opción               | DESCRIPCIÓN|
| ----------------- | ------------ |
| --model o -m  | Clase de modelo que se va a usar. |
| --dataContext o -dc  | La clase `DbContext` que se va a usar. |
| --bootstrapVersion o -b  | Especifica la versión de arranque. Los valores válidos son `3` o `4`. El valor predeterminado es `4`. Si es necesario y no existe, se crea un directorio *wwwroot* que incluye los archivos de arranque de la versión especificada. |
| --referenceScriptLibraries o -scripts |  Bibliotecas de scripts de referencia en las vistas generadas. Agrega `_ValidationScriptsPartial` a las páginas Editar y Crear. |
| --layout o -l | Página de diseño personalizado que se va a usar. |
| --useDefaultLayout o -udl | Usa el diseño predeterminado de las vistas. |
| --force o -f | Sobrescribe los archivos existentes. |
| --relativeFolderPath o -outDir | Ruta de acceso relativa a la carpeta de salida del proyecto donde se genera el archivo. Si no se especifica, los archivos se generan en la carpeta del proyecto. |