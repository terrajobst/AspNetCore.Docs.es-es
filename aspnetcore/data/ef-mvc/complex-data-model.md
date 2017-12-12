---
title: "Núcleo de ASP.NET MVC con EF Core - modelo de datos: 5 de 10"
author: tdykstra
description: "En este tutorial agregará más entidades y relaciones y personalizar el modelo de datos mediante la especificación de formato, validación y reglas de asignación de la base de datos."
keywords: Anotaciones de datos de ASP.NET Core, Entity Framework Core,
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 0dd63913-a041-48b6-96a4-3aeaedbdf5d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: cacb23441e5f5ab06c6be27f3068276f21ff4ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a><span data-ttu-id="65b28-104">Crear un modelo de datos complejos - Core EF con el tutorial de MVC de ASP.NET Core (5 de 10)</span><span class="sxs-lookup"><span data-stu-id="65b28-104">Creating a complex data model - EF Core with ASP.NET Core MVC tutorial (5 of 10)</span></span>

<span data-ttu-id="65b28-105">Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="65b28-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="65b28-106">La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core con Entity Framework Core y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="65b28-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="65b28-107">Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="65b28-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="65b28-108">En los tutoriales anteriores, ha trabajado con un modelo de datos simple que se compone de tres entidades.</span><span class="sxs-lookup"><span data-stu-id="65b28-108">In the previous tutorials, you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="65b28-109">En este tutorial, agregará más entidades y relaciones y personalizará el modelo de datos mediante la especificación de formato, validación y reglas de asignación de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="65b28-109">In this tutorial, you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="65b28-110">Cuando haya terminado, las clases de entidad que conformarán el modelo de datos completa que se muestra en la siguiente ilustración:</span><span class="sxs-lookup"><span data-stu-id="65b28-110">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![Diagrama de entidad](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="65b28-112">Personalizar el modelo de datos mediante el uso de atributos</span><span class="sxs-lookup"><span data-stu-id="65b28-112">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="65b28-113">En esta sección verá cómo personalizar el modelo de datos mediante el uso de atributos que especifican el formato, validación y las reglas de asignación de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="65b28-113">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="65b28-114">A continuación, en varias de las siguientes secciones que creará el modelo de datos School completando mediante la adición de atributos a las clases ya ha creado y crear clases nuevas para los demás tipos de entidad en el modelo.</span><span class="sxs-lookup"><span data-stu-id="65b28-114">Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="65b28-115">El atributo de tipo de datos</span><span class="sxs-lookup"><span data-stu-id="65b28-115">The DataType attribute</span></span>

<span data-ttu-id="65b28-116">Para las fechas de inscripción de estudiantes, todas las páginas web mostrar actualmente la hora junto con la fecha, aunque todo lo que le interesa para este campo es la fecha.</span><span class="sxs-lookup"><span data-stu-id="65b28-116">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="65b28-117">Con los atributos de anotación de datos, puede realizar un cambio que se solucionará el formato de presentación en cada vista que muestra los datos de código.</span><span class="sxs-lookup"><span data-stu-id="65b28-117">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="65b28-118">Para ver un ejemplo de cómo hacerlo, deberá agregar un atributo a la `EnrollmentDate` propiedad en la `Student` clase.</span><span class="sxs-lookup"><span data-stu-id="65b28-118">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="65b28-119">En *Models/Student.cs*, agregar un `using` instrucción para el `System.ComponentModel.DataAnnotations` espacio de nombres y agregue `DataType` y `DisplayFormat` atributos a la `EnrollmentDate` propiedad, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="65b28-119">In *Models/Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="65b28-120">El atributo `DataType` se usa para especificar un tipo de datos más específico que el tipo intrínseco de base de datos.</span><span class="sxs-lookup"><span data-stu-id="65b28-120">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="65b28-121">En este caso sólo desea realizar un seguimiento de la fecha, no la fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="65b28-121">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="65b28-122">El `DataType` enumeración proporciona para muchos tipos de datos, como fecha, hora, número de teléfono, moneda, EmailAddress y mucho más.</span><span class="sxs-lookup"><span data-stu-id="65b28-122">The  `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="65b28-123">El atributo `DataType` también puede permitir que la aplicación proporcione automáticamente características específicas del tipo.</span><span class="sxs-lookup"><span data-stu-id="65b28-123">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="65b28-124">Por ejemplo, se puede crear un vínculo `mailto:` para `DataType.EmailAddress` y se puede proporcionar un selector de datos para `DataType.Date` en exploradores compatibles con HTML5.</span><span class="sxs-lookup"><span data-stu-id="65b28-124">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="65b28-125">El `DataType` atributo emite HTML 5 `data-` atributos (pronunciado datos dash) que pueden entender exploradores HTML 5.</span><span class="sxs-lookup"><span data-stu-id="65b28-125">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="65b28-126">El `DataType` atributos no proporcionan ninguna validación.</span><span class="sxs-lookup"><span data-stu-id="65b28-126">The `DataType` attributes do not provide any validation.</span></span>

<span data-ttu-id="65b28-127">`DataType.Date` no especifica el formato de la fecha que se muestra.</span><span class="sxs-lookup"><span data-stu-id="65b28-127">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="65b28-128">De forma predeterminada, se muestra el campo de datos según los formatos predeterminados según CultureInfo del servidor.</span><span class="sxs-lookup"><span data-stu-id="65b28-128">By default, the data field is displayed according to the default formats based on the server's CultureInfo.</span></span>

<span data-ttu-id="65b28-129">El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:</span><span class="sxs-lookup"><span data-stu-id="65b28-129">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="65b28-130">El valor `ApplyFormatInEditMode` especifica que el formato se debe aplicar también cuando el valor se muestra en un cuadro de texto para su edición.</span><span class="sxs-lookup"><span data-stu-id="65b28-130">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="65b28-131">(No conviene que para algunos campos, por ejemplo, para los valores de moneda, no puede el símbolo de moneda en el cuadro de texto para su edición.)</span><span class="sxs-lookup"><span data-stu-id="65b28-131">(You might not want that for some fields -- for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="65b28-132">Puede usar el `DisplayFormat` atributo por sí mismo, pero suele ser una buena idea usar el `DataType` atributo también.</span><span class="sxs-lookup"><span data-stu-id="65b28-132">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute also.</span></span> <span data-ttu-id="65b28-133">El `DataType` atributo transmite la semántica de los datos en lugar de cómo representar en una pantalla y ofrece las siguientes ventajas que no obtendrá con `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="65b28-133">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

* <span data-ttu-id="65b28-134">El explorador puede habilitar características de HTML5 (por ejemplo para mostrar un control de calendario, el símbolo de moneda adecuado para la configuración regional, los vínculos de correo electrónico, algunos comandos de cliente de entrada de validación, etcetera.).</span><span class="sxs-lookup"><span data-stu-id="65b28-134">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>

* <span data-ttu-id="65b28-135">De manera predeterminada, el explorador representa los datos con el formato correcto según la configuración regional.</span><span class="sxs-lookup"><span data-stu-id="65b28-135">By default, the browser will render data using the correct format based on your locale.</span></span>

<span data-ttu-id="65b28-136">Para obtener más información, consulte el [ \<entrada > documentación de la aplicación auxiliar de etiquetas](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="65b28-136">For more information, see the [\<input> tag helper documentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span></span>

<span data-ttu-id="65b28-137">Ejecutar la aplicación, vaya a la página de índice de los alumnos y tenga en cuenta que veces ya no se muestran las fechas de inscripción.</span><span class="sxs-lookup"><span data-stu-id="65b28-137">Run the app, go to the Students Index page and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="65b28-138">El mismo será true para cualquier vista que usa el modelo de estudiante.</span><span class="sxs-lookup"><span data-stu-id="65b28-138">The same will be true for any view that uses the Student model.</span></span>

![Página de índice de los alumnos mostrando las fechas sin veces](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="65b28-140">El atributo StringLength</span><span class="sxs-lookup"><span data-stu-id="65b28-140">The StringLength attribute</span></span>

<span data-ttu-id="65b28-141">También puede especificar reglas de validación de datos y mensajes de error de validación utilizando atributos.</span><span class="sxs-lookup"><span data-stu-id="65b28-141">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="65b28-142">El `StringLength` atributo establece la longitud máxima de la base de datos y proporciona el cliente y el lado servidor validación de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="65b28-142">The `StringLength` attribute sets the maximum length  in the database and provides client side and server side validation for ASP.NET MVC.</span></span> <span data-ttu-id="65b28-143">También puede especificar la longitud mínima de la cadena en este atributo, pero el valor mínimo no influye en el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="65b28-143">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="65b28-144">Imagine que desea asegurarse de que los usuarios no escriban más de 50 caracteres para un nombre.</span><span class="sxs-lookup"><span data-stu-id="65b28-144">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="65b28-145">Para agregar esta limitación, agregue `StringLength` atributos a la `LastName` y `FirstMidName` propiedades, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="65b28-145">To add this limitation, add `StringLength` attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="65b28-146">El `StringLength` atributo no impedir que un usuario introducir un espacio en blanco para un nombre.</span><span class="sxs-lookup"><span data-stu-id="65b28-146">The `StringLength` attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="65b28-147">Puede usar el `RegularExpression` atributo para aplicar restricciones a la entrada.</span><span class="sxs-lookup"><span data-stu-id="65b28-147">You can use the `RegularExpression` attribute to apply restrictions to the input.</span></span> <span data-ttu-id="65b28-148">Por ejemplo, el siguiente código requiere el primer carácter que se va letra mayúscula y el resto de caracteres sea alfabético:</span><span class="sxs-lookup"><span data-stu-id="65b28-148">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

<span data-ttu-id="65b28-149">El `MaxLength` atributo proporciona una funcionalidad similar a la `StringLength` atributo pero no proporciona el cliente validación.</span><span class="sxs-lookup"><span data-stu-id="65b28-149">The `MaxLength` attribute provides functionality similar to the `StringLength` attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="65b28-150">Ahora ha cambiado el modelo de base de datos de forma que requiera un cambio en el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="65b28-150">The database model has now changed in a way that requires a change in the database schema.</span></span> <span data-ttu-id="65b28-151">Deberá usar migraciones para actualizar el esquema sin perder los datos que ha agregado a la base de datos mediante el interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="65b28-151">You'll use migrations to update the schema without losing any data that you may have added to the database by using the application UI.</span></span>

<span data-ttu-id="65b28-152">Guarde los cambios y compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="65b28-152">Save your changes and build the project.</span></span> <span data-ttu-id="65b28-153">A continuación, abra la ventana de comandos en la carpeta de proyecto y escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="65b28-153">Then open the command window in the project folder and enter the following commands:</span></span>

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

<span data-ttu-id="65b28-154">El `migrations add` comando advierte de que se puede producir pérdida de datos, porque hace que el cambio de la longitud máxima más corta de dos columnas.</span><span class="sxs-lookup"><span data-stu-id="65b28-154">The `migrations add` command warns that data loss may occur, because the change makes the maximum length shorter for two columns.</span></span>  <span data-ttu-id="65b28-155">Migraciones crea un archivo denominado  *\<timeStamp > _MaxLengthOnNames.cs*.</span><span class="sxs-lookup"><span data-stu-id="65b28-155">Migrations creates a file named *\<timeStamp>_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="65b28-156">Este archivo contiene código en el `Up` método que actualizará la base de datos para que coincida con el modelo de datos actual.</span><span class="sxs-lookup"><span data-stu-id="65b28-156">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="65b28-157">El `database update` que el código ejecutó el comando.</span><span class="sxs-lookup"><span data-stu-id="65b28-157">The `database update` command ran that code.</span></span>

<span data-ttu-id="65b28-158">La marca de tiempo como precedida el nombre de archivo de las migraciones se usa por Entity Framework para ordenar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="65b28-158">The timestamp prefixed to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="65b28-159">Puede crear varias migraciones antes de ejecutar el comando de actualización de bases de datos y, a continuación, todas las migraciones se aplican en el orden en el que se crearon.</span><span class="sxs-lookup"><span data-stu-id="65b28-159">You can create multiple migrations before running the update-database command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="65b28-160">Ejecutar la aplicación, seleccione la **estudiantes** , haga clic en **crear nuevo**y escriba cualquier nombre de más de 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="65b28-160">Run the app, select the **Students** tab, click **Create New**, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="65b28-161">Al hacer clic en **crear**, validación del lado cliente muestra un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="65b28-161">When you click **Create**, client side validation shows an error message.</span></span>

![Página que muestra los errores de longitud de cadena de índice de estudiantes](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a><span data-ttu-id="65b28-163">El atributo de columna</span><span class="sxs-lookup"><span data-stu-id="65b28-163">The Column attribute</span></span>

<span data-ttu-id="65b28-164">También puede usar atributos para controlar cómo se asignan las clases y propiedades para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="65b28-164">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="65b28-165">Imagine que hubiera usado el nombre `FirstMidName` para el nombre de campo ya que el campo puede contener también un nombre de medio.</span><span class="sxs-lookup"><span data-stu-id="65b28-165">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="65b28-166">Pero desea que la columna de base de datos se denomine `FirstName`, ya que los usuarios que se va a escribir las consultas ad hoc en la base de datos están acostumbrados a dicho nombre.</span><span class="sxs-lookup"><span data-stu-id="65b28-166">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="65b28-167">Para realizar esta asignación, puede usar el `Column` atributo.</span><span class="sxs-lookup"><span data-stu-id="65b28-167">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="65b28-168">El `Column` atributo especifica que cuando se crea la base de datos, la columna de la `Student` tabla que se asigna a la `FirstMidName` propiedad se denominará `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="65b28-168">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="65b28-169">En otras palabras, cuando el código hace referencia a `Student.FirstMidName`, los datos proceden o actualizados en el `FirstName` columna de la `Student` tabla.</span><span class="sxs-lookup"><span data-stu-id="65b28-169">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="65b28-170">Si no especifica nombres de columna, se facilita el mismo nombre que el nombre de propiedad.</span><span class="sxs-lookup"><span data-stu-id="65b28-170">If you don't specify column names, they are given the same name as the property name.</span></span>

<span data-ttu-id="65b28-171">En el *Student.cs* , agregue un `using` instrucción para `System.ComponentModel.DataAnnotations.Schema` y agregue el atributo de nombre de columna para el `FirstMidName` propiedad, como se muestra en el código resaltado siguiente:</span><span class="sxs-lookup"><span data-stu-id="65b28-171">In the *Student.cs* file, add a `using` statement for `System.ComponentModel.DataAnnotations.Schema` and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="65b28-172">La adición de la `Column` atributo cambia la copia de seguridad de modelo la `SchoolContext`, por lo que no coincida con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="65b28-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`, so it won't match the database.</span></span>

<span data-ttu-id="65b28-173">Guarde los cambios y compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="65b28-173">Save your changes and build the project.</span></span> <span data-ttu-id="65b28-174">A continuación, abra la ventana de comandos en la carpeta de proyecto y escriba los comandos siguientes para crear otra migración:</span><span class="sxs-lookup"><span data-stu-id="65b28-174">Then open the command window in the project folder and enter the following commands to create another migration:</span></span>

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

<span data-ttu-id="65b28-175">En **Explorador de objetos de SQL Server**, abra el Diseñador de tablas de Student haciendo doble clic en el **estudiante** tabla.</span><span class="sxs-lookup"><span data-stu-id="65b28-175">In **SQL Server Object Explorer**, open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabla de estudiantes en SSOX después de las migraciones](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="65b28-177">Antes de aplicar las dos primeras migraciones, eran las columnas del nombre de tipo nvarchar (max).</span><span class="sxs-lookup"><span data-stu-id="65b28-177">Before you applied the first two migrations, the name columns were of type nvarchar(MAX).</span></span> <span data-ttu-id="65b28-178">Ahora son nvarchar (50) y el nombre de la columna ha cambiado de FirstMidName a FirstName.</span><span class="sxs-lookup"><span data-stu-id="65b28-178">They are now nvarchar(50) and the column name has changed from FirstMidName to FirstName.</span></span>

> [!Note]
> <span data-ttu-id="65b28-179">Si intenta compilar antes de que termine de crear todas las clases de entidad en las secciones siguientes, podría obtener errores del compilador.</span><span class="sxs-lookup"><span data-stu-id="65b28-179">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>

## <a name="final-changes-to-the-student-entity"></a><span data-ttu-id="65b28-180">Cambios finales a la entidad Student</span><span class="sxs-lookup"><span data-stu-id="65b28-180">Final changes to the Student entity</span></span>

![Entidad Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="65b28-182">En *Models/Student.cs*, reemplace el código que agregó anteriormente con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="65b28-182">In *Models/Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="65b28-183">Los cambios aparecen resaltados.</span><span class="sxs-lookup"><span data-stu-id="65b28-183">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="65b28-184">El atributo requerido</span><span class="sxs-lookup"><span data-stu-id="65b28-184">The Required attribute</span></span>

<span data-ttu-id="65b28-185">El `Required` atributo hace que los campos obligatorios de propiedades de nombre.</span><span class="sxs-lookup"><span data-stu-id="65b28-185">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="65b28-186">El `Required` atributo no es necesario para los tipos que no aceptan valores NULL, como tipos de valor (fecha y hora, int, double, float, etcetera.).</span><span class="sxs-lookup"><span data-stu-id="65b28-186">The `Required` attribute is not needed for non-nullable types such as value types (DateTime, int, double, float, etc.).</span></span> <span data-ttu-id="65b28-187">Tipos que no pueden ser nulos se tratan automáticamente como campos obligatorios.</span><span class="sxs-lookup"><span data-stu-id="65b28-187">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="65b28-188">Puede quitar el `Required` de atributo y reemplácelo por un parámetro de longitud mínima para la `StringLength` atributo:</span><span class="sxs-lookup"><span data-stu-id="65b28-188">You could remove the `Required` attribute and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="65b28-189">El atributo de visualización</span><span class="sxs-lookup"><span data-stu-id="65b28-189">The Display attribute</span></span>

<span data-ttu-id="65b28-190">El `Display` atributo especifica que el título de los cuadros de texto debe ser "Nombre", "Last Name", "Nombre completo" y "Fecha de inscripción" en lugar del nombre de propiedad en cada instancia (que no tiene ningún espacio al dividir las palabras).</span><span class="sxs-lookup"><span data-stu-id="65b28-190">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="65b28-191">La propiedad FullName calculado</span><span class="sxs-lookup"><span data-stu-id="65b28-191">The FullName calculated property</span></span>

<span data-ttu-id="65b28-192">`FullName`es una propiedad calculada que devuelve un valor que se crea concatenando dos otras propiedades.</span><span class="sxs-lookup"><span data-stu-id="65b28-192">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="65b28-193">Por lo tanto, tiene solo un descriptor de acceso get y no `FullName` columna se genera en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="65b28-193">Therefore it has only a get accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="65b28-194">Crear la entidad Instructor</span><span class="sxs-lookup"><span data-stu-id="65b28-194">Create the Instructor Entity</span></span>

![Entidad instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="65b28-196">Crear *Models/Instructor.cs*, reemplace el código de plantilla con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="65b28-196">Create *Models/Instructor.cs*, replacing the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="65b28-197">Tenga en cuenta que varias propiedades son los mismos en las entidades de Student y Instructor.</span><span class="sxs-lookup"><span data-stu-id="65b28-197">Notice that several properties are the same in the Student and Instructor entities.</span></span> <span data-ttu-id="65b28-198">En el [implementar herencia](inheritance.md) tutorial más adelante en esta serie, deberá refactorizar este código para eliminar la redundancia.</span><span class="sxs-lookup"><span data-stu-id="65b28-198">In the [Implementing Inheritance](inheritance.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="65b28-199">Puede colocar varios atributos en una sola línea, por lo que también puede escribir el `HireDate` atributos como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="65b28-199">You can put multiple attributes on one line, so you could also write the `HireDate` attributes as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="65b28-200">Las propiedades de navegación CourseAssignments y OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="65b28-200">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="65b28-201">El `CourseAssignments` y `OfficeAssignment` las propiedades son propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="65b28-201">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="65b28-202">Un instructor puede enseñar a cualquier número de cursos, por lo que `CourseAssignments` se define como una colección.</span><span class="sxs-lookup"><span data-stu-id="65b28-202">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="65b28-203">Si una propiedad de navegación puede contener varias entidades, su tipo debe ser una lista en el que se pueden se agrega, elimina y actualiza las entradas.</span><span class="sxs-lookup"><span data-stu-id="65b28-203">If a navigation property can hold multiple entities, its type must be a list in which entries can be added, deleted, and updated.</span></span>  <span data-ttu-id="65b28-204">Puede especificar `ICollection<T>` o un tipo como `List<T>` o `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="65b28-204">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="65b28-205">Si especifica `ICollection<T>`, EF crea un `HashSet<T>` colección de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="65b28-205">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="65b28-206">El motivo por qué se trata `CourseAssignment` entidades se explica a continuación en la sección acerca de las relaciones de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="65b28-206">The reason why these are `CourseAssignment` entities is explained below in the section about many-to-many relationships.</span></span>

<span data-ttu-id="65b28-207">Estado de reglas de negocios de la Universidad de Contoso que un instructor solo puede tener a lo sumo una oficina, por lo que el `OfficeAssignment` propiedad contiene una única entidad OfficeAssignment (que puede ser null si no se asigna ningún office).</span><span class="sxs-lookup"><span data-stu-id="65b28-207">Contoso University business rules state that an instructor can only have at most one office, so the `OfficeAssignment` property holds a single OfficeAssignment entity (which may be null if no office is assigned).</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="65b28-208">Crear la entidad OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="65b28-208">Create the OfficeAssignment entity</span></span>

![Entidad OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="65b28-210">Crear *Models/OfficeAssignment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="65b28-210">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="65b28-211">Key (atributo)</span><span class="sxs-lookup"><span data-stu-id="65b28-211">The Key attribute</span></span>

<span data-ttu-id="65b28-212">Hay una relación de uno a cero o uno entre el Instructor y las entidades OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="65b28-212">There's a one-to-zero-or-one relationship  between the Instructor and the OfficeAssignment entities.</span></span> <span data-ttu-id="65b28-213">Solo existe una asignación de oficina en relación con el instructor se asigna a y, por lo tanto, su clave principal también es la clave externa para la entidad Instructor.</span><span class="sxs-lookup"><span data-stu-id="65b28-213">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity.</span></span> <span data-ttu-id="65b28-214">Pero Entity Framework no se reconoce automáticamente InstructorID como la clave principal de esta entidad porque su nombre no sigue la convención de nomenclatura de Id. o classnameID.</span><span class="sxs-lookup"><span data-stu-id="65b28-214">But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="65b28-215">Por lo tanto, la `Key` atributo se utiliza para identificarlo como la clave:</span><span class="sxs-lookup"><span data-stu-id="65b28-215">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="65b28-216">También puede usar el `Key` atributo si la entidad tiene su propia clave principal, pero desea asignar nombre a la propiedad algo que no sean classnameID o identificador.</span><span class="sxs-lookup"><span data-stu-id="65b28-216">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.</span></span>

<span data-ttu-id="65b28-217">De forma predeterminada, EF trata la clave como no generada por base de datos porque la columna es para una relación de identificación.</span><span class="sxs-lookup"><span data-stu-id="65b28-217">By default, EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="65b28-218">La propiedad de navegación de Instructor</span><span class="sxs-lookup"><span data-stu-id="65b28-218">The Instructor navigation property</span></span>

<span data-ttu-id="65b28-219">La entidad Instructor tiene una que aceptan valores NULL `OfficeAssignment` propiedad de navegación (porque un instructor no podría tener una asignación de oficina), y la entidad OfficeAssignment tiene un acepta valores NULL `Instructor` propiedad de navegación (debido a una asignación de office no se puede existir sin un instructor-- `InstructorID` no acepta valores NULL).</span><span class="sxs-lookup"><span data-stu-id="65b28-219">The Instructor entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="65b28-220">Cuando una entidad Instructor tiene una entidad OfficeAssignment relacionada, cada entidad tendrá una referencia a la otra en la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="65b28-220">When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="65b28-221">Podría poner un `[Required]` atributo en la propiedad de navegación de Instructor para especificar que debe haber un instructor relacionado, pero no tiene que hacerlo porque el `InstructorID` clave externa (que también es la clave para esta tabla) es que no aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="65b28-221">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the `InstructorID` foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="65b28-222">Modificar la entidad de curso</span><span class="sxs-lookup"><span data-stu-id="65b28-222">Modify the Course Entity</span></span>

![Entidad de curso](complex-data-model/_static/course-entity.png)

<span data-ttu-id="65b28-224">En *Models/Course.cs*, reemplace el código que agregó anteriormente con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="65b28-224">In *Models/Course.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="65b28-225">Los cambios aparecen resaltados.</span><span class="sxs-lookup"><span data-stu-id="65b28-225">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="65b28-226">La entidad de curso tiene una propiedad de clave externa `DepartmentID` que señala a la entidad relacionada de departamento y tiene un `Department` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="65b28-226">The course entity has a foreign key property `DepartmentID` which points to the related Department entity and it has a `Department` navigation property.</span></span>

<span data-ttu-id="65b28-227">Entity Framework no requiere que agregue una propiedad de clave externa para el modelo de datos cuando tenga una propiedad de navegación para una entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="65b28-227">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span>  <span data-ttu-id="65b28-228">EF crea claves externas en la base de datos cuando son necesarias y crea automáticamente [sombrear propiedades](https://docs.microsoft.com/ef/core/modeling/shadow-properties) para ellos.</span><span class="sxs-lookup"><span data-stu-id="65b28-228">EF automatically creates foreign keys in the database wherever they are needed and creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for them.</span></span> <span data-ttu-id="65b28-229">Pero con la clave externa en el modelo de datos puede hacer que las actualizaciones más sencillo y más eficaz.</span><span class="sxs-lookup"><span data-stu-id="65b28-229">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="65b28-230">Por ejemplo, al recuperar una entidad de curso para editar, la entidad Department es null si no lo carga, por lo que cuando se actualiza la entidad de curso, deberá primero capturar la entidad Department.</span><span class="sxs-lookup"><span data-stu-id="65b28-230">For example, when you fetch a course entity to edit, the  Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity.</span></span> <span data-ttu-id="65b28-231">Cuando la propiedad de clave externa `DepartmentID` se incluye en el modelo de datos, no es necesario capturar la entidad Department antes de actualizar.</span><span class="sxs-lookup"><span data-stu-id="65b28-231">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the Department entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="65b28-232">El atributo DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="65b28-232">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="65b28-233">El `DatabaseGenerated` atribuir a la `None` parámetro en el `CourseID` propiedad especifica que los valores de clave principales se proporcionado por el usuario, en lugar de generado por la base de datos.</span><span class="sxs-lookup"><span data-stu-id="65b28-233">The `DatabaseGenerated` attribute with the `None` parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="65b28-234">De forma predeterminada, Entity Framework se da por supuesto que la base de datos genera valores de clave principal.</span><span class="sxs-lookup"><span data-stu-id="65b28-234">By default, Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="65b28-235">Es lo que desea en la mayoría de los escenarios.</span><span class="sxs-lookup"><span data-stu-id="65b28-235">That's what you want in most scenarios.</span></span> <span data-ttu-id="65b28-236">Sin embargo, para las entidades de curso, podrá utilizar un número de curso especificado por el usuario como una serie de 1000 para un departamento, una serie de 2000 para otro departamento y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="65b28-236">However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

<span data-ttu-id="65b28-237">El `DatabaseGenerated` atributo también se puede usar para generar valores predeterminados, como en el caso de columnas de base de datos que se usan para registrar la fecha se crea o actualiza una fila.</span><span class="sxs-lookup"><span data-stu-id="65b28-237">The `DatabaseGenerated` attribute can also be used to generate default values, as in the case of database columns used to record the date a row was created or updated.</span></span>  <span data-ttu-id="65b28-238">Para obtener más información, consulte [genera propiedades](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="65b28-238">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="65b28-239">Propiedades de navegación y de clave externa</span><span class="sxs-lookup"><span data-stu-id="65b28-239">Foreign key and navigation properties</span></span>

<span data-ttu-id="65b28-240">Las propiedades de clave externa y propiedades de navegación de la entidad de curso reflejan las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="65b28-240">The foreign key properties and navigation properties in the Course entity reflect the following relationships:</span></span>

<span data-ttu-id="65b28-241">Un curso se asigna a un departamento, por lo que hay un `DepartmentID` clave externa y un `Department` propiedad de navegación por las razones mencionadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="65b28-241">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="65b28-242">Un curso puede tener cualquier número de alumnos inscritos en él, por lo que el `Enrollments` propiedad de navegación es una colección:</span><span class="sxs-lookup"><span data-stu-id="65b28-242">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="65b28-243">Un curso puede ser imparten por varios instructores, por lo que la `CourseAssignments` propiedad de navegación es una colección (el tipo `CourseAssignment` se explica [más adelante](#many-to-many-relationships)):</span><span class="sxs-lookup"><span data-stu-id="65b28-243">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection (the type `CourseAssignment` is explained [later](#many-to-many-relationships)):</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a><span data-ttu-id="65b28-244">Crear la entidad Department</span><span class="sxs-lookup"><span data-stu-id="65b28-244">Create the Department entity</span></span>

![Entidad Department](complex-data-model/_static/department-entity.png)


<span data-ttu-id="65b28-246">Crear *Models/Department.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="65b28-246">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="65b28-247">El atributo de columna</span><span class="sxs-lookup"><span data-stu-id="65b28-247">The Column attribute</span></span>

<span data-ttu-id="65b28-248">Versiones anteriores utilizan el `Column` atributo para cambiar la asignación de nombres de columna.</span><span class="sxs-lookup"><span data-stu-id="65b28-248">Earlier you used the `Column` attribute to change column name mapping.</span></span> <span data-ttu-id="65b28-249">En el código de la entidad Department, el `Column` atributo es que se va a utilizar para cambiar SQL asignar tipos de datos para que la columna se definirá con el tipo de moneda de SQL Server en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="65b28-249">In the code for the Department entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="65b28-250">Asignación de columnas por lo general no es necesaria, dado que Entity Framework elige el tipo de datos de SQL Server adecuado en función del tipo CLR que se define para la propiedad.</span><span class="sxs-lookup"><span data-stu-id="65b28-250">Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="65b28-251">El CLR `decimal` tipo se asigna a un servidor SQL Server `decimal` tipo.</span><span class="sxs-lookup"><span data-stu-id="65b28-251">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="65b28-252">Pero en este caso se sabe que la columna se pueden contener cantidades de moneda, y es más adecuado para el tipo de datos de moneda.</span><span class="sxs-lookup"><span data-stu-id="65b28-252">But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="65b28-253">Propiedades de navegación y de clave externa</span><span class="sxs-lookup"><span data-stu-id="65b28-253">Foreign key and navigation properties</span></span>

<span data-ttu-id="65b28-254">Las propiedades de navegación y de clave externas reflejen las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="65b28-254">The foreign key and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="65b28-255">Un departamento puede tener o no un administrador, y un administrador es siempre un instructor.</span><span class="sxs-lookup"><span data-stu-id="65b28-255">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="65b28-256">Por lo tanto, la `InstructorID` propiedad se incluye como la clave externa para la entidad Instructor y se agrega un signo de interrogación después de la `int` escriba designación para marcar la propiedad como que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="65b28-256">Therefore the `InstructorID` property is included as the foreign key to the Instructor entity, and a question mark is added after the `int` type designation to mark the property as nullable.</span></span> <span data-ttu-id="65b28-257">La propiedad de navegación se denomina `Administrator` pero contiene una entidad Instructor:</span><span class="sxs-lookup"><span data-stu-id="65b28-257">The navigation property is named `Administrator` but holds an Instructor entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="65b28-258">Un departamento puede tener varios cursos, por lo que es una propiedad de navegación de cursos:</span><span class="sxs-lookup"><span data-stu-id="65b28-258">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> <span data-ttu-id="65b28-259">Por convención, Entity Framework permite la eliminación en cascada para las claves externas que no aceptan valores NULL y para las relaciones de varios a varios.</span><span class="sxs-lookup"><span data-stu-id="65b28-259">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="65b28-260">Esto puede dar lugar a eliminar reglas de cascada circular, lo que producirán una excepción al intentar agregar una migración.</span><span class="sxs-lookup"><span data-stu-id="65b28-260">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="65b28-261">Por ejemplo, si no define la propiedad Department.InstructorID como que acepta valores NULL, EF podría configurar una regla de eliminación en cascada para eliminar el instructor cuando se elimina el departamento, que no es lo que desea que ocurra.</span><span class="sxs-lookup"><span data-stu-id="65b28-261">For example, if you didn't define the Department.InstructorID property as nullable, EF would configure a cascade delete rule to delete the instructor when you delete the department, which is not what you want to have happen.</span></span> <span data-ttu-id="65b28-262">Si es necesario sus reglas de negocios el `InstructorID` propiedad no aceptan valores NULL, se tendría que usar la siguiente instrucción de la API fluida para deshabilitar la eliminación en cascada en la relación:</span><span class="sxs-lookup"><span data-stu-id="65b28-262">If your business rules required the `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="65b28-263">Modificar la entidad de inscripción</span><span class="sxs-lookup"><span data-stu-id="65b28-263">Modify the Enrollment entity</span></span>

![Entidad de inscripción](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="65b28-265">En *Models/Enrollment.cs*, reemplace el código que agregó anteriormente con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="65b28-265">In *Models/Enrollment.cs*, replace the code you added earlier with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="65b28-266">Propiedades de navegación y de clave externa</span><span class="sxs-lookup"><span data-stu-id="65b28-266">Foreign key and navigation properties</span></span>

<span data-ttu-id="65b28-267">Las propiedades de clave externa y propiedades de navegación reflejan las relaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="65b28-267">The foreign key properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="65b28-268">Un registro de inscripción es para un solo curso, por lo que hay un `CourseID` propiedad de clave externa y un `Course` propiedad de navegación:</span><span class="sxs-lookup"><span data-stu-id="65b28-268">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="65b28-269">Un registro de inscripción es para un estudiante único, por lo que hay un `StudentID` propiedad de clave externa y un `Student` propiedad de navegación:</span><span class="sxs-lookup"><span data-stu-id="65b28-269">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="65b28-270">Relaciones Varios a Varios</span><span class="sxs-lookup"><span data-stu-id="65b28-270">Many-to-Many Relationships</span></span>

<span data-ttu-id="65b28-271">Hay una relación de varios a varios entre las entidades de estudiante y curso y la entidad de inscripción funciona como una tabla de combinación-to-many *con carga* en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="65b28-271">There's a many-to-many relationship between the Student and Course entities, and the Enrollment entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="65b28-272">"Con una carga" significa que la tabla Enrollment contiene datos adicionales además de las claves externas de las tablas combinadas (en este caso, una clave principal y una propiedad de grado).</span><span class="sxs-lookup"><span data-stu-id="65b28-272">"With payload" means that the Enrollment table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a Grade property).</span></span>

<span data-ttu-id="65b28-273">En la siguiente ilustración muestra el aspecto de estas relaciones en un diagrama de la entidad.</span><span class="sxs-lookup"><span data-stu-id="65b28-273">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="65b28-274">(Este diagrama se ha generado mediante las herramientas de alimentación de Entity Framework para EF 6.x; crear el diagrama no forma parte del tutorial, se utilizan simplemente aquí como una ilustración.)</span><span class="sxs-lookup"><span data-stu-id="65b28-274">(This diagram was generated using the Entity Framework Power Tools for EF 6.x; creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![Curso de estudiante muchos a muchos relación](complex-data-model/_static/student-course.png)

<span data-ttu-id="65b28-276">Cada línea de relación tiene un 1 en un extremo y un asterisco (*) en el otro, que indica una relación uno a varios.</span><span class="sxs-lookup"><span data-stu-id="65b28-276">Each relationship line has a 1 at one end and an asterisk (*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="65b28-277">Si la tabla Enrollment no incluye la información de categoría, que solo sería necesario contener las dos claves externas CourseID y StudentID.</span><span class="sxs-lookup"><span data-stu-id="65b28-277">If the Enrollment table didn't include grade information, it would only need to contain the two foreign keys CourseID and StudentID.</span></span> <span data-ttu-id="65b28-278">En ese caso, sería una tabla sin carga una combinación de varios a varios (o una tabla combinada pura) en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="65b28-278">In that case, it would be a many-to-many join table without payload (or a pure join table) in the database.</span></span> <span data-ttu-id="65b28-279">Las entidades Instructor y el curso tienen ese tipo de relación de varios a varios, y el paso siguiente consiste en crear una clase de entidad para que funcione como una tabla de combinación sin carga.</span><span class="sxs-lookup"><span data-stu-id="65b28-279">The Instructor and Course entities have that kind of many-to-many relationship, and your next step is to create an entity class to function as a join table without payload.</span></span>

<span data-ttu-id="65b28-280">(EF 6.x es compatible con las tablas de unión implícita para relaciones de varios a varios, pero el núcleo de EF no lo hace.</span><span class="sxs-lookup"><span data-stu-id="65b28-280">(EF 6.x supports implicit join tables for many-to-many relationships, but EF Core does not.</span></span> <span data-ttu-id="65b28-281">Para obtener más información, consulte el [explicación en el repositorio de GitHub de núcleo de EF](https://github.com/aspnet/EntityFramework/issues/1368).)</span><span class="sxs-lookup"><span data-stu-id="65b28-281">For more information, see the [discussion in the EF Core GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span></span> 

## <a name="the-courseassignment-entity"></a><span data-ttu-id="65b28-282">La entidad CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="65b28-282">The CourseAssignment entity</span></span>

![Entidad CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="65b28-284">Crear *Models/CourseAssignment.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="65b28-284">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a><span data-ttu-id="65b28-285">Unirse a nombres de entidad</span><span class="sxs-lookup"><span data-stu-id="65b28-285">Join entity names</span></span>

<span data-ttu-id="65b28-286">Se requiere una tabla de combinación en la base de datos para la relación de varios a varios Instructor a cursos y tiene que ser representado por un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="65b28-286">A join table is required in the database for the Instructor-to-Courses many-to-many relationship, and it has to be represented by an entity set.</span></span> <span data-ttu-id="65b28-287">Es común para el nombre de una entidad de combinación `EntityName1EntityName2`, que en este caso sería `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="65b28-287">It's common to name a join entity `EntityName1EntityName2`, which in this case would be `CourseInstructor`.</span></span> <span data-ttu-id="65b28-288">Sin embargo, se recomienda que elija un nombre que describa la relación.</span><span class="sxs-lookup"><span data-stu-id="65b28-288">However, we recommend that you choose a name that describes the relationship.</span></span> <span data-ttu-id="65b28-289">Modelos de datos empiezan de manera sencilla y crecen con combinaciones de carga no obtener con frecuencia cargas más tarde.</span><span class="sxs-lookup"><span data-stu-id="65b28-289">Data models start out simple and grow, with no-payload joins frequently getting payloads later.</span></span> <span data-ttu-id="65b28-290">Si empieza con un nombre descriptivo de entidad, no tendrás que cambiar el nombre más adelante.</span><span class="sxs-lookup"><span data-stu-id="65b28-290">If you start with a descriptive entity name, you won't have to change the name later.</span></span> <span data-ttu-id="65b28-291">Idealmente, la entidad de combinación tendrá su propio nombre (posiblemente sola palabra) natural en el dominio de negocio.</span><span class="sxs-lookup"><span data-stu-id="65b28-291">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="65b28-292">Por ejemplo, podrían vincularse a través de las clasificaciones de libros y los clientes.</span><span class="sxs-lookup"><span data-stu-id="65b28-292">For example, Books and Customers could be linked through Ratings.</span></span> <span data-ttu-id="65b28-293">Para esta relación, `CourseAssignment` es una opción mejor que `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="65b28-293">For this relationship, `CourseAssignment` is a better choice than `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="65b28-294">Clave compuesta</span><span class="sxs-lookup"><span data-stu-id="65b28-294">Composite key</span></span>

<span data-ttu-id="65b28-295">Puesto que las claves externas no son que aceptan valores NULL y juntos forma única identifican cada fila de la tabla, no es necesario para una clave principal independiente.</span><span class="sxs-lookup"><span data-stu-id="65b28-295">Since the foreign keys are not nullable and together uniquely identify each row of the table, there is no need for a separate primary key.</span></span> <span data-ttu-id="65b28-296">El *InstructorID* y *CourseID* propiedades deben funcionar como una clave principal compuesta.</span><span class="sxs-lookup"><span data-stu-id="65b28-296">The *InstructorID* and *CourseID* properties should function as a composite primary key.</span></span> <span data-ttu-id="65b28-297">Es la única manera de identificar las claves principales compuestas a EF mediante la *API fluida* (no se puede realizar mediante el uso de atributos).</span><span class="sxs-lookup"><span data-stu-id="65b28-297">The only way to identify composite primary keys to EF is by using the *fluent API* (it can't be done by using attributes).</span></span> <span data-ttu-id="65b28-298">Verá cómo configurar la clave principal compuesta en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="65b28-298">You'll see how to configure the composite primary key in the next section.</span></span>

<span data-ttu-id="65b28-299">La clave compuesta garantiza que, aunque es posible tener varias filas para un curso y varias filas para un instructor, no puede tener varias filas para el mismo instructor y el curso.</span><span class="sxs-lookup"><span data-stu-id="65b28-299">The composite key ensures that while you can have multiple rows for one course, and multiple rows for one instructor, you can't have multiple rows for the same instructor and course.</span></span> <span data-ttu-id="65b28-300">El `Enrollment` combinación entidad define su propia clave principal, por lo que son posibles duplicados de este tipo.</span><span class="sxs-lookup"><span data-stu-id="65b28-300">The `Enrollment` join entity defines its own primary key, so duplicates of this sort are possible.</span></span> <span data-ttu-id="65b28-301">Para evitar los duplicados, podría agregar un índice único en los campos de clave externos o configurar `Enrollment` con una clave compuesta principal similar a `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="65b28-301">To prevent such duplicates, you could add a unique index on the foreign key fields, or configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="65b28-302">Para obtener más información, consulte [índices](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="65b28-302">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="65b28-303">Actualizar el contexto de base de datos</span><span class="sxs-lookup"><span data-stu-id="65b28-303">Update the database context</span></span>

<span data-ttu-id="65b28-304">Agregue el código resaltado siguiente a la *Data/SchoolContext.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="65b28-304">Add the following highlighted code to the *Data/SchoolContext.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="65b28-305">Este código agrega las nuevas entidades y configura la clave principal compuesta de la entidad CourseAssignment.</span><span class="sxs-lookup"><span data-stu-id="65b28-305">This code adds the new entities and configures the CourseAssignment entity's composite primary key.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="65b28-306">Alternativa de API fluida a atributos</span><span class="sxs-lookup"><span data-stu-id="65b28-306">Fluent API alternative to attributes</span></span>

<span data-ttu-id="65b28-307">El código en el `OnModelCreating` método de la `DbContext` clase utiliza el *API fluida* para configurar el comportamiento EF.</span><span class="sxs-lookup"><span data-stu-id="65b28-307">The code in the `OnModelCreating` method of the `DbContext` class uses the *fluent API* to configure EF behavior.</span></span> <span data-ttu-id="65b28-308">La API se denomina "fluida" porque a menudo se usa por tender una serie de llamadas al método juntas en una única instrucción, como en este ejemplo desde la [documentación principal de EF](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span><span class="sxs-lookup"><span data-stu-id="65b28-308">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement, as in this example from the [EF Core documentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="65b28-309">En este tutorial, que está usando la API fluida solo para la asignación de base de datos que no se puede realizar con atributos.</span><span class="sxs-lookup"><span data-stu-id="65b28-309">In this tutorial, you're using the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="65b28-310">Sin embargo, también puede utilizar la API fluida para especificar casi todo el formato, la validación y las reglas de asignación que se pueden realizar mediante el uso de atributos.</span><span class="sxs-lookup"><span data-stu-id="65b28-310">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="65b28-311">Algunos atributos como `MinimumLength` no se puede aplicar con la API fluida.</span><span class="sxs-lookup"><span data-stu-id="65b28-311">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="65b28-312">Como se mencionó anteriormente, `MinimumLength` no cambia el esquema, sólo se aplica una regla de validación del lado cliente y el servidor.</span><span class="sxs-lookup"><span data-stu-id="65b28-312">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule.</span></span>

<span data-ttu-id="65b28-313">Algunos desarrolladores prefieren usar la API fluida exclusivamente para que pueden mantener las clases de entidad "limpia".</span><span class="sxs-lookup"><span data-stu-id="65b28-313">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="65b28-314">Puede mezclar atributos y API fluida si quieres, hay algunas personalizaciones que solo pueden realizarse mediante el uso de la API fluida, pero en general la práctica recomendada es elegir uno de estos dos enfoques y utilizar ese constantemente lo máximo posible.</span><span class="sxs-lookup"><span data-stu-id="65b28-314">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span> <span data-ttu-id="65b28-315">Si utiliza ambos, tenga en cuenta que siempre que hay un conflicto, API fluida de invalidaciones de atributos.</span><span class="sxs-lookup"><span data-stu-id="65b28-315">If you do use both, note that wherever there is a conflict, Fluent API overrides attributes.</span></span>

<span data-ttu-id="65b28-316">Para obtener más información acerca de los atributos frente a la API fluida, consulte [métodos de configuración](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="65b28-316">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="65b28-317">Diagrama mostrando relaciones de entidad</span><span class="sxs-lookup"><span data-stu-id="65b28-317">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="65b28-318">En la siguiente ilustración muestra el diagrama creados por las herramientas de alimentación de Entity Framework para el modelo School completado.</span><span class="sxs-lookup"><span data-stu-id="65b28-318">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![Diagrama de entidad](complex-data-model/_static/diagram.png)

<span data-ttu-id="65b28-320">Además de las líneas de relación de uno a varios (1 a \*), aquí se puede ver la línea de relación de uno a cero o uno (1 a 0.. 1) entre las entidades Instructor y OfficeAssignment y la línea de relación de cero-o-uno a varios (0.. 1 a *) entre el Entidades de instructor y departamento.</span><span class="sxs-lookup"><span data-stu-id="65b28-320">Besides the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the Instructor and OfficeAssignment entities and the zero-or-one-to-many relationship line (0..1 to *) between the Instructor and Department entities.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="65b28-321">Valor de inicialización de la base de datos con datos de prueba</span><span class="sxs-lookup"><span data-stu-id="65b28-321">Seed the Database with Test Data</span></span>

<span data-ttu-id="65b28-322">Reemplace el código de la *Data/DbInitializer.cs* archivo con el código siguiente con el fin de proporcionar datos de inicialización para las nuevas entidades que ha creado.</span><span class="sxs-lookup"><span data-stu-id="65b28-322">Replace the code in the *Data/DbInitializer.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="65b28-323">Como vimos en el primer tutorial, la mayor parte de este código simplemente crea nuevos objetos de entidad y carga los datos de ejemplo en propiedades según sea necesario para las pruebas.</span><span class="sxs-lookup"><span data-stu-id="65b28-323">As you saw in the first tutorial, most of this code simply creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="65b28-324">Tenga en cuenta cómo se administran las relaciones de varios a varios: el código crea relaciones mediante la creación de entidades en el `Enrollments` y `CourseAssignment` combinar conjuntos de entidades.</span><span class="sxs-lookup"><span data-stu-id="65b28-324">Notice how the many-to-many relationships are handled: the code creates relationships by creating entities in the `Enrollments` and `CourseAssignment` join entity sets.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="65b28-325">Agregar una migración</span><span class="sxs-lookup"><span data-stu-id="65b28-325">Add a migration</span></span>

<span data-ttu-id="65b28-326">Guarde los cambios y compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="65b28-326">Save your changes and build the project.</span></span> <span data-ttu-id="65b28-327">A continuación, abra la ventana de comandos en la carpeta del proyecto y escriba el `migrations add` comando (no el comando update-database aún):</span><span class="sxs-lookup"><span data-stu-id="65b28-327">Then open the command window in the project folder and enter the `migrations add` command (don't do the update-database command yet):</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="65b28-328">Recibe una advertencia acerca de la posible pérdida de datos.</span><span class="sxs-lookup"><span data-stu-id="65b28-328">You get a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="65b28-329">Si intenta ejecutar la `database update` de comandos en este momento (no lo hace aún), se obtendrá el error siguiente:</span><span class="sxs-lookup"><span data-stu-id="65b28-329">If you tried to run the `database update` command at this point (don't do it yet), you would get the following error:</span></span>

> <span data-ttu-id="65b28-330">La instrucción ALTER TABLE entran en conflicto con la restricción FOREIGN KEY "FK_dbo. Course_dbo. Department_DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="65b28-330">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span></span> <span data-ttu-id="65b28-331">Se produjo el conflicto en la tabla base de datos "ContosoUniversity", "dbo. Departamento", columna 'DepartmentID'.</span><span class="sxs-lookup"><span data-stu-id="65b28-331">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.</span></span>

<span data-ttu-id="65b28-332">En ocasiones, al ejecutar migraciones con los datos existentes, debe insertar los datos de código auxiliar en la base de datos para satisfacer las restricciones foreign key.</span><span class="sxs-lookup"><span data-stu-id="65b28-332">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints.</span></span> <span data-ttu-id="65b28-333">El código generado en el `Up` método agrega una clave externa de DepartmentID no acepta valores NULL a la tabla Course.</span><span class="sxs-lookup"><span data-stu-id="65b28-333">The generated code in the `Up` method adds a non-nullable DepartmentID foreign key to the Course table.</span></span> <span data-ttu-id="65b28-334">Si ya hay filas en la tabla Course cuando se ejecuta el código, el `AddColumn` se produce un error en la operación porque SQL Server no conoce el valor que se incluirán en la columna que no puede ser nula.</span><span class="sxs-lookup"><span data-stu-id="65b28-334">If there are already rows in the Course table when the code runs, the `AddColumn` operation fails because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="65b28-335">En este tutorial se va a ejecutar la migración en una base de datos, pero en una aplicación de producción tendría que realizar la migración a administrar los datos existentes, por lo que las instrucciones siguientes muestran un ejemplo de cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="65b28-335">For this tutorial you'll run the migration on a new database, but in a production application you'd have to make the migration handle existing data, so the following directions show an example of how to do that.</span></span>

<span data-ttu-id="65b28-336">Para realizar esta migración trabajar con datos existentes, que tiene que cambiar el código para asignar un valor predeterminado de la nueva columna y crear un departamento de recibos denominado "Temp" para que actúe como el departamento de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="65b28-336">To make this migration work with existing data you have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="65b28-337">Como resultado, existente filas curso estarán todos relacionados con el departamento "Temp" después de la `Up` ejecuciones de método.</span><span class="sxs-lookup"><span data-stu-id="65b28-337">As a result, existing Course rows will all be related to the "Temp" department after the `Up` method runs.</span></span>

* <span data-ttu-id="65b28-338">Abra la *{timestamp}_ComplexDataModel.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="65b28-338">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span> 

* <span data-ttu-id="65b28-339">Convertir en comentario la línea de código que agrega la columna DepartmentID a la tabla Course.</span><span class="sxs-lookup"><span data-stu-id="65b28-339">Comment out the line of code that adds the DepartmentID column to the Course table.</span></span>

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* <span data-ttu-id="65b28-340">Agregue el código resaltado siguiente después del código que crea la tabla de departamento:</span><span class="sxs-lookup"><span data-stu-id="65b28-340">Add the following highlighted code after the code that creates the Department table:</span></span>

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="65b28-341">En una aplicación de producción, debería escribir código o secuencias de comandos para agregar filas de departamento y relacionados con filas de curso para las nuevas filas de departamento.</span><span class="sxs-lookup"><span data-stu-id="65b28-341">In a production application, you would write code or scripts to add Department rows and relate Course rows to the new Department rows.</span></span> <span data-ttu-id="65b28-342">A continuación, ya no tendría el departamento "Temp" o el valor predeterminado en la columna Course.DepartmentID.</span><span class="sxs-lookup"><span data-stu-id="65b28-342">You would then no longer need the "Temp" department or the default value on the Course.DepartmentID column.</span></span>

<span data-ttu-id="65b28-343">Guarde los cambios y compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="65b28-343">Save your changes and build the project.</span></span>

## <a name="change-the-connection-string-and-update-the-database"></a><span data-ttu-id="65b28-344">Cambie la cadena de conexión y actualizar la base de datos</span><span class="sxs-lookup"><span data-stu-id="65b28-344">Change the connection string and update the database</span></span>

<span data-ttu-id="65b28-345">Ahora tienes código nuevo el `DbInitializer` clase que agrega datos de inicialización para las nuevas entidades a una base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="65b28-345">You now have new code in the `DbInitializer` class that adds seed data for the new entities to an empty database.</span></span> <span data-ttu-id="65b28-346">Para asegurarse de EF crear una nueva base de datos vacía, cambia el nombre de la base de datos en la cadena de conexión en *appSettings.JSON que se* a ContosoUniversity3 o algún otro nombre que no has utilizado en el equipo que está utilizando.</span><span class="sxs-lookup"><span data-stu-id="65b28-346">To make EF create a new empty database, change the name of the database in the connection string in *appsettings.json* to ContosoUniversity3 or some other name that you haven't used on the computer you're using.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

<span data-ttu-id="65b28-347">Guarde el cambio a *appSettings.JSON que se*.</span><span class="sxs-lookup"><span data-stu-id="65b28-347">Save your change to *appsettings.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="65b28-348">Como alternativa a cambiar el nombre de la base de datos, puede eliminar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="65b28-348">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="65b28-349">Use **Explorador de objetos de SQL Server** (SSOX) o `database drop` comando de CLI:</span><span class="sxs-lookup"><span data-stu-id="65b28-349">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```

<span data-ttu-id="65b28-350">Después de haber cambiado el nombre de la base de datos o eliminar la base de datos, ejecute el `database update` comando en la ventana de comandos para ejecutar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="65b28-350">After you have changed the database name or deleted the database, run the `database update` command in the command window to execute the migrations.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="65b28-351">Ejecutar la aplicación para hacer que el `DbInitializer.Initialize` método para ejecutar y rellenar la base de datos nueva.</span><span class="sxs-lookup"><span data-stu-id="65b28-351">Run the app to cause the `DbInitializer.Initialize` method to run and populate the new database.</span></span>

<span data-ttu-id="65b28-352">Abra la base de datos de SSOX como lo hizo anteriormente y expanda el **tablas** nodo para ver que todas las tablas se han creado.</span><span class="sxs-lookup"><span data-stu-id="65b28-352">Open the database in SSOX as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="65b28-353">(Si sigue teniendo SSOX abrir desde la hora anterior, haga clic en el **actualizar** botón.)</span><span class="sxs-lookup"><span data-stu-id="65b28-353">(If you still have SSOX open from the earlier time, click the **Refresh** button.)</span></span>

![Tablas de SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="65b28-355">Ejecute la aplicación para desencadenar el código de inicializador que propaga la base de datos.</span><span class="sxs-lookup"><span data-stu-id="65b28-355">Run the app to trigger the initializer code that seeds the database.</span></span>

<span data-ttu-id="65b28-356">Haga clic en el **CourseAssignment** de tabla y seleccione **ver datos** para comprobar que tiene datos en ella.</span><span class="sxs-lookup"><span data-stu-id="65b28-356">Right-click the **CourseAssignment** table and select **View Data** to verify that it has data in it.</span></span>

![Datos de CourseAssignment en SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a><span data-ttu-id="65b28-358">Resumen</span><span class="sxs-lookup"><span data-stu-id="65b28-358">Summary</span></span>

<span data-ttu-id="65b28-359">Ahora tiene un modelo de datos más compleja y una base de datos correspondiente.</span><span class="sxs-lookup"><span data-stu-id="65b28-359">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="65b28-360">En el siguiente tutorial, aprenderá más acerca de cómo obtener acceso a datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="65b28-360">In the following tutorial, you'll learn more about how to access related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="65b28-361">[Anterior](migrations.md)
[Siguiente](read-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="65b28-361">[Previous](migrations.md)
[Next](read-related-data.md)</span></span>  
