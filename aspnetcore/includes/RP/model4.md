<span data-ttu-id="e644b-101">En la tabla siguiente se incluyen los detalles de los parámetros de los generadores de código de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="e644b-101">The following table details the ASP.NET Core code generators\` parameters:</span></span>

| <span data-ttu-id="e644b-102">Parámetro</span><span class="sxs-lookup"><span data-stu-id="e644b-102">Parameter</span></span>               | <span data-ttu-id="e644b-103">Description</span><span class="sxs-lookup"><span data-stu-id="e644b-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="e644b-104">-m</span><span class="sxs-lookup"><span data-stu-id="e644b-104">-m</span></span>  | <span data-ttu-id="e644b-105">Nombre del modelo</span><span class="sxs-lookup"><span data-stu-id="e644b-105">The name of the model.</span></span> |
| <span data-ttu-id="e644b-106">-dc</span><span class="sxs-lookup"><span data-stu-id="e644b-106">-dc</span></span>  | <span data-ttu-id="e644b-107">Contexto de datos</span><span class="sxs-lookup"><span data-stu-id="e644b-107">The data context.</span></span> |
| <span data-ttu-id="e644b-108">-udl</span><span class="sxs-lookup"><span data-stu-id="e644b-108">-udl</span></span> | <span data-ttu-id="e644b-109">Usa el diseño predeterminado.</span><span class="sxs-lookup"><span data-stu-id="e644b-109">Use the default layout.</span></span> |
| <span data-ttu-id="e644b-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="e644b-110">-outDir</span></span> | <span data-ttu-id="e644b-111">Ruta de acceso relativa de la carpeta de salida para crear las vistas</span><span class="sxs-lookup"><span data-stu-id="e644b-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="e644b-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="e644b-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="e644b-113">Agrega `_ValidationScriptsPartial` a las páginas Editar y Crear.</span><span class="sxs-lookup"><span data-stu-id="e644b-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="e644b-114">Active o desactive `h` para obtener ayuda con el comando `aspnet-codegenerator razorpage`:</span><span class="sxs-lookup"><span data-stu-id="e644b-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="e644b-115">Probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="e644b-115">Test the app</span></span>

* <span data-ttu-id="e644b-116">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="e644b-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="e644b-117">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="e644b-117">Test the **Create** link.</span></span>

  ![Página Crear](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="e644b-119">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="e644b-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="e644b-120">Si recibe un error similar al siguiente, verifique que haya realizado las migraciones y que la base de datos esté actualizada:</span><span class="sxs-lookup"><span data-stu-id="e644b-120">If you get the error similar to the following, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
'no such table: Movie'.
```
