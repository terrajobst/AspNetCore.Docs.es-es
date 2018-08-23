---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Agregar un nuevo campo a la tabla y el modelo de película | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 45e6de410266af9d7909cea6d03258d2db7cb240
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829142"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Agregar un nuevo campo a la tabla y el modelo de película
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestran las características más.


En esta sección usará migraciones de Entity Framework Code First para migrar algunos cambios a las clases del modelo, por lo que el cambio se aplica a la base de datos.

De forma predeterminada, al usar Entity Framework Code First para crear automáticamente una base de datos, como hizo anteriormente en este tutorial, Code First agrega una tabla para ayudar a saber si el esquema de la base de datos está sincronizado con las clases del modelo se generó a partir de la base de datos. Si no están sincronizados, Entity Framework produce un error. Esto facilita realizar un seguimiento de los problemas en tiempo de desarrollo en caso contrario, podría encontrar solamente (por errores poco conocidos) en tiempo de ejecución.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configuración de migraciones de Code First para cambios en el modelo

Si utiliza Visual Studio 2012, haga doble clic en el *Movies.mdf* archivo desde el Explorador de soluciones para abrir la herramienta de la base de datos. Visual Studio Express para Web mostrará el Explorador de base de datos, que Visual Studio 2012 se mostrará el Explorador de servidores. Si utiliza Visual Studio 2010, use el Explorador de objetos de SQL Server.

En la herramienta de la base de datos (Explorador de base de datos, Explorador de servidores o explorador de objetos de SQL Server), haga clic en `MovieDBContext` y seleccione **eliminar** va a quitar de la base de datos de películas.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Navegue hasta el Explorador de soluciones. Haga clic con el botón derecho en el *Movies.mdf* de archivo y seleccione **eliminar** para quitar la base de datos de películas.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Compile la aplicación para asegurarse de que no hay ningún error.

Desde el **herramientas** menú, haga clic en **Administrador de paquetes de biblioteca** y, a continuación, **Package Manager Console**.

![Agregar módulo Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

En el **Package Manager Console** ventana en la `PM>` símbolo del sistema escriba "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

El **Enable-Migrations** comando (mostrado arriba) crea un *Configuration.cs* archivo en una nueva *migraciones* carpeta.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio abre el *Configuration.cs* archivo. Reemplace el `Seed` método en el *Configuration.cs* archivo con el código siguiente:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Haga clic con el botón derecho en la línea ondulada roja bajo `Movie` y seleccione **resolver** , a continuación, **mediante** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Si lo hace, agrega la siguiente instrucción using:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> El código llama a las migraciones de primera la `Seed` método después de cada migración (es decir, una llamada a **Actualizar base de datos** en la consola de administrador de paquetes), y este método actualiza las filas que ya se han insertado o insertan si se aún no existen.


**Presione CTRL-MAYÚS-B para compilar el proyecto.** (Se producirá un error en los pasos siguientes si su no se compilan en este momento.)

El siguiente paso es crear un `DbMigration` clase para la migración inicial. Esta migración crea una nueva base de datos, por eso se eliminó el *movie.mdf* archivo en el paso anterior.

En el **Package Manager Console** ventana, escriba el comando "add-migration Initial" para crear la migración inicial. El nombre "Inicial" es arbitrario y se usa un nombre al archivo de migración creado.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migraciones de Code First crea otro archivo de clase en el *migraciones* carpeta (con el nombre *{fecha}\_Initial.cs* ), y esta clase contiene código que crea el esquema de base de datos. Previamente se ha corregido el nombre de archivo de migración con una marca de tiempo para ayudar a con la ordenación. Examine el *{fecha}\_Initial.cs* archivo, contiene las instrucciones para crear la tabla de películas para la base de datos de la película. Al actualizar la base de datos en las instrucciones que aparecen a continuación, esto *{fecha}\_Initial.cs* archivo ejecutará y crear el esquema de base de datos. El **inicialización** método se ejecutará para rellenar la base de datos con datos de prueba.

En el **Package Manager Console**, escriba el comando "update-database" para crear la base de datos y ejecutar el **inicialización** método.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Si se produce un error que indica una tabla ya existe y no se puede crear, probablemente es porque se ejecutó la aplicación después de eliminar la base de datos y antes de ejecutar `update-database`. En ese caso, elimine la *Movies.mdf* archivo nuevo y vuelva a intentar la `update-database` comando. Si sigue apareciendo un error, elimine la carpeta de migraciones y contenido, comience con las instrucciones que aparecen en la parte superior de esta página (que es eliminar el *Movies.mdf* de archivos, a continuación, continúe con Enable-Migrations).

Ejecute la aplicación y vaya a la */Movies* dirección URL. Se muestran los datos de inicialización.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adición de una propiedad de clasificación al modelo Movie

Empiece agregando un nuevo `Rating` propiedad existente `Movie` clase. Abra el *Models\Movie.cs* y agréguele el `Rating` propiedad como esta:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

La completa `Movie` clase ahora parece similar al código siguiente:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Compilar la aplicación mediante el **compilar** &gt; **crear películas** menú comando o presionando CTRL-MAYÚS-B.

Ahora que ha actualizado el `Model` (clase), también deberá actualizar el *\Views\Movies\Index.cshtml* y *\Views\Movies\Create.cshtml* ver las plantillas con el fin de mostrar el nuevo `Rating`propiedad en la vista de explorador.

Abra el<em>\Views\Movies\Index.cshtml</em> archivo y agregue un `<th>Rating</th>` justo después de encabezado de columna la <strong>precio</strong> columna. A continuación, agregue un `<td>` columna cerca del final de la plantilla para representar el `@item.Rating` valor. A continuación es lo que la actualización <em>Index.cshtml</em> plantilla de vista es similar:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

A continuación, abra el *\Views\Movies\Create.cshtml* archivo y agregue el siguiente marcado cerca del final del formulario. Esto representa un cuadro de texto para que pueda especificar una clasificación cuando se crea una nueva película.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Se ha actualizado el código de aplicación para admitir los nuevos `Rating` propiedad.

Ahora ejecute la aplicación y vaya a la */Movies* dirección URL. Al hacerlo, sin embargo, verá uno de los errores siguientes:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Si ve este error porque la actualización `Movie` ahora es diferente del esquema de clase de modelo en la aplicación la `Movie` tabla de la base de datos existente. (No hay ninguna columna `Rating` en la tabla de la base de datos).


Este error se puede resolver de varias maneras:

1. Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo. Este enfoque resulta muy conveniente al realizar el desarrollo activo en una base de datos de prueba; permite desarrollar rápidamente el esquema de modelo y la base de datos entre sí. La desventaja, sin embargo, es la que se pierden los datos existentes en la base de datos, por lo que se *no* va a utilizar este enfoque en una base de datos de producción. Usar a un inicializador para inicializar automáticamente una base de datos con datos de prueba a menudo es una manera productiva de desarrollar una aplicación. Para obtener más información sobre los inicializadores de base de datos de Entity Framework, vea de Tom Dykstra [tutorial de ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo. La ventaja de este enfoque es que se conservan los datos. Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.
3. Use Migraciones de Code First para actualizar el esquema de la base de datos.


Para este tutorial se usa Migraciones de Code First.

Actualice el método de inicialización para que proporcione un valor para la nueva columna. Abrir archivo migrations\configuration. cs y agregue un campo de clasificación para cada objeto de película.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Compile la solución y, a continuación, abra el **Package Manager Console** ventana y escriba el siguiente comando:

`add-migration AddRatingMig`

El `add-migration` comando indica el marco de trabajo de migración para examinar el modelo de película actual con el esquema actual de la base de datos de la película y crear el código necesario para migrar la base de datos al nuevo modelo. El AddRatingMig es arbitrario y se usa un nombre al archivo de migración. Resulta útil usar un nombre descriptivo para el paso de migración.

Cuando finalice este comando, Visual Studio abre el archivo de clase que define la nueva `DbMIgration` clase derivada y en el `Up` método puede ver el código que crea la nueva columna.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Compile la solución y, a continuación, escriba el comando "update-database" en el **Package Manager Console** ventana.

La siguiente imagen muestra la salida en el **Package Manager Console** ventana (será diferente la marca de fecha anteposición AddRatingMig.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Vuelva a ejecutar la aplicación y vaya a la URL /Movies. Puede ver el nuevo campo de clasificación.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Haga clic en el **crear nuevo** el vínculo para agregar una nueva película. Tenga en cuenta que puede agregar una clasificación.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Haga clic en **Crear**. La nueva película, incluida la clasificación, se muestra ahora en la lista de películas:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

También debe agregar el `Rating` campo para la edición, detalles e SearchIndex plantillas de vista.

Puede escribir el comando "update-database" en el **Package Manager Console** ventana nuevo y no hay cambios que se aplicarían, porque el esquema coincide con el modelo.

En esta sección, ha visto cómo puede modificar los objetos de modelo y mantener sincronizados con los cambios de la base de datos. También ha aprendido cómo rellenar una base de datos recién creada con datos de ejemplo para que pueda probar escenarios. A continuación, echemos un vistazo a cómo puede agregar lógica de validación más completa para las clases del modelo y habilitar algunas reglas de negocios que se aplicará.

> [!div class="step-by-step"]
> [Anterior](examining-the-edit-methods-and-edit-view.md)
> [Siguiente](adding-validation-to-the-model.md)
