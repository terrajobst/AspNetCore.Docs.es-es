<!-- THIS INCLUDE USED BY MVC AND RP -->
<span data-ttu-id="68ab9-101">Agregue las propiedades siguientes a la clase `Movie`:</span><span class="sxs-lookup"><span data-stu-id="68ab9-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

<span data-ttu-id="68ab9-102">la clase `Movie` contiene:</span><span class="sxs-lookup"><span data-stu-id="68ab9-102">The `Movie` class contains:</span></span>

* <span data-ttu-id="68ab9-103">La base de datos requiere el campo `ID` para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="68ab9-103">The `ID` field is required by the database for the primary key.</span></span>
* <span data-ttu-id="68ab9-104">`[DataType(DataType.Date)]`:  El atributo [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) especifica el tipo de datos (Date).</span><span class="sxs-lookup"><span data-stu-id="68ab9-104">`[DataType(DataType.Date)]`:  The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute specifies the type of the data (Date).</span></span> <span data-ttu-id="68ab9-105">Con este atributo:</span><span class="sxs-lookup"><span data-stu-id="68ab9-105">With this attribute:</span></span>

  * <span data-ttu-id="68ab9-106">El usuario no tiene que especificar información horaria en el campo de fecha.</span><span class="sxs-lookup"><span data-stu-id="68ab9-106">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="68ab9-107">Solo se muestra la fecha, no información horaria.</span><span class="sxs-lookup"><span data-stu-id="68ab9-107">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="68ab9-108">Los elementos [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) se tratan en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="68ab9-108">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>