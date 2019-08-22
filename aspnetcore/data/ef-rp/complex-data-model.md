---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Modelo de datos (5 de 8)'
author: tdykstra
description: En este tutorial agregará más entidades y relaciones, y personalizará el modelo de datos especificando reglas de formato, validación y asignación.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 8a1c0759453b02f4ce1c45471a8f93da626f8261
ms.sourcegitcommit: 257cc3fe8c1d61341aa3b07e5bc0fa3d1c1c1d1c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2019
ms.locfileid: "69583286"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="0161e-103">Páginas de Razor con EF Core en ASP.NET Core: Modelo de datos (5 de 8)</span><span class="sxs-lookup"><span data-stu-id="0161e-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

<span data-ttu-id="0161e-104">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0161e-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0161e-105">En los tutoriales anteriores se trabajaba con un modelo de datos básico que se componía de tres entidades.</span><span class="sxs-lookup"><span data-stu-id="0161e-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="0161e-106">En este tutorial:</span><span class="sxs-lookup"><span data-stu-id="0161e-106">In this tutorial:</span></span>

* <span data-ttu-id="0161e-107">Se agregan más entidades y relaciones.</span><span class="sxs-lookup"><span data-stu-id="0161e-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="0161e-108">Se personaliza el modelo de datos especificando el formato, la validación y las reglas de asignación de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="0161e-109">El modelo de datos completo se muestra en la ilustración siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-109">The completed data model is shown in the following illustration:</span></span>

![Diagrama de entidades](complex-data-model/_static/diagram.png)

## <a name="the-student-entity"></a><span data-ttu-id="0161e-111">La entidad Student</span><span class="sxs-lookup"><span data-stu-id="0161e-111">The Student entity</span></span>

![Entidad Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="0161e-113">Reemplace el código de *Models/Student.cs* por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-113">Replace the code in *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Student.cs)]

<span data-ttu-id="0161e-114">En el código anterior se agrega una propiedad `FullName` y los atributos siguientes a las propiedades existentes:</span><span class="sxs-lookup"><span data-stu-id="0161e-114">The preceding code adds a `FullName` property and adds the following attributes to existing properties:</span></span>

* `[DataType]`
* `[DisplayFormat]`
* `[StringLength]`
* `[Column]`
* `[Required]`
* `[Display]`

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="0161e-115">La propiedad calculada FullName</span><span class="sxs-lookup"><span data-stu-id="0161e-115">The FullName calculated property</span></span>

<span data-ttu-id="0161e-116">`FullName` es una propiedad calculada que devuelve un valor que se crea mediante la concatenación de otras dos propiedades.</span><span class="sxs-lookup"><span data-stu-id="0161e-116">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="0161e-117">No se puede establecer `FullName`, por lo que solo tiene un descriptor de acceso get.</span><span class="sxs-lookup"><span data-stu-id="0161e-117">`FullName` can't be set, so it has only a get accessor.</span></span> <span data-ttu-id="0161e-118">No se crea ninguna columna `FullName` en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-118">No `FullName` column is created in the database.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="0161e-119">El atributo DataType</span><span class="sxs-lookup"><span data-stu-id="0161e-119">The DataType attribute</span></span>

```csharp
[DataType(DataType.Date)]
```

<span data-ttu-id="0161e-120">Para las fechas de inscripción de alumnos, en todas las páginas se muestra actualmente la hora del día junto con la fecha, aunque solo es relevante la fecha.</span><span class="sxs-lookup"><span data-stu-id="0161e-120">For student enrollment dates, all of the pages currently display the time of day along with the date, although only the date is relevant.</span></span> <span data-ttu-id="0161e-121">Mediante los atributos de anotación de datos, puede realizar un cambio de código que fijará el formato de presentación en todas las páginas en la que se muestren los datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-121">By using data annotation attributes, you can make one code change that will fix the display format in every page that shows the data.</span></span> 

<span data-ttu-id="0161e-122">El atributo [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) especifica un tipo de datos más específico que el tipo intrínseco de base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-122">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="0161e-123">En este caso solo se debe mostrar la fecha, no la fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="0161e-123">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="0161e-124">La [enumeración DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) proporciona muchos tipos de datos, como Date (Fecha), Time (Hora), PhoneNumber (Número de teléfono), Currency (Divisa), EmailAddress (Dirección de correo electrónico), etc. El atributo `DataType` también puede permitir que la aplicación proporcione automáticamente características específicas del tipo.</span><span class="sxs-lookup"><span data-stu-id="0161e-124">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="0161e-125">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0161e-125">For example:</span></span>

* <span data-ttu-id="0161e-126">El vínculo `mailto:` se crea automáticamente para `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="0161e-126">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="0161e-127">El selector de fecha se proporciona para `DataType.Date` en la mayoría de los exploradores.</span><span class="sxs-lookup"><span data-stu-id="0161e-127">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="0161e-128">El atributo `DataType` emite atributos `data-` de HTML 5 (se pronuncia "datos dash").</span><span class="sxs-lookup"><span data-stu-id="0161e-128">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes.</span></span> <span data-ttu-id="0161e-129">Los atributos `DataType` no proporcionan validación.</span><span class="sxs-lookup"><span data-stu-id="0161e-129">The `DataType` attributes don't provide validation.</span></span>

### <a name="the-displayformat-attribute"></a><span data-ttu-id="0161e-130">El atributo DisplayFormat</span><span class="sxs-lookup"><span data-stu-id="0161e-130">The DisplayFormat attribute</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="0161e-131">`DataType.Date` no especifica el formato de la fecha que se muestra.</span><span class="sxs-lookup"><span data-stu-id="0161e-131">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="0161e-132">De manera predeterminada, el campo de fecha se muestra según los formatos predeterminados basados en el elemento [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) del servidor.</span><span class="sxs-lookup"><span data-stu-id="0161e-132">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="0161e-133">El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="0161e-133">The `DisplayFormat` attribute is used to explicitly specify the date format.</span></span> <span data-ttu-id="0161e-134">La configuración `ApplyFormatInEditMode` especifica que el formato también debe aplicarse a la interfaz de usuario de edición.</span><span class="sxs-lookup"><span data-stu-id="0161e-134">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="0161e-135">Algunos campos no deben usar `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="0161e-135">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="0161e-136">Por ejemplo, el símbolo de divisa generalmente no debe mostrarse en un cuadro de texto de edición.</span><span class="sxs-lookup"><span data-stu-id="0161e-136">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="0161e-137">El atributo `DisplayFormat` puede usarse por sí solo.</span><span class="sxs-lookup"><span data-stu-id="0161e-137">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="0161e-138">Normalmente se recomienda usar el atributo `DataType` con el atributo `DisplayFormat`.</span><span class="sxs-lookup"><span data-stu-id="0161e-138">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="0161e-139">El atributo `DataType` transmite la semántica de los datos en lugar de cómo se representan en una pantalla.</span><span class="sxs-lookup"><span data-stu-id="0161e-139">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="0161e-140">El atributo `DataType` proporciona las siguientes ventajas que no están disponibles en `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="0161e-140">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="0161e-141">El explorador puede habilitar características de HTML5.</span><span class="sxs-lookup"><span data-stu-id="0161e-141">The browser can enable HTML5 features.</span></span> <span data-ttu-id="0161e-142">Por ejemplo, mostrar un control de calendario, el símbolo de divisa adecuado según la configuración regional, vínculos de correo electrónico y validación de entradas del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="0161e-142">For example, show a calendar control, the locale-appropriate currency symbol, email links, and client-side input validation.</span></span>
* <span data-ttu-id="0161e-143">De manera predeterminada, el explorador representa los datos con el formato correcto según la configuración regional.</span><span class="sxs-lookup"><span data-stu-id="0161e-143">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="0161e-144">Para obtener más información, vea la [documentación del asistente de etiquetas \<entrada&gt;](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="0161e-144">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

### <a name="the-stringlength-attribute"></a><span data-ttu-id="0161e-145">El atributo StringLength</span><span class="sxs-lookup"><span data-stu-id="0161e-145">The StringLength attribute</span></span>

```csharp
[StringLength(50, ErrorMessage = "First name cannot be longer than 50 characters.")]
```

<span data-ttu-id="0161e-146">Las reglas de validación de datos y los mensajes de error de validación se pueden especificar con atributos.</span><span class="sxs-lookup"><span data-stu-id="0161e-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="0161e-147">El atributo [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) especifica la longitud mínima y máxima de caracteres que se permite en un campo de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-147">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="0161e-148">En el código que se muestra se limitan los nombres a un máximo de 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="0161e-148">The code shown limits names to no more than 50 characters.</span></span> <span data-ttu-id="0161e-149">[Más adelante](#the-required-attribute) se muestra un ejemplo en el que se establece la longitud mínima de la cadena.</span><span class="sxs-lookup"><span data-stu-id="0161e-149">An example that sets the minimum string length is shown [later](#the-required-attribute).</span></span>

<span data-ttu-id="0161e-150">El atributo `StringLength` también proporciona validación del lado cliente y del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="0161e-150">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="0161e-151">El valor mínimo no influye en el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-151">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="0161e-152">El atributo `StringLength` no impide que un usuario escriba un espacio en blanco para un nombre.</span><span class="sxs-lookup"><span data-stu-id="0161e-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="0161e-153">El atributo [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) se puede usar para aplicar restricciones a la entrada.</span><span class="sxs-lookup"><span data-stu-id="0161e-153">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute can be used to apply restrictions to the input.</span></span> <span data-ttu-id="0161e-154">Por ejemplo, el código siguiente requiere que el primer carácter sea una letra mayúscula y el resto de caracteres sean alfabéticos:</span><span class="sxs-lookup"><span data-stu-id="0161e-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0161e-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0161e-155">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0161e-156">En el **Explorador de objetos de SQL Server**, (SSOX) abra el diseñador de tablas de Student haciendo doble clic en la tabla **Student**.</span><span class="sxs-lookup"><span data-stu-id="0161e-156">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabla de estudiantes en SSOX antes de las migraciones](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="0161e-158">La imagen anterior muestra el esquema para la tabla `Student`.</span><span class="sxs-lookup"><span data-stu-id="0161e-158">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="0161e-159">Los campos de nombre tienen el tipo `nvarchar(MAX)`.</span><span class="sxs-lookup"><span data-stu-id="0161e-159">The name fields have type `nvarchar(MAX)`.</span></span> <span data-ttu-id="0161e-160">Cuando más adelante en este tutorial se cree y se aplique una migración, los campos de nombre se convierten en `nvarchar(50)` como resultado de los atributos de longitud de cadena.</span><span class="sxs-lookup"><span data-stu-id="0161e-160">When a migration is created and applied later in this tutorial, the name fields become `nvarchar(50)` as a result of the string length attributes.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0161e-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0161e-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0161e-162">En la herramienta SQLite, examine las definiciones de columna para la tabla `Student`.</span><span class="sxs-lookup"><span data-stu-id="0161e-162">In your SQLite tool, examine the column definitions for the `Student` table.</span></span> <span data-ttu-id="0161e-163">Los campos de nombre tienen el tipo `Text`.</span><span class="sxs-lookup"><span data-stu-id="0161e-163">The name fields have type `Text`.</span></span> <span data-ttu-id="0161e-164">Observe que el primer campo de nombre se denomina `FirstMidName`.</span><span class="sxs-lookup"><span data-stu-id="0161e-164">Notice that the first name field is called `FirstMidName`.</span></span> <span data-ttu-id="0161e-165">En la sección siguiente, cambiará el nombre de esa columna por `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="0161e-165">In the next section, you change the name of that column to `FirstName`.</span></span>

---

### <a name="the-column-attribute"></a><span data-ttu-id="0161e-166">El atributo Column</span><span class="sxs-lookup"><span data-stu-id="0161e-166">The Column attribute</span></span>

```csharp
[Column("FirstName")]
public string FirstMidName { get; set; }
```

<span data-ttu-id="0161e-167">Los atributos pueden controlar cómo se asignan las clases y propiedades a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-167">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="0161e-168">En el modelo `Student`, el atributo `Column` se usa para asignar el nombre de la propiedad `FirstMidName` a "FirstName" en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-168">In the `Student` model, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the database.</span></span>

<span data-ttu-id="0161e-169">Cuando se crea la base de datos, los nombres de propiedad en el modelo se usan para los nombres de columna (excepto cuando se usa el atributo `Column`).</span><span class="sxs-lookup"><span data-stu-id="0161e-169">When the database is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span> <span data-ttu-id="0161e-170">El modelo `Student` usa `FirstMidName` para el nombre de campo por la posibilidad de que el campo contenga también un segundo nombre.</span><span class="sxs-lookup"><span data-stu-id="0161e-170">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="0161e-171">Con el atributo `[Column]`, `Student.FirstMidName` en el modelo de datos se asigna a la columna `FirstName` de la tabla `Student`.</span><span class="sxs-lookup"><span data-stu-id="0161e-171">With the `[Column]` attribute, `Student.FirstMidName` in the data model maps to the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="0161e-172">La adición del atributo `Column` cambia el modelo de respaldo de `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="0161e-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="0161e-173">El modelo que está haciendo la copia de seguridad de `SchoolContext` ya no coincide con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="0161e-174">Más adelante en este tutorial se agregará una migración para resolver esa discrepancia.</span><span class="sxs-lookup"><span data-stu-id="0161e-174">That discrepancy will be resolved by adding a migration later in this tutorial.</span></span>

### <a name="the-required-attribute"></a><span data-ttu-id="0161e-175">El atributo Required</span><span class="sxs-lookup"><span data-stu-id="0161e-175">The Required attribute</span></span>

```csharp
[Required]
```

<span data-ttu-id="0161e-176">El atributo `Required` hace que las propiedades de nombre sean campos obligatorios.</span><span class="sxs-lookup"><span data-stu-id="0161e-176">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="0161e-177">El atributo `Required` no es necesario para los tipos que no aceptan valores NULL, como los tipos de valor (por ejemplo `DateTime`, `int` y `double`).</span><span class="sxs-lookup"><span data-stu-id="0161e-177">The `Required` attribute isn't needed for non-nullable types such as value types (for example, `DateTime`, `int`, and `double`).</span></span> <span data-ttu-id="0161e-178">Los tipos que no aceptan valores NULL se tratan automáticamente como campos obligatorios.</span><span class="sxs-lookup"><span data-stu-id="0161e-178">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="0161e-179">El atributo `Required` se podría reemplazar con un parámetro de longitud mínima en el atributo `StringLength`:</span><span class="sxs-lookup"><span data-stu-id="0161e-179">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="0161e-180">El atributo Display</span><span class="sxs-lookup"><span data-stu-id="0161e-180">The Display attribute</span></span>

```csharp
[Display(Name = "Last Name")]
```

<span data-ttu-id="0161e-181">El atributo `Display` especifica que el título de los cuadros de texto debe ser "First Name", "Last Name", "Full Name" y "Enrollment Date".</span><span class="sxs-lookup"><span data-stu-id="0161e-181">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="0161e-182">Los títulos predeterminados no tenían ningún espacio de división de palabras, por ejemplo "Lastname".</span><span class="sxs-lookup"><span data-stu-id="0161e-182">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="create-a-migration"></a><span data-ttu-id="0161e-183">Crear una migración</span><span class="sxs-lookup"><span data-stu-id="0161e-183">Create a migration</span></span>

<span data-ttu-id="0161e-184">Ejecute la aplicación y vaya a la página Students.</span><span class="sxs-lookup"><span data-stu-id="0161e-184">Run the app and go to the Students page.</span></span> <span data-ttu-id="0161e-185">Se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="0161e-185">An exception is thrown.</span></span> <span data-ttu-id="0161e-186">El atributo `[Column]` hace que EF espere encontrar una columna denominada `FirstName`, pero el nombre de la columna en la base de datos sigue siendo `FirstMidName`.</span><span class="sxs-lookup"><span data-stu-id="0161e-186">The `[Column]` attribute causes EF to expect to find a column named `FirstName`, but the column name in the database is still `FirstMidName`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0161e-187">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0161e-187">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0161e-188">El mensaje de error es similar al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-188">The error message is similar to the following example:</span></span>

```
SqlException: Invalid column name 'FirstName'.
```

* <span data-ttu-id="0161e-189">En la Consola del administrador de paquetes, escriba los comandos siguientes para crear una migración y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="0161e-189">In the PMC, enter the following commands to create a new migration and update the database:</span></span>

  ```powershell
  Add-Migration ColumnFirstName
  Update-Database
  ```

  <span data-ttu-id="0161e-190">El primero de estos comandos genera el siguiente mensaje de advertencia:</span><span class="sxs-lookup"><span data-stu-id="0161e-190">The first of these commands generates the following warning message:</span></span>

  ```text
  An operation was scaffolded that may result in the loss of data.
  Please review the migration for accuracy.
  ```

  <span data-ttu-id="0161e-191">La advertencia se genera porque los campos de nombre ahora están limitados a 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="0161e-191">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="0161e-192">Si un nombre en la base de datos tenía más de 50 caracteres, se perderían desde el 51 hasta el último carácter.</span><span class="sxs-lookup"><span data-stu-id="0161e-192">If a name in the database had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="0161e-193">Abra la tabla de estudiantes en SSOX:</span><span class="sxs-lookup"><span data-stu-id="0161e-193">Open the Student table in SSOX:</span></span>

  ![Tabla de estudiantes en SSOX después de las migraciones](complex-data-model/_static/ssox-after-migration.png)

  <span data-ttu-id="0161e-195">Antes de aplicar la migración, las columnas de nombre eran de tipo [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="0161e-195">Before the migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="0161e-196">Las columnas de nombre ahora son `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="0161e-196">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="0161e-197">El nombre de columna ha cambiado de `FirstMidName` a `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="0161e-197">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0161e-198">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0161e-198">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0161e-199">El mensaje de error es similar al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-199">The error message is similar to the following example:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: s.FirstName'.
```

* <span data-ttu-id="0161e-200">Abra una ventana de comandos en la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="0161e-200">Open a command window in the project folder.</span></span> <span data-ttu-id="0161e-201">Escriba los comandos siguientes para crear una migración y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="0161e-201">Enter the following commands to create a new migration and update the database:</span></span>

  ```console
  dotnet ef migrations add ColumnFirstName
  dotnet ef database update
  ```

  <span data-ttu-id="0161e-202">El comando de actualización de base de datos muestra un error similar al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-202">The database update command displays an error like the following example:</span></span>

  ```text
  SQLite does not support this migration operation ('AlterColumnOperation'). For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
  ```

<span data-ttu-id="0161e-203">En este tutorial, la manera de solucionar este error consiste en eliminar y volver a crear la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="0161e-203">For this tutorial, the way to get past this error is to delete and re-create the initial migration.</span></span> <span data-ttu-id="0161e-204">Para obtener más información, vea la nota de advertencia de SQLite en la parte superior del [tutorial de migraciones](xref:data/ef-rp/migrations).</span><span class="sxs-lookup"><span data-stu-id="0161e-204">For more information, see the SQLite warning note at the top of the [migrations tutorial](xref:data/ef-rp/migrations).</span></span>

* <span data-ttu-id="0161e-205">Elimine la carpeta *Migrations*.</span><span class="sxs-lookup"><span data-stu-id="0161e-205">Delete the *Migrations* folder.</span></span>
* <span data-ttu-id="0161e-206">Ejecute los comandos siguientes para quitar la base de datos, crear una migración inicial y aplicarla:</span><span class="sxs-lookup"><span data-stu-id="0161e-206">Run the following commands to drop the database, create a new initial migration, and apply the migration:</span></span>

  ```console
  dotnet ef database drop --force
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

* <span data-ttu-id="0161e-207">Examine la tabla Student en la herramienta SQLite.</span><span class="sxs-lookup"><span data-stu-id="0161e-207">Examine the Student table in your SQLite tool.</span></span> <span data-ttu-id="0161e-208">La columna que antes era FirstMidName ahora es FirstName.</span><span class="sxs-lookup"><span data-stu-id="0161e-208">The column that was FirstMidName is now FirstName.</span></span>

---

* <span data-ttu-id="0161e-209">Ejecute la aplicación y vaya a la página Students.</span><span class="sxs-lookup"><span data-stu-id="0161e-209">Run the app and go to the Students page.</span></span>
* <span data-ttu-id="0161e-210">Observe que las horas no se escriben ni se muestran junto con las fechas.</span><span class="sxs-lookup"><span data-stu-id="0161e-210">Notice that times are not input or displayed along with dates.</span></span>
* <span data-ttu-id="0161e-211">Seleccione **Crear nuevo** e intente escribir un nombre de más de 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="0161e-211">Select **Create New**, and try to enter a name longer than 50 characters.</span></span>

> [!Note]
> <span data-ttu-id="0161e-212">En las secciones siguientes, la creación de la aplicación en algunas de las fases genera errores del compilador.</span><span class="sxs-lookup"><span data-stu-id="0161e-212">In the following sections, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="0161e-213">Las instrucciones especifican cuándo se debe compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0161e-213">The instructions specify when to build the app.</span></span>

## <a name="the-instructor-entity"></a><span data-ttu-id="0161e-214">La entidad Instructor</span><span class="sxs-lookup"><span data-stu-id="0161e-214">The Instructor Entity</span></span>

![La entidad Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="0161e-216">Cree *Models/Instructor.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-216">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Instructor.cs)]

<span data-ttu-id="0161e-217">En una sola línea puede haber varios atributos.</span><span class="sxs-lookup"><span data-stu-id="0161e-217">Multiple attributes can be on one line.</span></span> <span data-ttu-id="0161e-218">Los atributos `HireDate` pudieron escribirse de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-218">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="navigation-properties"></a><span data-ttu-id="0161e-219">Propiedades de navegación</span><span class="sxs-lookup"><span data-stu-id="0161e-219">Navigation properties</span></span>

<span data-ttu-id="0161e-220">`CourseAssignments` y `OfficeAssignment` son propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="0161e-220">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="0161e-221">Un instructor puede impartir cualquier número de cursos, por lo que `CourseAssignments` se define como una colección.</span><span class="sxs-lookup"><span data-stu-id="0161e-221">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="0161e-222">Un instructor puede tener como máximo una oficina, por lo que la propiedad `OfficeAssignment` contiene una sola entidad `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="0161e-222">An instructor can have at most one office, so the `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="0161e-223">`OfficeAssignment` es NULL si no se asigna ninguna oficina.</span><span class="sxs-lookup"><span data-stu-id="0161e-223">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="the-officeassignment-entity"></a><span data-ttu-id="0161e-224">La entidad OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="0161e-224">The OfficeAssignment entity</span></span>

![Entidad OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="0161e-226">Cree *Models/OfficeAssignment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-226">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="0161e-227">El atributo Key</span><span class="sxs-lookup"><span data-stu-id="0161e-227">The Key attribute</span></span>

<span data-ttu-id="0161e-228">El atributo `[Key]` se usa para identificar una propiedad como la clave principal (PK) cuando el nombre de propiedad es diferente de classnameID o ID.</span><span class="sxs-lookup"><span data-stu-id="0161e-228">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="0161e-229">Hay una relación de uno a cero o uno entre las entidades `Instructor` y `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="0161e-229">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="0161e-230">Solo existe una asignación de oficina en relación con el instructor a la que está asignada.</span><span class="sxs-lookup"><span data-stu-id="0161e-230">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="0161e-231">La clave principal de `OfficeAssignment` también es la clave externa (FK) para la entidad `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="0161e-231">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span>

<span data-ttu-id="0161e-232">EF Core no puede reconocer automáticamente `InstructorID` como la clave principal de `OfficeAssignment` porque `InstructorID` no sigue la convención de nomenclatura de ID o classnameID.</span><span class="sxs-lookup"><span data-stu-id="0161e-232">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because `InstructorID` doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="0161e-233">Por tanto, se usa el atributo `Key` para identificar `InstructorID` como la clave principal:</span><span class="sxs-lookup"><span data-stu-id="0161e-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="0161e-234">De forma predeterminada, EF Core trata la clave como no generada por la base de datos porque la columna es para una relación de identificación.</span><span class="sxs-lookup"><span data-stu-id="0161e-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="0161e-235">La propiedad de navegación Instructor</span><span class="sxs-lookup"><span data-stu-id="0161e-235">The Instructor navigation property</span></span>

<span data-ttu-id="0161e-236">La propiedad de navegación `Instructor.OfficeAssignment` puede ser NULL porque es posible que no haya una fila `OfficeAssignment` para un instructor determinado.</span><span class="sxs-lookup"><span data-stu-id="0161e-236">The `Instructor.OfficeAssignment` navigation property can be null because there might not be an `OfficeAssignment` row for a given instructor.</span></span> <span data-ttu-id="0161e-237">Un instructor podría no tener una asignación de oficina.</span><span class="sxs-lookup"><span data-stu-id="0161e-237">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="0161e-238">La propiedad de navegación `OfficeAssignment.Instructor` siempre tendrá una entidad de instructor porque el tipo `InstructorID` de clave externa es `int`, un tipo de valor que no acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="0161e-238">The `OfficeAssignment.Instructor` navigation property will always have an instructor entity because the foreign key `InstructorID` type is `int`, a non-nullable value type.</span></span> <span data-ttu-id="0161e-239">Una asignación de oficina no puede existir sin un instructor.</span><span class="sxs-lookup"><span data-stu-id="0161e-239">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="0161e-240">Cuando una entidad `Instructor` tiene una entidad `OfficeAssignment` relacionada, cada entidad tiene una referencia a la otra en su propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="0161e-240">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="0161e-241">La entidad Course</span><span class="sxs-lookup"><span data-stu-id="0161e-241">The Course Entity</span></span>

![La entidad Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="0161e-243">Actualice *Models/Course.cs* con el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="0161e-243">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Course.cs?highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="0161e-244">La entidad `Course` tiene una propiedad de clave externa (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="0161e-244">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="0161e-245">`DepartmentID` apunta a la entidad relacionada `Department`.</span><span class="sxs-lookup"><span data-stu-id="0161e-245">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="0161e-246">La entidad `Course` tiene una propiedad de navegación `Department`.</span><span class="sxs-lookup"><span data-stu-id="0161e-246">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="0161e-247">EF Core no requiere una propiedad de clave externa para un modelo de datos cuando el modelo tiene una propiedad de navegación para una entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="0161e-247">EF Core doesn't require a foreign key property for a data model when the model has a navigation property for a related entity.</span></span> <span data-ttu-id="0161e-248">EF Core crea automáticamente claves externas en la base de datos siempre que se necesiten.</span><span class="sxs-lookup"><span data-stu-id="0161e-248">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="0161e-249">EF Core crea [propiedades paralelas](/ef/core/modeling/shadow-properties) para las claves externas creadas automáticamente.</span><span class="sxs-lookup"><span data-stu-id="0161e-249">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="0161e-250">Pero la inclusión explícita de la clave externa en el modelo de datos puede hacer que las actualizaciones sean más sencillas y eficaces.</span><span class="sxs-lookup"><span data-stu-id="0161e-250">However, explicitly including the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="0161e-251">Por ejemplo, considere la posibilidad de un modelo donde la propiedad de la clave externa `DepartmentID` *no* está incluida.</span><span class="sxs-lookup"><span data-stu-id="0161e-251">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="0161e-252">Cuando se captura una entidad de curso para editar:</span><span class="sxs-lookup"><span data-stu-id="0161e-252">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="0161e-253">La propiedad `Department` es NULL si no se carga de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="0161e-253">The `Department` property is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="0161e-254">Para actualizar la entidad Course, la entidad `Department` debe capturarse en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="0161e-254">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="0161e-255">Cuando se incluye la propiedad de clave externa `DepartmentID` en el modelo de datos, no es necesario capturar la entidad `Department` antes de una actualización.</span><span class="sxs-lookup"><span data-stu-id="0161e-255">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="0161e-256">El atributo DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="0161e-256">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="0161e-257">El atributo `[DatabaseGenerated(DatabaseGeneratedOption.None)]` especifica que la aplicación proporciona la clave principal, en lugar de generarla la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-257">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="0161e-258">De forma predeterminada, EF Core asume que la base de datos genera valores de clave principal.</span><span class="sxs-lookup"><span data-stu-id="0161e-258">By default, EF Core assumes that PK values are generated by the database.</span></span> <span data-ttu-id="0161e-259">La generación por parte de la base de datos suele ser el mejor enfoque.</span><span class="sxs-lookup"><span data-stu-id="0161e-259">Database-generated is generally the best approach.</span></span> <span data-ttu-id="0161e-260">Para las entidades `Course`, el usuario especifica la clave principal.</span><span class="sxs-lookup"><span data-stu-id="0161e-260">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="0161e-261">Por ejemplo, un número de curso como una serie de 1000 para el departamento de matemáticas, una serie de 2000 para el departamento de inglés.</span><span class="sxs-lookup"><span data-stu-id="0161e-261">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="0161e-262">También se puede usar el atributo `DatabaseGenerated` para generar valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="0161e-262">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="0161e-263">Por ejemplo, la base de datos puede generar de forma automática un campo de fecha para registrar la fecha en que se crea o actualiza una fila.</span><span class="sxs-lookup"><span data-stu-id="0161e-263">For example, the database can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="0161e-264">Para obtener más información, vea [Propiedades generadas](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="0161e-264">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="0161e-265">Propiedades de clave externa y de navegación</span><span class="sxs-lookup"><span data-stu-id="0161e-265">Foreign key and navigation properties</span></span>

<span data-ttu-id="0161e-266">Las propiedades de clave externa (FK) y las de navegación de la entidad `Course` reflejan las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="0161e-266">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="0161e-267">Un curso se asigna a un departamento, por lo que hay una clave externa `DepartmentID` y una propiedad de navegación `Department`.</span><span class="sxs-lookup"><span data-stu-id="0161e-267">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="0161e-268">Un curso puede tener cualquier número de alumnos inscritos en él, por lo que la propiedad de navegación `Enrollments` es una colección:</span><span class="sxs-lookup"><span data-stu-id="0161e-268">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="0161e-269">Un curso puede ser impartido por varios instructores, por lo que la propiedad de navegación `CourseAssignments` es una colección:</span><span class="sxs-lookup"><span data-stu-id="0161e-269">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="0161e-270">`CourseAssignment` se explica [más adelante](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="0161e-270">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="the-department-entity"></a><span data-ttu-id="0161e-271">La entidad Department</span><span class="sxs-lookup"><span data-stu-id="0161e-271">The Department entity</span></span>

![La entidad Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="0161e-273">Cree *Models/Department.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-273">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Models/Department1.cs)]

### <a name="the-column-attribute"></a><span data-ttu-id="0161e-274">El atributo Column</span><span class="sxs-lookup"><span data-stu-id="0161e-274">The Column attribute</span></span>

<span data-ttu-id="0161e-275">Anteriormente se usó el atributo `Column` para cambiar la asignación de nombres de columna.</span><span class="sxs-lookup"><span data-stu-id="0161e-275">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="0161e-276">En el código de la entidad `Department`, se usó el atributo `Column` para cambiar la asignación de tipos de datos de SQL.</span><span class="sxs-lookup"><span data-stu-id="0161e-276">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="0161e-277">La columna `Budget` se define mediante el tipo money de SQL Server en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="0161e-277">The `Budget` column is defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="0161e-278">Por lo general, la asignación de columnas no es necesaria.</span><span class="sxs-lookup"><span data-stu-id="0161e-278">Column mapping is generally not required.</span></span> <span data-ttu-id="0161e-279">EF Core elige el tipo de datos de SQL Server apropiado en función del tipo CLR para la propiedad.</span><span class="sxs-lookup"><span data-stu-id="0161e-279">EF Core chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="0161e-280">El tipo CLR `decimal` se asigna a un tipo `decimal` de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0161e-280">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="0161e-281">`Budget` es para la divisa, y el tipo de datos money es más adecuado para la divisa.</span><span class="sxs-lookup"><span data-stu-id="0161e-281">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="0161e-282">Propiedades de clave externa y de navegación</span><span class="sxs-lookup"><span data-stu-id="0161e-282">Foreign key and navigation properties</span></span>

<span data-ttu-id="0161e-283">Las propiedades de clave externa y de navegación reflejan las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="0161e-283">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="0161e-284">Un departamento puede tener o no un administrador.</span><span class="sxs-lookup"><span data-stu-id="0161e-284">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="0161e-285">Un administrador siempre es un instructor.</span><span class="sxs-lookup"><span data-stu-id="0161e-285">An administrator is always an instructor.</span></span> <span data-ttu-id="0161e-286">Por lo tanto, la propiedad `InstructorID` se incluye como la clave externa para la entidad `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="0161e-286">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="0161e-287">La propiedad de navegación se denomina `Administrator` pero contiene una entidad `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="0161e-287">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="0161e-288">El signo de interrogación (?) en el código anterior especifica que la propiedad acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="0161e-288">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="0161e-289">Un departamento puede tener varios cursos, por lo que hay una propiedad de navegación Courses:</span><span class="sxs-lookup"><span data-stu-id="0161e-289">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="0161e-290">Por convención, EF Core permite la eliminación en cascada de las claves externas que no acepten valores NULL ni relaciones de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="0161e-290">By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="0161e-291">Este comportamiento predeterminado puede dar lugar a reglas de eliminación en cascada circular.</span><span class="sxs-lookup"><span data-stu-id="0161e-291">This default behavior can result in circular cascade delete rules.</span></span> <span data-ttu-id="0161e-292">Las reglas de eliminación en cascada circular inician una excepción cuando se agrega una migración.</span><span class="sxs-lookup"><span data-stu-id="0161e-292">Circular cascade delete rules cause an exception when a migration is added.</span></span>

<span data-ttu-id="0161e-293">Por ejemplo, si la propiedad `Department.InstructorID` se ha definido como que no acepta valores NULL, EF Core configurará una regla de eliminación en cascada.</span><span class="sxs-lookup"><span data-stu-id="0161e-293">For example, if the `Department.InstructorID` property was defined as non-nullable, EF Core would configure a cascade delete rule.</span></span> <span data-ttu-id="0161e-294">En ese caso, el departamento se eliminará cuando se elimine el instructor asignado como administrador.</span><span class="sxs-lookup"><span data-stu-id="0161e-294">In that case, the department would be deleted when the instructor assigned as its administrator is deleted.</span></span> <span data-ttu-id="0161e-295">En este escenario, una regla de restricción tendría más sentido.</span><span class="sxs-lookup"><span data-stu-id="0161e-295">In this scenario, a restrict rule would make more sense.</span></span> <span data-ttu-id="0161e-296">La API fluida siguiente establecería una regla de restricción y deshabilitaría la eliminación en cascada.</span><span class="sxs-lookup"><span data-stu-id="0161e-296">The following fluent API would set a restrict rule and disable cascade delete.</span></span>

  ```csharp
  modelBuilder.Entity<Department>()
     .HasOne(d => d.Administrator)
     .WithMany()
     .OnDelete(DeleteBehavior.Restrict)
  ```

## <a name="the-enrollment-entity"></a><span data-ttu-id="0161e-297">La entidad Enrollment</span><span class="sxs-lookup"><span data-stu-id="0161e-297">The Enrollment entity</span></span>

<span data-ttu-id="0161e-298">Un registro de inscripción corresponde a un curso realizado por un alumno.</span><span class="sxs-lookup"><span data-stu-id="0161e-298">An enrollment record is for one course taken by one student.</span></span>

![La entidad Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="0161e-300">Actualice *Models/Enrollment.cs* con el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="0161e-300">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Enrollment.cs?highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="0161e-301">Propiedades de clave externa y de navegación</span><span class="sxs-lookup"><span data-stu-id="0161e-301">Foreign key and navigation properties</span></span>

<span data-ttu-id="0161e-302">Las propiedades de clave externa y de navegación reflejan las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="0161e-302">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="0161e-303">Un registro de inscripción es para un curso, por lo que hay una propiedad de clave externa `CourseID` y una propiedad de navegación `Course`:</span><span class="sxs-lookup"><span data-stu-id="0161e-303">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="0161e-304">Un registro de inscripción es para un alumno, por lo que hay una propiedad de clave externa `StudentID` y una propiedad de navegación `Student`:</span><span class="sxs-lookup"><span data-stu-id="0161e-304">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="0161e-305">Relaciones Varios a Varios</span><span class="sxs-lookup"><span data-stu-id="0161e-305">Many-to-Many Relationships</span></span>

<span data-ttu-id="0161e-306">Hay una relación de varios a varios entre las entidades `Student` y `Course`.</span><span class="sxs-lookup"><span data-stu-id="0161e-306">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="0161e-307">La entidad `Enrollment` funciona como una tabla combinada varios a varios *con carga útil* en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-307">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="0161e-308">"Con carga útil" significa que la tabla `Enrollment` contiene datos adicionales, además de claves externas de las tablas combinadas (en este caso, la clave principal y `Grade`).</span><span class="sxs-lookup"><span data-stu-id="0161e-308">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="0161e-309">En la ilustración siguiente se muestra el aspecto de estas relaciones en un diagrama de entidades.</span><span class="sxs-lookup"><span data-stu-id="0161e-309">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="0161e-310">(Este diagrama se ha generado mediante [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) para EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="0161e-310">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="0161e-311">Crear el diagrama no forma parte del tutorial).</span><span class="sxs-lookup"><span data-stu-id="0161e-311">Creating the diagram isn't part of the tutorial.)</span></span>

![Relación de varios a varios entre estudiantes y cursos](complex-data-model/_static/student-course.png)

<span data-ttu-id="0161e-313">Cada línea de relación tiene un 1 en un extremo y un asterisco (\*) en el otro, para indicar una relación uno a varios.</span><span class="sxs-lookup"><span data-stu-id="0161e-313">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="0161e-314">Si la tabla `Enrollment` no incluyera información de calificaciones, solo tendría que contener las dos claves externas (`CourseID` y `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="0161e-314">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="0161e-315">Una tabla combinada de varios a varios sin carga útil se suele denominar una tabla combinada pura (PJT).</span><span class="sxs-lookup"><span data-stu-id="0161e-315">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="0161e-316">Las entidades `Instructor` y `Course` tienen una relación de varios a varios con una tabla combinada pura.</span><span class="sxs-lookup"><span data-stu-id="0161e-316">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="0161e-317">Nota: EF 6.x es compatible con las tablas de combinación implícitas con relaciones de varios a varios, pero EF Core, no.</span><span class="sxs-lookup"><span data-stu-id="0161e-317">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="0161e-318">Para obtener más información, consulte [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/) (Relaciones de varios a varios en EF Core 2.0).</span><span class="sxs-lookup"><span data-stu-id="0161e-318">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="0161e-319">La entidad CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="0161e-319">The CourseAssignment entity</span></span>

![La entidad CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="0161e-321">Cree *Models/CourseAssignment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-321">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/CourseAssignment.cs)]

<span data-ttu-id="0161e-322">La relación de varios a varios entre instructores y cursos requiere una tabla de combinación y la entidad para esa tabla de combinación es CourseAssignment.</span><span class="sxs-lookup"><span data-stu-id="0161e-322">The Instructor-to-Courses many-to-many relationship requires a join table, and the entity for that join table is CourseAssignment.</span></span>

![Relación de varios a varios Instructor-to-Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="0161e-324">Es común denominar una entidad de combinación `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="0161e-324">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="0161e-325">Por ejemplo, la tabla de combinación Instructor-to-Courses con este patrón sería `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="0161e-325">For example, the Instructor-to-Courses join table using this pattern would be `CourseInstructor`.</span></span> <span data-ttu-id="0161e-326">Pero se recomienda usar un nombre que describa la relación.</span><span class="sxs-lookup"><span data-stu-id="0161e-326">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="0161e-327">Los modelos de datos empiezan de manera sencilla y crecen.</span><span class="sxs-lookup"><span data-stu-id="0161e-327">Data models start out simple and grow.</span></span> <span data-ttu-id="0161e-328">Las tablas combinadas sin carga útil (PJT) evolucionan con frecuencia para incluir la carga útil.</span><span class="sxs-lookup"><span data-stu-id="0161e-328">Join tables without payload (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="0161e-329">A partir de un nombre de entidad descriptivo, no es necesario cambiar el nombre cuando la tabla combinada cambia.</span><span class="sxs-lookup"><span data-stu-id="0161e-329">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="0161e-330">Idealmente, la entidad de combinación tendrá su propio nombre natural (posiblemente una sola palabra) en el dominio de empresa.</span><span class="sxs-lookup"><span data-stu-id="0161e-330">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="0161e-331">Por ejemplo, Books y Customers podrían vincularse a través de una entidad combinada denominada Ratings.</span><span class="sxs-lookup"><span data-stu-id="0161e-331">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="0161e-332">Para la relación de varios a varios Instructor-to-Courses, se prefiere `CourseAssignment` a `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="0161e-332">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="0161e-333">Clave compuesta</span><span class="sxs-lookup"><span data-stu-id="0161e-333">Composite key</span></span>

<span data-ttu-id="0161e-334">Las dos claves externas en `CourseAssignment` (`InstructorID` y `CourseID`) juntas identifican de forma única cada fila de la tabla `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="0161e-334">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="0161e-335">`CourseAssignment` no requiere una clave principal dedicada.</span><span class="sxs-lookup"><span data-stu-id="0161e-335">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="0161e-336">Las propiedades `InstructorID` y `CourseID` funcionan como una clave principal compuesta.</span><span class="sxs-lookup"><span data-stu-id="0161e-336">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="0161e-337">La única manera de especificar claves principales compuestas en EF Core es con la *API fluida*.</span><span class="sxs-lookup"><span data-stu-id="0161e-337">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="0161e-338">La sección siguiente muestra cómo configurar la clave principal compuesta.</span><span class="sxs-lookup"><span data-stu-id="0161e-338">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="0161e-339">La clave compuesta asegura que:</span><span class="sxs-lookup"><span data-stu-id="0161e-339">The composite key ensures that:</span></span>

* <span data-ttu-id="0161e-340">Se permiten varias filas para un curso.</span><span class="sxs-lookup"><span data-stu-id="0161e-340">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="0161e-341">Se permiten varias filas para un instructor.</span><span class="sxs-lookup"><span data-stu-id="0161e-341">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="0161e-342">No se permiten varias filas para el mismo instructor y curso.</span><span class="sxs-lookup"><span data-stu-id="0161e-342">Multiple rows aren't allowed for the same instructor and course.</span></span>

<span data-ttu-id="0161e-343">La entidad de combinación `Enrollment` define su propia clave principal, por lo que este tipo de duplicados son posibles.</span><span class="sxs-lookup"><span data-stu-id="0161e-343">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="0161e-344">Para evitar los duplicados:</span><span class="sxs-lookup"><span data-stu-id="0161e-344">To prevent such duplicates:</span></span>

* <span data-ttu-id="0161e-345">Agregue un índice único en los campos de clave externa, o</span><span class="sxs-lookup"><span data-stu-id="0161e-345">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="0161e-346">Configure `Enrollment` con una clave compuesta principal similar a `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="0161e-346">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="0161e-347">Para obtener más información, vea [Índices](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="0161e-347">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="0161e-348">Actualizar el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="0161e-348">Update the database context</span></span>

<span data-ttu-id="0161e-349">Actualice *Data/SchoolContext.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-349">Update *Data/SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Data/SchoolContext.cs?highlight=15-18,25-31)]

<span data-ttu-id="0161e-350">El código anterior agrega las nuevas entidades y configura la clave principal compuesta de la entidad `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="0161e-350">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="0161e-351">Alternativa de la API fluida a los atributos</span><span class="sxs-lookup"><span data-stu-id="0161e-351">Fluent API alternative to attributes</span></span>

<span data-ttu-id="0161e-352">El método `OnModelCreating` del código anterior usa la *API fluida* para configurar el comportamiento de EF Core.</span><span class="sxs-lookup"><span data-stu-id="0161e-352">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="0161e-353">La API se denomina "fluida" porque a menudo se usa para encadenar una serie de llamadas de método en una única instrucción.</span><span class="sxs-lookup"><span data-stu-id="0161e-353">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="0161e-354">El [código siguiente](/ef/core/modeling/#use-fluent-api-to-configure-a-model) es un ejemplo de la API fluida:</span><span class="sxs-lookup"><span data-stu-id="0161e-354">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="0161e-355">En este tutorial, la API fluida solo se usa para la asignación de base de datos que no se puede realizar con atributos.</span><span class="sxs-lookup"><span data-stu-id="0161e-355">In this tutorial, the fluent API is used only for database mapping that can't be done with attributes.</span></span> <span data-ttu-id="0161e-356">Pero la API fluida puede especificar casi todas las reglas de formato, validación y asignación que se pueden realizar mediante el uso de atributos.</span><span class="sxs-lookup"><span data-stu-id="0161e-356">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="0161e-357">Algunos atributos como `MinimumLength` no se pueden aplicar con la API fluida.</span><span class="sxs-lookup"><span data-stu-id="0161e-357">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="0161e-358">`MinimumLength` no cambia el esquema, solo aplica una regla de validación de longitud mínima.</span><span class="sxs-lookup"><span data-stu-id="0161e-358">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="0161e-359">Algunos desarrolladores prefieren usar la API fluida exclusivamente para mantener "limpias" las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="0161e-359">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="0161e-360">Se pueden mezclar atributos y la API fluida.</span><span class="sxs-lookup"><span data-stu-id="0161e-360">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="0161e-361">Hay algunas configuraciones que solo se pueden realizar con la API fluida (especificando una clave principal compuesta).</span><span class="sxs-lookup"><span data-stu-id="0161e-361">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="0161e-362">Hay algunas configuraciones que solo se pueden realizar con atributos (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="0161e-362">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="0161e-363">La práctica recomendada para el uso de atributos o API fluida:</span><span class="sxs-lookup"><span data-stu-id="0161e-363">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="0161e-364">Elija uno de estos dos enfoques.</span><span class="sxs-lookup"><span data-stu-id="0161e-364">Choose one of these two approaches.</span></span>
* <span data-ttu-id="0161e-365">Use el enfoque elegido de forma tan coherente como sea posible.</span><span class="sxs-lookup"><span data-stu-id="0161e-365">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="0161e-366">Algunos de los atributos utilizados en este tutorial se usan para:</span><span class="sxs-lookup"><span data-stu-id="0161e-366">Some of the attributes used in this tutorial are used for:</span></span>

* <span data-ttu-id="0161e-367">Solo validación (por ejemplo, `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="0161e-367">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="0161e-368">Solo configuración de EF Core (por ejemplo, `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="0161e-368">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="0161e-369">Validación y configuración de EF Core (por ejemplo, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="0161e-369">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="0161e-370">Para obtener más información sobre la diferencia entre los atributos y la API fluida, vea [Métodos de configuración](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="0161e-370">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram"></a><span data-ttu-id="0161e-371">Diagrama de entidades</span><span class="sxs-lookup"><span data-stu-id="0161e-371">Entity diagram</span></span>

<span data-ttu-id="0161e-372">En la siguiente ilustración se muestra el diagrama creado por EF Power Tools para el modelo School completado.</span><span class="sxs-lookup"><span data-stu-id="0161e-372">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagrama de entidades](complex-data-model/_static/diagram.png)

<span data-ttu-id="0161e-374">El diagrama anterior muestra:</span><span class="sxs-lookup"><span data-stu-id="0161e-374">The preceding diagram shows:</span></span>

* <span data-ttu-id="0161e-375">Varias líneas de relación uno a varios (1 a \*).</span><span class="sxs-lookup"><span data-stu-id="0161e-375">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="0161e-376">La línea de relación de uno a cero o uno (1 a 0..1) entre las entidades `Instructor` y `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="0161e-376">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="0161e-377">La línea de relación de cero o uno o varios (0..1 a \*) entre las entidades `Instructor` y `Department`.</span><span class="sxs-lookup"><span data-stu-id="0161e-377">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="0161e-378">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="0161e-378">Seed the database</span></span>

<span data-ttu-id="0161e-379">Actualice el código en *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="0161e-379">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu30/Data/DbInitializer.cs)]

<span data-ttu-id="0161e-380">El código anterior proporciona datos de inicialización para las nuevas entidades.</span><span class="sxs-lookup"><span data-stu-id="0161e-380">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="0161e-381">La mayor parte de este código crea objetos de entidad y carga los datos de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="0161e-381">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="0161e-382">Los datos de ejemplo se usan para pruebas.</span><span class="sxs-lookup"><span data-stu-id="0161e-382">The sample data is used for testing.</span></span> <span data-ttu-id="0161e-383">Consulte `Enrollments` y `CourseAssignments` para obtener ejemplos de cómo pueden inicializarse las tablas de combinación de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="0161e-383">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="0161e-384">Agregar una migración</span><span class="sxs-lookup"><span data-stu-id="0161e-384">Add a migration</span></span>

<span data-ttu-id="0161e-385">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="0161e-385">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0161e-386">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0161e-386">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0161e-387">En la Consola del administrador de paquetes, ejecute el comando siguiente.</span><span class="sxs-lookup"><span data-stu-id="0161e-387">In PMC, run the following command.</span></span>

```powershell
Add-Migration ComplexDataModel
```

<span data-ttu-id="0161e-388">El comando anterior muestra una advertencia sobre la posible pérdida de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-388">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="0161e-389">Si se ejecuta el comando `database update`, se genera el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-389">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="0161e-390">En la sección siguiente, verá qué puede hacer con respecto a este error.</span><span class="sxs-lookup"><span data-stu-id="0161e-390">In the next section, you see what to do about this error.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0161e-391">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0161e-391">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0161e-392">Si agrega una migración y ejecuta el comando `database update`, se producirá el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-392">If you add a migration and run the `database update` command, the following error would be produced:</span></span>

```text
SQLite does not support this migration operation ('DropForeignKeyOperation').
For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
```

<span data-ttu-id="0161e-393">En la sección siguiente, verá cómo evitar este error.</span><span class="sxs-lookup"><span data-stu-id="0161e-393">In the next section, you see how to avoid this error.</span></span>

---

## <a name="apply-the-migration-or-drop-and-re-create"></a><span data-ttu-id="0161e-394">Aplicar la migración o quitar y volver a crear</span><span class="sxs-lookup"><span data-stu-id="0161e-394">Apply the migration or drop and re-create</span></span>

<span data-ttu-id="0161e-395">Ahora que tiene una base de datos existente, debe pensar en cómo aplicarle cambios.</span><span class="sxs-lookup"><span data-stu-id="0161e-395">Now that you have an existing database, you need to think about how to apply changes to it.</span></span> <span data-ttu-id="0161e-396">En este tutorial se muestran dos alternativas:</span><span class="sxs-lookup"><span data-stu-id="0161e-396">This tutorial shows two alternatives:</span></span>

* <span data-ttu-id="0161e-397">[Quitar y volver a crear la base de datos](#drop).</span><span class="sxs-lookup"><span data-stu-id="0161e-397">[Drop and re-create the database](#drop).</span></span> <span data-ttu-id="0161e-398">Elija esta sección si va a usar SQLite.</span><span class="sxs-lookup"><span data-stu-id="0161e-398">Choose this section if you're using SQLite.</span></span>
* <span data-ttu-id="0161e-399">[Aplicar la migración a la base de datos existente](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="0161e-399">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="0161e-400">Las instrucciones de esta sección solo funcionan para SQL Server, **no para SQLite**.</span><span class="sxs-lookup"><span data-stu-id="0161e-400">The instructions in this section work for SQL Server only, **not for SQLite**.</span></span> 

<span data-ttu-id="0161e-401">Ambas opciones funcionan para SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0161e-401">Either choice works for SQL Server.</span></span> <span data-ttu-id="0161e-402">Aunque el método de aplicación de la migración es más complejo y lento, es el enfoque preferido para entornos de producción del mundo real.</span><span class="sxs-lookup"><span data-stu-id="0161e-402">While the apply-migration method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> 

<a name="drop"></a>

## <a name="drop-and-re-create-the-database"></a><span data-ttu-id="0161e-403">Quitar y volver a crear la base de datos</span><span class="sxs-lookup"><span data-stu-id="0161e-403">Drop and re-create the database</span></span>

<span data-ttu-id="0161e-404">[Omita esta sección](#apply-the-migration) si usa SQL Server y quiere realizar el enfoque de aplicación de la migración en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="0161e-404">[Skip this section](#apply-the-migration) if you're using SQL Server and want to do the apply-migration approach in the following section.</span></span>

<span data-ttu-id="0161e-405">Para obligar a EF Core a crear una base de datos, quite y actualice la base de datos:</span><span class="sxs-lookup"><span data-stu-id="0161e-405">To force EF Core to create a new database, drop and update the database:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0161e-406">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0161e-406">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0161e-407">En la **Consola del Administrador de paquetes** (PMC), ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-407">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Drop-Database
  ```

* <span data-ttu-id="0161e-408">Elimine la carpeta *Migrations* y, después, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-408">Delete the *Migrations* folder, then run the following command:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0161e-409">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0161e-409">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0161e-410">Abra una ventana de comandos y desplácese hasta la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="0161e-410">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="0161e-411">La carpeta del proyecto contiene el archivo *ContosoUniversity.csproj*.</span><span class="sxs-lookup"><span data-stu-id="0161e-411">The project folder contains the *ContosoUniversity.csproj* file.</span></span>

* <span data-ttu-id="0161e-412">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="0161e-412">Run the following command:</span></span>

  ```console
  dotnet ef database drop --force
  ```

* <span data-ttu-id="0161e-413">Elimine la carpeta *Migrations* y, después, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-413">Delete the *Migrations* folder, then run the following command:</span></span>

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

---

<span data-ttu-id="0161e-414">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0161e-414">Run the app.</span></span> <span data-ttu-id="0161e-415">Ejecutar la aplicación ejecuta el método `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="0161e-415">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="0161e-416">`DbInitializer.Initialize` rellena la base de datos nueva.</span><span class="sxs-lookup"><span data-stu-id="0161e-416">The `DbInitializer.Initialize` populates the new database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0161e-417">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0161e-417">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0161e-418">Abra la base de datos en SSOX:</span><span class="sxs-lookup"><span data-stu-id="0161e-418">Open the database in SSOX:</span></span>

* <span data-ttu-id="0161e-419">Si anteriormente se abrió SSOX, haga clic en el botón **Actualizar**.</span><span class="sxs-lookup"><span data-stu-id="0161e-419">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="0161e-420">Expanda el nodo **Tablas**.</span><span class="sxs-lookup"><span data-stu-id="0161e-420">Expand the **Tables** node.</span></span> <span data-ttu-id="0161e-421">Se muestran las tablas creadas.</span><span class="sxs-lookup"><span data-stu-id="0161e-421">The created tables are displayed.</span></span>

  ![Tablas en SSOX](complex-data-model/_static/ssox-tables.png)

* <span data-ttu-id="0161e-423">Examine la tabla **CourseAssignment**:</span><span class="sxs-lookup"><span data-stu-id="0161e-423">Examine the **CourseAssignment** table:</span></span>

  * <span data-ttu-id="0161e-424">Haga clic con el botón derecho en la tabla **CourseAssignment** y seleccione **Ver datos**.</span><span class="sxs-lookup"><span data-stu-id="0161e-424">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
  * <span data-ttu-id="0161e-425">Compruebe que la tabla **CourseAssignment** contiene datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-425">Verify the **CourseAssignment** table contains data.</span></span>

  ![Datos de CourseAssignment en SSOX](complex-data-model/_static/ssox-ci-data.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0161e-427">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0161e-427">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0161e-428">Use la herramienta SQLite para examinar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="0161e-428">Use your SQLite tool to examine the database:</span></span>

* <span data-ttu-id="0161e-429">Nuevas tablas y columnas.</span><span class="sxs-lookup"><span data-stu-id="0161e-429">New tables and columns.</span></span>
* <span data-ttu-id="0161e-430">Datos inicializados en tablas, por ejemplo, la tabla **CourseAssignment**.</span><span class="sxs-lookup"><span data-stu-id="0161e-430">Seeded data in tables, for example the **CourseAssignment** table.</span></span>

---

<a name="applyexisting"></a>

## <a name="apply-the-migration"></a><span data-ttu-id="0161e-431">Aplicar la migración</span><span class="sxs-lookup"><span data-stu-id="0161e-431">Apply the migration</span></span>

<span data-ttu-id="0161e-432">Esta sección es opcional.</span><span class="sxs-lookup"><span data-stu-id="0161e-432">This section is optional.</span></span> <span data-ttu-id="0161e-433">Estos pasos solo funcionan para SQL Server LocalDB y si ha pasado por alto la sección [Quitar y volver a crear la base de datos](#drop) anterior.</span><span class="sxs-lookup"><span data-stu-id="0161e-433">These steps work only for SQL Server LocalDB and only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="0161e-434">Cuando se ejecutan migraciones con datos existentes, puede haber restricciones de clave externa que no se cumplen con los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="0161e-434">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="0161e-435">Con los datos de producción, se deben realizar algunos pasos para migrar los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="0161e-435">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="0161e-436">En esta sección se proporciona un ejemplo de corrección de las infracciones de restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="0161e-436">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="0161e-437">No realice estos cambios de código sin hacer una copia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="0161e-437">Don't make these code changes without a backup.</span></span> <span data-ttu-id="0161e-438">No realice estos cambios de código si ha completado la sección anterior [Quitar y volver a crear la base de datos](#drop).</span><span class="sxs-lookup"><span data-stu-id="0161e-438">Don't make these code changes if you completed the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="0161e-439">El archivo *{marca_de_tiempo}_ComplexDataModel.cs* contiene el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-439">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="0161e-440">El código anterior agrega una clave externa `DepartmentID` que acepta valores NULL a la tabla `Course`.</span><span class="sxs-lookup"><span data-stu-id="0161e-440">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="0161e-441">La base de datos del tutorial anterior contiene filas en `Course`, por lo que esa tabla no se puede actualizar mediante migraciones.</span><span class="sxs-lookup"><span data-stu-id="0161e-441">The database from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="0161e-442">Para realizar la migración de `ComplexDataModel`, trabaje con los datos existentes:</span><span class="sxs-lookup"><span data-stu-id="0161e-442">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="0161e-443">Cambie el código para asignar a la nueva columna (`DepartmentID`) un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0161e-443">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="0161e-444">Cree un departamento falso denominado "Temp" para que actúe como el departamento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0161e-444">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="0161e-445">Corregir las restricciones de clave externa</span><span class="sxs-lookup"><span data-stu-id="0161e-445">Fix the foreign key constraints</span></span>

<span data-ttu-id="0161e-446">En la clase de migración `ComplexDataModel`, actualice el método `Up`:</span><span class="sxs-lookup"><span data-stu-id="0161e-446">In the `ComplexDataModel` migration class, update the `Up` method:</span></span>

* <span data-ttu-id="0161e-447">Abra el archivo *{marca_de_tiempo}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="0161e-447">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="0161e-448">Convierta en comentario la línea de código que agrega la columna `DepartmentID` a la tabla `Course`.</span><span class="sxs-lookup"><span data-stu-id="0161e-448">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="0161e-449">Agregue el código resaltado siguiente.</span><span class="sxs-lookup"><span data-stu-id="0161e-449">Add the following highlighted code.</span></span> <span data-ttu-id="0161e-450">El nuevo código va después del bloque `.CreateTable( name: "Department"`:</span><span class="sxs-lookup"><span data-stu-id="0161e-450">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

<span data-ttu-id="0161e-451">[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=23-31)]</span><span class="sxs-lookup"><span data-stu-id="0161e-451">[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=23-31)]</span></span>

<span data-ttu-id="0161e-452">Con los cambios anteriores, las filas `Course` existentes estarán relacionadas con el departamento "Temp" después de ejecutar el método `ComplexDataModel.Up`.</span><span class="sxs-lookup"><span data-stu-id="0161e-452">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel.Up` method runs.</span></span>

<span data-ttu-id="0161e-453">Para este tutorial se ha simplificado la manera de controlar la situación que se muestra aquí.</span><span class="sxs-lookup"><span data-stu-id="0161e-453">The way of handling the situation shown here is simplified for this tutorial.</span></span> <span data-ttu-id="0161e-454">Una aplicación de producción debería:</span><span class="sxs-lookup"><span data-stu-id="0161e-454">A production app would:</span></span>

* <span data-ttu-id="0161e-455">Incluir código o scripts para agregar filas de `Department` y filas de `Course` relacionadas a las nuevas filas de `Department`.</span><span class="sxs-lookup"><span data-stu-id="0161e-455">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="0161e-456">No use el departamento "Temp" o el valor predeterminado de `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="0161e-456">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0161e-457">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0161e-457">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0161e-458">En la **Consola del Administrador de paquetes** (PMC), ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-458">In the **Package Manager Console** (PMC), run the following command:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="0161e-459">Como el método `DbInitializer.Initialize` está diseñado para funcionar solo con una base de datos vacía, use SSOX para eliminar todas las filas de las tablas Student y Course.</span><span class="sxs-lookup"><span data-stu-id="0161e-459">Because the `DbInitializer.Initialize` method is designed to work only with an empty database, use SSOX to delete all the rows in the Student and Course tables.</span></span> <span data-ttu-id="0161e-460">(La eliminación en cascada se encargará de la tabla Enrollment).</span><span class="sxs-lookup"><span data-stu-id="0161e-460">(Cascade delete will take care of the Enrollment table.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0161e-461">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0161e-461">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0161e-462">Si usa SQL Server LocalDB con Visual Studio Code, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-462">If you're using SQL Server LocalDB with Visual Studio Code, run the following command:</span></span>

  ```console
  dotnet ef database update
  ```

---

<span data-ttu-id="0161e-463">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0161e-463">Run the app.</span></span> <span data-ttu-id="0161e-464">Ejecutar la aplicación ejecuta el método `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="0161e-464">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="0161e-465">`DbInitializer.Initialize` rellena la base de datos nueva.</span><span class="sxs-lookup"><span data-stu-id="0161e-465">The `DbInitializer.Initialize` populates the new database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0161e-466">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="0161e-466">Next steps</span></span>

<span data-ttu-id="0161e-467">En los dos tutoriales siguientes se muestra cómo leer y actualizar los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="0161e-467">The next two tutorials show how to read and update related data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0161e-468">[Tutorial anterior](xref:data/ef-rp/migrations)
> [Tutorial siguiente](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="0161e-468">[Previous tutorial](xref:data/ef-rp/migrations)
[Next tutorial](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0161e-469">En los tutoriales anteriores se trabajaba con un modelo de datos básico que se componía de tres entidades.</span><span class="sxs-lookup"><span data-stu-id="0161e-469">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="0161e-470">En este tutorial:</span><span class="sxs-lookup"><span data-stu-id="0161e-470">In this tutorial:</span></span>

* <span data-ttu-id="0161e-471">Se agregan más entidades y relaciones.</span><span class="sxs-lookup"><span data-stu-id="0161e-471">More entities and relationships are added.</span></span>
* <span data-ttu-id="0161e-472">Se personaliza el modelo de datos especificando el formato, la validación y las reglas de asignación de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-472">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="0161e-473">Las clases de entidad para el modelo de datos completado se muestran en la ilustración siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-473">The entity classes for the completed data model are shown in the following illustration:</span></span>

![Diagrama de entidades](complex-data-model/_static/diagram.png)

<span data-ttu-id="0161e-475">Si experimenta problemas que no puede resolver, descargue la [aplicación completada](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="0161e-475">If you run into problems you can't solve, download the [completed app](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="0161e-476">Personalizar el modelo de datos con atributos</span><span class="sxs-lookup"><span data-stu-id="0161e-476">Customize the data model with attributes</span></span>

<span data-ttu-id="0161e-477">En esta sección, se personaliza el modelo de datos mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="0161e-477">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="0161e-478">El atributo DataType</span><span class="sxs-lookup"><span data-stu-id="0161e-478">The DataType attribute</span></span>

<span data-ttu-id="0161e-479">Las páginas de alumno actualmente muestran la hora de la fecha de inscripción.</span><span class="sxs-lookup"><span data-stu-id="0161e-479">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="0161e-480">Normalmente, los campos de fecha muestran solo la fecha y no la hora.</span><span class="sxs-lookup"><span data-stu-id="0161e-480">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="0161e-481">Actualice *Models/Student.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="0161e-481">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="0161e-482">El atributo [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) especifica un tipo de datos más específico que el tipo intrínseco de base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-482">The [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="0161e-483">En este caso solo se debe mostrar la fecha, no la fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="0161e-483">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="0161e-484">La [enumeración DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) proporciona muchos tipos de datos, como Date (Fecha), Time (Hora), PhoneNumber (Número de teléfono), Currency (Divisa), EmailAddress (Dirección de correo electrónico), etc. El atributo `DataType` también puede permitir que la aplicación proporcione automáticamente características específicas del tipo.</span><span class="sxs-lookup"><span data-stu-id="0161e-484">The [DataType Enumeration](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="0161e-485">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0161e-485">For example:</span></span>

* <span data-ttu-id="0161e-486">El vínculo `mailto:` se crea automáticamente para `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="0161e-486">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="0161e-487">El selector de fecha se proporciona para `DataType.Date` en la mayoría de los exploradores.</span><span class="sxs-lookup"><span data-stu-id="0161e-487">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="0161e-488">El atributo `DataType` emite atributos HTML 5 `data-` (se pronuncia "datos dash") para su uso por parte de los exploradores HTML 5.</span><span class="sxs-lookup"><span data-stu-id="0161e-488">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="0161e-489">Los atributos `DataType` no proporcionan validación.</span><span class="sxs-lookup"><span data-stu-id="0161e-489">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="0161e-490">`DataType.Date` no especifica el formato de la fecha que se muestra.</span><span class="sxs-lookup"><span data-stu-id="0161e-490">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="0161e-491">De manera predeterminada, el campo de fecha se muestra según los formatos predeterminados basados en el elemento [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) del servidor.</span><span class="sxs-lookup"><span data-stu-id="0161e-491">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="0161e-492">El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:</span><span class="sxs-lookup"><span data-stu-id="0161e-492">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="0161e-493">La configuración `ApplyFormatInEditMode` especifica que el formato también debe aplicarse a la interfaz de usuario de edición.</span><span class="sxs-lookup"><span data-stu-id="0161e-493">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="0161e-494">Algunos campos no deben usar `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="0161e-494">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="0161e-495">Por ejemplo, el símbolo de divisa generalmente no debe mostrarse en un cuadro de texto de edición.</span><span class="sxs-lookup"><span data-stu-id="0161e-495">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="0161e-496">El atributo `DisplayFormat` puede usarse por sí solo.</span><span class="sxs-lookup"><span data-stu-id="0161e-496">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="0161e-497">Normalmente se recomienda usar el atributo `DataType` con el atributo `DisplayFormat`.</span><span class="sxs-lookup"><span data-stu-id="0161e-497">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="0161e-498">El atributo `DataType` transmite la semántica de los datos en lugar de cómo se representan en una pantalla.</span><span class="sxs-lookup"><span data-stu-id="0161e-498">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="0161e-499">El atributo `DataType` proporciona las siguientes ventajas que no están disponibles en `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="0161e-499">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="0161e-500">El explorador puede habilitar características de HTML5.</span><span class="sxs-lookup"><span data-stu-id="0161e-500">The browser can enable HTML5 features.</span></span> <span data-ttu-id="0161e-501">Por ejemplo, mostrar un control de calendario, el símbolo de divisa adecuado según la configuración regional, vínculos de correo electrónico, validación de entradas del lado cliente, etc.</span><span class="sxs-lookup"><span data-stu-id="0161e-501">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="0161e-502">De manera predeterminada, el explorador representa los datos con el formato correcto según la configuración regional.</span><span class="sxs-lookup"><span data-stu-id="0161e-502">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="0161e-503">Para obtener más información, vea la [documentación del asistente de etiquetas \<entrada&gt;](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="0161e-503">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="0161e-504">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0161e-504">Run the app.</span></span> <span data-ttu-id="0161e-505">Vaya a la página de índice de Students.</span><span class="sxs-lookup"><span data-stu-id="0161e-505">Navigate to the Students Index page.</span></span> <span data-ttu-id="0161e-506">Ya no se muestran las horas.</span><span class="sxs-lookup"><span data-stu-id="0161e-506">Times are no longer displayed.</span></span> <span data-ttu-id="0161e-507">Todas las vistas que usa el modelo `Student` muestran la fecha sin hora.</span><span class="sxs-lookup"><span data-stu-id="0161e-507">Every view that uses the `Student` model displays the date without time.</span></span>

![Página de índice de estudiantes en la que se muestran las fechas sin horas](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="0161e-509">El atributo StringLength</span><span class="sxs-lookup"><span data-stu-id="0161e-509">The StringLength attribute</span></span>

<span data-ttu-id="0161e-510">Las reglas de validación de datos y los mensajes de error de validación se pueden especificar con atributos.</span><span class="sxs-lookup"><span data-stu-id="0161e-510">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="0161e-511">El atributo [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) especifica la longitud mínima y máxima de caracteres que se permite en un campo de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-511">The [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="0161e-512">El atributo `StringLength` también proporciona validación del lado cliente y del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="0161e-512">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="0161e-513">El valor mínimo no influye en el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-513">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="0161e-514">Actualice el modelo `Student` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-514">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="0161e-515">El código anterior limita los nombres a no más de 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="0161e-515">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="0161e-516">El atributo `StringLength` no impide que un usuario escriba un espacio en blanco para un nombre.</span><span class="sxs-lookup"><span data-stu-id="0161e-516">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="0161e-517">El atributo [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) se usa para aplicar restricciones a la entrada.</span><span class="sxs-lookup"><span data-stu-id="0161e-517">The [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="0161e-518">Por ejemplo, el código siguiente requiere que el primer carácter sea una letra mayúscula y el resto de caracteres sean alfabéticos:</span><span class="sxs-lookup"><span data-stu-id="0161e-518">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="0161e-519">Ejecute la aplicación:</span><span class="sxs-lookup"><span data-stu-id="0161e-519">Run the app:</span></span>

* <span data-ttu-id="0161e-520">Vaya a la página Students.</span><span class="sxs-lookup"><span data-stu-id="0161e-520">Navigate to the Students page.</span></span>
* <span data-ttu-id="0161e-521">Seleccione **Create New** y escriba un nombre más de 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="0161e-521">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="0161e-522">Seleccione **Create**, la validación del lado cliente muestra un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="0161e-522">Select **Create**, client-side validation shows an error message.</span></span>

![Página de índice de estudiantes en la que se muestran errores de longitud de cadena](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="0161e-524">En el **Explorador de objetos de SQL Server**, (SSOX) abra el diseñador de tablas de Student haciendo doble clic en la tabla **Student**.</span><span class="sxs-lookup"><span data-stu-id="0161e-524">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabla de estudiantes en SSOX antes de las migraciones](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="0161e-526">La imagen anterior muestra el esquema para la tabla `Student`.</span><span class="sxs-lookup"><span data-stu-id="0161e-526">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="0161e-527">Los campos de nombre tienen tipo `nvarchar(MAX)` porque las migraciones no se han ejecutado en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-527">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="0161e-528">Cuando se ejecutan las migraciones más adelante en este tutorial, los campos de nombre se convierten en `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="0161e-528">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="0161e-529">El atributo Column</span><span class="sxs-lookup"><span data-stu-id="0161e-529">The Column attribute</span></span>

<span data-ttu-id="0161e-530">Los atributos pueden controlar cómo se asignan las clases y propiedades a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-530">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="0161e-531">En esta sección, el atributo `Column` se usa para asignar el nombre de la propiedad `FirstMidName` a "FirstName" en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-531">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="0161e-532">Cuando se crea la base de datos, los nombres de propiedad en el modelo se usan para los nombres de columna (excepto cuando se usa el atributo `Column`).</span><span class="sxs-lookup"><span data-stu-id="0161e-532">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="0161e-533">El modelo `Student` usa `FirstMidName` para el nombre de campo por la posibilidad de que el campo contenga también un segundo nombre.</span><span class="sxs-lookup"><span data-stu-id="0161e-533">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="0161e-534">Actualice el archivo *Models/Student.cs* con el siguiente código resaltado:</span><span class="sxs-lookup"><span data-stu-id="0161e-534">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="0161e-535">Con el cambio anterior, `Student.FirstMidName` en la aplicación se asigna a la columna `FirstName` de la tabla `Student`.</span><span class="sxs-lookup"><span data-stu-id="0161e-535">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="0161e-536">La adición del atributo `Column` cambia el modelo de respaldo de `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="0161e-536">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="0161e-537">El modelo que está haciendo la copia de seguridad de `SchoolContext` ya no coincide con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-537">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="0161e-538">Si la aplicación se ejecuta antes de aplicar las migraciones, se genera la siguiente excepción:</span><span class="sxs-lookup"><span data-stu-id="0161e-538">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```

<span data-ttu-id="0161e-539">Para actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="0161e-539">To update the DB:</span></span>

* <span data-ttu-id="0161e-540">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="0161e-540">Build the project.</span></span>
* <span data-ttu-id="0161e-541">Abra una ventana de comandos en la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="0161e-541">Open a command window in the project folder.</span></span> <span data-ttu-id="0161e-542">Escriba los comandos siguientes para crear una migración y actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="0161e-542">Enter the following commands to create a new migration and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0161e-543">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0161e-543">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0161e-544">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0161e-544">Visual Studio Code</span></span>](#tab/visual-studio-code)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

---

<span data-ttu-id="0161e-545">El comando `migrations add ColumnFirstName` genera el siguiente mensaje de advertencia:</span><span class="sxs-lookup"><span data-stu-id="0161e-545">The `migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="0161e-546">La advertencia se genera porque los campos de nombre ahora están limitados a 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="0161e-546">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="0161e-547">Si un nombre en la base de datos tenía más de 50 caracteres, se perderían desde el 51 hasta el último carácter.</span><span class="sxs-lookup"><span data-stu-id="0161e-547">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="0161e-548">Pruebe la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0161e-548">Test the app.</span></span>

<span data-ttu-id="0161e-549">Abra la tabla de estudiantes en SSOX:</span><span class="sxs-lookup"><span data-stu-id="0161e-549">Open the Student table in SSOX:</span></span>

![Tabla de estudiantes en SSOX después de las migraciones](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="0161e-551">Antes de aplicar la migración, las columnas de nombre eran de tipo [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="0161e-551">Before migration was applied, the name columns were of type [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="0161e-552">Las columnas de nombre ahora son `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="0161e-552">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="0161e-553">El nombre de columna ha cambiado de `FirstMidName` a `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="0161e-553">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="0161e-554">En la sección siguiente, la creación de la aplicación en algunas de las fases genera errores del compilador.</span><span class="sxs-lookup"><span data-stu-id="0161e-554">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="0161e-555">Las instrucciones especifican cuándo se debe compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0161e-555">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="0161e-556">Actualizar la entidad Student</span><span class="sxs-lookup"><span data-stu-id="0161e-556">Student entity update</span></span>

![Entidad Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="0161e-558">Actualice *Models/Student.cs* con el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="0161e-558">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="0161e-559">El atributo Required</span><span class="sxs-lookup"><span data-stu-id="0161e-559">The Required attribute</span></span>

<span data-ttu-id="0161e-560">El atributo `Required` hace que las propiedades de nombre sean campos obligatorios.</span><span class="sxs-lookup"><span data-stu-id="0161e-560">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="0161e-561">El atributo `Required` no es necesario para los tipos que no aceptan valores NULL, como los tipos de valor (`DateTime`, `int`, `double`, etc.).</span><span class="sxs-lookup"><span data-stu-id="0161e-561">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="0161e-562">Los tipos que no aceptan valores NULL se tratan automáticamente como campos obligatorios.</span><span class="sxs-lookup"><span data-stu-id="0161e-562">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="0161e-563">El atributo `Required` se podría reemplazar con un parámetro de longitud mínima en el atributo `StringLength`:</span><span class="sxs-lookup"><span data-stu-id="0161e-563">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="0161e-564">El atributo Display</span><span class="sxs-lookup"><span data-stu-id="0161e-564">The Display attribute</span></span>

<span data-ttu-id="0161e-565">El atributo `Display` especifica que el título de los cuadros de texto debe ser "First Name", "Last Name", "Full Name" y "Enrollment Date".</span><span class="sxs-lookup"><span data-stu-id="0161e-565">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="0161e-566">Los títulos predeterminados no tenían ningún espacio de división de palabras, por ejemplo "Lastname".</span><span class="sxs-lookup"><span data-stu-id="0161e-566">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="0161e-567">La propiedad calculada FullName</span><span class="sxs-lookup"><span data-stu-id="0161e-567">The FullName calculated property</span></span>

<span data-ttu-id="0161e-568">`FullName` es una propiedad calculada que devuelve un valor que se crea mediante la concatenación de otras dos propiedades.</span><span class="sxs-lookup"><span data-stu-id="0161e-568">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="0161e-569">No se puede establecer `FullName`, tiene solo un descriptor de acceso get.</span><span class="sxs-lookup"><span data-stu-id="0161e-569">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="0161e-570">No se crea ninguna columna `FullName` en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-570">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="0161e-571">Crear la entidad Instructor</span><span class="sxs-lookup"><span data-stu-id="0161e-571">Create the Instructor Entity</span></span>

![La entidad Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="0161e-573">Cree *Models/Instructor.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-573">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

<span data-ttu-id="0161e-574">En una sola línea puede haber varios atributos.</span><span class="sxs-lookup"><span data-stu-id="0161e-574">Multiple attributes can be on one line.</span></span> <span data-ttu-id="0161e-575">Los atributos `HireDate` pudieron escribirse de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-575">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="0161e-576">Las propiedades de navegación CourseAssignments y OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="0161e-576">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="0161e-577">`CourseAssignments` y `OfficeAssignment` son propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="0161e-577">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="0161e-578">Un instructor puede impartir cualquier número de cursos, por lo que `CourseAssignments` se define como una colección.</span><span class="sxs-lookup"><span data-stu-id="0161e-578">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="0161e-579">Si una propiedad de navegación contiene varias entidades:</span><span class="sxs-lookup"><span data-stu-id="0161e-579">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="0161e-580">Debe ser un tipo de lista, donde se pueden agregar, eliminar y actualizar las entradas.</span><span class="sxs-lookup"><span data-stu-id="0161e-580">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="0161e-581">Los tipos de propiedad de navegación incluyen:</span><span class="sxs-lookup"><span data-stu-id="0161e-581">Navigation property types include:</span></span>

* `ICollection<T>`
* `List<T>`
* `HashSet<T>`

<span data-ttu-id="0161e-582">Si se especifica `ICollection<T>`, EF Core crea una colección `HashSet<T>` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0161e-582">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="0161e-583">La entidad `CourseAssignment` se explica en la sección sobre las relaciones de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="0161e-583">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="0161e-584">Las reglas de negocio de Contoso University establecen que un instructor puede tener, a lo sumo, una oficina.</span><span class="sxs-lookup"><span data-stu-id="0161e-584">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="0161e-585">La propiedad `OfficeAssignment` contiene una única instancia de `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="0161e-585">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="0161e-586">`OfficeAssignment` es NULL si no se asigna ninguna oficina.</span><span class="sxs-lookup"><span data-stu-id="0161e-586">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="0161e-587">Crear la entidad OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="0161e-587">Create the OfficeAssignment entity</span></span>

![Entidad OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="0161e-589">Cree *Models/OfficeAssignment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-589">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="0161e-590">El atributo Key</span><span class="sxs-lookup"><span data-stu-id="0161e-590">The Key attribute</span></span>

<span data-ttu-id="0161e-591">El atributo `[Key]` se usa para identificar una propiedad como la clave principal (PK) cuando el nombre de propiedad es diferente de classnameID o ID.</span><span class="sxs-lookup"><span data-stu-id="0161e-591">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="0161e-592">Hay una relación de uno a cero o uno entre las entidades `Instructor` y `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="0161e-592">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="0161e-593">Solo existe una asignación de oficina en relación con el instructor a la que está asignada.</span><span class="sxs-lookup"><span data-stu-id="0161e-593">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="0161e-594">La clave principal de `OfficeAssignment` también es la clave externa (FK) para la entidad `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="0161e-594">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="0161e-595">EF Core no reconoce automáticamente `InstructorID` como la clave principal de `OfficeAssignment` porque:</span><span class="sxs-lookup"><span data-stu-id="0161e-595">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="0161e-596">`InstructorID` no sigue la convención de nomenclatura de ID o classnameID.</span><span class="sxs-lookup"><span data-stu-id="0161e-596">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="0161e-597">Por tanto, se usa el atributo `Key` para identificar `InstructorID` como la clave principal:</span><span class="sxs-lookup"><span data-stu-id="0161e-597">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="0161e-598">De forma predeterminada, EF Core trata la clave como no generada por la base de datos porque la columna es para una relación de identificación.</span><span class="sxs-lookup"><span data-stu-id="0161e-598">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="0161e-599">La propiedad de navegación Instructor</span><span class="sxs-lookup"><span data-stu-id="0161e-599">The Instructor navigation property</span></span>

<span data-ttu-id="0161e-600">La propiedad de navegación `OfficeAssignment` para la entidad `Instructor` acepta valores NULL porque:</span><span class="sxs-lookup"><span data-stu-id="0161e-600">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="0161e-601">Los tipos de referencia, como las clases, aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="0161e-601">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="0161e-602">Un instructor podría no tener una asignación de oficina.</span><span class="sxs-lookup"><span data-stu-id="0161e-602">An instructor might not have an office assignment.</span></span>

<span data-ttu-id="0161e-603">La entidad `OfficeAssignment` tiene una propiedad de navegación `Instructor` que no acepta valores NULL porque:</span><span class="sxs-lookup"><span data-stu-id="0161e-603">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="0161e-604">`InstructorID` no acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="0161e-604">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="0161e-605">Una asignación de oficina no puede existir sin un instructor.</span><span class="sxs-lookup"><span data-stu-id="0161e-605">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="0161e-606">Cuando una entidad `Instructor` tiene una entidad `OfficeAssignment` relacionada, cada entidad tiene una referencia a la otra en su propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="0161e-606">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="0161e-607">El atributo `[Required]` puede aplicarse a la propiedad de navegación `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="0161e-607">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="0161e-608">El código anterior especifica que debe haber un instructor relacionado.</span><span class="sxs-lookup"><span data-stu-id="0161e-608">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="0161e-609">El código anterior no es necesario porque la clave externa `InstructorID`, que también es la clave principal, no acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="0161e-609">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="0161e-610">Modificar la entidad Course</span><span class="sxs-lookup"><span data-stu-id="0161e-610">Modify the Course Entity</span></span>

![La entidad Course](complex-data-model/_static/course-entity.png)

<span data-ttu-id="0161e-612">Actualice *Models/Course.cs* con el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="0161e-612">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="0161e-613">La entidad `Course` tiene una propiedad de clave externa (FK) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="0161e-613">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="0161e-614">`DepartmentID` apunta a la entidad relacionada `Department`.</span><span class="sxs-lookup"><span data-stu-id="0161e-614">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="0161e-615">La entidad `Course` tiene una propiedad de navegación `Department`.</span><span class="sxs-lookup"><span data-stu-id="0161e-615">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="0161e-616">EF Core no requiere una propiedad de clave externa para un modelo de datos cuando el modelo tiene una propiedad de navegación para una entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="0161e-616">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="0161e-617">EF Core crea automáticamente claves externas en la base de datos siempre que se necesiten.</span><span class="sxs-lookup"><span data-stu-id="0161e-617">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="0161e-618">EF Core crea [propiedades paralelas](/ef/core/modeling/shadow-properties) para las claves externas creadas automáticamente.</span><span class="sxs-lookup"><span data-stu-id="0161e-618">EF Core creates [shadow properties](/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="0161e-619">Tener la clave externa en el modelo de datos puede hacer que las actualizaciones sean más sencillas y eficaces.</span><span class="sxs-lookup"><span data-stu-id="0161e-619">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="0161e-620">Por ejemplo, considere la posibilidad de un modelo donde la propiedad de la clave externa `DepartmentID` *no* está incluida.</span><span class="sxs-lookup"><span data-stu-id="0161e-620">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="0161e-621">Cuando se captura una entidad de curso para editar:</span><span class="sxs-lookup"><span data-stu-id="0161e-621">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="0161e-622">La entidad `Department` es NULL si no se carga explícitamente.</span><span class="sxs-lookup"><span data-stu-id="0161e-622">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="0161e-623">Para actualizar la entidad Course, la entidad `Department` debe capturarse en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="0161e-623">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="0161e-624">Cuando se incluye la propiedad de clave externa `DepartmentID` en el modelo de datos, no es necesario capturar la entidad `Department` antes de una actualización.</span><span class="sxs-lookup"><span data-stu-id="0161e-624">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="0161e-625">El atributo DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="0161e-625">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="0161e-626">El atributo `[DatabaseGenerated(DatabaseGeneratedOption.None)]` especifica que la aplicación proporciona la clave principal, en lugar de generarla la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-626">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="0161e-627">De forma predeterminada, EF Core da por supuesto que la base de datos genera valores de clave principal.</span><span class="sxs-lookup"><span data-stu-id="0161e-627">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="0161e-628">Los valores de clave principal generados por la base de datos suelen ser el mejor método.</span><span class="sxs-lookup"><span data-stu-id="0161e-628">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="0161e-629">Para las entidades `Course`, el usuario especifica la clave principal.</span><span class="sxs-lookup"><span data-stu-id="0161e-629">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="0161e-630">Por ejemplo, un número de curso como una serie de 1000 para el departamento de matemáticas, una serie de 2000 para el departamento de inglés.</span><span class="sxs-lookup"><span data-stu-id="0161e-630">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="0161e-631">También se puede usar el atributo `DatabaseGenerated` para generar valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="0161e-631">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="0161e-632">Por ejemplo, la base de datos puede generar automáticamente un campo de fecha para registrar la fecha en que se crea o actualiza una fila.</span><span class="sxs-lookup"><span data-stu-id="0161e-632">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="0161e-633">Para obtener más información, vea [Propiedades generadas](/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="0161e-633">For more information, see [Generated Properties](/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="0161e-634">Propiedades de clave externa y de navegación</span><span class="sxs-lookup"><span data-stu-id="0161e-634">Foreign key and navigation properties</span></span>

<span data-ttu-id="0161e-635">Las propiedades de clave externa (FK) y las de navegación de la entidad `Course` reflejan las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="0161e-635">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="0161e-636">Un curso se asigna a un departamento, por lo que hay una clave externa `DepartmentID` y una propiedad de navegación `Department`.</span><span class="sxs-lookup"><span data-stu-id="0161e-636">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="0161e-637">Un curso puede tener cualquier número de alumnos inscritos en él, por lo que la propiedad de navegación `Enrollments` es una colección:</span><span class="sxs-lookup"><span data-stu-id="0161e-637">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="0161e-638">Un curso puede ser impartido por varios instructores, por lo que la propiedad de navegación `CourseAssignments` es una colección:</span><span class="sxs-lookup"><span data-stu-id="0161e-638">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="0161e-639">`CourseAssignment` se explica [más adelante](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="0161e-639">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="0161e-640">Crear la entidad Department</span><span class="sxs-lookup"><span data-stu-id="0161e-640">Create the Department entity</span></span>

![La entidad Department](complex-data-model/_static/department-entity.png)

<span data-ttu-id="0161e-642">Cree *Models/Department.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-642">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="0161e-643">El atributo Column</span><span class="sxs-lookup"><span data-stu-id="0161e-643">The Column attribute</span></span>

<span data-ttu-id="0161e-644">Anteriormente se usó el atributo `Column` para cambiar la asignación de nombres de columna.</span><span class="sxs-lookup"><span data-stu-id="0161e-644">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="0161e-645">En el código de la entidad `Department`, se usó el atributo `Column` para cambiar la asignación de tipos de datos de SQL.</span><span class="sxs-lookup"><span data-stu-id="0161e-645">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="0161e-646">La columna `Budget` se define mediante el tipo money de SQL Server en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="0161e-646">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="0161e-647">Por lo general, la asignación de columnas no es necesaria.</span><span class="sxs-lookup"><span data-stu-id="0161e-647">Column mapping is generally not required.</span></span> <span data-ttu-id="0161e-648">EF Core generalmente elige el tipo de datos de SQL Server apropiado en función del tipo CLR para la propiedad.</span><span class="sxs-lookup"><span data-stu-id="0161e-648">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="0161e-649">El tipo CLR `decimal` se asigna a un tipo `decimal` de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0161e-649">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="0161e-650">`Budget` es para la divisa, y el tipo de datos money es más adecuado para la divisa.</span><span class="sxs-lookup"><span data-stu-id="0161e-650">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="0161e-651">Propiedades de clave externa y de navegación</span><span class="sxs-lookup"><span data-stu-id="0161e-651">Foreign key and navigation properties</span></span>

<span data-ttu-id="0161e-652">Las propiedades de clave externa y de navegación reflejan las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="0161e-652">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="0161e-653">Un departamento puede tener o no un administrador.</span><span class="sxs-lookup"><span data-stu-id="0161e-653">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="0161e-654">Un administrador siempre es un instructor.</span><span class="sxs-lookup"><span data-stu-id="0161e-654">An administrator is always an instructor.</span></span> <span data-ttu-id="0161e-655">Por lo tanto, la propiedad `InstructorID` se incluye como la clave externa para la entidad `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="0161e-655">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="0161e-656">La propiedad de navegación se denomina `Administrator` pero contiene una entidad `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="0161e-656">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="0161e-657">El signo de interrogación (?) en el código anterior especifica que la propiedad acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="0161e-657">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="0161e-658">Un departamento puede tener varios cursos, por lo que hay una propiedad de navegación Courses:</span><span class="sxs-lookup"><span data-stu-id="0161e-658">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="0161e-659">Nota: Por convención, EF Core permite la eliminación en cascada de las claves externas que no acepten valores NULL ni relaciones de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="0161e-659">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="0161e-660">La eliminación en cascada puede dar lugar a reglas de eliminación en cascada circular.</span><span class="sxs-lookup"><span data-stu-id="0161e-660">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="0161e-661">Las reglas de eliminación en cascada circular provocan una excepción cuando se agrega una migración.</span><span class="sxs-lookup"><span data-stu-id="0161e-661">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="0161e-662">Por ejemplo, si la propiedad `Department.InstructorID` no se ha definido como que acepta valores NULL:</span><span class="sxs-lookup"><span data-stu-id="0161e-662">For example, if the `Department.InstructorID` property was defined as non-nullable:</span></span>

* <span data-ttu-id="0161e-663">EF Core configura una regla de eliminación en cascada para eliminar el departamento cuando se elimina el instructor.</span><span class="sxs-lookup"><span data-stu-id="0161e-663">EF Core configures a cascade delete rule to delete the department when the instructor is deleted.</span></span>
* <span data-ttu-id="0161e-664">Eliminar el departamento cuando se elimine el instructor no es el comportamiento previsto.</span><span class="sxs-lookup"><span data-stu-id="0161e-664">Deleting the department when the instructor is deleted isn't the intended behavior.</span></span>
* <span data-ttu-id="0161e-665">La API fluida siguiente establecería una regla de restricción en lugar de en cascada.</span><span class="sxs-lookup"><span data-stu-id="0161e-665">The following fluent API would set a restrict rule instead of cascade.</span></span>

   ```csharp
   modelBuilder.Entity<Department>()
      .HasOne(d => d.Administrator)
      .WithMany()
      .OnDelete(DeleteBehavior.Restrict)
  ```

<span data-ttu-id="0161e-666">El código anterior deshabilita la eliminación en cascada en la relación de instructor y departamento.</span><span class="sxs-lookup"><span data-stu-id="0161e-666">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="0161e-667">Actualizar la entidad Enrollment</span><span class="sxs-lookup"><span data-stu-id="0161e-667">Update the Enrollment entity</span></span>

<span data-ttu-id="0161e-668">Un registro de inscripción corresponde a un curso realizado por un alumno.</span><span class="sxs-lookup"><span data-stu-id="0161e-668">An enrollment record is for one course taken by one student.</span></span>

![La entidad Enrollment](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="0161e-670">Actualice *Models/Enrollment.cs* con el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="0161e-670">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="0161e-671">Propiedades de clave externa y de navegación</span><span class="sxs-lookup"><span data-stu-id="0161e-671">Foreign key and navigation properties</span></span>

<span data-ttu-id="0161e-672">Las propiedades de clave externa y de navegación reflejan las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="0161e-672">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="0161e-673">Un registro de inscripción es para un curso, por lo que hay una propiedad de clave externa `CourseID` y una propiedad de navegación `Course`:</span><span class="sxs-lookup"><span data-stu-id="0161e-673">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="0161e-674">Un registro de inscripción es para un alumno, por lo que hay una propiedad de clave externa `StudentID` y una propiedad de navegación `Student`:</span><span class="sxs-lookup"><span data-stu-id="0161e-674">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="0161e-675">Relaciones Varios a Varios</span><span class="sxs-lookup"><span data-stu-id="0161e-675">Many-to-Many Relationships</span></span>

<span data-ttu-id="0161e-676">Hay una relación de varios a varios entre las entidades `Student` y `Course`.</span><span class="sxs-lookup"><span data-stu-id="0161e-676">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="0161e-677">La entidad `Enrollment` funciona como una tabla combinada varios a varios *con carga útil* en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-677">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="0161e-678">"Con carga útil" significa que la tabla `Enrollment` contiene datos adicionales, además de claves externas de las tablas combinadas (en este caso, la clave principal y `Grade`).</span><span class="sxs-lookup"><span data-stu-id="0161e-678">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="0161e-679">En la ilustración siguiente se muestra el aspecto de estas relaciones en un diagrama de entidades.</span><span class="sxs-lookup"><span data-stu-id="0161e-679">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="0161e-680">(Este diagrama se ha generado mediante [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) para EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="0161e-680">(This diagram was generated using [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) for EF 6.x.</span></span> <span data-ttu-id="0161e-681">Crear el diagrama no forma parte del tutorial).</span><span class="sxs-lookup"><span data-stu-id="0161e-681">Creating the diagram isn't part of the tutorial.)</span></span>

![Relación de varios a varios entre estudiantes y cursos](complex-data-model/_static/student-course.png)

<span data-ttu-id="0161e-683">Cada línea de relación tiene un 1 en un extremo y un asterisco (\*) en el otro, para indicar una relación uno a varios.</span><span class="sxs-lookup"><span data-stu-id="0161e-683">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="0161e-684">Si la tabla `Enrollment` no incluyera información de calificaciones, solo tendría que contener las dos claves externas (`CourseID` y `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="0161e-684">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="0161e-685">Una tabla combinada de varios a varios sin carga útil se suele denominar una tabla combinada pura (PJT).</span><span class="sxs-lookup"><span data-stu-id="0161e-685">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="0161e-686">Las entidades `Instructor` y `Course` tienen una relación de varios a varios con una tabla combinada pura.</span><span class="sxs-lookup"><span data-stu-id="0161e-686">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="0161e-687">Nota: EF 6.x es compatible con las tablas de combinación implícitas con relaciones de varios a varios, pero EF Core, no.</span><span class="sxs-lookup"><span data-stu-id="0161e-687">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="0161e-688">Para obtener más información, consulte [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/) (Relaciones de varios a varios en EF Core 2.0).</span><span class="sxs-lookup"><span data-stu-id="0161e-688">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="0161e-689">La entidad CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="0161e-689">The CourseAssignment entity</span></span>

![La entidad CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="0161e-691">Cree *Models/CourseAssignment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-691">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="0161e-692">Relación Instructor-to-Courses</span><span class="sxs-lookup"><span data-stu-id="0161e-692">Instructor-to-Courses</span></span>

![Relación de varios a varios Instructor-to-Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="0161e-694">La relación de varios a varios Instructor-to-Courses:</span><span class="sxs-lookup"><span data-stu-id="0161e-694">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="0161e-695">Requiere una tabla de combinación que debe estar representada por un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="0161e-695">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="0161e-696">Es una tabla combinada pura (tabla sin carga útil).</span><span class="sxs-lookup"><span data-stu-id="0161e-696">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="0161e-697">Es común denominar una entidad de combinación `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="0161e-697">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="0161e-698">Por ejemplo, la tabla de combinación Instructor-to-Courses usando este patrón es `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="0161e-698">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="0161e-699">Pero se recomienda usar un nombre que describa la relación.</span><span class="sxs-lookup"><span data-stu-id="0161e-699">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="0161e-700">Los modelos de datos empiezan de manera sencilla y crecen.</span><span class="sxs-lookup"><span data-stu-id="0161e-700">Data models start out simple and grow.</span></span> <span data-ttu-id="0161e-701">Las tablas combinadas sin carga útil (PJT) evolucionan con frecuencia para incluir la carga útil.</span><span class="sxs-lookup"><span data-stu-id="0161e-701">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="0161e-702">A partir de un nombre de entidad descriptivo, no es necesario cambiar el nombre cuando la tabla combinada cambia.</span><span class="sxs-lookup"><span data-stu-id="0161e-702">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="0161e-703">Idealmente, la entidad de combinación tendrá su propio nombre natural (posiblemente una sola palabra) en el dominio de empresa.</span><span class="sxs-lookup"><span data-stu-id="0161e-703">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="0161e-704">Por ejemplo, Books y Customers podrían vincularse a través de una entidad combinada denominada Ratings.</span><span class="sxs-lookup"><span data-stu-id="0161e-704">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="0161e-705">Para la relación de varios a varios Instructor-to-Courses, se prefiere `CourseAssignment` a `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="0161e-705">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="0161e-706">Clave compuesta</span><span class="sxs-lookup"><span data-stu-id="0161e-706">Composite key</span></span>

<span data-ttu-id="0161e-707">Las claves externas no aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="0161e-707">FKs are not nullable.</span></span> <span data-ttu-id="0161e-708">Las dos claves externas en `CourseAssignment` (`InstructorID` y `CourseID`) juntas identifican de forma única cada fila de la tabla `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="0161e-708">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="0161e-709">`CourseAssignment` no requiere una clave principal dedicada.</span><span class="sxs-lookup"><span data-stu-id="0161e-709">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="0161e-710">Las propiedades `InstructorID` y `CourseID` funcionan como una clave principal compuesta.</span><span class="sxs-lookup"><span data-stu-id="0161e-710">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="0161e-711">La única manera de especificar claves principales compuestas en EF Core es con la *API fluida*.</span><span class="sxs-lookup"><span data-stu-id="0161e-711">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="0161e-712">La sección siguiente muestra cómo configurar la clave principal compuesta.</span><span class="sxs-lookup"><span data-stu-id="0161e-712">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="0161e-713">La clave compuesta asegura que:</span><span class="sxs-lookup"><span data-stu-id="0161e-713">The composite key ensures:</span></span>

* <span data-ttu-id="0161e-714">Se permiten varias filas para un curso.</span><span class="sxs-lookup"><span data-stu-id="0161e-714">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="0161e-715">Se permiten varias filas para un instructor.</span><span class="sxs-lookup"><span data-stu-id="0161e-715">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="0161e-716">No se permiten varias filas para el mismo instructor y curso.</span><span class="sxs-lookup"><span data-stu-id="0161e-716">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="0161e-717">La entidad de combinación `Enrollment` define su propia clave principal, por lo que este tipo de duplicados son posibles.</span><span class="sxs-lookup"><span data-stu-id="0161e-717">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="0161e-718">Para evitar los duplicados:</span><span class="sxs-lookup"><span data-stu-id="0161e-718">To prevent such duplicates:</span></span>

* <span data-ttu-id="0161e-719">Agregue un índice único en los campos de clave externa, o</span><span class="sxs-lookup"><span data-stu-id="0161e-719">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="0161e-720">Configure `Enrollment` con una clave compuesta principal similar a `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="0161e-720">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="0161e-721">Para obtener más información, vea [Índices](/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="0161e-721">For more information, see [Indexes](/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="0161e-722">Actualizar el contexto de la base de datos</span><span class="sxs-lookup"><span data-stu-id="0161e-722">Update the DB context</span></span>

<span data-ttu-id="0161e-723">Agregue el código resaltado siguiente a *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="0161e-723">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="0161e-724">El código anterior agrega las nuevas entidades y configura la clave principal compuesta de la entidad `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="0161e-724">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="0161e-725">Alternativa de la API fluida a los atributos</span><span class="sxs-lookup"><span data-stu-id="0161e-725">Fluent API alternative to attributes</span></span>

<span data-ttu-id="0161e-726">El método `OnModelCreating` del código anterior usa la *API fluida* para configurar el comportamiento de EF Core.</span><span class="sxs-lookup"><span data-stu-id="0161e-726">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="0161e-727">La API se denomina "fluida" porque a menudo se usa para encadenar una serie de llamadas de método en una única instrucción.</span><span class="sxs-lookup"><span data-stu-id="0161e-727">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="0161e-728">El [código siguiente](/ef/core/modeling/#use-fluent-api-to-configure-a-model) es un ejemplo de la API fluida:</span><span class="sxs-lookup"><span data-stu-id="0161e-728">The [following code](/ef/core/modeling/#use-fluent-api-to-configure-a-model) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="0161e-729">En este tutorial, la API fluida se usa solo para la asignación de base de datos que no se puede realizar con atributos.</span><span class="sxs-lookup"><span data-stu-id="0161e-729">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="0161e-730">Pero la API fluida puede especificar casi todas las reglas de formato, validación y asignación que se pueden realizar mediante el uso de atributos.</span><span class="sxs-lookup"><span data-stu-id="0161e-730">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="0161e-731">Algunos atributos como `MinimumLength` no se pueden aplicar con la API fluida.</span><span class="sxs-lookup"><span data-stu-id="0161e-731">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="0161e-732">`MinimumLength` no cambia el esquema, solo aplica una regla de validación de longitud mínima.</span><span class="sxs-lookup"><span data-stu-id="0161e-732">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="0161e-733">Algunos desarrolladores prefieren usar la API fluida exclusivamente para mantener "limpias" las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="0161e-733">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="0161e-734">Se pueden mezclar atributos y la API fluida.</span><span class="sxs-lookup"><span data-stu-id="0161e-734">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="0161e-735">Hay algunas configuraciones que solo se pueden realizar con la API fluida (especificando una clave principal compuesta).</span><span class="sxs-lookup"><span data-stu-id="0161e-735">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="0161e-736">Hay algunas configuraciones que solo se pueden realizar con atributos (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="0161e-736">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="0161e-737">La práctica recomendada para el uso de atributos o API fluida:</span><span class="sxs-lookup"><span data-stu-id="0161e-737">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="0161e-738">Elija uno de estos dos enfoques.</span><span class="sxs-lookup"><span data-stu-id="0161e-738">Choose one of these two approaches.</span></span>
* <span data-ttu-id="0161e-739">Use el enfoque elegido de forma tan coherente como sea posible.</span><span class="sxs-lookup"><span data-stu-id="0161e-739">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="0161e-740">Algunos de los atributos utilizados en este tutorial se usan para:</span><span class="sxs-lookup"><span data-stu-id="0161e-740">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="0161e-741">Solo validación (por ejemplo, `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="0161e-741">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="0161e-742">Solo configuración de EF Core (por ejemplo, `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="0161e-742">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="0161e-743">Validación y configuración de EF Core (por ejemplo, `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="0161e-743">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="0161e-744">Para obtener más información sobre la diferencia entre los atributos y la API fluida, vea [Métodos de configuración](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="0161e-744">For more information about attributes vs. fluent API, see [Methods of configuration](/ef/core/modeling/).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="0161e-745">Diagrama de entidades en el que se muestran las relaciones</span><span class="sxs-lookup"><span data-stu-id="0161e-745">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="0161e-746">En la siguiente ilustración se muestra el diagrama creado por EF Power Tools para el modelo School completado.</span><span class="sxs-lookup"><span data-stu-id="0161e-746">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Diagrama de entidades](complex-data-model/_static/diagram.png)

<span data-ttu-id="0161e-748">El diagrama anterior muestra:</span><span class="sxs-lookup"><span data-stu-id="0161e-748">The preceding diagram shows:</span></span>

* <span data-ttu-id="0161e-749">Varias líneas de relación uno a varios (1 a \*).</span><span class="sxs-lookup"><span data-stu-id="0161e-749">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="0161e-750">La línea de relación de uno a cero o uno (1 a 0..1) entre las entidades `Instructor` y `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="0161e-750">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="0161e-751">La línea de relación de cero o uno o varios (0..1 a \*) entre las entidades `Instructor` y `Department`.</span><span class="sxs-lookup"><span data-stu-id="0161e-751">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="0161e-752">Inicializar la base de datos con datos de prueba</span><span class="sxs-lookup"><span data-stu-id="0161e-752">Seed the DB with Test Data</span></span>

<span data-ttu-id="0161e-753">Actualice el código en *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="0161e-753">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="0161e-754">El código anterior proporciona datos de inicialización para las nuevas entidades.</span><span class="sxs-lookup"><span data-stu-id="0161e-754">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="0161e-755">La mayor parte de este código crea objetos de entidad y carga los datos de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="0161e-755">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="0161e-756">Los datos de ejemplo se usan para pruebas.</span><span class="sxs-lookup"><span data-stu-id="0161e-756">The sample data is used for testing.</span></span> <span data-ttu-id="0161e-757">Consulte `Enrollments` y `CourseAssignments` para obtener ejemplos de cómo pueden inicializarse las tablas de combinación de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="0161e-757">See `Enrollments` and `CourseAssignments` for examples of how many-to-many join tables can be seeded.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="0161e-758">Agregar una migración</span><span class="sxs-lookup"><span data-stu-id="0161e-758">Add a migration</span></span>

<span data-ttu-id="0161e-759">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="0161e-759">Build the project.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0161e-760">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0161e-760">Visual Studio</span></span>](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0161e-761">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0161e-761">Visual Studio Code</span></span>](#tab/visual-studio-code)

```console
dotnet ef migrations add ComplexDataModel
```

---

<span data-ttu-id="0161e-762">El comando anterior muestra una advertencia sobre la posible pérdida de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-762">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="0161e-763">Si se ejecuta el comando `database update`, se genera el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-763">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a><span data-ttu-id="0161e-764">Aplicar la migración</span><span class="sxs-lookup"><span data-stu-id="0161e-764">Apply the migration</span></span>

<span data-ttu-id="0161e-765">Ahora que tiene una base de datos existente, debe pensar cómo aplicar los cambios futuros en ella.</span><span class="sxs-lookup"><span data-stu-id="0161e-765">Now that you have an existing database, you need to think about how to apply future changes to it.</span></span> <span data-ttu-id="0161e-766">En este tutorial se muestran dos enfoques:</span><span class="sxs-lookup"><span data-stu-id="0161e-766">This tutorial shows two approaches:</span></span>

* [<span data-ttu-id="0161e-767">Quitar y volver a crear la base de datos</span><span class="sxs-lookup"><span data-stu-id="0161e-767">Drop and re-create the database</span></span>](#drop)
* <span data-ttu-id="0161e-768">[Aplicar la migración a la base de datos existente](#applyexisting).</span><span class="sxs-lookup"><span data-stu-id="0161e-768">[Apply the migration to the existing database](#applyexisting).</span></span> <span data-ttu-id="0161e-769">Aunque este método es más complejo y lento, es el método preferido para entornos de producción del mundo real.</span><span class="sxs-lookup"><span data-stu-id="0161e-769">While this method is more complex and time-consuming, it's the preferred approach for real-world, production environments.</span></span> <span data-ttu-id="0161e-770">**Nota**: Esta sección del tutorial es opcional.</span><span class="sxs-lookup"><span data-stu-id="0161e-770">**Note**: This is an optional section of the tutorial.</span></span> <span data-ttu-id="0161e-771">Puede realizar la operación de quitar y volver a crear, y omitir esta sección.</span><span class="sxs-lookup"><span data-stu-id="0161e-771">You can do the drop and re-create steps and skip this section.</span></span> <span data-ttu-id="0161e-772">Si quiere seguir los pasos descritos en esta sección, no realice la operación de quitar y volver a crear.</span><span class="sxs-lookup"><span data-stu-id="0161e-772">If you do want to follow the steps in this section, don't do the drop and re-create steps.</span></span> 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="0161e-773">Quitar y volver a crear la base de datos</span><span class="sxs-lookup"><span data-stu-id="0161e-773">Drop and re-create the database</span></span>

<span data-ttu-id="0161e-774">El código en la `DbInitializer` actualizada agrega los datos de inicialización para las nuevas entidades.</span><span class="sxs-lookup"><span data-stu-id="0161e-774">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="0161e-775">Para obligar a EF Core a crear una base de datos, quite y actualice la base de datos:</span><span class="sxs-lookup"><span data-stu-id="0161e-775">To force EF Core to create a new  DB, drop and update the DB:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0161e-776">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0161e-776">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0161e-777">En la **Consola del Administrador de paquetes** (PMC), ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-777">In the **Package Manager Console** (PMC), run the following command:</span></span>

```PMC
Drop-Database
Update-Database
```

<span data-ttu-id="0161e-778">Ejecute `Get-Help about_EntityFrameworkCore` desde PMC para obtener información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="0161e-778">Run `Get-Help about_EntityFrameworkCore` from the PMC to get help information.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0161e-779">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0161e-779">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0161e-780">Abra una ventana de comandos y desplácese hasta la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="0161e-780">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="0161e-781">La carpeta del proyecto contiene el archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="0161e-781">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="0161e-782">Escriba lo siguiente en la ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="0161e-782">Enter the following in the command window:</span></span>

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

---

<span data-ttu-id="0161e-783">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0161e-783">Run the app.</span></span> <span data-ttu-id="0161e-784">Ejecutar la aplicación ejecuta el método `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="0161e-784">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="0161e-785">`DbInitializer.Initialize` rellena la base de datos nueva.</span><span class="sxs-lookup"><span data-stu-id="0161e-785">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="0161e-786">Abra la base de datos en SSOX:</span><span class="sxs-lookup"><span data-stu-id="0161e-786">Open the DB in SSOX:</span></span>

* <span data-ttu-id="0161e-787">Si anteriormente se abrió SSOX, haga clic en el botón **Actualizar**.</span><span class="sxs-lookup"><span data-stu-id="0161e-787">If SSOX was opened previously, click the **Refresh** button.</span></span>
* <span data-ttu-id="0161e-788">Expanda el nodo **Tablas**.</span><span class="sxs-lookup"><span data-stu-id="0161e-788">Expand the **Tables** node.</span></span> <span data-ttu-id="0161e-789">Se muestran las tablas creadas.</span><span class="sxs-lookup"><span data-stu-id="0161e-789">The created tables are displayed.</span></span>

![Tablas en SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="0161e-791">Examine la tabla **CourseAssignment**:</span><span class="sxs-lookup"><span data-stu-id="0161e-791">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="0161e-792">Haga clic con el botón derecho en la tabla **CourseAssignment** y seleccione **Ver datos**.</span><span class="sxs-lookup"><span data-stu-id="0161e-792">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="0161e-793">Compruebe que la tabla **CourseAssignment** contiene datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-793">Verify the **CourseAssignment** table contains data.</span></span>

![Datos de CourseAssignment en SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a><span data-ttu-id="0161e-795">Aplicar la migración a la base de datos existente</span><span class="sxs-lookup"><span data-stu-id="0161e-795">Apply the migration to the existing database</span></span>

<span data-ttu-id="0161e-796">Esta sección es opcional.</span><span class="sxs-lookup"><span data-stu-id="0161e-796">This section is optional.</span></span> <span data-ttu-id="0161e-797">Estos pasos solo funcionan si pasó por alto la sección [Quitar y volver a crear la base de datos](#drop).</span><span class="sxs-lookup"><span data-stu-id="0161e-797">These steps work only if you skipped the preceding [Drop and re-create the database](#drop) section.</span></span>

<span data-ttu-id="0161e-798">Cuando se ejecutan migraciones con datos existentes, puede haber restricciones de clave externa que no se cumplen con los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="0161e-798">When migrations are run with existing data, there may be FK constraints that are not satisfied with the existing data.</span></span> <span data-ttu-id="0161e-799">Con los datos de producción, se deben realizar algunos pasos para migrar los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="0161e-799">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="0161e-800">En esta sección se proporciona un ejemplo de corrección de las infracciones de restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="0161e-800">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="0161e-801">No realice estos cambios de código sin hacer una copia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="0161e-801">Don't make these code changes without a backup.</span></span> <span data-ttu-id="0161e-802">No realice estos cambios de código si realizó la sección anterior y actualizó la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0161e-802">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="0161e-803">El archivo *{marca_de_tiempo}_ComplexDataModel.cs* contiene el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="0161e-803">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="0161e-804">El código anterior agrega una clave externa `DepartmentID` que acepta valores NULL a la tabla `Course`.</span><span class="sxs-lookup"><span data-stu-id="0161e-804">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="0161e-805">La base de datos del tutorial anterior contiene filas en `Course`, por lo que no se puede actualizar esa tabla mediante migraciones.</span><span class="sxs-lookup"><span data-stu-id="0161e-805">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="0161e-806">Para realizar la migración de `ComplexDataModel`, trabaje con los datos existentes:</span><span class="sxs-lookup"><span data-stu-id="0161e-806">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="0161e-807">Cambie el código para asignar a la nueva columna (`DepartmentID`) un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0161e-807">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="0161e-808">Cree un departamento falso denominado "Temp" para que actúe como el departamento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0161e-808">Create a fake department named "Temp" to act as the default department.</span></span>

#### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="0161e-809">Corregir las restricciones de clave externa</span><span class="sxs-lookup"><span data-stu-id="0161e-809">Fix the foreign key constraints</span></span>

<span data-ttu-id="0161e-810">Actualice el método `Up` de las clases `ComplexDataModel`:</span><span class="sxs-lookup"><span data-stu-id="0161e-810">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="0161e-811">Abra el archivo *{marca_de_tiempo}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="0161e-811">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="0161e-812">Convierta en comentario la línea de código que agrega la columna `DepartmentID` a la tabla `Course`.</span><span class="sxs-lookup"><span data-stu-id="0161e-812">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="0161e-813">Agregue el código resaltado siguiente.</span><span class="sxs-lookup"><span data-stu-id="0161e-813">Add the following highlighted code.</span></span> <span data-ttu-id="0161e-814">El nuevo código va después del bloque `.CreateTable( name: "Department"`:</span><span class="sxs-lookup"><span data-stu-id="0161e-814">The new code goes after the `.CreateTable( name: "Department"` block:</span></span>

 [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="0161e-815">Con los cambios anteriores, las filas `Course` existentes estarán relacionadas con el departamento "Temp" después de ejecutar el método `ComplexDataModel` de `Up`.</span><span class="sxs-lookup"><span data-stu-id="0161e-815">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="0161e-816">Una aplicación de producción debería:</span><span class="sxs-lookup"><span data-stu-id="0161e-816">A production app would:</span></span>

* <span data-ttu-id="0161e-817">Incluir código o scripts para agregar filas de `Department` y filas de `Course` relacionadas a las nuevas filas de `Department`.</span><span class="sxs-lookup"><span data-stu-id="0161e-817">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="0161e-818">No use el departamento "Temp" o el valor predeterminado de `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="0161e-818">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="0161e-819">El siguiente tutorial trata los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="0161e-819">The next tutorial covers related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0161e-820">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0161e-820">Additional resources</span></span>

* [<span data-ttu-id="0161e-821">Versión en YouTube de este tutorial (parte 1)</span><span class="sxs-lookup"><span data-stu-id="0161e-821">YouTube version of this tutorial(Part 1)</span></span>](https://www.youtube.com/watch?v=0n2f0ObgCoA)
* [<span data-ttu-id="0161e-822">Versión en YouTube de este tutorial (parte 2)</span><span class="sxs-lookup"><span data-stu-id="0161e-822">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=Je0Z5K1TNmY)



> [!div class="step-by-step"]
> <span data-ttu-id="0161e-823">[Anterior](xref:data/ef-rp/migrations)
> [Siguiente](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="0161e-823">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>

::: moniker-end