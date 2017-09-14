En la tabla siguiente se incluyen los detalles de los parámetros de los generadores de código de ASP.NET Core:

| Parámetro               | Descripción|
| ----------------- | ------------ |
| -m  | Nombre del modelo |
| -dc  | Contexto de datos |
| -udl | Usa el diseño predeterminado. |
| -outDir | Ruta de acceso relativa de la carpeta de salida para crear las vistas |
| --referenceScriptLibraries | Agrega `_ValidationScriptsPartial` a las páginas Editar y Crear. |

Active o desactive `h` para obtener ayuda con el comando `aspnet-codegenerator razorpage`:

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a>Probar la aplicación

* Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador (`http://localhost:port/movies`).
* Pruebe el vínculo **Crear**.

 ![Página Crear](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.

Si recibe el error siguiente, compruebe que haya realizado las migraciones y que la base de datos esté actualizada:

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```