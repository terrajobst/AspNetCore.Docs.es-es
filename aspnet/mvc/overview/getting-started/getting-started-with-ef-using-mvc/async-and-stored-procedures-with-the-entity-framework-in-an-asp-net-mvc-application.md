---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: Usar procedimientos almacenados y asincrónicos con EF en una aplicación de MVC de ASP.NET'
description: En este tutorial verá cómo implementar el modelo de programación asincrónico y obtenga información sobre cómo utilizar procedimientos almacenados.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 0896664174bc2fee65b73ecf256d994f2abacc0a
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444368"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a>Tutorial: Usar procedimientos almacenados y asincrónicos con EF en una aplicación de MVC de ASP.NET

En los tutoriales anteriores, ha aprendido cómo leer y actualizar datos mediante el modelo de programación sincrónica. En este tutorial verá cómo implementar el modelo de programación asincrónico. El código asincrónico puede ayudar a una aplicación a un mejor rendimiento porque hace un mejor uso de recursos del servidor.

En este tutorial también verá cómo usar procedimientos almacenados para insert, update y las operaciones de eliminación en una entidad.

Por último, volver a implementar la aplicación en Azure, junto con todos los cambios de base de datos que ha implementado desde la primera vez que se ha implementado.

En las ilustraciones siguientes se muestran algunas de las páginas con las que va a trabajar.

![Página de los departamentos de TI](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Creación de departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

En este tutorial ha:

> [!div class="checklist"]
> * Obtenga información sobre el código asincrónico
> * Crear un controlador de departamento
> * Usar procedimientos almacenados
> * Implementar en Azure

## <a name="prerequisites"></a>Requisitos previos

* [Actualización de datos relacionados](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a>¿Por qué usar código asincrónico

Un servidor web tiene un número limitado de subprocesos disponibles y, en situaciones de carga alta, es posible que todos los subprocesos disponibles estén en uso. Cuando esto ocurre, el servidor no puede procesar nuevas solicitudes hasta que los subprocesos se liberen. Con el código sincrónico, se pueden acumular muchos subprocesos mientras no estén realizando ningún trabajo porque están a la espera de que finalice la E/S. Con el código asincrónico, cuando un proceso está a la espera de que finalice la E/S, se libera su subproceso para el que el servidor lo use para el procesamiento de otras solicitudes. Como resultado, el código asincrónico permite que los recursos de servidor se puede utilizar con mayor eficiencia y el servidor está habilitado para administrar más tráfico sin retrasos.

En versiones anteriores de. NET, estaban complejo, escribir y probar el código asincrónico propenso a errores y difícil de depurar. .NET Framework 4.5, es mucho más fácil que generalmente debería escribir código asincrónico a menos que tenga una razón para no escribir, probar y depurar código asincrónico. El código asincrónico introduce una pequeña cantidad de sobrecarga, pero para situaciones de poco tráfico la disminución del rendimiento es insignificante, mientras que en situaciones de tráfico elevado, es importante la mejora del rendimiento potencial.

Para obtener más información sobre la programación asincrónica, vea [soporte para asincronía para evitar el bloqueo de las llamadas de uso .NET 4.5](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-department-controller"></a>Crear el controlador de departamento

Crear un controlador de departamento, la misma manera que los controladores anteriores, salvo que esta vez seleccione el **usar acciones de controlador asincrónicas** casilla de verificación.

Los puntos destacados siguientes muestran lo que se agregó al código sincrónico para el `Index` método para que sea asincrónica:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Cuatro cambios se aplicaron para habilitar la consulta de base de datos de Entity Framework ejecutar de forma asincrónica:

- El método está marcado con el `async` palabra clave, que le indica al compilador que genere devoluciones de llamada para partes del cuerpo del método y para crear automáticamente el `Task<ActionResult>` objeto devuelto.
- Se ha cambiado el tipo de valor devuelto de `ActionResult` a `Task<ActionResult>`. El `Task<T>` tipo representa el trabajo en curso con un resultado de tipo `T`.
- El `await` palabra clave se aplicó a la llamada al servicio web. Cuando el compilador ve esta palabra clave, en segundo plano divide el método en dos partes. La primera parte termina con la operación que se inicia de forma asincrónica. La segunda parte se coloca en un método de devolución de llamada que se llama cuando se complete la operación.
- La versión asincrónica de la `ToList` se llamó al método de extensión.

¿Por qué es el `departments.ToList` instrucción pero no puede modificar el `departments = db.Departments` instrucción? El motivo es que sólo las instrucciones que hacen que las consultas o comandos se envíen a la base de datos se ejecutan de forma asincrónica. El `departments = db.Departments` instrucción establece una consulta, pero no se ejecuta la consulta hasta la `ToList` se llama al método. Por lo tanto, solo el `ToList` método se ejecuta de forma asincrónica.

En el `Details` método y el `HttpGet` `Edit` y `Delete` métodos, el `Find` método es lo que hace que una consulta para enviarse a la base de datos, por lo que es el método que se ejecuta de forma asincrónica:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

En el `Create`, `HttpPost Edit`, y `DeleteConfirmed` métodos, es el `SaveChanges` llamada a método que hace que se ejecuta un comando, no las instrucciones como `db.Departments.Add(department)` lo que solo hacer que las entidades en memoria que se puede modificar.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Abra *Views\Department\Index.cshtml*y reemplace el código de plantilla con el código siguiente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Este código cambia el título de índice a los departamentos de TI, mueve el nombre de administrador a la derecha y proporciona el nombre completo del administrador.

En el crear, eliminar, detalles y editar vistas, cambie el título de la `InstructorID` campo a la misma manera que cambió el campo de nombre de departamento a "Departamento" en las vistas de Course "administrador".

En la creación y edición de las vistas de usar el código siguiente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

En las vistas Delete y Details, use el código siguiente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Ejecute la aplicación y haga clic en el **departamentos** ficha.

Todo funciona igual que en los demás controladores, pero en este controlador de todas las consultas SQL se ejecutan de forma asincrónica.

Algunos aspectos que tener en cuenta cuando se utiliza la programación asincrónica con Entity Framework:

- El código asincrónico no es seguro para subprocesos. En otras palabras, en otras palabras, no intente realizar varias operaciones en paralelo con la misma instancia de contexto.
- Si quiere aprovechar las ventajas de rendimiento del código asincrónico, asegúrese de que en los paquetes de biblioteca que use (por ejemplo para paginación), también se usa async si llaman a cualquier método de Entity Framework que haga que las consultas se envíen a la base de datos.

## <a name="use-stored-procedures"></a>Usar procedimientos almacenados

Algunos desarrolladores y administradores prefieren usar procedimientos almacenados para el acceso a la base de datos. En versiones anteriores de Entity Framework puede recuperar datos mediante un procedimiento almacenado por [ejecutando una consulta SQL sin formato](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), pero no se puede indicar a EF que use los procedimientos almacenados para las operaciones de actualización. En EF 6 es fácil de configurar Code First para utilizar procedimientos almacenados.

1. En *DAL\SchoolContext.cs*, agregue el código resaltado a la `OnModelCreating` método.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Este código indica a Entity Framework a los procedimientos de uso almacenado para insertar, actualizar y eliminar operaciones en el `Department` entidad.
2. En la consola de administración de paquetes, escriba el siguiente comando:

    `add-migration DepartmentSP`

    Abra *migraciones\&lt; marca de tiempo&gt;\_DepartmentSP.cs* para ver el código en el `Up` método que crea Insert, Update y Delete de procedimientos almacenados:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. En la consola de administración de paquetes, escriba el siguiente comando:

     `update-database`
4. Ejecute la aplicación en modo de depuración, haga clic en el **departamentos** pestaña y, a continuación, haga clic en **crear nuevo**.
5. Escriba los datos de un nuevo departamento y, a continuación, haga clic en **crear**.

6. En Visual Studio, examine los registros en el **salida** ventana para ver que se usó un procedimiento almacenado para insertar la nueva fila de departamento.

     ![SP de inserción de departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Código primero crea nombres de procedimientos almacenados de forma predeterminada. Si usa una base de datos existente, es posible que deba personalizar los nombres de procedimiento almacenado con el fin de utilizar procedimientos almacenados definidos en la base de datos. Para obtener información acerca de cómo hacerlo, consulte [Entity Framework código primera Insertar/Actualizar/Eliminar procedimientos almacenados](https://msdn.microsoft.com/data/dn468673).

Si desea personalizar lo que genera los procedimientos almacenados, puede editar el código con scaffolding para las migraciones `Up` método que crea el procedimiento almacenado. De este modo los cambios se reflejan siempre que la migración se ejecuta y se aplicarán a la base de datos de producción cuando las migraciones se ejecuta automáticamente en producción después de la implementación.

Si desea cambiar un procedimiento almacenado existente que se creó en una migración anterior, puede usar el comando Add-Migration para generar una migración en blanco y, a continuación, escribir manualmente el código que llama el [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) (método) .

## <a name="deploy-to-azure"></a>Implementar en Azure

En esta sección, deberá haber completado opcional **implementar la aplicación en Azure** sección la [implementación y migraciones](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial de esta serie. Si tenía errores de las migraciones que puede resolver mediante la eliminación de la base de datos en su proyecto local, omita esta sección.

1. En Visual Studio, haga clic en el proyecto en **el Explorador de soluciones** y seleccione **publicar** en el menú contextual.
2. Haga clic en **Publicar**.

    Visual Studio implementa la aplicación en Azure y la aplicación se abre en el explorador predeterminado, que se ejecuta en Azure.
3. Probar la aplicación para comprobar funciona.

    La primera vez que ejecute una página que tiene acceso a la base de datos, Entity Framework se ejecuta todas las migraciones `Up` métodos necesarios para la base de datos al día con el modelo de datos actual. Ahora puede usar todas las páginas web que ha agregado desde la última vez que se ha implementado, incluidas las páginas de departamento que agregó en este tutorial.

## <a name="get-the-code"></a>Obtención del código

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a>Recursos adicionales

Pueden encontrar vínculos a otros recursos de Entity Framework en el [acceso a datos de ASP.NET - recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha:

> [!div class="checklist"]
> * Aprendió a utilizar código asincrónico
> * Crea un controlador de departamento
> * Usar procedimientos almacenados
> * Implementar en Azure

Avance al siguiente artículo para obtener información sobre cómo controlar los conflictos cuando varios usuarios actualizan la misma entidad al mismo tiempo.
> [!div class="nextstepaction"]
> [Control de simultaneidad](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
