---
title: "Páginas de Razor con EF Core - modelo de datos - 5 de 8"
author: rick-anderson
description: "En este tutorial agregará más entidades y relaciones y personalizar el modelo de datos mediante la especificación de formato, validación y reglas de asignación de la base de datos."
keywords: Anotaciones de datos de ASP.NET Core, Entity Framework Core,
ms.author: riande
manager: wpickett
ms.date: 10/25/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: c2761f79fa4836d29541526782969bb0fd47778b
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a><span data-ttu-id="b897e-104">Crear un modelo de datos complejos - Core EF con el tutorial de las páginas de Razor (5 de 8)</span><span class="sxs-lookup"><span data-stu-id="b897e-104">Creating a complex data model - EF Core with Razor Pages tutorial (5 of 8)</span></span>

<span data-ttu-id="b897e-105">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b897e-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="b897e-106">Los tutoriales anteriores ha trabajado con un modelo de datos básicos que se compone de tres entidades.</span><span class="sxs-lookup"><span data-stu-id="b897e-106">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="b897e-107">En este tutorial:</span><span class="sxs-lookup"><span data-stu-id="b897e-107">In this tutorial:</span></span>

* <span data-ttu-id="b897e-108">Se agregan más entidades y relaciones.</span><span class="sxs-lookup"><span data-stu-id="b897e-108">More entities and relationships are added.</span></span>
* <span data-ttu-id="b897e-109">El modelo de datos se ha personalizado mediante la especificación de formato, validación y reglas de asignación de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b897e-109">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="b897e-110">Las clases de entidad para el modelo de datos completa se muestra en la siguiente ilustración:</span><span class="sxs-lookup"><span data-stu-id="b897e-110">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Diagrama de entidad](complex-data-model/_static/diagram.png)

<span data-ttu-id="b897e-112">Si experimenta problemas no puede resolver, descargue el [aplicación final de esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span><span class="sxs-lookup"><span data-stu-id="b897e-112">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="b897e-113">Personalizar el modelo de datos con atributos</span><span class="sxs-lookup"><span data-stu-id="b897e-113">Customize the data model with attributes</span></span>

<span data-ttu-id="b897e-114">En esta sección, el modelo de datos para personalizar el uso de atributos.</span><span class="sxs-lookup"><span data-stu-id="b897e-114">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="b897e-115">El atributo de tipo de datos</span><span class="sxs-lookup"><span data-stu-id="b897e-115">The DataType attribute</span></span>

<span data-ttu-id="b897e-116">Las páginas de estudiante actualmente muestra la hora de la fecha de inscripción.</span><span class="sxs-lookup"><span data-stu-id="b897e-116">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="b897e-117">Normalmente, los campos de fecha muestran solo la fecha y no la hora.</span><span class="sxs-lookup"><span data-stu-id="b897e-117">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="b897e-118">Actualización *Models/Student.cs* aparece resaltado este código con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b897e-118">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="b897e-119">El [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) atributo especifica un tipo de datos que es más específico que el tipo intrínseco de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b897e-119">The [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="b897e-120">En este caso que se debe mostrar solo la fecha, no la fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="b897e-120">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="b897e-121">El [enumeración DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) proporciona para muchos tipos de datos, como fecha, hora, número de teléfono, moneda, EmailAddress, etcetera. El `DataType` atributo también puede permitir que la aplicación proporcionar automáticamente las características específicas del tipo.</span><span class="sxs-lookup"><span data-stu-id="b897e-121">The [DataType Enumeration](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="b897e-122">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b897e-122">For example:</span></span>

* <span data-ttu-id="b897e-123">El `mailto:` vínculo se crea automáticamente para `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="b897e-123">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="b897e-124">El selector de fecha se proporciona para `DataType.Date` en la mayoría de los exploradores.</span><span class="sxs-lookup"><span data-stu-id="b897e-124">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="b897e-125">El `DataType` atributo emite HTML 5 `data-` atributos (pronunciado datos dash) que usan los exploradores HTML 5.</span><span class="sxs-lookup"><span data-stu-id="b897e-125">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="b897e-126">El `DataType` atributos no proporcione la validación.</span><span class="sxs-lookup"><span data-stu-id="b897e-126">The `DataType` attributes do not provide validation.</span></span>

<span data-ttu-id="b897e-127">`DataType.Date` no especifica el formato de la fecha que se muestra.</span><span class="sxs-lookup"><span data-stu-id="b897e-127">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="b897e-128">De forma predeterminada, el campo de fecha se muestra según los formatos predeterminados en función del servidor [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span><span class="sxs-lookup"><span data-stu-id="b897e-128">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="b897e-129">El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:</span><span class="sxs-lookup"><span data-stu-id="b897e-129">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="b897e-130">El `ApplyFormatInEditMode` configuración especifica que el formato debe también se aplica a la interfaz de usuario de edición.</span><span class="sxs-lookup"><span data-stu-id="b897e-130">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="b897e-131">Algunos campos no deben usar `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="b897e-131">Some fields should not use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="b897e-132">Por ejemplo, el símbolo de moneda generalmente no debe mostrarse en un cuadro de texto de edición.</span><span class="sxs-lookup"><span data-stu-id="b897e-132">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="b897e-133">El `DisplayFormat` atributo puede usarse por sí mismo.</span><span class="sxs-lookup"><span data-stu-id="b897e-133">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="b897e-134">Suele ser una buena idea usar el `DataType` atribuir a la `DisplayFormat` atributo.</span><span class="sxs-lookup"><span data-stu-id="b897e-134">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="b897e-135">El `DataType` atributo transmite la semántica de los datos en lugar de cómo representar en una pantalla.</span><span class="sxs-lookup"><span data-stu-id="b897e-135">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="b897e-136">El `DataType` atributo proporciona las siguientes ventajas que no están disponibles en `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="b897e-136">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="b897e-137">El explorador puede habilitar características de HTML5.</span><span class="sxs-lookup"><span data-stu-id="b897e-137">The browser can enable HTML5 features.</span></span> <span data-ttu-id="b897e-138">Por ejemplo, mostrar un control de calendario, el símbolo de moneda adecuado para la configuración regional, vínculos de correo electrónico, validación de entrada del lado cliente, etcetera.</span><span class="sxs-lookup"><span data-stu-id="b897e-138">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="b897e-139">De forma predeterminada, el explorador representa datos con el formato correcto según la configuración regional.</span><span class="sxs-lookup"><span data-stu-id="b897e-139">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="b897e-140">Para obtener más información, consulte el [ \<entrada > documentación de la aplicación auxiliar de etiqueta](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="b897e-140">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="b897e-141">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b897e-141">Run the app.</span></span> <span data-ttu-id="b897e-142">Navegue a la página de índice de los alumnos.</span><span class="sxs-lookup"><span data-stu-id="b897e-142">Navigate to the Students Index page.</span></span> <span data-ttu-id="b897e-143">Veces ya no se muestran.</span><span class="sxs-lookup"><span data-stu-id="b897e-143">Times are no longer displayed.</span></span> <span data-ttu-id="b897e-144">Todas las vistas que usa el `Student` modelo muestra la fecha sin hora.</span><span class="sxs-lookup"><span data-stu-id="b897e-144">Every view that uses the `Student` model displays the date without time.</span></span>

![Página de índice de los alumnos mostrando las fechas sin veces](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="b897e-146">El atributo StringLength</span><span class="sxs-lookup"><span data-stu-id="b897e-146">The StringLength attribute</span></span>

<span data-ttu-id="b897e-147">Reglas de validación de datos y mensajes de error de validación pueden especificarse con atributos.</span><span class="sxs-lookup"><span data-stu-id="b897e-147">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="b897e-148">El [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) atributo especifica la longitud mínima y máxima de caracteres que se permiten en un campo de datos.</span><span class="sxs-lookup"><span data-stu-id="b897e-148">The [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="b897e-149">El `StringLength` atributo también proporciona validación de cliente y servidor.</span><span class="sxs-lookup"><span data-stu-id="b897e-149">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="b897e-150">El valor mínimo no influye en el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b897e-150">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="b897e-151">Actualización de la `Student` modelo con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b897e-151">Update the `Student` model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="b897e-152">El código anterior limita los nombres a no más de 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="b897e-152">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="b897e-153">El `StringLength` atributo no impide que un usuario entraran en un espacio en blanco para un nombre.</span><span class="sxs-lookup"><span data-stu-id="b897e-153">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="b897e-154">El [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) atributo se usa para aplicar restricciones a la entrada.</span><span class="sxs-lookup"><span data-stu-id="b897e-154">The [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="b897e-155">Por ejemplo, el siguiente código requiere el primer carácter que se va letra mayúscula y el resto de caracteres sea alfabético:</span><span class="sxs-lookup"><span data-stu-id="b897e-155">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

<span data-ttu-id="b897e-156">Ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="b897e-156">Run the app:</span></span>

* <span data-ttu-id="b897e-157">Navegue a la página de los alumnos.</span><span class="sxs-lookup"><span data-stu-id="b897e-157">Navigate to the Students page.</span></span>
* <span data-ttu-id="b897e-158">Seleccione **crear nuevo**y escriba un nombre más de 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="b897e-158">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="b897e-159">Seleccione **crear**, validación del lado cliente muestra un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="b897e-159">Select **Create**, client-side validation shows an error message.</span></span>

![Página que muestra los errores de longitud de cadena de índice de estudiantes](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="b897e-161">En **Explorador de objetos de SQL Server** (SSOX), abra el Diseñador de tablas de Student haciendo doble clic en el **estudiante** tabla.</span><span class="sxs-lookup"><span data-stu-id="b897e-161">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabla de estudiantes en SSOX antes de las migraciones](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="b897e-163">La imagen anterior muestra el esquema para el `Student` tabla.</span><span class="sxs-lookup"><span data-stu-id="b897e-163">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="b897e-164">Los campos de nombre tengan tipo `nvarchar(MAX)` porque las migraciones no se ha ejecutado en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b897e-164">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="b897e-165">Cuando se ejecutan las migraciones más adelante en este tutorial, se convierten en los campos de nombre `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="b897e-165">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="b897e-166">El atributo de columna</span><span class="sxs-lookup"><span data-stu-id="b897e-166">The Column attribute</span></span>

<span data-ttu-id="b897e-167">Atributos pueden controlar cómo se asignan las clases y propiedades de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b897e-167">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="b897e-168">En esta sección, el `Column` atributo se utiliza para asignar el nombre de la `FirstMidName` propiedad en "FirstName" en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b897e-168">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="b897e-169">Cuando se crea la base de datos, los nombres de propiedad en el modelo se usan para los nombres de columna (excepto cuando la `Column` atributo se usa).</span><span class="sxs-lookup"><span data-stu-id="b897e-169">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="b897e-170">El `Student` modelo utiliza `FirstMidName` para el nombre de campo ya que el campo puede contener también un segundo nombre.</span><span class="sxs-lookup"><span data-stu-id="b897e-170">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="b897e-171">Actualización de la *Student.cs* archivo con el código resaltado siguiente:</span><span class="sxs-lookup"><span data-stu-id="b897e-171">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="b897e-172">Con el cambio anterior, `Student.FirstMidName` en la aplicación se asigna a la `FirstName` columna de la `Student` tabla.</span><span class="sxs-lookup"><span data-stu-id="b897e-172">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="b897e-173">La adición de la `Column` atributo cambia la copia de seguridad de modelo la `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="b897e-173">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="b897e-174">La copia de seguridad de modelo el `SchoolContext` ya no coincide con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b897e-174">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="b897e-175">Si la aplicación se ejecuta antes de aplicar las migraciones, se genera la siguiente excepción:</span><span class="sxs-lookup"><span data-stu-id="b897e-175">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```
<span data-ttu-id="b897e-176">Para actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="b897e-176">To update the DB:</span></span>

* <span data-ttu-id="b897e-177">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="b897e-177">Build the project.</span></span>
* <span data-ttu-id="b897e-178">Abra una ventana de comandos en la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b897e-178">Open a command window in the project folder.</span></span> <span data-ttu-id="b897e-179">Escriba los comandos siguientes para crear una nueva migración y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="b897e-179">Enter the following commands to create a new migration and update the DB:</span></span>

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

<span data-ttu-id="b897e-180">El `dotnet ef migrations add ColumnFirstName` comando genera el mensaje de advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="b897e-180">The `dotnet ef migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="b897e-181">Se genera la advertencia porque los campos de nombre ahora están limitadas a 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="b897e-181">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="b897e-182">Si un nombre en la base de datos tenía más de 50 caracteres, se perderían el 51 hasta el último carácter.</span><span class="sxs-lookup"><span data-stu-id="b897e-182">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="b897e-183">Probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b897e-183">Test the app.</span></span>

<span data-ttu-id="b897e-184">Abra la tabla de estudiantes en SSOX:</span><span class="sxs-lookup"><span data-stu-id="b897e-184">Open the Student table in SSOX:</span></span>

![Tabla de estudiantes en SSOX después de las migraciones](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="b897e-186">Antes de que se ha aplicado la migración, las columnas del nombre eran del tipo [nvarchar (max)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="b897e-186">Before migration was applied, the name columns were of type [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="b897e-187">Las columnas de nombre son ahora `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="b897e-187">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="b897e-188">Ha cambiado el nombre de columna de `FirstMidName` a `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="b897e-188">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="b897e-189">En la sección siguiente, la creación de la aplicación en algunas de las fases genera errores del compilador.</span><span class="sxs-lookup"><span data-stu-id="b897e-189">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="b897e-190">Las instrucciones de especifican cuándo se deben compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b897e-190">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="b897e-191">Actualizar la entidad Student</span><span class="sxs-lookup"><span data-stu-id="b897e-191">Student entity update</span></span>

![Entidad Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="b897e-193">Actualización *Models/Student.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b897e-193">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="b897e-194">El atributo requerido</span><span class="sxs-lookup"><span data-stu-id="b897e-194">The Required attribute</span></span>

<span data-ttu-id="b897e-195">El `Required` atributo hace que los campos obligatorios de propiedades de nombre.</span><span class="sxs-lookup"><span data-stu-id="b897e-195">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="b897e-196">El `Required` atributo no es necesario para los tipos que no aceptan valores NULL, como tipos de valor (`DateTime`, `int`, `double`, etcetera.).</span><span class="sxs-lookup"><span data-stu-id="b897e-196">The `Required` attribute is not needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="b897e-197">Tipos que no pueden ser nulos se tratan automáticamente como campos obligatorios.</span><span class="sxs-lookup"><span data-stu-id="b897e-197">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="b897e-198">El `Required` atributo se podría reemplazar con un parámetro de longitud mínima en el `StringLength` atributo:</span><span class="sxs-lookup"><span data-stu-id="b897e-198">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="b897e-199">El atributo de visualización</span><span class="sxs-lookup"><span data-stu-id="b897e-199">The Display attribute</span></span>

<span data-ttu-id="b897e-200">El `Display` atributo especifica que el título de los cuadros de texto debe ser "Nombre", "Last Name", "Nombre completo" y "Fecha de inscripción".</span><span class="sxs-lookup"><span data-stu-id="b897e-200">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="b897e-201">Los títulos predeterminados que no tenían ningún espacio al dividir las palabras, por ejemplo "apellido".</span><span class="sxs-lookup"><span data-stu-id="b897e-201">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="b897e-202">La propiedad FullName calculado</span><span class="sxs-lookup"><span data-stu-id="b897e-202">The FullName calculated property</span></span>

<span data-ttu-id="b897e-203">`FullName`es una propiedad calculada que devuelve un valor que se crea concatenando dos otras propiedades.</span><span class="sxs-lookup"><span data-stu-id="b897e-203">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="b897e-204">`FullName`no se puede establecer, tiene solo un descriptor de acceso get.</span><span class="sxs-lookup"><span data-stu-id="b897e-204">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="b897e-205">Ya no `FullName` columna se crea en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b897e-205">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="b897e-206">Crear la entidad Instructor</span><span class="sxs-lookup"><span data-stu-id="b897e-206">Create the Instructor Entity</span></span>

![Entidad instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="b897e-208">Crear *Models/Instructor.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b897e-208">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="b897e-209">Tenga en cuenta que varias propiedades son los mismos en el `Student` y `Instructor` entidades.</span><span class="sxs-lookup"><span data-stu-id="b897e-209">Notice that several properties are the same in the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="b897e-210">En el tutorial de herencia de implementación más adelante en esta serie, este código se refactoriza para eliminar la redundancia.</span><span class="sxs-lookup"><span data-stu-id="b897e-210">In the Implementing Inheritance tutorial later in this series, this code is refactored to eliminate the redundancy.</span></span>

<span data-ttu-id="b897e-211">Varios atributos pueden estar en una sola línea.</span><span class="sxs-lookup"><span data-stu-id="b897e-211">Multiple attributes can be on one line.</span></span> <span data-ttu-id="b897e-212">El `HireDate` atributos pudieron escribirse de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="b897e-212">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="b897e-213">Las propiedades de navegación CourseAssignments y OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="b897e-213">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="b897e-214">El `CourseAssignments` y `OfficeAssignment` las propiedades son propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="b897e-214">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="b897e-215">Un instructor puede enseñar a cualquier número de cursos, por lo que `CourseAssignments` se define como una colección.</span><span class="sxs-lookup"><span data-stu-id="b897e-215">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="b897e-216">Si una propiedad de navegación contiene varias entidades:</span><span class="sxs-lookup"><span data-stu-id="b897e-216">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="b897e-217">Debe ser un tipo de lista, donde pueden agregar, eliminar y actualizar las entradas.</span><span class="sxs-lookup"><span data-stu-id="b897e-217">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="b897e-218">Los tipos de propiedad de navegación incluyen:</span><span class="sxs-lookup"><span data-stu-id="b897e-218">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="b897e-219">Si `ICollection<T>` se especifica, Core EF crea un `HashSet<T>` colección de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b897e-219">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="b897e-220">El `CourseAssignment` entidad se explica en la sección acerca de las relaciones de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="b897e-220">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="b897e-221">Estado que un instructor puede tener a lo sumo una oficina de reglas de negocios de la Universidad de contoso.</span><span class="sxs-lookup"><span data-stu-id="b897e-221">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="b897e-222">El `OfficeAssignment` propiedad contiene un único `OfficeAssignment` entidad.</span><span class="sxs-lookup"><span data-stu-id="b897e-222">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="b897e-223">`OfficeAssignment`es null si no se asigna ningún office.</span><span class="sxs-lookup"><span data-stu-id="b897e-223">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="b897e-224">Crear la entidad OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="b897e-224">Create the OfficeAssignment entity</span></span>

![Entidad OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="b897e-226">Crear *Models/OfficeAssignment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b897e-226">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="b897e-227">Key (atributo)</span><span class="sxs-lookup"><span data-stu-id="b897e-227">The Key attribute</span></span>

<span data-ttu-id="b897e-228">El `[Key]` atributo se usa para identificar una propiedad como la clave principal (PK) cuando el nombre de propiedad es algo que no sean classnameID o identificador.</span><span class="sxs-lookup"><span data-stu-id="b897e-228">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="b897e-229">Hay una relación de uno a cero o uno entre la `Instructor` y `OfficeAssignment` entidades.</span><span class="sxs-lookup"><span data-stu-id="b897e-229">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="b897e-230">Solo existe una asignación de oficina en relación con el instructor se asigna a.</span><span class="sxs-lookup"><span data-stu-id="b897e-230">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="b897e-231">El `OfficeAssignment` PK también es la clave externa (FK) a la `Instructor` entidad.</span><span class="sxs-lookup"><span data-stu-id="b897e-231">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="b897e-232">El núcleo de EF no se reconoce automáticamente `InstructorID` como la clave principal de `OfficeAssignment` porque:</span><span class="sxs-lookup"><span data-stu-id="b897e-232">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="b897e-233">`InstructorID`no siga la convención de nomenclatura de Id. o classnameID.</span><span class="sxs-lookup"><span data-stu-id="b897e-233">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="b897e-234">Por lo tanto, la `Key` atributo se usa para identificar `InstructorID` como la clave principal:</span><span class="sxs-lookup"><span data-stu-id="b897e-234">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="b897e-235">De forma predeterminada, Core EF trata la clave como no generada por base de datos porque la columna es para una relación de identificación.</span><span class="sxs-lookup"><span data-stu-id="b897e-235">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="b897e-236">La propiedad de navegación de Instructor</span><span class="sxs-lookup"><span data-stu-id="b897e-236">The Instructor navigation property</span></span>

<span data-ttu-id="b897e-237">El `OfficeAssignment` propiedad de navegación para el `Instructor` entidad es que acepta valores NULL porque:</span><span class="sxs-lookup"><span data-stu-id="b897e-237">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="b897e-238">Tipos de referencia (como clases admiten valores NULL).</span><span class="sxs-lookup"><span data-stu-id="b897e-238">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="b897e-239">Un instructor podría no tener una asignación de oficina.</span><span class="sxs-lookup"><span data-stu-id="b897e-239">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="b897e-240">El `OfficeAssignment` entidad tiene un acepta valores NULL `Instructor` propiedad de navegación porque:</span><span class="sxs-lookup"><span data-stu-id="b897e-240">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="b897e-241">`InstructorID`es que no aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="b897e-241">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="b897e-242">Una asignación de office no puede existir sin un instructor.</span><span class="sxs-lookup"><span data-stu-id="b897e-242">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="b897e-243">Cuando un `Instructor` entidad tiene un relacionados `OfficeAssignment` entidad, cada entidad tiene una referencia a la otra en la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="b897e-243">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="b897e-244">El `[Required]` atributo puede aplicarse a la `Instructor` propiedad de navegación:</span><span class="sxs-lookup"><span data-stu-id="b897e-244">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="b897e-245">El código anterior especifica que debe haber un instructor relacionado.</span><span class="sxs-lookup"><span data-stu-id="b897e-245">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="b897e-246">El código anterior no es necesario porque el `InstructorID` clave externa (que también es la clave principal) es que no aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="b897e-246">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="b897e-247">Modificar la entidad de curso</span><span class="sxs-lookup"><span data-stu-id="b897e-247">Modify the Course Entity</span></span>

![Entidad de curso](complex-data-model/_static/course-entity.png)

<span data-ttu-id="b897e-249">Actualización *Models/Course.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b897e-249">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="b897e-250">El `Course` entidad tiene una propiedad de clave externa (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="b897e-250">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="b897e-251">`DepartmentID`apunta a relacionado `Department` entidad.</span><span class="sxs-lookup"><span data-stu-id="b897e-251">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="b897e-252">El `Course` entidad tiene un `Department` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="b897e-252">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="b897e-253">Núcleo EF no requiere una propiedad de clave externa para un modelo de datos cuando el modelo tiene una propiedad de navegación para una entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="b897e-253">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="b897e-254">Núcleo EF crea automáticamente claves externas en la base de datos cuando son necesarias.</span><span class="sxs-lookup"><span data-stu-id="b897e-254">EF Core automatically creates FKs in the database wherever they are needed.</span></span> <span data-ttu-id="b897e-255">Núcleo EF crea [sombrear propiedades](https://docs.microsoft.com/ef/core/modeling/shadow-properties) para claves externas creadas automáticamente.</span><span class="sxs-lookup"><span data-stu-id="b897e-255">EF Core creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="b897e-256">Con la clave externa en el modelo de datos puede realizar actualizaciones más sencillo y más eficaz.</span><span class="sxs-lookup"><span data-stu-id="b897e-256">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="b897e-257">Por ejemplo, considere la posibilidad de un modelo donde la propiedad FK `DepartmentID` es *no* incluido.</span><span class="sxs-lookup"><span data-stu-id="b897e-257">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="b897e-258">Cuando se captura una entidad de curso para editar:</span><span class="sxs-lookup"><span data-stu-id="b897e-258">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="b897e-259">El `Department` entity es null si no explícitamente está cargado.</span><span class="sxs-lookup"><span data-stu-id="b897e-259">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="b897e-260">Para actualizar la entidad de curso, la `Department` en primer lugar debe buscarse en entidad.</span><span class="sxs-lookup"><span data-stu-id="b897e-260">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="b897e-261">Cuando la propiedad FK `DepartmentID` se incluye en el modelo de datos, no es necesario para capturar el `Department` entidad antes de una actualización.</span><span class="sxs-lookup"><span data-stu-id="b897e-261">When the FK property `DepartmentID` is included in the data model, there is no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="b897e-262">El atributo DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="b897e-262">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="b897e-263">El `[DatabaseGenerated(DatabaseGeneratedOption.None)]` atributo especifica que la clave principal es proporcionada por la aplicación, en lugar de generado por la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b897e-263">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="b897e-264">De forma predeterminada, EF Core se da por supuesto que la base de datos genera valores de clave principal.</span><span class="sxs-lookup"><span data-stu-id="b897e-264">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="b897e-265">Base de datos genera PK valores suele ser el mejor método.</span><span class="sxs-lookup"><span data-stu-id="b897e-265">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="b897e-266">Para `Course` entidades, el usuario especifica la PK.</span><span class="sxs-lookup"><span data-stu-id="b897e-266">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="b897e-267">Por ejemplo, un número de curso como una serie de 1000 departamento matemáticas, una serie de 2000 para el departamento de inglés.</span><span class="sxs-lookup"><span data-stu-id="b897e-267">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="b897e-268">El `DatabaseGenerated` atributo también se puede usar para generar valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="b897e-268">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="b897e-269">Por ejemplo, la base de datos puede generar automáticamente un campo de fecha para registrar la fecha en que se crea o actualiza una fila.</span><span class="sxs-lookup"><span data-stu-id="b897e-269">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="b897e-270">Para obtener más información, consulte [genera propiedades](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="b897e-270">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="b897e-271">Propiedades de navegación y de clave externa</span><span class="sxs-lookup"><span data-stu-id="b897e-271">Foreign key and navigation properties</span></span>

<span data-ttu-id="b897e-272">Las propiedades de clave externa (FK) y propiedades de navegación de la `Course` entidad reflejen las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="b897e-272">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="b897e-273">Un curso se asigna a un departamento, por lo que hay un `DepartmentID` FK y un `Department` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="b897e-273">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="b897e-274">Un curso puede tener cualquier número de alumnos inscritos en él, por lo que el `Enrollments` propiedad de navegación es una colección:</span><span class="sxs-lookup"><span data-stu-id="b897e-274">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="b897e-275">Un curso puede ser imparten por varios instructores, por lo que el `CourseAssignments` propiedad de navegación es una colección:</span><span class="sxs-lookup"><span data-stu-id="b897e-275">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="b897e-276">`CourseAssignment`se explica [más adelante](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="b897e-276">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="b897e-277">Crear la entidad Department</span><span class="sxs-lookup"><span data-stu-id="b897e-277">Create the Department entity</span></span>

![Entidad Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="b897e-279">Crear *Models/Department.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b897e-279">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="b897e-280">El atributo de columna</span><span class="sxs-lookup"><span data-stu-id="b897e-280">The Column attribute</span></span>

<span data-ttu-id="b897e-281">Anteriormente la `Column` atributo se utiliza para cambiar la asignación de nombre de columna.</span><span class="sxs-lookup"><span data-stu-id="b897e-281">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="b897e-282">En el código de la `Department` entidad, el `Column` atributo se utiliza para cambiar la asignación de tipos de datos SQL.</span><span class="sxs-lookup"><span data-stu-id="b897e-282">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="b897e-283">La `Budget` columna se define mediante el tipo de moneda de SQL Server en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="b897e-283">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="b897e-284">Asignación de columnas por lo general no es necesaria.</span><span class="sxs-lookup"><span data-stu-id="b897e-284">Column mapping is generally not required.</span></span> <span data-ttu-id="b897e-285">Núcleo EF generalmente elige el tipo de datos de SQL Server apropiado en función del tipo CLR para la propiedad.</span><span class="sxs-lookup"><span data-stu-id="b897e-285">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="b897e-286">El CLR `decimal` tipo se asigna a un servidor SQL Server `decimal` tipo.</span><span class="sxs-lookup"><span data-stu-id="b897e-286">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="b897e-287">`Budget`es para la moneda, y el tipo de datos de moneda es más adecuado para la moneda.</span><span class="sxs-lookup"><span data-stu-id="b897e-287">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="b897e-288">Propiedades de navegación y de clave externa</span><span class="sxs-lookup"><span data-stu-id="b897e-288">Foreign key and navigation properties</span></span>

<span data-ttu-id="b897e-289">Las propiedades de navegación y FK reflejan las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="b897e-289">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="b897e-290">Un departamento puede tener o no un administrador.</span><span class="sxs-lookup"><span data-stu-id="b897e-290">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="b897e-291">Un administrador es siempre un instructor.</span><span class="sxs-lookup"><span data-stu-id="b897e-291">An administrator is always an instructor.</span></span> <span data-ttu-id="b897e-292">Por lo tanto, la `InstructorID` propiedad se incluye como la clave externa a la `Instructor` entidad.</span><span class="sxs-lookup"><span data-stu-id="b897e-292">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="b897e-293">La propiedad de navegación se denomina `Administrator` pero contiene un `Instructor` entidad:</span><span class="sxs-lookup"><span data-stu-id="b897e-293">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="b897e-294">El signo de interrogación (?) en el código anterior especifica la propiedad admite valores NULL.</span><span class="sxs-lookup"><span data-stu-id="b897e-294">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="b897e-295">Un departamento puede tener varios cursos, por lo que es una propiedad de navegación de cursos:</span><span class="sxs-lookup"><span data-stu-id="b897e-295">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="b897e-296">Nota: Por convención, núcleo de EF permite la eliminación en cascada para claves externas que no aceptan valores NULL y para las relaciones de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="b897e-296">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="b897e-297">Eliminación en cascada puede dar lugar a las reglas de eliminación en cascada circular.</span><span class="sxs-lookup"><span data-stu-id="b897e-297">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="b897e-298">Circular eliminación en cascada causas reglas una excepción cuando se agrega una migración.</span><span class="sxs-lookup"><span data-stu-id="b897e-298">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="b897e-299">Por ejemplo, si la `Department.InstructorID` propiedad no se definió como que acepta valores NULL:</span><span class="sxs-lookup"><span data-stu-id="b897e-299">For example, if the `Department.InstructorID` property was not defined as nullable:</span></span>

* <span data-ttu-id="b897e-300">Núcleo EF configura una regla de eliminación en cascada para eliminar el instructor cuando se elimina el departamento.</span><span class="sxs-lookup"><span data-stu-id="b897e-300">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="b897e-301">Eliminar el instructor cuando se elimina el departamento no es el comportamiento deseado.</span><span class="sxs-lookup"><span data-stu-id="b897e-301">Deleting the instructor when the department is deleted is not the intended behavior.</span></span>

<span data-ttu-id="b897e-302">Si es necesario de las reglas de negocios el `InstructorID` propiedad ser acepta valores NULL, utilice la siguiente instrucción de la API fluida:</span><span class="sxs-lookup"><span data-stu-id="b897e-302">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="b897e-303">El código anterior deshabilita la eliminación en cascada en la relación de instructor de departamento.</span><span class="sxs-lookup"><span data-stu-id="b897e-303">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="b897e-304">Actualizar la entidad de inscripción</span><span class="sxs-lookup"><span data-stu-id="b897e-304">Update the Enrollment entity</span></span>

<span data-ttu-id="b897e-305">Es un registro de inscripción para un uno curso realizado por un alumno.</span><span class="sxs-lookup"><span data-stu-id="b897e-305">An enrollment record is for a one course taken by one student.</span></span>

![Entidad de inscripción](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="b897e-307">Actualización *Models/Enrollment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b897e-307">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="b897e-308">Propiedades de navegación y de clave externa</span><span class="sxs-lookup"><span data-stu-id="b897e-308">Foreign key and navigation properties</span></span>

<span data-ttu-id="b897e-309">Las propiedades de clave externa y propiedades de navegación reflejen las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="b897e-309">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="b897e-310">Un registro de inscripción es para un curso, por lo que hay un `CourseID` propiedad de clave externa y un `Course` propiedad de navegación:</span><span class="sxs-lookup"><span data-stu-id="b897e-310">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="b897e-311">Es un registro de inscripción de un estudiante, por lo que hay un `StudentID` propiedad de clave externa y un `Student` propiedad de navegación:</span><span class="sxs-lookup"><span data-stu-id="b897e-311">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="b897e-312">Relaciones Varios a Varios</span><span class="sxs-lookup"><span data-stu-id="b897e-312">Many-to-Many Relationships</span></span>

<span data-ttu-id="b897e-313">Hay una relación de varios a varios entre la `Student` y `Course` entidades.</span><span class="sxs-lookup"><span data-stu-id="b897e-313">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="b897e-314">El `Enrollment` entidad funciona como una tabla de combinación-to-many *con carga* en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b897e-314">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="b897e-315">"Con una carga" significa que la `Enrollment` tabla contiene datos adicionales además de claves externas de las tablas combinadas (en este caso, la clave principal y `Grade`).</span><span class="sxs-lookup"><span data-stu-id="b897e-315">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="b897e-316">En la siguiente ilustración muestra el aspecto de estas relaciones en un diagrama de la entidad.</span><span class="sxs-lookup"><span data-stu-id="b897e-316">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="b897e-317">(Este diagrama se ha generado mediante herramientas avanzadas de EF de EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="b897e-317">(This diagram was generated using EF Power Tools for EF 6.x.</span></span> <span data-ttu-id="b897e-318">Crear el diagrama no forma parte del tutorial.)</span><span class="sxs-lookup"><span data-stu-id="b897e-318">Creating the diagram isn't part of the tutorial.)</span></span>

![Curso de estudiante muchos a muchos relación](complex-data-model/_static/student-course.png)

<span data-ttu-id="b897e-320">Cada línea de relación tiene un 1 en un extremo y un asterisco (\\*) en el otro, que indica una relación uno a varios.</span><span class="sxs-lookup"><span data-stu-id="b897e-320">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="b897e-321">Si el `Enrollment` tabla no incluir información de categoría, que solo sería necesario contener las claves dos externas (`CourseID` y `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="b897e-321">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="b897e-322">Una tabla sin carga una combinación de varios a varios se suele denominar una tabla combinada pura (PJT).</span><span class="sxs-lookup"><span data-stu-id="b897e-322">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="b897e-323">El `Instructor` y `Course` entidades tienen una relación de varios a varios con una tabla combinada pura.</span><span class="sxs-lookup"><span data-stu-id="b897e-323">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="b897e-324">Nota: EF 6.x es compatible con las tablas de unión implícita para relaciones de varios a varios, pero el núcleo de EF no lo hace.</span><span class="sxs-lookup"><span data-stu-id="b897e-324">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core does not.</span></span> <span data-ttu-id="b897e-325">Para obtener más información, consulte [-to-many relaciones en EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="b897e-325">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="b897e-326">La entidad CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="b897e-326">The CourseAssignment entity</span></span>

![Entidad CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="b897e-328">Crear *Models/CourseAssignment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b897e-328">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="b897e-329">Cursos de instructor</span><span class="sxs-lookup"><span data-stu-id="b897e-329">Instructor-to-Courses</span></span>

![Instructor a cursos m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="b897e-331">La relación de varios a varios Instructor a cursos:</span><span class="sxs-lookup"><span data-stu-id="b897e-331">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="b897e-332">Requiere una tabla de combinación que debe estar representada por un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="b897e-332">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="b897e-333">Es una tabla combinada pura (tabla sin carga).</span><span class="sxs-lookup"><span data-stu-id="b897e-333">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="b897e-334">Es común para el nombre de una entidad de combinación `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="b897e-334">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="b897e-335">Por ejemplo, la tabla de combinación Instructor a cursos mediante este patrón es `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="b897e-335">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="b897e-336">Sin embargo, se recomienda usar un nombre que describe la relación.</span><span class="sxs-lookup"><span data-stu-id="b897e-336">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="b897e-337">Modelos de datos empiezan de manera sencilla y crecen.</span><span class="sxs-lookup"><span data-stu-id="b897e-337">Data models start out simple and grow.</span></span> <span data-ttu-id="b897e-338">No carga combinaciones (PJTs) evolucionan con frecuencia para incluir la carga.</span><span class="sxs-lookup"><span data-stu-id="b897e-338">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="b897e-339">A partir de un nombre de entidad descriptivo, el nombre no tiene que cambiar cuando cambia de la tabla de combinación.</span><span class="sxs-lookup"><span data-stu-id="b897e-339">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="b897e-340">Idealmente, la entidad de combinación tendrá su propio nombre (posiblemente sola palabra) natural en el dominio de negocio.</span><span class="sxs-lookup"><span data-stu-id="b897e-340">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="b897e-341">Por ejemplo, libros y los clientes pueden vincularse a una entidad de combinación denominada clasificaciones.</span><span class="sxs-lookup"><span data-stu-id="b897e-341">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="b897e-342">La relación de varios a varios Instructor a cursos, `CourseAssignment` es preferible `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="b897e-342">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="b897e-343">Clave compuesta</span><span class="sxs-lookup"><span data-stu-id="b897e-343">Composite key</span></span>

<span data-ttu-id="b897e-344">Claves externas no admiten valores NULL.</span><span class="sxs-lookup"><span data-stu-id="b897e-344">FKs are not nullable.</span></span> <span data-ttu-id="b897e-345">Las claves dos externas en `CourseAssignment` (`InstructorID` y `CourseID`) juntos identifican de forma única cada fila de la `CourseAssignment` tabla.</span><span class="sxs-lookup"><span data-stu-id="b897e-345">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="b897e-346">`CourseAssignment`no requiere un PK dedicado.</span><span class="sxs-lookup"><span data-stu-id="b897e-346">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="b897e-347">El `InstructorID` y `CourseID` propiedades funcionan como un PK compuesto.</span><span class="sxs-lookup"><span data-stu-id="b897e-347">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="b897e-348">Es la única manera de especificar PK compuesto a EF Core con el *API fluida*.</span><span class="sxs-lookup"><span data-stu-id="b897e-348">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="b897e-349">La sección siguiente muestra cómo configurar la PK compuesto.</span><span class="sxs-lookup"><span data-stu-id="b897e-349">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="b897e-350">Se asegura de la clave compuesta:</span><span class="sxs-lookup"><span data-stu-id="b897e-350">The composite key ensures:</span></span>

* <span data-ttu-id="b897e-351">Se permiten varias filas para un curso.</span><span class="sxs-lookup"><span data-stu-id="b897e-351">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="b897e-352">Se permiten varias filas para un instructor.</span><span class="sxs-lookup"><span data-stu-id="b897e-352">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="b897e-353">No se permiten varias filas para el mismo instructor y curso.</span><span class="sxs-lookup"><span data-stu-id="b897e-353">Multiple rows for the same instructor and course is not allowed.</span></span>

<span data-ttu-id="b897e-354">El `Enrollment` combinación entidad define su propio PK, por lo que son posibles duplicados de este tipo.</span><span class="sxs-lookup"><span data-stu-id="b897e-354">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="b897e-355">Para evitar los duplicados:</span><span class="sxs-lookup"><span data-stu-id="b897e-355">To prevent such duplicates:</span></span>

* <span data-ttu-id="b897e-356">Agregar un índice único en los campos de clave externa, o</span><span class="sxs-lookup"><span data-stu-id="b897e-356">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="b897e-357">Configurar `Enrollment` con una clave compuesta principal similar a `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="b897e-357">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="b897e-358">Para obtener más información, consulte [índices](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="b897e-358">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="b897e-359">Actualizar el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="b897e-359">Update the DB context</span></span>

<span data-ttu-id="b897e-360">Agregue el código que aparece resaltado a *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="b897e-360">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="b897e-361">El código anterior agrega las nuevas entidades y configura el `CourseAssignment` PK. compuesto de la entidad</span><span class="sxs-lookup"><span data-stu-id="b897e-361">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="b897e-362">Alternativa de API fluida a atributos</span><span class="sxs-lookup"><span data-stu-id="b897e-362">Fluent API alternative to attributes</span></span>

<span data-ttu-id="b897e-363">El `OnModelCreating` método en el anterior código usa el *API fluida* para configurar el comportamiento básico de EF.</span><span class="sxs-lookup"><span data-stu-id="b897e-363">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="b897e-364">La API se denomina "fluida" porque a menudo se usa por tender una serie de llamadas al método juntos en una única instrucción.</span><span class="sxs-lookup"><span data-stu-id="b897e-364">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="b897e-365">El [sigue código](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) es un ejemplo de la API fluida de:</span><span class="sxs-lookup"><span data-stu-id="b897e-365">The [following code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="b897e-366">En este tutorial, la API fluida se usa solo para la asignación de base de datos que no se puede realizar con atributos.</span><span class="sxs-lookup"><span data-stu-id="b897e-366">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="b897e-367">Sin embargo, la API fluida puede especificar casi todo el formato, la validación y las reglas de asignación que se pueden realizar con atributos.</span><span class="sxs-lookup"><span data-stu-id="b897e-367">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="b897e-368">Algunos atributos como `MinimumLength` no se puede aplicar con la API fluida.</span><span class="sxs-lookup"><span data-stu-id="b897e-368">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="b897e-369">`MinimumLength`no cambia el esquema, sólo se aplica una regla de validación de longitud mínima.</span><span class="sxs-lookup"><span data-stu-id="b897e-369">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="b897e-370">Algunos desarrolladores prefieren usar la API fluida exclusivamente para que pueden mantener las clases de entidad "limpia".</span><span class="sxs-lookup"><span data-stu-id="b897e-370">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="b897e-371">Se pueden mezclar atributos y la API fluida.</span><span class="sxs-lookup"><span data-stu-id="b897e-371">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="b897e-372">Hay algunas configuraciones que solo pueden realizarse con la API fluida (especificar una clave principal compuesta).</span><span class="sxs-lookup"><span data-stu-id="b897e-372">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="b897e-373">Hay algunas configuraciones que solo pueden realizarse con atributos (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="b897e-373">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="b897e-374">La práctica recomendada para el uso de atributos o API fluida:</span><span class="sxs-lookup"><span data-stu-id="b897e-374">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="b897e-375">Elija uno de estos dos enfoques.</span><span class="sxs-lookup"><span data-stu-id="b897e-375">Choose one of these two approaches.</span></span>
* <span data-ttu-id="b897e-376">Usar el método elegido constantemente lo máximo posible.</span><span class="sxs-lookup"><span data-stu-id="b897e-376">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="b897e-377">Algunos de los atributos utilizados en este tutorial se utilizan para:</span><span class="sxs-lookup"><span data-stu-id="b897e-377">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="b897e-378">Validación solo (por ejemplo, `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="b897e-378">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="b897e-379">Solo la configuración de núcleo EF (por ejemplo, `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="b897e-379">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="b897e-380">Configuración de validación y EF principal (por ejemplo, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="b897e-380">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="b897e-381">Para obtener más información acerca de los atributos frente a la API fluida, consulte [métodos de configuración](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="b897e-381">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="b897e-382">Diagrama mostrando relaciones de entidad</span><span class="sxs-lookup"><span data-stu-id="b897e-382">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="b897e-383">En la siguiente ilustración muestra el diagrama que EF Power Tools crear para el modelo School completado.</span><span class="sxs-lookup"><span data-stu-id="b897e-383">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagrama de entidad](complex-data-model/_static/diagram.png)

<span data-ttu-id="b897e-385">Muestra el diagrama anterior:</span><span class="sxs-lookup"><span data-stu-id="b897e-385">The preceding diagram shows:</span></span>

* <span data-ttu-id="b897e-386">Varias líneas de relación de uno a varios (1 a \\*).</span><span class="sxs-lookup"><span data-stu-id="b897e-386">Several one-to-many relationship lines (1 to \\*).</span></span>
* <span data-ttu-id="b897e-387">La línea de relación de uno a cero o uno (1 a 0.. 1) entre el `Instructor` y `OfficeAssignment` entidades.</span><span class="sxs-lookup"><span data-stu-id="b897e-387">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="b897e-388">La línea de relación de cero-o-uno a varios (0.. 1 a \*) entre el `Instructor` y `Department` entidades.</span><span class="sxs-lookup"><span data-stu-id="b897e-388">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="b897e-389">Valor de inicialización de la base de datos con datos de prueba</span><span class="sxs-lookup"><span data-stu-id="b897e-389">Seed the DB with Test Data</span></span>

<span data-ttu-id="b897e-390">Actualice el código en *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="b897e-390">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="b897e-391">El código anterior proporciona datos de inicialización para las nuevas entidades.</span><span class="sxs-lookup"><span data-stu-id="b897e-391">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="b897e-392">La mayor parte de este código crea nuevos objetos de entidad y carga los datos de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b897e-392">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="b897e-393">Los datos de ejemplo se utilizan para pruebas.</span><span class="sxs-lookup"><span data-stu-id="b897e-393">The sample data is used for testing.</span></span> <span data-ttu-id="b897e-394">El código anterior crea las siguientes relaciones de varios a varios:</span><span class="sxs-lookup"><span data-stu-id="b897e-394">The preceding code creates the following many-to-many relationships:</span></span>

* `Enrollments`
* `CourseAssignment`

<span data-ttu-id="b897e-395">Nota: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) admitirá [propagación de datos](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span><span class="sxs-lookup"><span data-stu-id="b897e-395">Note: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) will support [data seeding](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="b897e-396">Agregar una migración</span><span class="sxs-lookup"><span data-stu-id="b897e-396">Add a migration</span></span>

<span data-ttu-id="b897e-397">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="b897e-397">Build the project.</span></span> <span data-ttu-id="b897e-398">Abra una ventana de comandos en la carpeta de proyecto y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="b897e-398">Open a command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="b897e-399">El comando anterior muestra una advertencia sobre la posible pérdida de datos.</span><span class="sxs-lookup"><span data-stu-id="b897e-399">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="b897e-400">Si el `database update` se ejecuta el comando, se genera el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="b897e-400">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="b897e-401">Cuando se ejecutan migraciones con los datos existentes, puede haber restricciones de clave externa que no están satisfecho con los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="b897e-401">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="b897e-402">Para este tutorial, se crea una nueva base de datos, por lo que no hay ningún infracciones de restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="b897e-402">For this tutorial, a new DB is created, so there are no FK constraint violations.</span></span> <span data-ttu-id="b897e-403">Vea [corregir las restricciones foreign key con datos heredados](#fk) para obtener instrucciones sobre cómo corregir las infracciones de clave externa en la base de datos actual.</span><span class="sxs-lookup"><span data-stu-id="b897e-403">See [Fixing foreign key constraints with legacy data](#fk) for instructions on how to fix the FK violations on the current DB.</span></span>

## <a name="change-the-connection-string-and-update-the-db"></a><span data-ttu-id="b897e-404">Cambie la cadena de conexión y actualizar la base de datos</span><span class="sxs-lookup"><span data-stu-id="b897e-404">Change the connection string and update the DB</span></span>

<span data-ttu-id="b897e-405">El código en la sección actualizada `DbInitializer` agrega los datos de inicialización para las nuevas entidades.</span><span class="sxs-lookup"><span data-stu-id="b897e-405">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="b897e-406">Para forzar el núcleo de EF para crear una nueva base de datos vacía:</span><span class="sxs-lookup"><span data-stu-id="b897e-406">To force EF Core to create a new empty DB:</span></span>

* <span data-ttu-id="b897e-407">Cambiar el nombre de cadena de conexión de base de datos en *appSettings.JSON que se* a ContosoUniversity3.</span><span class="sxs-lookup"><span data-stu-id="b897e-407">Change the DB connection string name in *appsettings.json* to ContosoUniversity3.</span></span> <span data-ttu-id="b897e-408">El nuevo nombre debe ser un nombre que no se ha usado en el equipo.</span><span class="sxs-lookup"><span data-stu-id="b897e-408">The new name must be a name that hasn't been used on the computer.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* <span data-ttu-id="b897e-409">O bien, elimine la base de datos utilizando:</span><span class="sxs-lookup"><span data-stu-id="b897e-409">Alternatively, delete the DB using:</span></span>

    * <span data-ttu-id="b897e-410">**Explorador de objetos SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="b897e-410">**SQL Server Object Explorer** (SSOX).</span></span>
    * <span data-ttu-id="b897e-411">El `database drop` comando de CLI:</span><span class="sxs-lookup"><span data-stu-id="b897e-411">The `database drop` CLI command:</span></span>

   ```console
   dotnet ef database drop
   ```

<span data-ttu-id="b897e-412">Ejecute `database update` en la ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="b897e-412">Run `database update` in the command window:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="b897e-413">El comando anterior ejecuta todas las migraciones.</span><span class="sxs-lookup"><span data-stu-id="b897e-413">The preceding command runs all the migrations.</span></span>

<span data-ttu-id="b897e-414">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b897e-414">Run the app.</span></span> <span data-ttu-id="b897e-415">Ejecuta la aplicación se ejecuta el `DbInitializer.Initialize` método.</span><span class="sxs-lookup"><span data-stu-id="b897e-415">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="b897e-416">El `DbInitializer.Initialize` rellena la base de datos nueva.</span><span class="sxs-lookup"><span data-stu-id="b897e-416">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="b897e-417">Abra la base de datos en SSOX:</span><span class="sxs-lookup"><span data-stu-id="b897e-417">Open the DB in SSOX:</span></span>

* <span data-ttu-id="b897e-418">Expanda el **tablas** nodo.</span><span class="sxs-lookup"><span data-stu-id="b897e-418">Expand the **Tables** node.</span></span> <span data-ttu-id="b897e-419">Se muestran las tablas creadas.</span><span class="sxs-lookup"><span data-stu-id="b897e-419">The created tables are displayed.</span></span>
* <span data-ttu-id="b897e-420">Si SSOX se abrió previamente, haga clic en el **actualizar** botón.</span><span class="sxs-lookup"><span data-stu-id="b897e-420">If SSOX was opened previously, click the **Refresh** button.</span></span>

![Tablas de SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="b897e-422">Examine la **CourseAssignment** tabla:</span><span class="sxs-lookup"><span data-stu-id="b897e-422">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="b897e-423">Haga clic en el **CourseAssignment** de tabla y seleccione **ver datos**.</span><span class="sxs-lookup"><span data-stu-id="b897e-423">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="b897e-424">Compruebe el **CourseAssignment** tabla contiene datos.</span><span class="sxs-lookup"><span data-stu-id="b897e-424">Verify the **CourseAssignment** table contains data.</span></span>

![Datos de CourseAssignment en SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a><span data-ttu-id="b897e-426">Corregir las restricciones foreign key con datos heredados</span><span class="sxs-lookup"><span data-stu-id="b897e-426">Fixing foreign key constraints with legacy data</span></span>

<span data-ttu-id="b897e-427">Esta sección es opcional.</span><span class="sxs-lookup"><span data-stu-id="b897e-427">This section is optional.</span></span>

<span data-ttu-id="b897e-428">Cuando se ejecutan migraciones con los datos existentes, puede haber restricciones de clave externa que no están satisfecho con los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="b897e-428">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="b897e-429">Con los datos de producción, se deben realizar los pasos para migrar los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="b897e-429">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="b897e-430">En esta sección se proporciona un ejemplo de corrección de las infracciones de restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="b897e-430">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="b897e-431">No realice estos cambios de código sin una copia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="b897e-431">Don't make these code changes without a backup.</span></span> <span data-ttu-id="b897e-432">No realice estos cambios de código si realizó la sección anterior y actualiza la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b897e-432">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="b897e-433">El *{timestamp}_ComplexDataModel.cs* archivo contiene el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="b897e-433">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="b897e-434">El código anterior agrega un acepta valores NULL `DepartmentID` FK a la `Course` tabla.</span><span class="sxs-lookup"><span data-stu-id="b897e-434">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="b897e-435">La base de datos desde el tutorial anterior contiene filas de `Course`, por lo que no se puede actualizar esa tabla por las migraciones.</span><span class="sxs-lookup"><span data-stu-id="b897e-435">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="b897e-436">Para realizar la `ComplexDataModel` trabajo de migración con los datos existentes:</span><span class="sxs-lookup"><span data-stu-id="b897e-436">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="b897e-437">Cambiar el código para asignar a la nueva columna (`DepartmentID`) un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="b897e-437">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="b897e-438">Cree un departamento falso denominado "Temp" para que actúe como el departamento de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b897e-438">Create a fake department named "Temp" to act as the default department.</span></span>

### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="b897e-439">Corregir las restricciones foreign key</span><span class="sxs-lookup"><span data-stu-id="b897e-439">Fix the foreign key constraints</span></span>

<span data-ttu-id="b897e-440">Actualización de la `ComplexDataModel` clases `Up` método:</span><span class="sxs-lookup"><span data-stu-id="b897e-440">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="b897e-441">Abra la *{timestamp}_ComplexDataModel.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="b897e-441">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="b897e-442">Comentario de la línea de código que agrega el `DepartmentID` columna a la `Course` tabla.</span><span class="sxs-lookup"><span data-stu-id="b897e-442">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="b897e-443">Agregue el código resaltado siguiente.</span><span class="sxs-lookup"><span data-stu-id="b897e-443">Add the following highlighted code.</span></span> <span data-ttu-id="b897e-444">El nuevo código va después de la `.CreateTable( name: "Department"` bloque:[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="b897e-444">The new code goes after the `.CreateTable( name: "Department"` block: [!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="b897e-445">Con los cambios anteriores, existente `Course` filas estarán relacionadas con el departamento "Temp" después de la `ComplexDataModel` `Up` ejecuciones de método.</span><span class="sxs-lookup"><span data-stu-id="b897e-445">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="b897e-446">Una aplicación de producción necesario lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b897e-446">A production app would:</span></span>

* <span data-ttu-id="b897e-447">Incluir código o secuencias de comandos para agregar `Department` filas y relacionados con `Course` filas a la nueva `Department` filas.</span><span class="sxs-lookup"><span data-stu-id="b897e-447">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="b897e-448">No se utilizaría el departamento "Temp" o el valor predeterminado de `Course.DepartmentID `.</span><span class="sxs-lookup"><span data-stu-id="b897e-448">Would not use the "Temp" department or the default value for `Course.DepartmentID `.</span></span>

<span data-ttu-id="b897e-449">El siguiente tutorial trata los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="b897e-449">The next tutorial covers related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b897e-450">[Anterior](xref:data/ef-rp/migrations)
[Siguiente](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="b897e-450">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>