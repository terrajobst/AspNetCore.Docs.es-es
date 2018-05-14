<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Aplicar scaffolding al modelo de película

* Ejecute lo siguiente desde la línea de comandos (en el directorio del proyecto que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*):

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

Si se produce un error:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

El error anterior se produce cuando el usuario se encuentra en el directorio incorrecto. Abra un shell de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*) y, después, ejecute el comando anterior.

Si se produce un error:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Salga de Visual Studio y vuelva a ejecutar el comando.
