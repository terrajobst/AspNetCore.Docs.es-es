<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="324f4-101">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="324f4-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="324f4-102">Ejecute lo siguiente desde la línea de comandos (en el directorio del proyecto que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*):</span><span class="sxs-lookup"><span data-stu-id="324f4-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="324f4-103">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="324f4-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="324f4-104">Abra un shell de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="324f4-104">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
