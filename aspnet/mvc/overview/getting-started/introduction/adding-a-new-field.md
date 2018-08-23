---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Agregar un nuevo campo | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 7326f3d80030cdc5db1c25efc31f59f5b51594f4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835466"
---
<a name="adding-a-new-field"></a>Adición de un nuevo campo
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

En esta sección usará migraciones de Entity Framework Code First para migrar algunos cambios a las clases del modelo, por lo que el cambio se aplica a la base de datos.

De forma predeterminada, al usar Entity Framework Code First para crear automáticamente una base de datos, como hizo anteriormente en este tutorial, Code First agrega una tabla para ayudar a saber si el esquema de la base de datos está sincronizado con las clases del modelo se generó a partir de la base de datos. Si no están sincronizados, Entity Framework produce un error. Esto facilita realizar un seguimiento de los problemas en tiempo de desarrollo en caso contrario, podría encontrar solamente (por errores poco conocidos) en tiempo de ejecución.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configuración de migraciones de Code First para cambios en el modelo

Vaya al explorador de soluciones. Haga clic con el botón derecho en el *Movies.mdf* de archivo y seleccione **eliminar** para quitar la base de datos de películas. Si no ve el *Movies.mdf* de archivos, haga clic en el **mostrar todos los archivos** icono se muestra a continuación, en el contorno rojo.

![](adding-a-new-field/_static/image1.png)

Compile la aplicación para asegurarse de que no hay ningún error.

Desde el **herramientas** menú, haga clic en **Administrador de paquetes de NuGet** y, a continuación, **Package Manager Console**.

![Agregar módulo Man](adding-a-new-field/_static/image2.png)

En el **Package Manager Console** ventana en la `PM>` símbolo del sistema escriba

Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

El **Enable-Migrations** comando (mostrado arriba) crea un *Configuration.cs* archivo en una nueva *migraciones* carpeta.

![](adding-a-new-field/_static/image4.png)

Visual Studio abre el *Configuration.cs* archivo. Reemplace el `Seed` método en el *Configuration.cs* archivo con el código siguiente:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Mantenga el mouse sobre la línea ondulada roja bajo `Movie` y haga clic en `Show Potential Fixes` y, a continuación, haga clic en **mediante** **MvcMovie.Models;**

![](adding-a-new-field/_static/image5.png)

Si lo hace, agrega la siguiente instrucción using:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> El código llama a las migraciones de primera la `Seed` método después de cada migración (es decir, una llamada a **Actualizar base de datos** en la consola de administrador de paquetes), y este método actualiza las filas que ya se han insertado o insertan si se aún no existen.
> 
> El [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método en el código siguiente realiza una operación "upsert":
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Dado que el [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método se ejecuta con cada migración, simplemente no se puede insertar datos, porque las filas que se está intentando agregar ya estarán allí después de la primera migración que crea la base de datos. El "[upsert](http://en.wikipedia.org/wiki/Upsert)" operación evita los errores que sucedería si se intenta insertar una fila que ya existe, pero reemplaza cualquier cambio en los datos que ha realizado durante las pruebas de la aplicación. Con datos de prueba en algunas tablas puede no ser conveniente que esto ocurra: en algunos casos cuando cambien los datos durante las pruebas que desee los cambios tras las actualizaciones de la base de datos. En ese caso en el que desea realizar una operación de inserción condicional: insertar una fila solo si ya no existe.   
> 
> El primer parámetro pasado a la [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método especifica la propiedad que se utilizará para comprobar si ya existe una fila. Para los datos de película de prueba que se va a proporcionar, el `Title` propiedad puede usarse para este propósito, puesto que cada título de la lista es único:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Este código supone que los títulos son únicos. Si agrega manualmente un título duplicado, obtendrá la siguiente excepción la próxima vez que realice una migración.   
> 
>  *La secuencia contiene más de un elemento*  
> 
> Para obtener más información sobre la [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método, consulte [tener cuidado con el método de EF 4.3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...


**Presione CTRL-MAYÚS-B para compilar el proyecto.** (Los pasos siguientes se producirá un error si no se crea en este momento.)

El siguiente paso es crear un `DbMigration` clase para la migración inicial. Esta migración crea una nueva base de datos, por eso se eliminó el *movie.mdf* archivo en el paso anterior.

En el **Package Manager Console** ventana, escriba el comando `add-migration Initial` para crear la migración inicial. El nombre "Inicial" es arbitrario y se usa un nombre al archivo de migración creado.

![](adding-a-new-field/_static/image6.png)

Migraciones de Code First crea otro archivo de clase en el *migraciones* carpeta (con el nombre *{fecha}\_Initial.cs* ), y esta clase contiene código que crea el esquema de base de datos. Previamente se ha corregido el nombre de archivo de migración con una marca de tiempo para ayudar a con la ordenación. Examine el *{fecha}\_Initial.cs* archivo, contiene las instrucciones para crear el `Movies` tabla para la base de datos de la película. Al actualizar la base de datos en las instrucciones que aparecen a continuación, esto *{fecha}\_Initial.cs* archivo ejecutará y crear el esquema de base de datos. El **inicialización** método se ejecutará para rellenar la base de datos con datos de prueba.

En el **Package Manager Console**, escriba el comando `update-database` para crear la base de datos y ejecutar el `Seed` método.

![](adding-a-new-field/_static/image7.png)

Si se produce un error que indica una tabla ya existe y no se puede crear, probablemente es porque se ejecutó la aplicación después de eliminar la base de datos y antes de ejecutar `update-database`. En ese caso, elimine la *Movies.mdf* archivo nuevo y vuelva a intentar la `update-database` comando. Si sigue apareciendo un error, elimine la carpeta de migraciones y contenido, comience con las instrucciones que aparecen en la parte superior de esta página (que es eliminar el *Movies.mdf* de archivos, a continuación, continúe con Enable-Migrations). Si sigue obteniendo un error, abra el Explorador de objetos de SQL Server y quitar la base de datos de la lista.

Ejecute la aplicación y vaya a la */Movies* dirección URL. Se muestran los datos de inicialización.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adición de una propiedad de clasificación al modelo Movie

Empiece agregando un nuevo `Rating` propiedad existente `Movie` clase. Abra el *Models\Movie.cs* y agréguele el `Rating` propiedad como esta:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

La completa `Movie` clase ahora parece similar al código siguiente:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Compile la aplicación (Ctrl + Mayús + B).

Dado que ha agregado un nuevo campo a la `Movie` (clase), también debe actualizar el enlace *lista de permitidos* por lo que se incluirá esta nueva propiedad. Actualización de la `bind` atributo `Create` y `Edit` métodos de acción para incluir el `Rating` propiedad:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

También debe actualizar las plantillas de vista para mostrar, crear y editar la nueva propiedad `Rating` en la vista del explorador.

Abra el *\Views\Movies\Index.cshtml* archivo y agregue un `<th>Rating</th>` justo después de encabezado de columna la **precio** columna. A continuación, agregue un `<td>` columna cerca del final de la plantilla para representar el `@item.Rating` valor. A continuación es lo que la actualización *Index.cshtml* plantilla de vista es similar:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

A continuación, abra el *\Views\Movies\Create.cshtml* y agréguele el `Rating` campo con el siguiente marcado marcadas. Esto representa un cuadro de texto para que pueda especificar una clasificación cuando se crea una nueva película.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Se ha actualizado el código de aplicación para admitir los nuevos `Rating` propiedad.

Ejecute la aplicación y vaya a la */Movies* dirección URL. Al hacerlo, sin embargo, verá uno de los errores siguientes:

![](adding-a-new-field/_static/image9.png)  
  
El modelo que respalda el contexto 'MovieDBContext' ha cambiado desde que se creó la base de datos. Considere la posibilidad de usar migraciones de Code First para actualizar la base de datos (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Si ve este error porque la actualización `Movie` ahora es diferente del esquema de clase de modelo en la aplicación la `Movie` tabla de la base de datos existente. (No hay ninguna columna `Rating` en la tabla de la base de datos).


Este error se puede resolver de varias maneras:

1. Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo. Este enfoque resulta muy conveniente al principio del ciclo de desarrollo cuando se está realizando el desarrollo activo en una base de datos de prueba; permite desarrollar rápidamente el esquema del modelo y la base de datos juntos. La desventaja, sin embargo, es la que se pierden los datos existentes en la base de datos, por lo que se *no* va a utilizar este enfoque en una base de datos de producción. Usar un inicializador para inicializar automáticamente una base de datos con datos de prueba suele ser una manera productiva de desarrollar una aplicación. Para obtener más información sobre los inicializadores de base de datos de Entity Framework, vea [tutorial de ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo. La ventaja de este enfoque es que se conservan los datos. Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.
3. Use Migraciones de Code First para actualizar el esquema de la base de datos.


Para este tutorial se usa Migraciones de Code First.

Actualice el método de inicialización para que proporcione un valor para la nueva columna. Abrir archivo migrations\configuration. cs y agregue un campo de clasificación para cada objeto de película.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Compile la solución y, a continuación, abra el **Package Manager Console** ventana y escriba el siguiente comando:

`add-migration Rating`

El `add-migration` comando indica el marco de trabajo de migración para examinar el modelo de película actual con el esquema actual de la base de datos de la película y crear el código necesario para migrar la base de datos al nuevo modelo. El nombre *clasificación* es arbitrario y se utiliza un nombre al archivo de migración. Resulta útil usar un nombre descriptivo para el paso de migración.

Cuando finalice este comando, Visual Studio abre el archivo de clase que define la nueva `DbMigration` clase derivada y en el `Up` método puede ver el código que crea la nueva columna.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Compile la solución y, a continuación, escriba el `update-database` comando en el **Package Manager Console** ventana.

La siguiente imagen muestra la salida en el **Package Manager Console** ventana (la anteposición de la marca de fecha *clasificación* será diferente.)

![](adding-a-new-field/_static/image11.png)

Vuelva a ejecutar la aplicación y vaya a la URL /Movies. Puede ver el nuevo campo de clasificación.

![](adding-a-new-field/_static/image12.png)

Haga clic en el **crear nuevo** el vínculo para agregar una nueva película. Tenga en cuenta que puede agregar una clasificación.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Haga clic en **Crear**. La nueva película, incluida la clasificación, se muestra ahora en la lista de películas:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Ahora que el proyecto usa las migraciones, no tendrá que quitar la base de datos al agregar un campo nuevo o actualizar en caso contrario, el esquema. En la sección siguiente, se podrá realizar más cambios de esquema y usar las migraciones para actualizar la base de datos.

También debe agregar el `Rating` campo a las plantillas de vista de edición, detalles y eliminación.

Puede escribir el comando "update-database" en el **Package Manager Console** vuelva a la ventana y ningún código de migración ejecutaría, porque el esquema coincide con el modelo. Sin embargo, ejecuta "update-database" ejecutará el `Seed` método nuevo, y si cambia cualquiera de los datos de inicialización, se perderán los cambios porque el `Seed` datos del método realiza una operación Upsert. Puede leer más sobre la `Seed` método de Tom Dykstra popular [tutorial de ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

En esta sección, ha visto cómo puede modificar los objetos de modelo y mantener sincronizados con los cambios de la base de datos. También ha aprendido cómo rellenar una base de datos recién creada con datos de ejemplo para que pueda probar escenarios. Esto era simplemente una breve introducción a Code First, consulte [creación de un modelo de datos de Entity Framework para una aplicación ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) para ver un tutorial más completo sobre el tema. A continuación, echemos un vistazo a cómo puede agregar lógica de validación más completa para las clases del modelo y habilitar algunas reglas de negocios que se aplicará.

> [!div class="step-by-step"]
> [Anterior](adding-search.md)
> [Siguiente](adding-validation.md)
