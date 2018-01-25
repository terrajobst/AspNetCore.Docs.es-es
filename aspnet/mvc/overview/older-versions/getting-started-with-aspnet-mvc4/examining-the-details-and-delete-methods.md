---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
title: "Examen de los detalles y métodos de Delete | Documentos de Microsoft"
author: Rick-Anderson
description: "Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 11425ff3-09fc-4efa-be9a-b53bce503460
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: f3c56356aaa595e200a16fe0045a8b00dc5823b7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="examining-the-details-and-delete-methods"></a>Examen de los detalles y los métodos de eliminación
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestra más características.


En esta parte del tutorial, podrá examinar generado automáticamente `Details` y `Delete` métodos.

## <a name="examining-the-details-and-delete-methods"></a>Examen de los detalles y los métodos de eliminación

Abra la `Movie` controlador y examine el `Details` método.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

El motor de scaffolding de MVC que creó este método de acción agrega un comentario que muestra una solicitud HTTP que invoca el método. En este caso es un `GET` solicitud con tres segmentos de dirección URL, el `Movies` controlador, el `Details` método y un `ID` valor.

Código primero resulta muy sencillo buscar datos mediante el `Find` método. Una característica de seguridad importante basada en el método es que el código comprueba que la `Find` método ha encontrado una película antes de que el código intenta hacer algo con él. Por ejemplo, un pirata informático podría producir errores en el sitio cambiando la dirección URL creada por los vínculos de `http://localhost:xxxx/Movies/Details/1` en algo similar a `http://localhost:xxxx/Movies/Details/12345` (o algún otro valor que no representa una película real). Si no activó a una película null, una película null produciría un error de base de datos.

Examine los métodos `Delete` y `DeleteConfirmed`.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Tenga en cuenta que la `HTTP Get``Delete` método no elimina la película especificada, devuelve una vista de la película donde puede enviar (`HttpPost`) la eliminación... La acción de efectuar una operación de eliminación en respuesta a una solicitud GET (o con este propósito efectuar una operación de edición, creación o cualquier otra operación que modifique los datos) genera una vulnerabilidad de seguridad. Para obtener más información sobre esto, vea la entrada del blog de Stephen Walther [ASP.NET MVC Sugerencia nº 46: no use eliminar vínculos porque crean vulnerabilidades de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

El método `HttpPost` que elimina los datos se denomina `DeleteConfirmed` para proporcionar al método HTTP POST una firma o nombre únicos. Las dos firmas de método se muestran a continuación:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Common Language Runtime (CLR) requiere métodos sobrecargados para disponer de una firma de parámetro única (mismo nombre de método, pero lista de parámetros diferente). Sin embargo, aquí deberá dos métodos de eliminación: uno para GET--y otro para POST que tienen la misma firma de parámetro. (ambos deben aceptar un número entero como parámetro).

Para ordenar este comando, puede hacer un par de cosas. Una consiste en asignar nombres diferentes a los métodos. que es lo que hizo el mecanismo de scaffolding en el ejemplo anterior. Pero esto implica un pequeño problema: ASP.NET asigna segmentos de una dirección URL a los métodos de acción por nombre y, si cambia el nombre de un método, normalmente el enrutamiento no podría encontrar ese método. La solución es la que ve en el ejemplo, que consiste en agregar el atributo `ActionName("Delete")` al método `DeleteConfirmed`. Esto realiza eficazmente asignación para el sistema de enrutamiento para que una dirección URL que incluya */Delete/*para una entrada de blog solicitud encontrará el `DeleteConfirmed` método.

Otro método común para evitar un problema con los métodos que tienen nombres idénticos y firmas es artificialmente cambiar la firma del método POST para incluir un parámetro sin usar. Por ejemplo, algunos desarrolladores agregar un tipo de parámetro `FormCollection` que se pasa al método POST y, a continuación, simplemente no usa el parámetro:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Resumen

Ahora tiene una aplicación de ASP.NET MVC completa que almacena los datos en una base de datos de la base de datos local. Puede crear, leer, actualizar, eliminar y buscar películas.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Pasos siguientes

Después de haber creado y probado una aplicación web, el paso siguiente es ponerlo a disposición de otras personas a través de Internet. Para ello, tendrá que implementarlo en un proveedor de hospedaje web. Microsoft ofrece el hospedaje web gratuito para hasta 10 sitios web en un [libre de la cuenta de prueba de Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Le sugiero a continuación siga el tutorial [implementar una aplicación de MVC de ASP.NET seguros con pertenencia, OAuth y base de datos SQL a un sitio Web de Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un excelente tutorial es el nivel intermedio de Tom Dykstra [crear un modelo de datos de Entity Framework para una aplicación de MVC de ASP.NET](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [Stackoverflow](http://stackoverflow.com/help) y [foros de ASP.NET MVC](https://forums.asp.net/1146.aspx) son una gran coloca para formular preguntas. Siga [me](https://twitter.com/RickAndMSFT) en twitter, por lo que puede obtener actualizaciones en mi tutoriales más recientes.

Comentarios son bienvenidos.

: [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter:[@RickAndMSFT](https://twitter.com/RickAndMSFT)  
: [Scott Hanselman](http://www.hanselman.com/blog/) twitter:[@shanselman](https://twitter.com/shanselman)

>[!div class="step-by-step"]
[Anterior](adding-validation-to-the-model.md)
