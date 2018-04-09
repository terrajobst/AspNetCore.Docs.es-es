---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Agregar un nuevo campo | Documentos de Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 0dac798eba586cdcc232cedd262e610b954004df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field"></a>Adición de un nuevo campo
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

En esta sección usará Entity Framework Code First Migrations migrar algunos cambios a las clases del modelo, por lo que el cambio se aplica a la base de datos.

De forma predeterminada, cuando se usa Entity Framework Code First para crear automáticamente una base de datos, como lo hizo anteriormente en este tutorial, Code First agrega una tabla para ayudar a realizar el seguimiento de si el esquema de la base de datos está sincronizado con las clases del modelo se generó a partir de la base de datos. Si no están sincronizados, Entity Framework produce un error. Esto hace más fácil realizar un seguimiento de los problemas en tiempo de desarrollo que en caso contrario, podría encontrar solamente (por errores poco claros) en tiempo de ejecución.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configuración de migraciones de Code First para cambios de modelo

Vaya al explorador de soluciones. Haga clic con el botón secundario en el *Movies.mdf* de archivos y seleccione **eliminar** para quitar la base de datos de películas. Si no ve el *Movies.mdf* de archivos, haga clic en el **mostrar todos los archivos** icono se muestra a continuación en el contorno de color rojo.

![](adding-a-new-field/_static/image1.png)

Compile la aplicación para asegurarse de que no hay ningún error.

Desde el **herramientas** menú, haga clic en **Administrador de paquetes de NuGet** y, a continuación, **Package Manager Console**.

![Agregar módulo Man](adding-a-new-field/_static/image2.png)

En el **Package Manager Console** ventana en el `PM>` símbolo del sistema escriba

Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

El **Enable-Migrations** comando (se muestra arriba) crea un *archivo Configuration.cs que* archivo en una nueva *migraciones* carpeta.

![](adding-a-new-field/_static/image4.png)

Visual Studio abre el *archivo Configuration.cs que* archivos. Reemplace el `Seed` método en el *archivo Configuration.cs que* archivos con el código siguiente:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Mantenga el mouse sobre la línea ondulada roja bajo `Movie` y haga clic en `Show Potential Fixes` y, a continuación, haga clic en **con** **MvcMovie.Models;**

![](adding-a-new-field/_static/image5.png)

Si lo hace, agrega la siguiente instrucción using:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Code First Migrations llamadas el `Seed` método después de cada migración (es decir, una llamada a **Actualizar base de datos** en la consola de administrador de paquetes), y este método actualiza las filas que ya se han insertado o insertan si únicamente aún no existen.
> 
> El [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método en el código siguiente realiza una operación "upsert":
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Dado que la [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método se ejecuta con cada migración, simplemente no se puede insertar datos, porque las filas que se está intentando agregar ya estarán ahí después de la primera migración que crea la base de datos. El "[upsert](http://en.wikipedia.org/wiki/Upsert)" operación evita los errores que sucedería si se intenta insertar una fila que ya existe, pero reemplaza los cambios a los datos que haya podido realizar durante la comprobación de la aplicación. Con datos de prueba en algunas tablas puede no ser conveniente que ocurra esto: en algunos casos si cambia los datos durante las pruebas se desea que permanecen después de las actualizaciones de base de datos los cambios. En ese caso en el que desea realizar una operación de inserción condicional: insertar una fila sólo si aún no existe.   
> 
> El primer parámetro pasado a la [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método especifica la propiedad que se va a usar para comprobar si ya existe una fila. Para los datos de la película de prueba que se va a proporcionar, el `Title` propiedad puede utilizarse para este propósito, puesto que cada título de la lista es único:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Este código supone que los títulos sean únicos. Si agrega manualmente un título duplicado, obtendrá la excepción siguiente la próxima vez que se realiza una migración.   
> 
>  *La secuencia contiene más de un elemento*  
> 
> Para obtener más información sobre la [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método, consulte [tener cuidado con el método de EF 4.3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...


**Presione CTRL-MAYÚS + B para compilar el proyecto.** (Los pasos siguientes se producirá un error si no se compilan en este momento.)

El paso siguiente consiste en crear un `DbMigration` clase para la migración inicial. Esta migración crea una nueva base de datos, que es por lo que se elimina la *movie.mdf* archivo en un paso anterior.

En el **Package Manager Console** ventana, escriba el comando `add-migration Initial` para crear la migración inicial. El nombre "Inicial" es arbitrario y se utiliza para asignar nombre al archivo de migración que creó.

![](adding-a-new-field/_static/image6.png)

Migraciones de Code First crea otro archivo de clase en el *migraciones* carpeta (con el nombre *{DateStamp}\_Initial.cs* ), y esta clase contiene código que crea el esquema de base de datos. El nombre de archivo de migración previamente se fija con una marca de tiempo para ayudar a con la ordenación. Examine la *{DateStamp}\_Initial.cs* archivo, contiene las instrucciones para crear el `Movies` tabla para la base de datos de la película. Al actualizar la base de datos en las instrucciones a continuación, esto *{DateStamp}\_Initial.cs* archivo ejecutará y crear el esquema de base de datos. La **inicialización** método se ejecutará para rellenar la base de datos con datos de prueba.

En el **Package Manager Console**, escriba el comando `update-database` para crear la base de datos y ejecutar el `Seed` método.

![](adding-a-new-field/_static/image7.png)

Si se produce un error que indica una tabla ya existe y no se puede crear, probablemente es porque ejecutó la aplicación después de eliminar la base de datos y antes de ejecutar `update-database`. En ese caso, elimine la *Movies.mdf* archivo nuevo y vuelva a intentar la `update-database` comando. Si sigue recibiendo un error, elimine la carpeta migrations y contenido e inicie con las instrucciones que aparecen en la parte superior de esta página (que es eliminar la *Movies.mdf* de archivos, a continuación, continúe con Enable-Migrations). Si sigue recibiendo un encontraron, abra el Explorador de objetos de SQL Server y quitar la base de datos de la lista.

Ejecute la aplicación y navegue hasta la */Movies* dirección URL. Se muestran los datos de inicialización.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adición de una propiedad de clasificación al modelo Movie

Empiece agregando un nuevo `Rating` propiedad existente `Movie` clase. Abra la *Models\Movie.cs* de archivos y agregar el `Rating` propiedad como esta:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

La sección completa `Movie` clase ahora parece similar al código siguiente:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Compilar la aplicación (Ctrl + Mayús + B).

Dado que ha agregado un nuevo campo a la `Movie` (clase), también debe actualizar el enlace *lista blanca* por lo que se incluirá esta nueva propiedad. Actualización de la `bind` atributo `Create` y `Edit` métodos de acción para incluir la `Rating` propiedad:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

También debe actualizar las plantillas de vista para mostrar, crear y editar la nueva propiedad `Rating` en la vista del explorador.

Abra la *\Views\Movies\Index.cshtml* de archivos y agregar un `<th>Rating</th>` justo después de encabezado de columna la **precio** columna. A continuación, agregue un `<td>` columna cerca del final de la plantilla para representar la `@item.Rating` valor. A continuación se muestra qué actualizado *Index.cshtml* plantilla de vista es similar:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

A continuación, abra el *\Views\Movies\Create.cshtml* de archivos y agregar el `Rating` campo con el siguiente marcado marcadas. Esto representa un cuadro de texto para que pueda especificar una clasificación cuando se crea una película nuevo.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Ahora ha actualizado el código de aplicación para admitir los nuevos `Rating` propiedad.

Ejecute la aplicación y navegue hasta la */Movies* dirección URL. Al hacer esto, sin embargo, verá uno de los siguientes errores:

![](adding-a-new-field/_static/image9.png)  
  
El modelo de realizar una copia del contexto de 'MovieDBContext' ha cambiado desde que se creó la base de datos. Considere la posibilidad de usar migraciones de Code First para actualizar la base de datos (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Está viendo este error porque la actualización `Movie` clase de modelo en la aplicación ahora es diferente que el esquema de la `Movie` tabla de la base de datos existente. (No hay ninguna columna `Rating` en la tabla de la base de datos).


Este error se puede resolver de varias maneras:

1. Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo. Este enfoque resulta muy conveniente al principio del ciclo de desarrollo cuando se está realizando el desarrollo activo en una base de datos de prueba; permite desarrollar rápidamente el esquema del modelo y la base de datos juntos. La desventaja, sin embargo, es que se pierden los datos existentes en la base de datos, por lo que *no* va a utilizar este enfoque en una base de datos de producción. Usar un inicializador para inicializar automáticamente una base de datos con datos de prueba suele ser una manera productiva de desarrollar una aplicación. Para obtener más información sobre los inicializadores de base de datos de Entity Framework, vea [tutorial de ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo. La ventaja de este enfoque es que se conservan los datos. Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.
3. Use Migraciones de Code First para actualizar el esquema de la base de datos.


Para este tutorial se usa Migraciones de Code First.

Actualizar el método de inicialización para que proporcione un valor para la nueva columna. Abra el archivo Migrations\Configuration.cs y agregar un campo de clasificación para cada objeto de la película.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Compile la solución y, a continuación, abra el **Package Manager Console** ventana y escriba el comando siguiente:

`add-migration Rating`

El `add-migration` comando indica el marco de trabajo de migración para examinar el modelo de película actual con el esquema actual de la base de datos de la película y crear el código necesario para migrar la base de datos al nuevo modelo. El nombre *clasificación* es arbitrario y se utiliza para asignar nombre al archivo de migración. Es útil emplear un nombre descriptivo para el paso de migración.

Cuando finaliza este comando, Visual Studio abre el archivo de clase que define la nueva `DbMigration` clase derivada y en el `Up` método puede ver el código que crea la nueva columna.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Compile la solución y, a continuación, escriba la `update-database` comando en el **Package Manager Console** ventana.

La siguiente imagen muestra la salida en el **Package Manager Console** ventana (la marca de fecha anteponiendo *clasificación* será diferente.)

![](adding-a-new-field/_static/image11.png)

Vuelva a ejecutar la aplicación y navegue a la dirección URL /Movies. Puede ver el nuevo campo de clasificación.

![](adding-a-new-field/_static/image12.png)

Haga clic en el **crear nuevo** el vínculo para agregar una película nuevo. Tenga en cuenta que puede agregar una clasificación.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Haga clic en **Crear**. La película nuevo, incluida la clasificación, se muestra ahora en la lista de películas:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Ahora que el proyecto está utilizando las migraciones, no debe quitar la base de datos al agregar un nuevo campo o en caso contrario, actualizar el esquema. En la siguiente sección, se podrá realizar más cambios de esquema y use migraciones para actualizar la base de datos.

También debe agregar el `Rating` campo a las plantillas de vista de edición, detalles y Delete.

Puede escribir el comando "update-database" en la **Package Manager Console** volver a la ventana y ningún código de migración se ejecutaría, porque el esquema coincide con el modelo. Sin embargo, se ejecutará la ejecución "update-database" el `Seed` método nuevo, y si ha cambiado alguno de los datos de inicialización, se perderán los cambios porque el `Seed` datos sobre métodos upserts. Más información sobre la `Seed` método en de Tom Dykstra popular [tutorial de ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

En esta sección se ha visto cómo puede modificar objetos de modelo y mantenerlos sincronizados con los cambios de la base de datos. También aprendió una forma de rellenar una base de datos recién creada con datos de ejemplo para probar escenarios. Esto era simplemente una breve introducción a Code First, vea [crear un modelo de datos de Entity Framework para una aplicación de MVC de ASP.NET](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) para ver un tutorial más completado sobre el tema. A continuación, echemos un vistazo a cómo puede agregar una lógica de validación a las clases del modelo y habilitar algunas reglas de negocios que se aplicará.

> [!div class="step-by-step"]
> [Anterior](adding-search.md)
> [Siguiente](adding-validation.md)
