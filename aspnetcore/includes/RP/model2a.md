<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="24474-101">Agregar una cadena de conexión de base de datos</span><span class="sxs-lookup"><span data-stu-id="24474-101">Add a database connection string</span></span>

<span data-ttu-id="24474-102">Agregue una cadena de conexión al archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="24474-102">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="24474-103">Registrar el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="24474-103">Register the database context</span></span>

<span data-ttu-id="24474-104">Registre el contexto de base de datos con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="24474-104">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>
