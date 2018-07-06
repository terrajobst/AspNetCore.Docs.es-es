---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
title: Mejora de los métodos Details y Delete (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: c5c14ef0-c128-4dc1-8c01-7f0fdb09e411
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 5b34c7d71e615215e933b607562a06f683c6c1ca
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819869"
---
<a name="improving-the-details-and-delete-methods-vb"></a>Mejora de los métodos Details y Delete (VB)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todos ellos haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos mediante los vínculos siguientes:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tiempo de ejecución de herramientas de soporte técnico +)
> 
> Si usa Visual Studio 2010, en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con código fuente VB.NET está disponible como acompañamiento de este tema. [Descargue la versión VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [C# versión](../cs/improving-the-details-and-delete-methods.md) de este tutorial.


En esta parte del tutorial, realizará algunas mejoras en el que se han generado automáticamente `Details` y `Delete` métodos. Estos cambios no son necesarios, pero con tan solo unos pequeños fragmentos de código, puede mejorar fácilmente la aplicación.

## <a name="improving-the-details-and-delete-methods"></a>Mejora de los métodos Details y Delete

Cuando se ha aplicado scaffolding la `Movie` , controlador de MVC de ASP.NET genera código que funcionaba muy bien, pero que se pueden realizar más sólida con tan solo unos pequeños cambios.

Abra el `Movie` controlador y modificar el `Details` método devolviendo `HttpNotFound` cuando no se encuentra una película. También se debe modificar el `Details` método para establecer un valor predeterminado para el identificador que se pasa a él. (Realizar modificaciones similares a los `Edit` método [parte 6](examining-the-edit-methods-and-edit-view.md) de este tutorial.) Sin embargo, debe cambiar el tipo de valor devuelto de la `Details` método desde `ViewResult` a `ActionResult`, porque el `HttpNotFound` método no devuelve un `ViewResult` objeto. En el ejemplo siguiente se muestra el texto modificado `Details` método.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample1.vb)]

Código primero simplifica la búsqueda de datos mediante el `Find` método. Una característica de seguridad importante que creamos en el método es que el código comprueba que el `Find` método ha encontrado una película antes de que el código intenta hacer algo con él. Por ejemplo, un pirata informático podría introducir errores en el sitio cambiando la dirección URL creada por los vínculos de `http://localhost:xxxx/Movies/Details/1` en algo similar a `http://localhost:xxxx/Movies/Details/12345` (o algún otro valor que no represente una película real). Si no lo hace la comprobación de una película null, esto podría dar lugar a un error de base de datos.

Del mismo modo, cambie el `Delete` y `DeleteConfirmed` métodos para especificar un valor predeterminado para el parámetro ID y devolver `HttpNotFound` cuando no se encuentra una película. La actualización `Delete` métodos en el `Movie` controlador se muestran a continuación.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample2.vb)]

Tenga en cuenta que el `Delete` método no elimina los datos. La acción de efectuar una operación de eliminación en respuesta a una solicitud GET (o con este propósito efectuar una operación de edición, creación o cualquier otra operación que modifique los datos) genera una vulnerabilidad de seguridad. Para obtener más información acerca de esto, consulte la entrada de blog de Stephen Walther [ASP.NET MVC sugerencia #46; no utilice los vínculos eliminar ya que crean vulnerabilidades de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

El método `HttpPost` que elimina los datos se denomina `DeleteConfirmed` para proporcionar al método HTTP POST una firma o nombre únicos. Las dos firmas de método se muestran a continuación:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample3.vb)]

Common language runtime (CLR) requiere métodos sobrecargados para tener una firma única (mismo nombre, lista de parámetros diferente). Sin embargo, aquí tiene dos métodos de eliminación: uno para GET y otro para POST que ambas requieren la misma firma. (ambos deben aceptar un número entero como parámetro).

Para ordenar este comando, puede hacer un par de cosas. Uno es dar a los métodos con nombres diferentes. Eso es lo que hicimos en el que el anterior ejemplo. Pero esto implica un pequeño problema: ASP.NET asigna segmentos de una dirección URL a los métodos de acción por nombre y, si cambia el nombre de un método, normalmente el enrutamiento no podría encontrar ese método. La solución es la que ve en el ejemplo, que consiste en agregar el atributo `ActionName("Delete")` al método `DeleteConfirmed`. Esto realiza eficazmente asignación para el sistema de enrutamiento para que una dirección URL que incluya <em>/Delete /</em>para una entrada de blog encontrará solicitud la `DeleteConfirmed` método.

Otra manera de evitar un problema con los métodos que tienen nombres idénticos y firmas es artificialmente cambiar la firma del método POST para que incluya un parámetro no utilizado. Por ejemplo, algunos desarrolladores agregar un tipo de parámetro `FormCollection` que se pasa al método POST y, a continuación, simplemente no usa el parámetro:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample4.vb)]

## <a name="wrapping-up"></a>Conclusión

Ahora tiene una aplicación ASP.NET MVC completa que almacena datos en una base de datos de SQL Server Compact. Puede crear, leer, actualizar, eliminar y buscar películas.

![](improving-the-details-and-delete-methods/_static/image1.png)

Este tutorial básico de lo que a realizar los controladores de su asociación con las vistas y pasan datos codificados de forma rígida. A continuación, ha creado y había diseñado un modelo de datos. Entity Framework Code First crea una base de datos del modelo de datos sobre la marcha, y el sistema de scaffolding de ASP.NET MVC genera automáticamente los métodos de acción y vistas de operaciones CRUD básicas. A continuación, agrega un formulario de búsqueda que permite a los usuarios buscar la base de datos. Puede cambiar la base de datos para incluir una nueva columna de datos y, a continuación, actualiza las dos páginas para crear y mostrar estos nuevos datos. Agregar validación marcando el modelo de datos con atributos de la `DataAnnotations` espacio de nombres. La validación resultante se ejecuta en el cliente y en el servidor.

Si desea implementar la aplicación, es útil probar primero la aplicación en el servidor local de IIS 7. Puede usar esto [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) vínculo para habilitar la configuración de IIS para las aplicaciones de ASP.NET. Consulte los vínculos de implementación siguientes:

- [Mapa de contenido de implementación de ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [Habilitar IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Implementación de proyectos de aplicación Web](https://msdn.microsoft.com/library/dd394698.aspx)

Ahora le animo a pasar a nuestro nivel intermedio [creación de un modelo de datos de Entity Framework para una aplicación ASP.NET MVC](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) y [MVC Music Store](../../mvc-music-store/mvc-music-store-part-1.md) tutoriales para explorar el [ASP.NET artículos en MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)y consulte los vídeos y recursos en muchas [ https://asp.net/mvc ](https://asp.net/mvc) para obtener más información sobre ASP.NET MVC. El [foros de ASP.NET MVC](https://forums.asp.net/1146.aspx) son un buen lugar para formular preguntas.

Disfrútelo.

— Scott Hanselman ([ http://hanselman.com ](http://hanselman.com) y [ @shanselman ](http://twitter.com/shanselman) en Twitter) y Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Anterior](adding-validation-to-the-model.md)
