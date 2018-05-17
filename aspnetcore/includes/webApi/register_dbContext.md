## <a name="register-the-database-context"></a><span data-ttu-id="3d265-101">Registrar el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="3d265-101">Register the database context</span></span>

<span data-ttu-id="3d265-102">En este paso, el contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3d265-102">In this step, the database context is registered with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="3d265-103">Los servicios (por ejemplo, el contexto de la base de datos) que se registran con el contenedor de inserción de dependencias (DI) están disponibles para los controladores.</span><span class="sxs-lookup"><span data-stu-id="3d265-103">Services (such as the DB context) that are registered with the dependency injection (DI) container are available to the controllers.</span></span>

<span data-ttu-id="3d265-104">Registre el contexto de la base de datos con el contenedor de servicio mediante la compatibilidad integrada para [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3d265-104">Register the DB context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3d265-105">Reemplace el contenido del archivo *Startup.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3d265-105">Replace the contents of the *Startup.cs* file with the following code:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="3d265-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span><span class="sxs-lookup"><span data-stu-id="3d265-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="3d265-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span><span class="sxs-lookup"><span data-stu-id="3d265-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span></span>
::: moniker-end

<span data-ttu-id="3d265-108">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="3d265-108">The preceding code:</span></span>

* <span data-ttu-id="3d265-109">Quita el código no usado.</span><span class="sxs-lookup"><span data-stu-id="3d265-109">Removes the unused code.</span></span>
* <span data-ttu-id="3d265-110">Especifica que se inserte una base de datos en memoria en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="3d265-110">Specifies an in-memory database is injected into the service container.</span></span>