---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Agregar un nuevo campo al modelo de película y tabla | Documentos de Microsoft
author: Rick-Anderson
description: 'Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d8a42e9acdce687ab6e9742071dd2949f244622f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Agregar un nuevo campo en el modelo de película como una tabla
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestra más características.


En esta sección usará Entity Framework Code First Migrations migrar algunos cambios a las clases del modelo, por lo que el cambio se aplica a la base de datos.

De forma predeterminada, cuando se usa Entity Framework Code First para crear automáticamente una base de datos, como lo hizo anteriormente en este tutorial, Code First agrega una tabla para ayudar a realizar el seguimiento de si el esquema de la base de datos está sincronizado con las clases del modelo se generó a partir de la base de datos. Si no están sincronizados, Entity Framework produce un error. Esto hace más fácil realizar un seguimiento de los problemas en tiempo de desarrollo que en caso contrario, podría encontrar solamente (por errores poco claros) en tiempo de ejecución.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configuración de migraciones de Code First para cambios de modelo

Si usas Visual Studio 2012, haga doble clic en el *Movies.mdf* archivo desde el Explorador de soluciones para abrir la herramienta de la base de datos. Visual Studio Express para Web mostrará el Explorador de base de datos, que Visual Studio 2012 se mostrará el Explorador de servidores. Si usa Visual Studio 2010, use el Explorador de objetos de SQL Server.

En la herramienta de la base de datos (Explorador de base de datos, Explorador de servidores o explorador de objetos de SQL Server), haga clic en `MovieDBContext` y seleccione **eliminar** para quitar la base de datos de películas.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Vaya al explorador de soluciones. Haga clic con el botón secundario en el *Movies.mdf* de archivos y seleccione **eliminar** para quitar la base de datos de películas.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Compile la aplicación para asegurarse de que no hay ningún error.

Desde el **herramientas** menú, haga clic en **Administrador de paquetes de biblioteca** y, a continuación, **Package Manager Console**.

![Agregar módulo Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

En el **Package Manager Console** ventana en el `PM>` símbolo del sistema escriba "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

El **Enable-Migrations** comando (se muestra arriba) crea un *archivo Configuration.cs que* archivo en una nueva *migraciones* carpeta.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio abre el *archivo Configuration.cs que* archivos. Reemplace el `Seed` método en el *archivo Configuration.cs que* archivos con el código siguiente:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Haga clic con el botón secundario en la línea ondulada roja bajo `Movie` y seleccione **resolver** , a continuación, **con** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Si lo hace, agrega la siguiente instrucción using:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Code First Migrations llamadas el `Seed` método después de cada migración (es decir, una llamada a **Actualizar base de datos** en la consola de administrador de paquetes), y este método actualiza las filas que ya se han insertado o insertan si únicamente aún no existen.


**Presione CTRL-MAYÚS + B para compilar el proyecto.** (Los pasos siguientes se producirá un error si el no se compilan en este momento.)

El paso siguiente consiste en crear un `DbMigration` clase para la migración inicial. Esta migración crea una nueva base de datos, que es por lo que se elimina la *movie.mdf* archivo en un paso anterior.

En el **Package Manager Console** ventana, escriba el comando "Agregar-migración inicial" para crear la migración inicial. El nombre "Inicial" es arbitrario y se utiliza para asignar nombre al archivo de migración que creó.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migraciones de Code First crea otro archivo de clase en el *migraciones* carpeta (con el nombre *{DateStamp}\_Initial.cs* ), y esta clase contiene código que crea el esquema de base de datos. El nombre de archivo de migración previamente se fija con una marca de tiempo para ayudar a con la ordenación. Examine la *{DateStamp}\_Initial.cs* archivo, contiene las instrucciones para crear la tabla de películas para la base de datos de la película. Al actualizar la base de datos en las instrucciones a continuación, esto *{DateStamp}\_Initial.cs* archivo ejecutará y crear el esquema de base de datos. La **inicialización** método se ejecutará para rellenar la base de datos con datos de prueba.

En el **Package Manager Console**, escriba el comando "update-database" para crear la base de datos y ejecutar el **inicialización** método.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Si se produce un error que indica una tabla ya existe y no se puede crear, probablemente es porque ejecutó la aplicación después de eliminar la base de datos y antes de ejecutar `update-database`. En ese caso, elimine la *Movies.mdf* archivo nuevo y vuelva a intentar la `update-database` comando. Si sigue recibiendo un error, elimine la carpeta migrations y contenido e inicie con las instrucciones que aparecen en la parte superior de esta página (que es eliminar la *Movies.mdf* de archivos, a continuación, continúe con Enable-Migrations).

Ejecute la aplicación y navegue hasta la */Movies* dirección URL. Se muestran los datos de inicialización.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adición de una propiedad de clasificación al modelo Movie

Empiece agregando un nuevo `Rating` propiedad existente `Movie` clase. Abra la *Models\Movie.cs* de archivos y agregar el `Rating` propiedad como esta:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

La sección completa `Movie` clase ahora parece similar al código siguiente:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Genere la aplicación mediante el **generar** &gt; **crear películas** menú comando o presionando CTRL-MAYÚS-B.

Ahora que ha actualizado el `Model` (clase), también debe actualizar el *\Views\Movies\Index.cshtml* y *\Views\Movies\Create.cshtml* ver plantillas para mostrar el nuevo `Rating`propiedad en la vista del explorador.

Abra la<em>\Views\Movies\Index.cshtml</em> de archivos y agregar un `<th>Rating</th>` justo después de encabezado de columna la <strong>precio</strong> columna. A continuación, agregue un `<td>` columna cerca del final de la plantilla para representar la `@item.Rating` valor. A continuación se muestra qué actualizado <em>Index.cshtml</em> plantilla de vista es similar:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

A continuación, abra el *\Views\Movies\Create.cshtml* de archivos y agregue el siguiente marcado cerca del final del formulario. Esto representa un cuadro de texto para que pueda especificar una clasificación cuando se crea una película nuevo.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Ahora ha actualizado el código de aplicación para admitir los nuevos `Rating` propiedad.

Ahora ejecute la aplicación y navegue hasta la */Movies* dirección URL. Al hacer esto, sin embargo, verá uno de los siguientes errores:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Está viendo este error porque la actualización `Movie` clase de modelo en la aplicación ahora es diferente que el esquema de la `Movie` tabla de la base de datos existente. (No hay ninguna columna `Rating` en la tabla de la base de datos).


Este error se puede resolver de varias maneras:

1. Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo. Este enfoque resulta muy cómoda para realizar el desarrollo activo en una base de datos de prueba; permite evolucione rápidamente el esquema de modelo y la base de datos entre sí. La desventaja, sin embargo, es que se pierden los datos existentes en la base de datos, por lo que *no* va a utilizar este enfoque en una base de datos de producción. Usando a un inicializador para propagar automáticamente una base de datos con datos de prueba a menudo es una manera productiva al desarrollar una aplicación. Para obtener más información sobre los inicializadores de base de datos de Entity Framework, vea de Tom Dykstra [tutorial de ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo. La ventaja de este enfoque es que se conservan los datos. Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.
3. Use Migraciones de Code First para actualizar el esquema de la base de datos.


Para este tutorial se usa Migraciones de Code First.

Actualizar el método de inicialización para que proporcione un valor para la nueva columna. Abra el archivo Migrations\Configuration.cs y agregar un campo de clasificación para cada objeto de la película.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Compile la solución y, a continuación, abra el **Package Manager Console** ventana y escriba el comando siguiente:

`add-migration AddRatingMig`

El `add-migration` comando indica el marco de trabajo de migración para examinar el modelo de película actual con el esquema actual de la base de datos de la película y crear el código necesario para migrar la base de datos al nuevo modelo. El AddRatingMig es arbitrario y se utiliza para asignar nombre al archivo de migración. Es útil emplear un nombre descriptivo para el paso de migración.

Cuando finaliza este comando, Visual Studio abre el archivo de clase que define la nueva `DbMIgration` clase derivada y en el `Up` método puede ver el código que crea la nueva columna.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Compile la solución y, a continuación, escriba el comando "update-database" en la **Package Manager Console** ventana.

La siguiente imagen muestra la salida en el **Package Manager Console** ventana (será diferente la marca de fecha anteponiendo AddRatingMig.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Vuelva a ejecutar la aplicación y navegue a la dirección URL /Movies. Puede ver el nuevo campo de clasificación.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Haga clic en el **crear nuevo** el vínculo para agregar una película nuevo. Tenga en cuenta que puede agregar una clasificación.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Haga clic en **Crear**. La película nuevo, incluida la clasificación, se muestra ahora en la lista de películas:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

También debe agregar el `Rating` campo para la edición, detalles e SearchIndex plantillas de vista.

Puede escribir el comando "update-database" en la **Package Manager Console** volver a la ventana y ningún cambio que se aplicarían, porque el esquema coincide con el modelo.

En esta sección se ha visto cómo puede modificar objetos de modelo y mantenerlos sincronizados con los cambios de la base de datos. También aprendió una forma de rellenar una base de datos recién creada con datos de ejemplo para probar escenarios. A continuación, echemos un vistazo a cómo puede agregar una lógica de validación a las clases del modelo y habilitar algunas reglas de negocios que se aplicará.

> [!div class="step-by-step"]
> [Anterior](examining-the-edit-methods-and-edit-view.md)
> [Siguiente](adding-validation-to-the-model.md)
