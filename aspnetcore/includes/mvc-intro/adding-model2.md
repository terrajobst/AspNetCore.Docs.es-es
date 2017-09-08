## <a name="add-initial-migration-and-update-the-database"></a>Agregar migración inicial y actualizar la base de datos

* Abra un símbolo del sistema y desplácese al directorio del proyecto. (El directorio que contiene el *Startup.cs* archivo).

* Ejecute los siguientes comandos en el símbolo del sistema:

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  [.NET core](http://go.microsoft.com/fwlink/?LinkID=517853) es una implementación multiplataforma de. NET. Esto es lo que hacer estos comandos:

  * `dotnet restore`: Descarga los paquetes de NuGet especificados en el *.csproj* archivo.
  * `dotnet ef migrations add Initial`Ejecuta el comando de migraciones de Entity Framework .NET Core CLI y crea la migración inicial. El parámetro después de "Agregar" es un nombre que se asigna a la migración. Aquí le da un nombre la migración "Inicial" porque es la migración de base de datos inicial. Esta operación crea el */migraciones de datos/\<fecha y hora > _Initial.cs* archivo que contiene los comandos de migración para agregar el *película* tabla a la base de datos.
  * `dotnet ef database update`Actualiza la base de datos con la migración que acabamos de crear.

Obtendrá información acerca de la cadena de conexión y la base de datos en el tutorial siguiente. Obtendrá información sobre los cambios de modelo de datos en el [agregar un campo](xref:tutorials/first-mvc-app/new-field) tutorial.