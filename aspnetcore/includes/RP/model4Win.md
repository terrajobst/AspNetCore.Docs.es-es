<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="dadb1-101">Aplicar scaffolding al modelo de película</span><span class="sxs-lookup"><span data-stu-id="dadb1-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="dadb1-102">Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="dadb1-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="dadb1-103">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="dadb1-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="dadb1-104">Si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="dadb1-104">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="dadb1-105">Salga de Visual Studio y vuelva a ejecutar el comando.</span><span class="sxs-lookup"><span data-stu-id="dadb1-105">Exit Visual Studio and run the command again.</span></span>