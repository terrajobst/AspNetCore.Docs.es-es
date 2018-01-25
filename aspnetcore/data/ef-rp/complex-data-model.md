---
title: "Páginas de Razor con EF Core - modelo de datos - 5 de 8"
author: rick-anderson
description: "En este tutorial agregará más entidades y relaciones y personalizar el modelo de datos mediante la especificación de formato, validación y reglas de asignación de la base de datos."
ms.author: riande
manager: wpickett
ms.date: 10/25/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 2446f4734e9bb1ab6829001f6e7888c4c14ee1b7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a><span data-ttu-id="3a976-103">Crear un modelo de datos complejos - Core EF con el tutorial de las páginas de Razor (5 de 8)</span><span class="sxs-lookup"><span data-stu-id="3a976-103">Creating a complex data model - EF Core with Razor Pages tutorial (5 of 8)</span></span>

<span data-ttu-id="3a976-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3a976-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="3a976-105">Los tutoriales anteriores ha trabajado con un modelo de datos básicos que se compone de tres entidades.</span><span class="sxs-lookup"><span data-stu-id="3a976-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="3a976-106">En este tutorial:</span><span class="sxs-lookup"><span data-stu-id="3a976-106">In this tutorial:</span></span>

* <span data-ttu-id="3a976-107">Se agregan más entidades y relaciones.</span><span class="sxs-lookup"><span data-stu-id="3a976-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="3a976-108">El modelo de datos se ha personalizado mediante la especificación de formato, validación y reglas de asignación de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3a976-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="3a976-109">Las clases de entidad para el modelo de datos completa se muestra en la siguiente ilustración:</span><span class="sxs-lookup"><span data-stu-id="3a976-109">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Diagrama de entidad](complex-data-model/_static/diagram.png)

<span data-ttu-id="3a976-111">Si experimenta problemas no puede resolver, descargue el [aplicación final de esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span><span class="sxs-lookup"><span data-stu-id="3a976-111">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="3a976-112">Personalizar el modelo de datos con atributos</span><span class="sxs-lookup"><span data-stu-id="3a976-112">Customize the data model with attributes</span></span>

<span data-ttu-id="3a976-113">En esta sección, el modelo de datos para personalizar el uso de atributos.</span><span class="sxs-lookup"><span data-stu-id="3a976-113">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="3a976-114">El atributo de tipo de datos</span><span class="sxs-lookup"><span data-stu-id="3a976-114">The DataType attribute</span></span>

<span data-ttu-id="3a976-115">Las páginas de estudiante actualmente muestra la hora de la fecha de inscripción.</span><span class="sxs-lookup"><span data-stu-id="3a976-115">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="3a976-116">Normalmente, los campos de fecha muestran solo la fecha y no la hora.</span><span class="sxs-lookup"><span data-stu-id="3a976-116">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="3a976-117">Actualización *Models/Student.cs* aparece resaltado este código con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a976-117">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="3a976-118">El [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) atributo especifica un tipo de datos que es más específico que el tipo intrínseco de base de datos.</span><span class="sxs-lookup"><span data-stu-id="3a976-118">The [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="3a976-119">En este caso que se debe mostrar solo la fecha, no la fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="3a976-119">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="3a976-120">El [enumeración DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) proporciona para muchos tipos de datos, como fecha, hora, número de teléfono, moneda, EmailAddress, etcetera. El `DataType` atributo también puede permitir que la aplicación proporcionar automáticamente las características específicas del tipo.</span><span class="sxs-lookup"><span data-stu-id="3a976-120">The [DataType Enumeration](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="3a976-121">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="3a976-121">For example:</span></span>

* <span data-ttu-id="3a976-122">El `mailto:` vínculo se crea automáticamente para `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="3a976-122">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="3a976-123">El selector de fecha se proporciona para `DataType.Date` en la mayoría de los exploradores.</span><span class="sxs-lookup"><span data-stu-id="3a976-123">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="3a976-124">El `DataType` atributo emite HTML 5 `data-` atributos (pronunciado datos dash) que usan los exploradores HTML 5.</span><span class="sxs-lookup"><span data-stu-id="3a976-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="3a976-125">El `DataType` atributos no proporcionan la validación.</span><span class="sxs-lookup"><span data-stu-id="3a976-125">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="3a976-126">`DataType.Date`no se especifica el formato de la fecha en que se muestra.</span><span class="sxs-lookup"><span data-stu-id="3a976-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="3a976-127">De forma predeterminada, el campo de fecha se muestra según los formatos predeterminados en función del servidor [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span><span class="sxs-lookup"><span data-stu-id="3a976-127">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="3a976-128">El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:</span><span class="sxs-lookup"><span data-stu-id="3a976-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="3a976-129">El `ApplyFormatInEditMode` configuración especifica que el formato debe también se aplica a la interfaz de usuario de edición.</span><span class="sxs-lookup"><span data-stu-id="3a976-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="3a976-130">Algunos campos no deben usar `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="3a976-130">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="3a976-131">Por ejemplo, el símbolo de moneda generalmente no debe mostrarse en un cuadro de texto de edición.</span><span class="sxs-lookup"><span data-stu-id="3a976-131">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="3a976-132">El `DisplayFormat` atributo puede usarse por sí mismo.</span><span class="sxs-lookup"><span data-stu-id="3a976-132">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="3a976-133">Suele ser una buena idea usar el `DataType` atribuir a la `DisplayFormat` atributo.</span><span class="sxs-lookup"><span data-stu-id="3a976-133">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="3a976-134">El `DataType` atributo transmite la semántica de los datos en lugar de cómo representar en una pantalla.</span><span class="sxs-lookup"><span data-stu-id="3a976-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="3a976-135">El `DataType` atributo proporciona las siguientes ventajas que no están disponibles en `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="3a976-135">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="3a976-136">El explorador puede habilitar características de HTML5.</span><span class="sxs-lookup"><span data-stu-id="3a976-136">The browser can enable HTML5 features.</span></span> <span data-ttu-id="3a976-137">Por ejemplo, mostrar un control de calendario, el símbolo de moneda adecuado para la configuración regional, vínculos de correo electrónico, validación de entrada del lado cliente, etcetera.</span><span class="sxs-lookup"><span data-stu-id="3a976-137">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="3a976-138">De forma predeterminada, el explorador representa datos con el formato correcto según la configuración regional.</span><span class="sxs-lookup"><span data-stu-id="3a976-138">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="3a976-139">Para obtener más información, consulte el [ \<entrada > documentación de la aplicación auxiliar de etiqueta](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="3a976-139">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="3a976-140">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3a976-140">Run the app.</span></span> <span data-ttu-id="3a976-141">Navegue a la página de índice de los alumnos.</span><span class="sxs-lookup"><span data-stu-id="3a976-141">Navigate to the Students Index page.</span></span> <span data-ttu-id="3a976-142">Veces ya no se muestran.</span><span class="sxs-lookup"><span data-stu-id="3a976-142">Times are no longer displayed.</span></span> <span data-ttu-id="3a976-143">Todas las vistas que usa el `Student` modelo muestra la fecha sin hora.</span><span class="sxs-lookup"><span data-stu-id="3a976-143">Every view that uses the `Student` model displays the date without time.</span></span>

![Página de índice de los alumnos mostrando las fechas sin veces](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="3a976-145">El atributo StringLength</span><span class="sxs-lookup"><span data-stu-id="3a976-145">The StringLength attribute</span></span>

<span data-ttu-id="3a976-146">Reglas de validación de datos y mensajes de error de validación pueden especificarse con atributos.</span><span class="sxs-lookup"><span data-stu-id="3a976-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="3a976-147">El [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) atributo especifica la longitud mínima y máxima de caracteres que se permiten en un campo de datos.</span><span class="sxs-lookup"><span data-stu-id="3a976-147">The [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="3a976-148">El `StringLength` atributo también proporciona validación de cliente y servidor.</span><span class="sxs-lookup"><span data-stu-id="3a976-148">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="3a976-149">El valor mínimo no influye en el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="3a976-149">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="3a976-150">Actualización de la `Student` modelo con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a976-150">Update the `Student` model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="3a976-151">El código anterior limita los nombres a no más de 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="3a976-151">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="3a976-152">El `StringLength` atributo no impide que un usuario entraran en un espacio en blanco para un nombre.</span><span class="sxs-lookup"><span data-stu-id="3a976-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="3a976-153">El [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) atributo se usa para aplicar restricciones a la entrada.</span><span class="sxs-lookup"><span data-stu-id="3a976-153">The [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="3a976-154">Por ejemplo, el siguiente código requiere el primer carácter que se va letra mayúscula y el resto de caracteres sea alfabético:</span><span class="sxs-lookup"><span data-stu-id="3a976-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="3a976-155">Ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="3a976-155">Run the app:</span></span>

* <span data-ttu-id="3a976-156">Navegue a la página de los alumnos.</span><span class="sxs-lookup"><span data-stu-id="3a976-156">Navigate to the Students page.</span></span>
* <span data-ttu-id="3a976-157">Seleccione **crear nuevo**y escriba un nombre más de 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="3a976-157">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="3a976-158">Seleccione **crear**, validación del lado cliente muestra un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="3a976-158">Select **Create**, client-side validation shows an error message.</span></span>

![Página que muestra los errores de longitud de cadena de índice de estudiantes](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="3a976-160">En **Explorador de objetos de SQL Server** (SSOX), abra el Diseñador de tablas de Student haciendo doble clic en el **estudiante** tabla.</span><span class="sxs-lookup"><span data-stu-id="3a976-160">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabla de estudiantes en SSOX antes de las migraciones](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="3a976-162">La imagen anterior muestra el esquema para el `Student` tabla.</span><span class="sxs-lookup"><span data-stu-id="3a976-162">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="3a976-163">Los campos de nombre tengan tipo `nvarchar(MAX)` porque las migraciones no se ha ejecutado en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3a976-163">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="3a976-164">Cuando se ejecutan las migraciones más adelante en este tutorial, se convierten en los campos de nombre `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="3a976-164">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="3a976-165">El atributo de columna</span><span class="sxs-lookup"><span data-stu-id="3a976-165">The Column attribute</span></span>

<span data-ttu-id="3a976-166">Atributos pueden controlar cómo se asignan las clases y propiedades de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3a976-166">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="3a976-167">En esta sección, el `Column` atributo se utiliza para asignar el nombre de la `FirstMidName` propiedad en "FirstName" en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3a976-167">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="3a976-168">Cuando se crea la base de datos, los nombres de propiedad en el modelo se usan para los nombres de columna (excepto cuando la `Column` atributo se usa).</span><span class="sxs-lookup"><span data-stu-id="3a976-168">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="3a976-169">El `Student` modelo utiliza `FirstMidName` para el nombre de campo ya que el campo puede contener también un segundo nombre.</span><span class="sxs-lookup"><span data-stu-id="3a976-169">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="3a976-170">Actualización de la *Student.cs* archivo con el código resaltado siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a976-170">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="3a976-171">Con el cambio anterior, `Student.FirstMidName` en la aplicación se asigna a la `FirstName` columna de la `Student` tabla.</span><span class="sxs-lookup"><span data-stu-id="3a976-171">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="3a976-172">La adición de la `Column` atributo cambia la copia de seguridad de modelo la `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="3a976-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="3a976-173">La copia de seguridad de modelo el `SchoolContext` ya no coincide con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3a976-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="3a976-174">Si la aplicación se ejecuta antes de aplicar las migraciones, se genera la siguiente excepción:</span><span class="sxs-lookup"><span data-stu-id="3a976-174">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```
<span data-ttu-id="3a976-175">Para actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="3a976-175">To update the DB:</span></span>

* <span data-ttu-id="3a976-176">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="3a976-176">Build the project.</span></span>
* <span data-ttu-id="3a976-177">Abra una ventana de comandos en la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="3a976-177">Open a command window in the project folder.</span></span> <span data-ttu-id="3a976-178">Escriba los comandos siguientes para crear una nueva migración y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="3a976-178">Enter the following commands to create a new migration and update the DB:</span></span>

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

<span data-ttu-id="3a976-179">El `dotnet ef migrations add ColumnFirstName` comando genera el mensaje de advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a976-179">The `dotnet ef migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="3a976-180">Se genera la advertencia porque los campos de nombre ahora están limitadas a 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="3a976-180">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="3a976-181">Si un nombre en la base de datos tenía más de 50 caracteres, se perderían el 51 hasta el último carácter.</span><span class="sxs-lookup"><span data-stu-id="3a976-181">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="3a976-182">Probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3a976-182">Test the app.</span></span>

<span data-ttu-id="3a976-183">Abra la tabla de estudiantes en SSOX:</span><span class="sxs-lookup"><span data-stu-id="3a976-183">Open the Student table in SSOX:</span></span>

![Tabla de estudiantes en SSOX después de las migraciones](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="3a976-185">Antes de que se ha aplicado la migración, las columnas del nombre eran del tipo [nvarchar (max)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="3a976-185">Before migration was applied, the name columns were of type [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="3a976-186">Las columnas de nombre son ahora `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="3a976-186">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="3a976-187">Ha cambiado el nombre de columna de `FirstMidName` a `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="3a976-187">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="3a976-188">En la sección siguiente, la creación de la aplicación en algunas de las fases genera errores del compilador.</span><span class="sxs-lookup"><span data-stu-id="3a976-188">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="3a976-189">Las instrucciones de especifican cuándo se deben compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3a976-189">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="3a976-190">Actualizar la entidad Student</span><span class="sxs-lookup"><span data-stu-id="3a976-190">Student entity update</span></span>

![Entidad Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="3a976-192">Actualización *Models/Student.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a976-192">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="3a976-193">El atributo requerido</span><span class="sxs-lookup"><span data-stu-id="3a976-193">The Required attribute</span></span>

<span data-ttu-id="3a976-194">El `Required` atributo hace que los campos obligatorios de propiedades de nombre.</span><span class="sxs-lookup"><span data-stu-id="3a976-194">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="3a976-195">El `Required` atributo no es necesario para los tipos que no aceptan valores NULL, como tipos de valor (`DateTime`, `int`, `double`, etcetera.).</span><span class="sxs-lookup"><span data-stu-id="3a976-195">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="3a976-196">Tipos que no pueden ser nulos se tratan automáticamente como campos obligatorios.</span><span class="sxs-lookup"><span data-stu-id="3a976-196">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="3a976-197">El `Required` atributo se podría reemplazar con un parámetro de longitud mínima en el `StringLength` atributo:</span><span class="sxs-lookup"><span data-stu-id="3a976-197">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="3a976-198">El atributo de visualización</span><span class="sxs-lookup"><span data-stu-id="3a976-198">The Display attribute</span></span>

<span data-ttu-id="3a976-199">El `Display` atributo especifica que el título de los cuadros de texto debe ser "Nombre", "Last Name", "Nombre completo" y "Fecha de inscripción".</span><span class="sxs-lookup"><span data-stu-id="3a976-199">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="3a976-200">Los títulos predeterminados que no tenían ningún espacio al dividir las palabras, por ejemplo "apellido".</span><span class="sxs-lookup"><span data-stu-id="3a976-200">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="3a976-201">La propiedad FullName calculado</span><span class="sxs-lookup"><span data-stu-id="3a976-201">The FullName calculated property</span></span>

<span data-ttu-id="3a976-202">`FullName`es una propiedad calculada que devuelve un valor que se crea concatenando dos otras propiedades.</span><span class="sxs-lookup"><span data-stu-id="3a976-202">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="3a976-203">`FullName`no se puede establecer, tiene solo un descriptor de acceso get.</span><span class="sxs-lookup"><span data-stu-id="3a976-203">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="3a976-204">Ya no `FullName` columna se crea en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3a976-204">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="3a976-205">Crear la entidad Instructor</span><span class="sxs-lookup"><span data-stu-id="3a976-205">Create the Instructor Entity</span></span>

![Entidad instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="3a976-207">Crear *Models/Instructor.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a976-207">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="3a976-208">Tenga en cuenta que varias propiedades son los mismos en el `Student` y `Instructor` entidades.</span><span class="sxs-lookup"><span data-stu-id="3a976-208">Notice that several properties are the same in the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="3a976-209">En el tutorial de herencia de implementación más adelante en esta serie, este código se refactoriza para eliminar la redundancia.</span><span class="sxs-lookup"><span data-stu-id="3a976-209">In the Implementing Inheritance tutorial later in this series, this code is refactored to eliminate the redundancy.</span></span>

<span data-ttu-id="3a976-210">Varios atributos pueden estar en una sola línea.</span><span class="sxs-lookup"><span data-stu-id="3a976-210">Multiple attributes can be on one line.</span></span> <span data-ttu-id="3a976-211">El `HireDate` atributos pudieron escribirse de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a976-211">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="3a976-212">Las propiedades de navegación CourseAssignments y OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="3a976-212">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="3a976-213">El `CourseAssignments` y `OfficeAssignment` las propiedades son propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="3a976-213">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="3a976-214">Un instructor puede enseñar a cualquier número de cursos, por lo que `CourseAssignments` se define como una colección.</span><span class="sxs-lookup"><span data-stu-id="3a976-214">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="3a976-215">Si una propiedad de navegación contiene varias entidades:</span><span class="sxs-lookup"><span data-stu-id="3a976-215">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="3a976-216">Debe ser un tipo de lista, donde pueden agregar, eliminar y actualizar las entradas.</span><span class="sxs-lookup"><span data-stu-id="3a976-216">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="3a976-217">Los tipos de propiedad de navegación incluyen:</span><span class="sxs-lookup"><span data-stu-id="3a976-217">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="3a976-218">Si `ICollection<T>` se especifica, Core EF crea un `HashSet<T>` colección de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="3a976-218">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="3a976-219">El `CourseAssignment` entidad se explica en la sección acerca de las relaciones de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="3a976-219">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="3a976-220">Estado que un instructor puede tener a lo sumo una oficina de reglas de negocios de la Universidad de contoso.</span><span class="sxs-lookup"><span data-stu-id="3a976-220">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="3a976-221">El `OfficeAssignment` propiedad contiene un único `OfficeAssignment` entidad.</span><span class="sxs-lookup"><span data-stu-id="3a976-221">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="3a976-222">`OfficeAssignment`es null si no se asigna ningún office.</span><span class="sxs-lookup"><span data-stu-id="3a976-222">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="3a976-223">Crear la entidad OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="3a976-223">Create the OfficeAssignment entity</span></span>

![Entidad OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="3a976-225">Crear *Models/OfficeAssignment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a976-225">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="3a976-226">Key (atributo)</span><span class="sxs-lookup"><span data-stu-id="3a976-226">The Key attribute</span></span>

<span data-ttu-id="3a976-227">El `[Key]` atributo se usa para identificar una propiedad como la clave principal (PK) cuando el nombre de propiedad es algo que no sean classnameID o identificador.</span><span class="sxs-lookup"><span data-stu-id="3a976-227">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="3a976-228">Hay una relación de uno a cero o uno entre la `Instructor` y `OfficeAssignment` entidades.</span><span class="sxs-lookup"><span data-stu-id="3a976-228">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="3a976-229">Solo existe una asignación de oficina en relación con el instructor se asigna a.</span><span class="sxs-lookup"><span data-stu-id="3a976-229">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="3a976-230">El `OfficeAssignment` PK también es la clave externa (FK) a la `Instructor` entidad.</span><span class="sxs-lookup"><span data-stu-id="3a976-230">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="3a976-231">El núcleo de EF no se reconoce automáticamente `InstructorID` como la clave principal de `OfficeAssignment` porque:</span><span class="sxs-lookup"><span data-stu-id="3a976-231">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="3a976-232">`InstructorID`no siga la convención de nomenclatura de Id. o classnameID.</span><span class="sxs-lookup"><span data-stu-id="3a976-232">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="3a976-233">Por lo tanto, la `Key` atributo se usa para identificar `InstructorID` como la clave principal:</span><span class="sxs-lookup"><span data-stu-id="3a976-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="3a976-234">De forma predeterminada, Core EF trata la clave como no generada por base de datos porque la columna es para una relación de identificación.</span><span class="sxs-lookup"><span data-stu-id="3a976-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="3a976-235">La propiedad de navegación de Instructor</span><span class="sxs-lookup"><span data-stu-id="3a976-235">The Instructor navigation property</span></span>

<span data-ttu-id="3a976-236">El `OfficeAssignment` propiedad de navegación para el `Instructor` entidad es que acepta valores NULL porque:</span><span class="sxs-lookup"><span data-stu-id="3a976-236">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="3a976-237">Tipos de referencia (como clases admiten valores NULL).</span><span class="sxs-lookup"><span data-stu-id="3a976-237">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="3a976-238">Un instructor podría no tener una asignación de oficina.</span><span class="sxs-lookup"><span data-stu-id="3a976-238">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="3a976-239">El `OfficeAssignment` entidad tiene un acepta valores NULL `Instructor` propiedad de navegación porque:</span><span class="sxs-lookup"><span data-stu-id="3a976-239">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="3a976-240">`InstructorID`es que no aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="3a976-240">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="3a976-241">Una asignación de office no puede existir sin un instructor.</span><span class="sxs-lookup"><span data-stu-id="3a976-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="3a976-242">Cuando un `Instructor` entidad tiene un relacionados `OfficeAssignment` entidad, cada entidad tiene una referencia a la otra en la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="3a976-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="3a976-243">El `[Required]` atributo puede aplicarse a la `Instructor` propiedad de navegación:</span><span class="sxs-lookup"><span data-stu-id="3a976-243">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="3a976-244">El código anterior especifica que debe haber un instructor relacionado.</span><span class="sxs-lookup"><span data-stu-id="3a976-244">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="3a976-245">El código anterior no es necesario porque el `InstructorID` clave externa (que también es la clave principal) es que no aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="3a976-245">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="3a976-246">Modificar la entidad de curso</span><span class="sxs-lookup"><span data-stu-id="3a976-246">Modify the Course Entity</span></span>

![Entidad de curso](complex-data-model/_static/course-entity.png)

<span data-ttu-id="3a976-248">Actualización *Models/Course.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a976-248">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="3a976-249">El `Course` entidad tiene una propiedad de clave externa (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="3a976-249">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="3a976-250">`DepartmentID`apunta a relacionado `Department` entidad.</span><span class="sxs-lookup"><span data-stu-id="3a976-250">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="3a976-251">El `Course` entidad tiene un `Department` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="3a976-251">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="3a976-252">Núcleo EF no requiere una propiedad de clave externa para un modelo de datos cuando el modelo tiene una propiedad de navegación para una entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="3a976-252">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="3a976-253">Núcleo EF crea automáticamente claves externas en la base de datos siempre que se necesiten.</span><span class="sxs-lookup"><span data-stu-id="3a976-253">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="3a976-254">Núcleo EF crea [sombrear propiedades](https://docs.microsoft.com/ef/core/modeling/shadow-properties) para claves externas creadas automáticamente.</span><span class="sxs-lookup"><span data-stu-id="3a976-254">EF Core creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="3a976-255">Con la clave externa en el modelo de datos puede realizar actualizaciones más sencillo y más eficaz.</span><span class="sxs-lookup"><span data-stu-id="3a976-255">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="3a976-256">Por ejemplo, considere la posibilidad de un modelo donde la propiedad FK `DepartmentID` es *no* incluido.</span><span class="sxs-lookup"><span data-stu-id="3a976-256">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="3a976-257">Cuando se captura una entidad de curso para editar:</span><span class="sxs-lookup"><span data-stu-id="3a976-257">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="3a976-258">El `Department` entity es null si no explícitamente está cargado.</span><span class="sxs-lookup"><span data-stu-id="3a976-258">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="3a976-259">Para actualizar la entidad de curso, la `Department` en primer lugar debe buscarse en entidad.</span><span class="sxs-lookup"><span data-stu-id="3a976-259">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="3a976-260">Cuando la propiedad FK `DepartmentID` se incluye en el modelo de datos, no es necesario para capturar el `Department` entidad antes de una actualización.</span><span class="sxs-lookup"><span data-stu-id="3a976-260">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="3a976-261">El atributo DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="3a976-261">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="3a976-262">El `[DatabaseGenerated(DatabaseGeneratedOption.None)]` atributo especifica que la clave principal es proporcionada por la aplicación, en lugar de generado por la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3a976-262">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="3a976-263">De forma predeterminada, EF Core se da por supuesto que la base de datos genera valores de clave principal.</span><span class="sxs-lookup"><span data-stu-id="3a976-263">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="3a976-264">Base de datos genera PK valores suele ser el mejor método.</span><span class="sxs-lookup"><span data-stu-id="3a976-264">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="3a976-265">Para `Course` entidades, el usuario especifica la PK.</span><span class="sxs-lookup"><span data-stu-id="3a976-265">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="3a976-266">Por ejemplo, un número de curso como una serie de 1000 departamento matemáticas, una serie de 2000 para el departamento de inglés.</span><span class="sxs-lookup"><span data-stu-id="3a976-266">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="3a976-267">El `DatabaseGenerated` atributo también se puede usar para generar valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="3a976-267">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="3a976-268">Por ejemplo, la base de datos puede generar automáticamente un campo de fecha para registrar la fecha en que se crea o actualiza una fila.</span><span class="sxs-lookup"><span data-stu-id="3a976-268">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="3a976-269">Para obtener más información, consulte [genera propiedades](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="3a976-269">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="3a976-270">Propiedades de navegación y de clave externa</span><span class="sxs-lookup"><span data-stu-id="3a976-270">Foreign key and navigation properties</span></span>

<span data-ttu-id="3a976-271">Las propiedades de clave externa (FK) y propiedades de navegación de la `Course` entidad reflejen las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="3a976-271">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="3a976-272">Un curso se asigna a un departamento, por lo que hay un `DepartmentID` FK y un `Department` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="3a976-272">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="3a976-273">Un curso puede tener cualquier número de alumnos inscritos en él, por lo que el `Enrollments` propiedad de navegación es una colección:</span><span class="sxs-lookup"><span data-stu-id="3a976-273">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="3a976-274">Un curso puede ser imparten por varios instructores, por lo que el `CourseAssignments` propiedad de navegación es una colección:</span><span class="sxs-lookup"><span data-stu-id="3a976-274">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="3a976-275">`CourseAssignment`se explica [más adelante](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="3a976-275">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="3a976-276">Crear la entidad Department</span><span class="sxs-lookup"><span data-stu-id="3a976-276">Create the Department entity</span></span>

![Entidad Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="3a976-278">Crear *Models/Department.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a976-278">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="3a976-279">El atributo de columna</span><span class="sxs-lookup"><span data-stu-id="3a976-279">The Column attribute</span></span>

<span data-ttu-id="3a976-280">Anteriormente la `Column` atributo se utiliza para cambiar la asignación de nombre de columna.</span><span class="sxs-lookup"><span data-stu-id="3a976-280">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="3a976-281">En el código de la `Department` entidad, el `Column` atributo se utiliza para cambiar la asignación de tipos de datos SQL.</span><span class="sxs-lookup"><span data-stu-id="3a976-281">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="3a976-282">La `Budget` columna se define mediante el tipo de moneda de SQL Server en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="3a976-282">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="3a976-283">Asignación de columnas por lo general no es necesaria.</span><span class="sxs-lookup"><span data-stu-id="3a976-283">Column mapping is generally not required.</span></span> <span data-ttu-id="3a976-284">Núcleo EF generalmente elige el tipo de datos de SQL Server apropiado en función del tipo CLR para la propiedad.</span><span class="sxs-lookup"><span data-stu-id="3a976-284">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="3a976-285">El CLR `decimal` tipo se asigna a un servidor SQL Server `decimal` tipo.</span><span class="sxs-lookup"><span data-stu-id="3a976-285">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="3a976-286">`Budget`es para la moneda, y el tipo de datos de moneda es más adecuado para la moneda.</span><span class="sxs-lookup"><span data-stu-id="3a976-286">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="3a976-287">Propiedades de navegación y de clave externa</span><span class="sxs-lookup"><span data-stu-id="3a976-287">Foreign key and navigation properties</span></span>

<span data-ttu-id="3a976-288">Las propiedades de navegación y FK reflejan las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="3a976-288">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="3a976-289">Un departamento puede tener o no un administrador.</span><span class="sxs-lookup"><span data-stu-id="3a976-289">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="3a976-290">Un administrador es siempre un instructor.</span><span class="sxs-lookup"><span data-stu-id="3a976-290">An administrator is always an instructor.</span></span> <span data-ttu-id="3a976-291">Por lo tanto, la `InstructorID` propiedad se incluye como la clave externa a la `Instructor` entidad.</span><span class="sxs-lookup"><span data-stu-id="3a976-291">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="3a976-292">La propiedad de navegación se denomina `Administrator` pero contiene un `Instructor` entidad:</span><span class="sxs-lookup"><span data-stu-id="3a976-292">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="3a976-293">El signo de interrogación (?) en el código anterior especifica la propiedad admite valores NULL.</span><span class="sxs-lookup"><span data-stu-id="3a976-293">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="3a976-294">Un departamento puede tener varios cursos, por lo que es una propiedad de navegación de cursos:</span><span class="sxs-lookup"><span data-stu-id="3a976-294">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="3a976-295">Nota: Por convención, núcleo de EF permite la eliminación en cascada para claves externas que no aceptan valores NULL y para las relaciones de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="3a976-295">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="3a976-296">Eliminación en cascada puede dar lugar a las reglas de eliminación en cascada circular.</span><span class="sxs-lookup"><span data-stu-id="3a976-296">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="3a976-297">Circular eliminación en cascada causas reglas una excepción cuando se agrega una migración.</span><span class="sxs-lookup"><span data-stu-id="3a976-297">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="3a976-298">Por ejemplo, si la `Department.InstructorID` propiedad no se ha definido como que acepta valores NULL:</span><span class="sxs-lookup"><span data-stu-id="3a976-298">For example, if the `Department.InstructorID` property wasn't defined as nullable:</span></span>

* <span data-ttu-id="3a976-299">Núcleo EF configura una regla de eliminación en cascada para eliminar el instructor cuando se elimina el departamento.</span><span class="sxs-lookup"><span data-stu-id="3a976-299">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="3a976-300">Eliminar el instructor cuando se elimina el departamento no es el comportamiento previsto.</span><span class="sxs-lookup"><span data-stu-id="3a976-300">Deleting the instructor when the department is deleted isn't the intended behavior.</span></span>

<span data-ttu-id="3a976-301">Si es necesario de las reglas de negocios el `InstructorID` propiedad ser acepta valores NULL, utilice la siguiente instrucción de la API fluida:</span><span class="sxs-lookup"><span data-stu-id="3a976-301">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="3a976-302">El código anterior deshabilita la eliminación en cascada en la relación de instructor de departamento.</span><span class="sxs-lookup"><span data-stu-id="3a976-302">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="3a976-303">Actualizar la entidad de inscripción</span><span class="sxs-lookup"><span data-stu-id="3a976-303">Update the Enrollment entity</span></span>

<span data-ttu-id="3a976-304">Es un registro de inscripción para un uno curso realizado por un alumno.</span><span class="sxs-lookup"><span data-stu-id="3a976-304">An enrollment record is for a one course taken by one student.</span></span>

![Entidad de inscripción](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="3a976-306">Actualización *Models/Enrollment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a976-306">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="3a976-307">Propiedades de navegación y de clave externa</span><span class="sxs-lookup"><span data-stu-id="3a976-307">Foreign key and navigation properties</span></span>

<span data-ttu-id="3a976-308">Las propiedades de clave externa y propiedades de navegación reflejen las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="3a976-308">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="3a976-309">Un registro de inscripción es para un curso, por lo que hay un `CourseID` propiedad de clave externa y un `Course` propiedad de navegación:</span><span class="sxs-lookup"><span data-stu-id="3a976-309">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="3a976-310">Es un registro de inscripción de un estudiante, por lo que hay un `StudentID` propiedad de clave externa y un `Student` propiedad de navegación:</span><span class="sxs-lookup"><span data-stu-id="3a976-310">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="3a976-311">Relaciones Varios a Varios</span><span class="sxs-lookup"><span data-stu-id="3a976-311">Many-to-Many Relationships</span></span>

<span data-ttu-id="3a976-312">Hay una relación de varios a varios entre la `Student` y `Course` entidades.</span><span class="sxs-lookup"><span data-stu-id="3a976-312">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="3a976-313">El `Enrollment` entidad funciona como una tabla de combinación-to-many *con carga* en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3a976-313">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="3a976-314">"Con una carga" significa que la `Enrollment` tabla contiene datos adicionales además de claves externas de las tablas combinadas (en este caso, la clave principal y `Grade`).</span><span class="sxs-lookup"><span data-stu-id="3a976-314">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="3a976-315">En la siguiente ilustración muestra el aspecto de estas relaciones en un diagrama de la entidad.</span><span class="sxs-lookup"><span data-stu-id="3a976-315">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="3a976-316">(Este diagrama se ha generado mediante herramientas avanzadas de EF de EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="3a976-316">(This diagram was generated using EF Power Tools for EF 6.x.</span></span> <span data-ttu-id="3a976-317">Crear el diagrama no forma parte del tutorial.)</span><span class="sxs-lookup"><span data-stu-id="3a976-317">Creating the diagram isn't part of the tutorial.)</span></span>

![Curso de estudiante muchos a muchos relación](complex-data-model/_static/student-course.png)

<span data-ttu-id="3a976-319">Cada línea de relación tiene un 1 en un extremo y un asterisco (\*) en el otro, que indica una relación uno a varios.</span><span class="sxs-lookup"><span data-stu-id="3a976-319">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="3a976-320">Si el `Enrollment` tabla no incluir información de categoría, que solo sería necesario contener las claves dos externas (`CourseID` y `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="3a976-320">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="3a976-321">Una tabla sin carga una combinación de varios a varios se suele denominar una tabla combinada pura (PJT).</span><span class="sxs-lookup"><span data-stu-id="3a976-321">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="3a976-322">El `Instructor` y `Course` entidades tienen una relación de varios a varios con una tabla combinada pura.</span><span class="sxs-lookup"><span data-stu-id="3a976-322">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="3a976-323">Nota: EF 6.x es compatible con las tablas de unión implícita para relaciones de varios a varios, pero el núcleo de EF no.</span><span class="sxs-lookup"><span data-stu-id="3a976-323">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="3a976-324">Para obtener más información, consulte [-to-many relaciones en EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="3a976-324">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="3a976-325">La entidad CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="3a976-325">The CourseAssignment entity</span></span>

![Entidad CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="3a976-327">Crear *Models/CourseAssignment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a976-327">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="3a976-328">Cursos de instructor</span><span class="sxs-lookup"><span data-stu-id="3a976-328">Instructor-to-Courses</span></span>

![Instructor a cursos m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="3a976-330">La relación de varios a varios Instructor a cursos:</span><span class="sxs-lookup"><span data-stu-id="3a976-330">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="3a976-331">Requiere una tabla de combinación que debe estar representada por un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="3a976-331">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="3a976-332">Es una tabla combinada pura (tabla sin carga).</span><span class="sxs-lookup"><span data-stu-id="3a976-332">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="3a976-333">Es común para el nombre de una entidad de combinación `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="3a976-333">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="3a976-334">Por ejemplo, la tabla de combinación Instructor a cursos mediante este patrón es `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="3a976-334">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="3a976-335">Sin embargo, se recomienda usar un nombre que describe la relación.</span><span class="sxs-lookup"><span data-stu-id="3a976-335">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="3a976-336">Modelos de datos empiezan de manera sencilla y crecen.</span><span class="sxs-lookup"><span data-stu-id="3a976-336">Data models start out simple and grow.</span></span> <span data-ttu-id="3a976-337">No carga combinaciones (PJTs) evolucionan con frecuencia para incluir la carga.</span><span class="sxs-lookup"><span data-stu-id="3a976-337">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="3a976-338">A partir de un nombre de entidad descriptivo, el nombre no tiene que cambiar cuando cambia de la tabla de combinación.</span><span class="sxs-lookup"><span data-stu-id="3a976-338">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="3a976-339">Idealmente, la entidad de combinación tendrá su propio nombre (posiblemente sola palabra) natural en el dominio de negocio.</span><span class="sxs-lookup"><span data-stu-id="3a976-339">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="3a976-340">Por ejemplo, libros y los clientes pueden vincularse a una entidad de combinación denominada clasificaciones.</span><span class="sxs-lookup"><span data-stu-id="3a976-340">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="3a976-341">La relación de varios a varios Instructor a cursos, `CourseAssignment` es preferible `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="3a976-341">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="3a976-342">Clave compuesta</span><span class="sxs-lookup"><span data-stu-id="3a976-342">Composite key</span></span>

<span data-ttu-id="3a976-343">Claves externas no admiten valores NULL.</span><span class="sxs-lookup"><span data-stu-id="3a976-343">FKs are not nullable.</span></span> <span data-ttu-id="3a976-344">Las claves dos externas en `CourseAssignment` (`InstructorID` y `CourseID`) juntos identifican de forma única cada fila de la `CourseAssignment` tabla.</span><span class="sxs-lookup"><span data-stu-id="3a976-344">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="3a976-345">`CourseAssignment`no requiere un PK dedicado.</span><span class="sxs-lookup"><span data-stu-id="3a976-345">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="3a976-346">El `InstructorID` y `CourseID` propiedades funcionan como un PK compuesto.</span><span class="sxs-lookup"><span data-stu-id="3a976-346">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="3a976-347">Es la única manera de especificar PK compuesto a EF Core con el *API fluida*.</span><span class="sxs-lookup"><span data-stu-id="3a976-347">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="3a976-348">La sección siguiente muestra cómo configurar la PK compuesto.</span><span class="sxs-lookup"><span data-stu-id="3a976-348">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="3a976-349">Se asegura de la clave compuesta:</span><span class="sxs-lookup"><span data-stu-id="3a976-349">The composite key ensures:</span></span>

* <span data-ttu-id="3a976-350">Se permiten varias filas para un curso.</span><span class="sxs-lookup"><span data-stu-id="3a976-350">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="3a976-351">Se permiten varias filas para un instructor.</span><span class="sxs-lookup"><span data-stu-id="3a976-351">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="3a976-352">No se permite varias filas para el mismo instructor y curso.</span><span class="sxs-lookup"><span data-stu-id="3a976-352">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="3a976-353">El `Enrollment` combinación entidad define su propio PK, por lo que son posibles duplicados de este tipo.</span><span class="sxs-lookup"><span data-stu-id="3a976-353">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="3a976-354">Para evitar los duplicados:</span><span class="sxs-lookup"><span data-stu-id="3a976-354">To prevent such duplicates:</span></span>

* <span data-ttu-id="3a976-355">Agregar un índice único en los campos de clave externa, o</span><span class="sxs-lookup"><span data-stu-id="3a976-355">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="3a976-356">Configurar `Enrollment` con una clave compuesta principal similar a `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="3a976-356">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="3a976-357">Para obtener más información, consulte [índices](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="3a976-357">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="3a976-358">Actualizar el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="3a976-358">Update the DB context</span></span>

<span data-ttu-id="3a976-359">Agregue el código que aparece resaltado a *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="3a976-359">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="3a976-360">El código anterior agrega las nuevas entidades y configura el `CourseAssignment` PK. compuesto de la entidad</span><span class="sxs-lookup"><span data-stu-id="3a976-360">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="3a976-361">Alternativa de API fluida a atributos</span><span class="sxs-lookup"><span data-stu-id="3a976-361">Fluent API alternative to attributes</span></span>

<span data-ttu-id="3a976-362">El `OnModelCreating` método en el anterior código usa el *API fluida* para configurar el comportamiento básico de EF.</span><span class="sxs-lookup"><span data-stu-id="3a976-362">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="3a976-363">La API se denomina "fluida" porque a menudo se usa por tender una serie de llamadas al método juntos en una única instrucción.</span><span class="sxs-lookup"><span data-stu-id="3a976-363">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="3a976-364">El [sigue código](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) es un ejemplo de la API fluida de:</span><span class="sxs-lookup"><span data-stu-id="3a976-364">The [following code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="3a976-365">En este tutorial, la API fluida se usa solo para la asignación de base de datos que no se puede realizar con atributos.</span><span class="sxs-lookup"><span data-stu-id="3a976-365">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="3a976-366">Sin embargo, la API fluida puede especificar casi todo el formato, la validación y las reglas de asignación que se pueden realizar con atributos.</span><span class="sxs-lookup"><span data-stu-id="3a976-366">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="3a976-367">Algunos atributos como `MinimumLength` no se puede aplicar con la API fluida.</span><span class="sxs-lookup"><span data-stu-id="3a976-367">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="3a976-368">`MinimumLength`no cambia el esquema, sólo se aplica una regla de validación de longitud mínima.</span><span class="sxs-lookup"><span data-stu-id="3a976-368">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="3a976-369">Algunos desarrolladores prefieren usar la API fluida exclusivamente para que pueden mantener las clases de entidad "limpia".</span><span class="sxs-lookup"><span data-stu-id="3a976-369">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="3a976-370">Se pueden mezclar atributos y la API fluida.</span><span class="sxs-lookup"><span data-stu-id="3a976-370">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="3a976-371">Hay algunas configuraciones que solo pueden realizarse con la API fluida (especificar una clave principal compuesta).</span><span class="sxs-lookup"><span data-stu-id="3a976-371">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="3a976-372">Hay algunas configuraciones que solo pueden realizarse con atributos (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="3a976-372">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="3a976-373">La práctica recomendada para el uso de atributos o API fluida:</span><span class="sxs-lookup"><span data-stu-id="3a976-373">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="3a976-374">Elija uno de estos dos enfoques.</span><span class="sxs-lookup"><span data-stu-id="3a976-374">Choose one of these two approaches.</span></span>
* <span data-ttu-id="3a976-375">Usar el método elegido constantemente lo máximo posible.</span><span class="sxs-lookup"><span data-stu-id="3a976-375">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="3a976-376">Algunos de los atributos utilizados en este tutorial se utilizan para:</span><span class="sxs-lookup"><span data-stu-id="3a976-376">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="3a976-377">Validación solo (por ejemplo, `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="3a976-377">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="3a976-378">Solo la configuración de núcleo EF (por ejemplo, `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="3a976-378">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="3a976-379">Configuración de validación y EF principal (por ejemplo, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="3a976-379">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="3a976-380">Para obtener más información acerca de los atributos frente a la API fluida, consulte [métodos de configuración](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="3a976-380">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="3a976-381">Diagrama mostrando relaciones de entidad</span><span class="sxs-lookup"><span data-stu-id="3a976-381">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="3a976-382">En la siguiente ilustración muestra el diagrama que EF Power Tools crear para el modelo School completado.</span><span class="sxs-lookup"><span data-stu-id="3a976-382">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagrama de entidad](complex-data-model/_static/diagram.png)

<span data-ttu-id="3a976-384">Muestra el diagrama anterior:</span><span class="sxs-lookup"><span data-stu-id="3a976-384">The preceding diagram shows:</span></span>

* <span data-ttu-id="3a976-385">Varias líneas de relación de uno a varios (1 a \*).</span><span class="sxs-lookup"><span data-stu-id="3a976-385">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="3a976-386">La línea de relación de uno a cero o uno (1 a 0.. 1) entre el `Instructor` y `OfficeAssignment` entidades.</span><span class="sxs-lookup"><span data-stu-id="3a976-386">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="3a976-387">La línea de relación de cero-o-uno a varios (0.. 1 a \*) entre el `Instructor` y `Department` entidades.</span><span class="sxs-lookup"><span data-stu-id="3a976-387">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="3a976-388">Valor de inicialización de la base de datos con datos de prueba</span><span class="sxs-lookup"><span data-stu-id="3a976-388">Seed the DB with Test Data</span></span>

<span data-ttu-id="3a976-389">Actualice el código en *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="3a976-389">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="3a976-390">El código anterior proporciona datos de inicialización para las nuevas entidades.</span><span class="sxs-lookup"><span data-stu-id="3a976-390">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="3a976-391">La mayor parte de este código crea nuevos objetos de entidad y carga los datos de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="3a976-391">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="3a976-392">Los datos de ejemplo se utilizan para pruebas.</span><span class="sxs-lookup"><span data-stu-id="3a976-392">The sample data is used for testing.</span></span> <span data-ttu-id="3a976-393">El código anterior crea las siguientes relaciones de varios a varios:</span><span class="sxs-lookup"><span data-stu-id="3a976-393">The preceding code creates the following many-to-many relationships:</span></span>

* `Enrollments`
* `CourseAssignment`

<span data-ttu-id="3a976-394">Nota: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) admitirá [propagación de datos](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span><span class="sxs-lookup"><span data-stu-id="3a976-394">Note: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) will support [data seeding](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="3a976-395">Agregar una migración</span><span class="sxs-lookup"><span data-stu-id="3a976-395">Add a migration</span></span>

<span data-ttu-id="3a976-396">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="3a976-396">Build the project.</span></span> <span data-ttu-id="3a976-397">Abra una ventana de comandos en la carpeta de proyecto y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="3a976-397">Open a command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="3a976-398">El comando anterior muestra una advertencia sobre la posible pérdida de datos.</span><span class="sxs-lookup"><span data-stu-id="3a976-398">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="3a976-399">Si el `database update` se ejecuta el comando, se genera el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a976-399">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="3a976-400">Cuando se ejecutan migraciones con los datos existentes, puede haber restricciones de clave externa que no están satisfecho con los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="3a976-400">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="3a976-401">Para este tutorial, se crea una nueva base de datos, por lo que no hay ningún infracciones de restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="3a976-401">For this tutorial, a new DB is created, so there are no FK constraint violations.</span></span> <span data-ttu-id="3a976-402">Vea [corregir las restricciones foreign key con datos heredados](#fk) para obtener instrucciones sobre cómo corregir las infracciones de clave externa en la base de datos actual.</span><span class="sxs-lookup"><span data-stu-id="3a976-402">See [Fixing foreign key constraints with legacy data](#fk) for instructions on how to fix the FK violations on the current DB.</span></span>

## <a name="change-the-connection-string-and-update-the-db"></a><span data-ttu-id="3a976-403">Cambie la cadena de conexión y actualizar la base de datos</span><span class="sxs-lookup"><span data-stu-id="3a976-403">Change the connection string and update the DB</span></span>

<span data-ttu-id="3a976-404">El código en la sección actualizada `DbInitializer` agrega los datos de inicialización para las nuevas entidades.</span><span class="sxs-lookup"><span data-stu-id="3a976-404">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="3a976-405">Para forzar el núcleo de EF para crear una nueva base de datos vacía:</span><span class="sxs-lookup"><span data-stu-id="3a976-405">To force EF Core to create a new empty DB:</span></span>

* <span data-ttu-id="3a976-406">Cambiar el nombre de cadena de conexión de base de datos en *appSettings.JSON que se* a ContosoUniversity3.</span><span class="sxs-lookup"><span data-stu-id="3a976-406">Change the DB connection string name in *appsettings.json* to ContosoUniversity3.</span></span> <span data-ttu-id="3a976-407">El nuevo nombre debe ser un nombre que no se ha usado en el equipo.</span><span class="sxs-lookup"><span data-stu-id="3a976-407">The new name must be a name that hasn't been used on the computer.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* <span data-ttu-id="3a976-408">O bien, elimine la base de datos utilizando:</span><span class="sxs-lookup"><span data-stu-id="3a976-408">Alternatively, delete the DB using:</span></span>

    * <span data-ttu-id="3a976-409">**Explorador de objetos SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="3a976-409">**SQL Server Object Explorer** (SSOX).</span></span>
    * <span data-ttu-id="3a976-410">El `database drop` comando de CLI:</span><span class="sxs-lookup"><span data-stu-id="3a976-410">The `database drop` CLI command:</span></span>

   ```console
   dotnet ef database drop
   ```

<span data-ttu-id="3a976-411">Ejecute `database update` en la ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="3a976-411">Run `database update` in the command window:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="3a976-412">El comando anterior ejecuta todas las migraciones.</span><span class="sxs-lookup"><span data-stu-id="3a976-412">The preceding command runs all the migrations.</span></span>

<span data-ttu-id="3a976-413">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3a976-413">Run the app.</span></span> <span data-ttu-id="3a976-414">Ejecuta la aplicación se ejecuta el `DbInitializer.Initialize` método.</span><span class="sxs-lookup"><span data-stu-id="3a976-414">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="3a976-415">El `DbInitializer.Initialize` rellena la base de datos nueva.</span><span class="sxs-lookup"><span data-stu-id="3a976-415">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="3a976-416">Abra la base de datos en SSOX:</span><span class="sxs-lookup"><span data-stu-id="3a976-416">Open the DB in SSOX:</span></span>

* <span data-ttu-id="3a976-417">Expanda el **tablas** nodo.</span><span class="sxs-lookup"><span data-stu-id="3a976-417">Expand the **Tables** node.</span></span> <span data-ttu-id="3a976-418">Se muestran las tablas creadas.</span><span class="sxs-lookup"><span data-stu-id="3a976-418">The created tables are displayed.</span></span>
* <span data-ttu-id="3a976-419">Si SSOX se abrió previamente, haga clic en el **actualizar** botón.</span><span class="sxs-lookup"><span data-stu-id="3a976-419">If SSOX was opened previously, click the **Refresh** button.</span></span>

![Tablas de SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="3a976-421">Examine la **CourseAssignment** tabla:</span><span class="sxs-lookup"><span data-stu-id="3a976-421">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="3a976-422">Haga clic en el **CourseAssignment** de tabla y seleccione **ver datos**.</span><span class="sxs-lookup"><span data-stu-id="3a976-422">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="3a976-423">Compruebe el **CourseAssignment** tabla contiene datos.</span><span class="sxs-lookup"><span data-stu-id="3a976-423">Verify the **CourseAssignment** table contains data.</span></span>

![Datos de CourseAssignment en SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a><span data-ttu-id="3a976-425">Corregir las restricciones foreign key con datos heredados</span><span class="sxs-lookup"><span data-stu-id="3a976-425">Fixing foreign key constraints with legacy data</span></span>

<span data-ttu-id="3a976-426">Esta sección es opcional.</span><span class="sxs-lookup"><span data-stu-id="3a976-426">This section is optional.</span></span>

<span data-ttu-id="3a976-427">Cuando se ejecutan migraciones con los datos existentes, puede haber restricciones de clave externa que no están satisfecho con los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="3a976-427">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="3a976-428">Con los datos de producción, se deben realizar los pasos para migrar los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="3a976-428">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="3a976-429">En esta sección se proporciona un ejemplo de corrección de las infracciones de restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="3a976-429">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="3a976-430">No realice estos cambios de código sin una copia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="3a976-430">Don't make these code changes without a backup.</span></span> <span data-ttu-id="3a976-431">No realice estos cambios de código si realizó la sección anterior y actualiza la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3a976-431">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="3a976-432">El *{timestamp}_ComplexDataModel.cs* archivo contiene el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a976-432">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="3a976-433">El código anterior agrega un acepta valores NULL `DepartmentID` FK a la `Course` tabla.</span><span class="sxs-lookup"><span data-stu-id="3a976-433">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="3a976-434">La base de datos desde el tutorial anterior contiene filas de `Course`, por lo que no se puede actualizar esa tabla por las migraciones.</span><span class="sxs-lookup"><span data-stu-id="3a976-434">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="3a976-435">Para realizar la `ComplexDataModel` trabajo de migración con los datos existentes:</span><span class="sxs-lookup"><span data-stu-id="3a976-435">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="3a976-436">Cambiar el código para asignar a la nueva columna (`DepartmentID`) un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="3a976-436">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="3a976-437">Cree un departamento falso denominado "Temp" para que actúe como el departamento de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="3a976-437">Create a fake department named "Temp" to act as the default department.</span></span>

### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="3a976-438">Corregir las restricciones foreign key</span><span class="sxs-lookup"><span data-stu-id="3a976-438">Fix the foreign key constraints</span></span>

<span data-ttu-id="3a976-439">Actualización de la `ComplexDataModel` clases `Up` método:</span><span class="sxs-lookup"><span data-stu-id="3a976-439">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="3a976-440">Abra la *{timestamp}_ComplexDataModel.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="3a976-440">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="3a976-441">Comentario de la línea de código que agrega el `DepartmentID` columna a la `Course` tabla.</span><span class="sxs-lookup"><span data-stu-id="3a976-441">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="3a976-442">Agregue el código resaltado siguiente.</span><span class="sxs-lookup"><span data-stu-id="3a976-442">Add the following highlighted code.</span></span> <span data-ttu-id="3a976-443">El nuevo código va después de la `.CreateTable( name: "Department"` bloque:[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="3a976-443">The new code goes after the `.CreateTable( name: "Department"` block: [!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="3a976-444">Con los cambios anteriores, existente `Course` filas estarán relacionadas con el departamento "Temp" después de la `ComplexDataModel` `Up` ejecuciones de método.</span><span class="sxs-lookup"><span data-stu-id="3a976-444">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="3a976-445">Una aplicación de producción necesario lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3a976-445">A production app would:</span></span>

* <span data-ttu-id="3a976-446">Incluir código o secuencias de comandos para agregar `Department` filas y relacionados con `Course` filas a la nueva `Department` filas.</span><span class="sxs-lookup"><span data-stu-id="3a976-446">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="3a976-447">No utilice el departamento "Temp" o el valor predeterminado de `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="3a976-447">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="3a976-448">El siguiente tutorial trata los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="3a976-448">The next tutorial covers related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3a976-449">[Anterior](xref:data/ef-rp/migrations)
[Siguiente](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="3a976-449">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>
