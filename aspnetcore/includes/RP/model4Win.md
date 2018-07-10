<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="a39f4-101">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="a39f4-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="a39f4-102">Ejecute lo siguiente desde la línea de comandos (en el directorio del proyecto que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*):</span><span class="sxs-lookup"><span data-stu-id="a39f4-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="a39f4-103">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="a39f4-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="a39f4-104">El error anterior se produce cuando el usuario se encuentra en el directorio incorrecto.</span><span class="sxs-lookup"><span data-stu-id="a39f4-104">The preceeding error happens when you are in the wrong directory.</span></span> <span data-ttu-id="a39f4-105">Abra un shell de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*) y, después, ejecute el comando anterior.</span><span class="sxs-lookup"><span data-stu-id="a39f4-105">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files), and then run the preceeding command.</span></span>

<span data-ttu-id="a39f4-106">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="a39f4-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="a39f4-107">Salga de Visual Studio y vuelva a ejecutar el comando.</span><span class="sxs-lookup"><span data-stu-id="a39f4-107">Exit Visual Studio and run the command again.</span></span>
