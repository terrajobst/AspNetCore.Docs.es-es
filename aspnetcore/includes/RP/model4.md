<span data-ttu-id="61c5f-101">En la tabla siguiente se incluyen los detalles de los parámetros de los generadores de código de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="61c5f-101">The following table details the ASP.NET Core code generators\` parameters:</span></span>

| <span data-ttu-id="61c5f-102">Parámetro</span><span class="sxs-lookup"><span data-stu-id="61c5f-102">Parameter</span></span>               | <span data-ttu-id="61c5f-103">Descripción</span><span class="sxs-lookup"><span data-stu-id="61c5f-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="61c5f-104">-m</span><span class="sxs-lookup"><span data-stu-id="61c5f-104">-m</span></span>  | <span data-ttu-id="61c5f-105">Nombre del modelo</span><span class="sxs-lookup"><span data-stu-id="61c5f-105">The name of the model.</span></span> |
| <span data-ttu-id="61c5f-106">-dc</span><span class="sxs-lookup"><span data-stu-id="61c5f-106">-dc</span></span>  | <span data-ttu-id="61c5f-107">Contexto de datos</span><span class="sxs-lookup"><span data-stu-id="61c5f-107">The data context.</span></span> |
| <span data-ttu-id="61c5f-108">-udl</span><span class="sxs-lookup"><span data-stu-id="61c5f-108">-udl</span></span> | <span data-ttu-id="61c5f-109">Usa el diseño predeterminado.</span><span class="sxs-lookup"><span data-stu-id="61c5f-109">Use the default layout.</span></span> |
| <span data-ttu-id="61c5f-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="61c5f-110">-outDir</span></span> | <span data-ttu-id="61c5f-111">Ruta de acceso relativa de la carpeta de salida para crear las vistas</span><span class="sxs-lookup"><span data-stu-id="61c5f-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="61c5f-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="61c5f-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="61c5f-113">Agrega `_ValidationScriptsPartial` a las páginas Editar y Crear.</span><span class="sxs-lookup"><span data-stu-id="61c5f-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="61c5f-114">Active o desactive `h` para obtener ayuda con el comando `aspnet-codegenerator razorpage`:</span><span class="sxs-lookup"><span data-stu-id="61c5f-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="61c5f-115">Probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="61c5f-115">Test the app</span></span>

* <span data-ttu-id="61c5f-116">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="61c5f-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="61c5f-117">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="61c5f-117">Test the **Create** link.</span></span>

 ![Página Crear](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="61c5f-119">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="61c5f-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="61c5f-120">Si recibe el error siguiente, compruebe que haya realizado las migraciones y que la base de datos esté actualizada:</span><span class="sxs-lookup"><span data-stu-id="61c5f-120">If you get the following error, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```