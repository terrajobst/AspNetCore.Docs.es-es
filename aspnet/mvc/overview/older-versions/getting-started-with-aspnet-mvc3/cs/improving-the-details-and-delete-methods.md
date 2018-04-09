---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
title: Mejora de los detalles y métodos de eliminación (C#) | Documentos de Microsoft
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 3f42edd9-c5b8-4712-9055-970f7d38e350
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 55945eb373c79fd6ae018fe8f896dc5e6bbe7744
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="improving-the-details-and-delete-methods-c"></a>Mejora de los detalles y métodos de eliminación (C#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestra más características.
> 
> 
> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar por separado los requisitos previos mediante los siguientes vínculos:
> 
> - [Requisitos previos visuales Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)
> 
> Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con el código fuente de C# está disponible como acompañamiento de este tema. [Descargue la versión de C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si prefiere Visual Basic, cambie a la [versión de Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de este tutorial.


En esta parte del tutorial, realizará algunas mejoras en el que se han generado automáticamente `Details` y `Delete` métodos. No están necesarios que estos cambios, pero con tan solo unos pequeños fragmentos de código, puede mejorar fácilmente la aplicación.

## <a name="improving-the-details-and-delete-methods"></a>Mejorar los detalles y los métodos de eliminación

Cuando scaffolding el `Movie` controlador de MVC de ASP.NET genera código que funcionaba bien, pero que pueden realizarse más sólida con tan solo unos pequeños cambios.

Abra la `Movie` controlador y modificar el `Details` método devolviendo `HttpNotFound` cuando no se encuentra una película. También debe modificar el `Details` método para establecer un valor predeterminado para el identificador que se pasa a él. (Realizar modificaciones similares a la `Edit` método [parte 6](examining-the-edit-methods-and-edit-view.md) de este tutorial.) Sin embargo, debe cambiar el tipo de valor devuelto de la `Details` método de `ViewResult` a `ActionResult`, porque la `HttpNotFound` método no devuelve un `ViewResult` objeto. En el ejemplo siguiente se muestra la modificación `Details` método.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample1.cs)]

Código primero resulta muy sencillo buscar datos mediante el `Find` método. Una característica de seguridad importante que creamos en el método es que el código comprueba que la `Find` método ha encontrado una película antes de que el código intenta hacer algo con él. Por ejemplo, un pirata informático podría producir errores en el sitio cambiando la dirección URL creada por los vínculos de `http://localhost:xxxx/Movies/Details/1` en algo similar a `http://localhost:xxxx/Movies/Details/12345` (o algún otro valor que no representa una película real). Si no lo hace la comprobación de una película null, se podrían un error de base de datos.

Del mismo modo, cambiar la `Delete` y `DeleteConfirmed` métodos para especificar un valor predeterminado para el parámetro ID y devolver `HttpNotFound` cuando no se encuentra una película. La actualización `Delete` métodos en el `Movie` controlador se muestran a continuación.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample2.cs)]

Tenga en cuenta que el `Delete` método no elimina los datos. La acción de efectuar una operación de eliminación en respuesta a una solicitud GET (o con este propósito efectuar una operación de edición, creación o cualquier otra operación que modifique los datos) genera una vulnerabilidad de seguridad. Para obtener más información sobre esto, vea la entrada del blog de Stephen Walther [ASP.NET MVC Sugerencia nº 46: no use eliminar vínculos porque crean vulnerabilidades de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

El método `HttpPost` que elimina los datos se denomina `DeleteConfirmed` para proporcionar al método HTTP POST una firma o nombre únicos. Las dos firmas de método se muestran a continuación:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample3.cs)]

Common language runtime (CLR) requiere métodos sobrecargados para tener una firma única (mismo nombre, lista de parámetros diferentes). Sin embargo, aquí deberá dos métodos de eliminación: uno para GET--y otro para POST ambos requieren la misma firma. (ambos deben aceptar un número entero como parámetro).

Para ordenar este comando, puede hacer un par de cosas. Una consiste en asignar nombres diferentes a los métodos. Eso es lo que hicimos en que el anterior ejemplo. Pero esto implica un pequeño problema: ASP.NET asigna segmentos de una dirección URL a los métodos de acción por nombre y, si cambia el nombre de un método, normalmente el enrutamiento no podría encontrar ese método. La solución es la que ve en el ejemplo, que consiste en agregar el atributo `ActionName("Delete")` al método `DeleteConfirmed`. Esto realiza eficazmente asignación para el sistema de enrutamiento para que una dirección URL que incluya <em>/Delete/</em>para una entrada de blog solicitud encontrará el `DeleteConfirmed` método.

Otra manera de evitar un problema con los métodos que tienen nombres idénticos y firmas es artificialmente cambiar la firma del método POST para incluir un parámetro sin usar. Por ejemplo, algunos desarrolladores agregar un tipo de parámetro `FormCollection` que se pasa al método POST y, a continuación, simplemente no usa el parámetro:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="wrapping-up"></a>Resumen

Ahora tiene una aplicación de ASP.NET MVC completa que almacena los datos en una base de datos de SQL Server Compact. Puede crear, leer, actualizar, eliminar y buscar películas.

![](improving-the-details-and-delete-methods/_static/image1.png)

Este tutorial básico de tenemos que pueda empezar a hacer que los controladores, asociarlos con las vistas y pasar sobre los datos codificados de forma rígida. A continuación, ha creado y había diseñado un modelo de datos. Entity Framework Code First, se crea una base de datos del modelo de datos sobre la marcha y el sistema de scaffolding de ASP.NET MVC genera automáticamente los métodos de acción y vistas de operaciones CRUD básicas. A continuación, agrega un formulario de búsqueda que permite a los usuarios buscar la base de datos. Cambiar la base de datos para incluir una nueva columna de datos y, a continuación, actualiza dos páginas para crear y mostrar los nuevos datos. Agregar validación marcando el modelo de datos con atributos de la `DataAnnotations` espacio de nombres. La validación resultante se ejecuta en el cliente y en el servidor.

Si desea implementar la aplicación, es útil para la primera prueba la aplicación en el servidor local de IIS 7. Puede utilizar esta [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) vínculo para habilitar la configuración de IIS para las aplicaciones ASP.NET. Vea los vínculos de implementación siguientes:

- [Mapa de contenido de implementación de ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [Habilitar IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Implementación de proyectos de aplicación Web](https://msdn.microsoft.com/library/dd394698.aspx)

Ahora le animo a pasar a nuestro nivel intermedio [crear un modelo de datos de Entity Framework para una aplicación de MVC de ASP.NET](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) y [tienda de música de MVC](../../mvc-music-store/mvc-music-store-part-1.md) tutoriales para explorar el [ASP.NET artículos en MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)y para desproteger los vídeos y recursos en muchas [ https://asp.net/mvc ](https://asp.net/mvc) para más información sobre ASP.NET MVC. El [foros de ASP.NET MVC](https://forums.asp.net/1146.aspx) son un buen lugar para hacer preguntas.

Disfrútelo.

: Scott Hanselman ([ http://hanselman.com ](http://hanselman.com) y [ @shanselman ](http://twitter.com/shanselman) en Twitter) y Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Anterior](adding-validation-to-the-model.md)
