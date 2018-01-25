---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Async y procedimientos almacenados con Entity Framework en una aplicación ASP.NET MVC | Documentos de Microsoft"
author: tdykstra
description: "La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 5 con Code First de Entity Framework 6 y Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 7412b32ac29179dfa319544781d4c7165c58196b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Async y procedimientos almacenados con Entity Framework en una aplicación ASP.NET MVC
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [descarga de PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 5 con Code First de Entity Framework 6 y Visual Studio 2013. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


En los tutoriales anteriores aprendió a leer y actualizar datos mediante el modelo de programación sincrónica. En este tutorial aprenderá a implementar el modelo de programación asincrónico. Código asincrónico puede ayudar a una aplicación tienen un mejor rendimiento porque hace un mejor uso de recursos del servidor.

En este tutorial también verá cómo usar procedimientos almacenados de inserción, actualización y las operaciones de eliminación en una entidad.

Por último, deberá volver a implementar la aplicación en Azure, junto con todos los cambios de base de datos que se han implementado desde la primera vez que se ha implementado.

Las ilustraciones siguientes muestran algunas de las páginas que va a trabajar con.

![Página de departamentos](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Crear departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>¿Por qué preocuparse por código asincrónico

Un servidor web tiene un número limitado de subprocesos disponibles y, en situaciones de alta carga todos los subprocesos disponibles pueden estar en uso. Cuando esto ocurre, el servidor no puede procesar nuevas solicitudes hasta que se liberan los subprocesos. Con código sincrónico, se pueden acumular muchos subprocesos la mientras que realmente no están realizando ningún trabajo porque está a la espera de e/s completar. Con código asincrónico, cuando un proceso está en espera de e/s completar, un subproceso se liberen para el servidor que se usará para procesando otras solicitudes. Como resultado, código asincrónico permite que los recursos de servidor que se utilizan de manera más eficaz y el servidor está habilitado para administrar más tráfico sin retrasos.

En versiones anteriores de. NET, estaban complejo, escribir y probar código asincrónico error propensa a y difícil de depurar. En .NET 4.5, son mucho más fácil que por lo general debe escribir código asincrónico a menos que tenga un motivo no a escribir, probar y depurar código asincrónico. Código asincrónico que introduce una pequeña cantidad de sobrecarga, pero para situaciones de poco tráfico la disminución del rendimiento es insignificante, mientras en situaciones de tráfico elevado, es importante la mejora del rendimiento potencial.

Para obtener más información sobre la programación asincrónica, vea [acepta el funcionamiento asincrónico de .NET 4.5 de uso para evitar el bloqueo llamadas](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-the-department-controller"></a>Crear el controlador de departamento

Crear un controlador de departamento del mismo modo que hizo los controladores anteriores, pero esta vez seleccione el **usar async controlador** casilla de verificación de acciones.

![Scaffolding de controlador de departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Resalta de las características agregadas al código sincrónico de la `Index` método para que sea asincrónica:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Para habilitar la consulta de base de datos de Entity Framework para ejecutar de forma asincrónica se aplicaran los cuatro cambios:

- El método se marca con la `async` palabra clave, que indica al compilador que se va a generar las devoluciones de llamada para las partes del cuerpo del método como crear automáticamente la `Task<ActionResult>` objeto que se devuelve.
- Se cambió el tipo de valor devuelto de `ActionResult` a `Task<ActionResult>`. El `Task<T>` tipo representa el trabajo en curso con un resultado de tipo `T`.
- El `await` (palabra clave) se aplicó al llamar al servicio web. Cuando el compilador ve esta palabra clave, en segundo plano divide el método en dos partes. La primera parte se termina con la operación que se inicia de forma asincrónica. La segunda parte se coloca en un método de devolución de llamada que se llama cuando finaliza la operación.
- La versión asincrónica de la `ToList` se llamó el método de extensión.

¿Por qué es el `departments.ToList` instrucción pero no modifica el `departments = db.Departments` instrucción? El motivo es que sólo las instrucciones que hacen que las consultas o comandos se envíen a la base de datos se ejecutan de forma asincrónica. El `departments = db.Departments` instrucción establece una consulta, pero no se ejecuta la consulta hasta la `ToList` se llama al método. Por lo tanto, solo el `ToList` método se ejecuta de forma asincrónica.

En el `Details` método y `HttpGet` `Edit` y `Delete` métodos, el `Find` método es el que se hace una consulta para enviarse a la base de datos, por lo que es el método que se ejecuta de forma asincrónica:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

En el `Create`, `HttpPost Edit`, y `DeleteConfirmed` métodos, es el `SaveChanges` llamada al método que hace que se ejecute un comando, no las instrucciones como `db.Departments.Add(department)` lo que solo hacer que las entidades en memoria que se puede modificar.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Abra *Views\Department\Index.cshtml*y reemplace el código de plantilla con el código siguiente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Este código cambia el título de índice para departamentos, mueve el nombre de administrador a la derecha y proporciona el nombre completo del administrador.

En crear, eliminar, obtener información detallada y editar vistas, cambie el título de la `InstructorID` campo "Administrador" de la misma manera que cambia el campo de nombre de departamento a "Departamento" en las vistas de curso.

En la creación y edición de vistas utilizan el código siguiente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

En las vistas de eliminación y los detalles, utilice el código siguiente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Ejecute la aplicación y haga clic en el **departamentos** ficha.

![Página de departamentos](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Todo funciona igual que en los demás controladores, pero en este controlador de todas las consultas SQL se ejecutan de forma asincrónica.

Algunos aspectos que tener en cuenta cuando se utiliza la programación asincrónica con Entity Framework:

- El código asincrónico no es seguro para subprocesos. En otras palabras, en otras palabras, no intentar llevar a cabo varias operaciones en paralelo con la misma instancia de contexto.
- Si desea aprovechar las ventajas de rendimiento de código asincrónico, asegúrese de que cualquier biblioteca de paquetes que está usando (por ejemplo de paginación), también usan asincronía si llama a los métodos de Entity Framework que hacer que las consultas se envían a la base de datos.

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>Usar procedimientos almacenados para insertar, actualizar y eliminar

Algunos desarrolladores y administradores prefieren usar procedimientos almacenados para el acceso a la base de datos. En versiones anteriores de Entity Framework puede recuperar datos mediante un procedimiento almacenado por [ejecutar una consulta SQL sin formato](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), pero no se puede indicar a EF utilizar procedimientos almacenados para las operaciones de actualización. En EF 6 es fácil de configurar Code First para utilizar procedimientos almacenados.

1. En *DAL\SchoolContext.cs*, agregue el código resaltado para el `OnModelCreating` método.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Este código indica a Entity Framework a los procedimientos de uso almacenado para insertar, actualizar y eliminar operaciones en el `Department` entidad.
2. En la consola de administración del paquete, escriba el siguiente comando:

    `add-migration DepartmentSP`

    Abra *migraciones\&lt; marca de tiempo&gt;\_DepartmentSP.cs* para ver el código en el `Up` método que crea Insert, Update y Delete procedimientos almacenados:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
- En la consola de administración del paquete, escriba el siguiente comando:

    `update-database`
- Ejecute la aplicación en modo de depuración, haga clic en el **departamentos** ficha y, a continuación, haga clic en **crear nuevo**.
- Escriba los datos de un departamento nuevo y, a continuación, haga clic en **crear**.

    ![Crear departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
- En Visual Studio, examine los registros en la **salida** ventana para ver que un procedimiento almacenado se utiliza para insertar la nueva fila de departamento.

    ![SP de inserción de departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Código crea primero los nombres de los procedimientos almacenados de forma predeterminada. Si usa una base de datos existente, debe personalizar los nombres de procedimientos almacenados con el fin de utilizar procedimientos almacenados ya definidos en la base de datos. Para obtener información acerca de cómo hacerlo, consulte [Entity Framework código primera Insert/Update/Delete procedimientos almacenados](https://msdn.microsoft.com/data/dn468673).

Si desea personalizar qué genera procedimientos almacenados, puede modificar el código con scaffolding para las migraciones `Up` método que crea el procedimiento almacenado. De este modo los cambios se reflejan cada vez que la migración se ejecuta y se aplicarán a la base de datos de producción cuando las migraciones se ejecuta automáticamente en producción después de la implementación.

Si desea cambiar un procedimiento almacenado existente que se creó en una migración anterior, puede usar el comando Add-Migration para generar una migración en blanco y, a continuación, escribir código que llama manualmente el [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) (método) .

## <a name="deploy-to-azure"></a>Implementar en Azure

Esta sección requiere que se complete la parte opcional **a implementar la aplicación en Azure** sección la [migraciones e implementación](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial de esta serie. Si tenía errores de las migraciones que resuelven mediante la eliminación de la base de datos en su proyecto local, omita esta sección.

1. En Visual Studio, haga clic en el proyecto en **el Explorador de soluciones** y seleccione **publicar** en el menú contextual.
2. Haga clic en **Publicar**.

    Visual Studio implementa la aplicación en Azure y la aplicación se abre en el explorador predeterminado, que se ejecuta en Azure.
3. Probar la aplicación para comprobar funciona.

    La primera vez que ejecute una página que tiene acceso a la base de datos, Entity Framework se ejecuta todas las migraciones `Up` métodos necesarios para poner la base de datos al día con el modelo de datos actual. Ahora puede usar todas las páginas web que agregan desde la última vez que se ha implementado, incluidas las páginas de departamento que agregó en este tutorial.

## <a name="summary"></a>Resumen

En este tutorial ha visto cómo mejorar la eficacia de servidor mediante la escritura de código que se ejecuta de forma asincrónica y cómo usar procedimientos almacenados para insertar, actualizar y eliminar operaciones. En el siguiente tutorial, verá cómo evitar la pérdida de datos cuando varios usuarios intentan editar el mismo registro a la vez.

Vínculos a otros recursos de Entity Framework pueden encontrarse en el [ASP.NET Data Access: recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Anterior](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Siguiente](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
