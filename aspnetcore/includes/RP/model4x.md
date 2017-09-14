<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="205d7-101">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="205d7-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="205d7-102">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="205d7-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="205d7-103">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="205d7-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```