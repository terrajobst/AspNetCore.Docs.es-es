Agregue las propiedades siguientes a la clase `Movie`:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

La base de datos requiere el campo `ID` para la clave principal.

<a name="dc"></a>
### <a name="add-a-database-context-class"></a>Agregar una clase de contexto de base de datos

Agregue una clase derivada `DbContext` con el nombre *MovieContext.cs* a la carpeta *Modelos*.
[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

El código anterior crea una propiedad `DbSet` para el conjunto de entidades. En la terminología de Entity Framework, un conjunto de entidades suele corresponderse con una tabla de base de datos, mientras que una entidad lo hace con una fila de la tabla.
