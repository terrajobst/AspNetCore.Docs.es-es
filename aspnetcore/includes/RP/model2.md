<span data-ttu-id="33391-101">Agregue las propiedades siguientes a la clase `Movie`:</span><span class="sxs-lookup"><span data-stu-id="33391-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="33391-102">La base de datos requiere el campo `ID` para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="33391-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="33391-103">Agregar una clase de contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="33391-103">Add a database context class</span></span>

<span data-ttu-id="33391-104">Agregue una clase derivada `DbContext` con el nombre *MovieContext.cs* a la carpeta *Modelos*.</span><span class="sxs-lookup"><span data-stu-id="33391-104">Add a `DbContext` derived class named *MovieContext.cs* to the *Models* folder.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?range=1-12,14-17,19-21)]

<span data-ttu-id="33391-105">El código anterior crea una propiedad `DbSet` para el conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="33391-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="33391-106">En la terminología de Entity Framework, un conjunto de entidades suele corresponderse con una tabla de base de datos, mientras que una entidad lo hace con una fila de la tabla.</span><span class="sxs-lookup"><span data-stu-id="33391-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span> <span data-ttu-id="33391-107">El nombre de la propiedad `DbSet` es `Movies`.</span><span class="sxs-lookup"><span data-stu-id="33391-107">The `DbSet` property name is `Movies`.</span></span> <span data-ttu-id="33391-108">Puesto que la base de datos usa nombres singulares, el ejemplo invalida `OnModelCreating` para usar la forma singular (`Movie`) para el nombre de tabla.</span><span class="sxs-lookup"><span data-stu-id="33391-108">Since the database uses singular names, the sample overrides `OnModelCreating` to use the singular form (`Movie`) for the table name.</span></span>
