---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: Vistas de MVC de ASP.NET de información general (C#) | Documentos de Microsoft
author: StephenWalther
description: ¿Qué es una vista de MVC de ASP.NET y cómo se diferencia de una página HTML? En este tutorial, Stephen Walther presenta vistas y se muestra cómo puede t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 5217994168ebac32a4a9754ae09e63e120804813
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870276"
---
<a name="aspnet-mvc-views-overview-c"></a>Vistas de MVC de ASP.NET de información general (C#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> ¿Qué es una vista de MVC de ASP.NET y cómo se diferencia de una página HTML? En este tutorial, Stephen Walther presenta vistas y se muestra cómo puede aprovechar las ventajas de la vista de datos y aplicaciones auxiliares HTML dentro de una vista.


El propósito de este tutorial es proporcionar una breve introducción a las vistas de MVC de ASP.NET, ver los datos y aplicaciones auxiliares HTML. Al final de este tutorial, debe entender cómo crear nuevas vistas, pasar datos de un controlador a una vista y usar aplicaciones auxiliares HTML para generar contenido en una vista.

## <a name="understanding-views"></a>Descripción de vistas

En ASP.NET o páginas Active Server, ASP.NET MVC no incluye todo lo que se corresponde directamente a una página. En una aplicación de MVC de ASP.NET, no hay una página en disco que se corresponde con la ruta de acceso en la dirección URL que se escribe en la barra de direcciones del explorador. Lo más cercano a una página en una aplicación ASP.NET MVC es algo llama un *vista*.

Las solicitudes entrantes, aplicación de explorador se asignan a las acciones del controlador de ASP.NET MVC. Una acción de controlador podría devolver una vista. Sin embargo, una acción de controlador puede realizar algún otro tipo de acción como le estamos redirigiendo a otra acción de controlador.

Lista 1 contiene un controlador simple denominado HomeController. HomeController expone dos acciones del controlador denominadas Index() y Details().

**Lista 1 - HomeController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

Puede invocar la primera acción, la acción Index(), escribiendo la dirección URL siguiente en la barra de direcciones del explorador:

/ Principal/índice

Puede invocar la segunda acción, la acción Details(), para ello, escriba esta dirección en el explorador:

/ Principal/detalles

La acción de Index() devuelve una vista. Mayoría de las acciones que crea, devolverá vistas. Sin embargo, una acción puede devolver otros tipos de resultados de la acción. Por ejemplo, la acción Details() devuelve un RedirectToActionResult que redirige la solicitud entrante a la acción de Index().

La acción de Index() contiene la única línea de código siguiente:

View();

Esta línea de código devuelve una vista que debe estar situada en la siguiente ruta de acceso en el servidor web:

\Views\Home\Index.aspx

La ruta de acceso a la vista se deduce el nombre del controlador y el nombre de la acción del controlador.

Si lo prefiere, pueden ser explícitas acerca de la vista. La siguiente línea de código devuelve una vista denominada a Fred:

Vista (Fred);

Cuando se ejecuta esta línea de código, se devuelve una vista de la ruta de acceso siguiente:

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Si tiene previsto crear pruebas unitarias para la aplicación de ASP.NET MVC es una buena idea ser explícito acerca de los nombres de vista. De este modo, puede crear una prueba unitaria para comprobar que la vista esperada devolvió una acción de controlador.


## <a name="adding-content-to-a-view"></a>Agregar contenido a una vista

Una vista es un estándar de (documento HTML que puede contener secuencias de comandos X). Utilizar secuencias de comandos para agregar contenido dinámico a una vista.

Por ejemplo, la vista en el listado 2 muestra la fecha y hora actuales.

**La lista 2 - \Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

Tenga en cuenta que el cuerpo de la página HTML en el listado 2 contiene el script siguiente:

&lt;% Response.Write(DateTime.Now); %&gt;

Utilice los delimitadores de secuencia de comandos &lt;% y %&gt; para marcar el principio y final de una secuencia de comandos. Este script se escribe en C#. Muestra la fecha y hora actual mediante una llamada al método Response.Write () para representar el contenido en el explorador. Los delimitadores de secuencia de comandos &lt;% y %&gt; puede utilizarse para ejecutar una o varias instrucciones.

Puesto que se llama a Response.Write () con tanta frecuencia, Microsoft le proporciona un acceso directo para llamar al método Response.Write (). La vista en la lista 3 utiliza los delimitadores &lt;% = y %&gt; como método abreviado para llamar a Response.Write ().

**El listado 3 - Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

Puede usar cualquier lenguaje .NET Framework para generar contenido dinámico en una vista. Normalmente, ll utilice .NET de Visual Basic o C# para escribir sus controladores y vistas.

## <a name="using-html-helpers-to-generate-view-content"></a>Usar aplicaciones auxiliares HTML para generar contenido de vista

Para que sea más fácil de agregar contenido a una vista, puede beneficiarse de lo que se denomina un *aplicación auxiliar HTML*. Una aplicación auxiliar de HTML, por lo general, es un método que genera una cadena. Puede usar aplicaciones auxiliares HTML para generar los elementos HTML estándar, como cuadros de texto, vínculos, listas desplegables y cuadros de lista.

Por ejemplo, la vista en el listado 4 se aprovecha de las aplicaciones auxiliares HTML tres--los ayudantes BeginForm(), TextBox() y Password()--para generar un inicio de sesión forman (consulte la figura 1).

**Listado 4--\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


[![El cuadro de diálogo nuevo proyecto](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)

**Figura 01**: un formulario de inicio de sesión estándar ([haga clic aquí para ver la imagen a tamaño completo](asp-net-mvc-views-overview-cs/_static/image2.png))


Todos los métodos de las aplicaciones auxiliares HTML se denominan en la propiedad Html de la vista. Por ejemplo, presentar un cuadro de texto llamando al método Html.TextBox().

Observe que utilice los delimitadores de secuencia de comandos &lt;% = y %&gt; al llamar a aplicaciones auxiliares de la Html.TextBox() y Html.Password(). Estas aplicaciones auxiliares simplemente devuelven una cadena. Debe llamar a Response.Write () para representar la cadena en el explorador.

Uso de métodos auxiliares HTML es opcional. Facilitan su vida al reducir la cantidad de HTML y el script que tiene que escribir. La vista en el listado 5 representa el mismo formato exacto que la vista en el listado 4 sin usar aplicaciones auxiliares HTML.

**Listado 5--\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

También tiene la opción de crear sus propias aplicaciones auxiliares de HTML. Por ejemplo, puede crear un método de aplicación auxiliar de GridView() que muestra un conjunto de registros de base de datos en una tabla HTML automáticamente. Exploramos en este tema del tutorial **aplicaciones auxiliares de HTML personalizado de creación de**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Usar datos de vista para pasar datos a una vista

Usar datos de la vista para pasar datos de un controlador a una vista. Piense en datos de la vista como un paquete que se envía a través del correo electrónico. Todos los datos que se pasan de un controlador a una vista deben enviarse con este paquete. Por ejemplo, el controlador en el listado 6 agrega un mensaje para ver los datos.

**Listado 6 - ProductController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

El controlador de propiedad ViewData representa una colección de pares nombre / valor. En el listado 6, el método Index() agrega un elemento a la colección de datos de vista con el nombre de mensaje con el valor Hello World!. Cuando la vista es devuelto por el método Index(), los datos de vista se pasan automáticamente a la vista.

La vista en la lista 7 recupera el mensaje de los datos de vista y procesa el mensaje en el explorador.

**Listado 7--\Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

Tenga en cuenta que la vista aprovecha las ventajas del método de aplicación auxiliar de HTML Html.Encode() cuando se procesa el mensaje. La aplicación auxiliar HTML Html.Encode() codifica caracteres especiales como &lt; y &gt; en caracteres que son seguros para mostrar en una página web. Cada vez que se representa el contenido que un usuario envía a un sitio Web, se debe codificar el contenido para evitar ataques de inyección de código de JavaScript.

(Dado que se creó el mensaje nosotros mismos en el ProductController, no queremos t sea realmente necesario codificar el mensaje. Sin embargo, es un hábito recomendable siempre llame al método Html.Encode() al mostrar el contenido recuperado de datos de vista en una vista.)

En la lista 7, aprovechamos de datos de vista que se va a pasar un mensaje de cadena simple de un controlador a una vista. También puede utilizar los datos de vista para pasar otros tipos de datos, como una colección de registros de base de datos, de un controlador a una vista. Por ejemplo, si desea mostrar el contenido de la tabla de base de datos de productos en una vista, a continuación, podría pasar la colección de base de datos en la vista registra datos.

También tiene la opción de pasar los datos de la vista fuertemente tipada de un controlador a una vista. Exploramos en este tema del tutorial **descripción fuertemente tipados vista de los datos y vistas**.

## <a name="summary"></a>Resumen

Este tutorial proporciona una breve introducción a las vistas de MVC de ASP.NET, ver los datos y aplicaciones auxiliares HTML. En la primera sección, aprendió a agregar nuevas vistas al proyecto. Ha aprendido que debe agregar una vista a la carpeta correcta para llamarlo desde un controlador determinado. A continuación, analizamos el tema de las aplicaciones auxiliares HTML. Ha aprendido cómo las aplicaciones auxiliares HTML le permiten generar fácilmente contenido HTML estándar. Por último, aprendió a aprovechar las ventajas de los datos de vista para pasar datos de un controlador a una vista.

> [!div class="step-by-step"]
> [Siguiente](creating-custom-html-helpers-cs.md)
