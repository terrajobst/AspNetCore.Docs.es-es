<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Agregar una cadena de conexión de base de datos

Agregue una cadena de conexión al archivo *appsettings.json*.

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Registrar el contexto de base de datos

Registre el contexto de base de datos con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el archivo *Startup.cs*.