---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: Vistas de ASP.NET MVC Overview (C#) | Microsoft Docs
author: StephenWalther
description: ¿Qué es una vista de MVC de ASP.NET y cómo se diferencia de una página HTML? En este tutorial, Stephen Walther presenta las vistas y demuestra cómo puede t...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: ac47caa46d93c6157926f1c9b5112555fae4f8f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837576"
---
<a name="aspnet-mvc-views-overview-c"></a>Vistas de ASP.NET MVC Overview (C#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> ¿Qué es una vista de MVC de ASP.NET y cómo se diferencia de una página HTML? En este tutorial, Stephen Walther presenta las vistas y se muestra cómo puede sacar partido de la vista de datos y aplicaciones auxiliares HTML dentro de una vista.


El propósito de este tutorial es proporcionar una breve introducción a las vistas de ASP.NET MVC, ver los datos y aplicaciones auxiliares HTML. Al final de este tutorial, debe comprender cómo crear nuevas vistas, pasar datos de un controlador a una vista y usar aplicaciones auxiliares HTML para generar contenido en una vista.

## <a name="understanding-views"></a>Descripción de vistas

Para ASP.NET o páginas Active Server, ASP.NET MVC no incluye todo lo que se corresponde directamente con una página. En una aplicación de ASP.NET MVC, no hay una página en disco que se corresponde con la ruta de acceso en la dirección URL que se escribe en la barra de direcciones del explorador. Lo más parecido a una página en una aplicación ASP.NET MVC es algo llamado un *vista*.

Las solicitudes del explorador de la aplicación, entrante se asignan a acciones del controlador de ASP.NET MVC. Una acción de controlador podría devolver una vista. Sin embargo, una acción de controlador podría realizar algún otro tipo de acción como se le redirigirá a otra acción de controlador.

Listado 1 contiene un controlador simple denominado HomeController. HomeController expone dos acciones de controlador denominadas Index() y Details().

**Listado 1 - HomeController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

Puede invocar la primera acción, la acción de Index() escribiendo la siguiente dirección URL en la barra de direcciones del explorador:

/ Home/Index

Puede invocar la segunda acción, la acción Details(), escribiendo esta dirección en el explorador:

/ Principal/detalles

La acción de Index() devuelve una vista. La mayoría de las acciones que cree devolverá las vistas. Sin embargo, una acción puede devolver otros tipos de resultados de acción. Por ejemplo, la acción Details() devuelve un RedirectToActionResult que redirige la solicitud entrante a la acción de Index().

La acción de Index() contiene la única línea siguiente de código:

View();

Esta línea de código devuelve una vista que debe estar ubicada en la siguiente ruta de acceso en el servidor web:

\Views\Home\Index.aspx

La ruta de acceso a la vista se deduce el nombre del controlador y el nombre de la acción del controlador.

Si lo prefiere, puede ser explícito acerca de la vista. La siguiente línea de código devuelve una vista denominada a Fred:

Vista (Fred);

Cuando se ejecuta esta línea de código, se devuelve una vista de la ruta de acceso siguiente:

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Si va a crear pruebas unitarias para la aplicación de ASP.NET MVC es una buena idea de ser explícito acerca de los nombres de vista. De este modo, puede crear una prueba unitaria para comprobar que la vista esperada se devolvió mediante una acción de controlador.


## <a name="adding-content-to-a-view"></a>Agregar contenido a una vista

Una vista es un estándar de (documento HTML que puede contener las secuencias de comandos X). Usar secuencias de comandos para agregar contenido dinámico a una vista.

Por ejemplo, la vista en el listado 2 muestra la fecha y hora actuales.

**Listado 2 - \Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

Tenga en cuenta que el cuerpo de la página HTML en el listado 2 contiene el script siguiente:

&lt;% Response.Write(DateTime.Now); %&gt;

Utilice los delimitadores de secuencia de comandos &lt;% y %&gt; para marcar el principio y al final de una secuencia de comandos. Este script se escribe en C#. Muestra la fecha y hora actual llamando al método Response.Write () para representar el contenido al explorador. Los delimitadores de secuencia de comandos &lt;% y %&gt; puede utilizarse para ejecutar una o varias instrucciones.

Dado que a menudo se llama a Response.Write (), Microsoft le proporciona un acceso directo para llamar al método Response.Write (). La vista en el listado 3 utiliza los delimitadores &lt;% = y %&gt; como método abreviado para llamar a Response.Write ().

**Listado 3 - Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

Puede usar cualquier lenguaje .NET para generar contenido dinámico en una vista. Normalmente, se deberá usar Visual Basic .NET o C# para escribir los controladores y vistas.

## <a name="using-html-helpers-to-generate-view-content"></a>Usar aplicaciones auxiliares HTML para generar contenido de la vista

Para que sea más fácil agregar contenido a una vista, puede sacar partido de algo llamado un *aplicación auxiliar HTML*. Una aplicación auxiliar HTML, por lo general, es un método que genera una cadena. Puede usar aplicaciones auxiliares HTML para generar los elementos HTML estándar, como cuadros de texto, vínculos, listas desplegables y cuadros de lista.

Por ejemplo, la vista en el listado 4 se aprovecha de tres aplicaciones auxiliares de HTML, los ayudantes BeginForm(), TextBox() y Password()--para generar un inicio de sesión de formulario (consulte la figura 1).

**Listado 4--\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


[![El cuadro de diálogo nuevo proyecto](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)

**Figura 01**: un formulario de inicio de sesión estándar ([haga clic aquí para ver imagen en tamaño completo](asp-net-mvc-views-overview-cs/_static/image2.png))


Todos los métodos de las aplicaciones auxiliares HTML se llaman en la propiedad Html de la vista. Por ejemplo, representa un cuadro de texto llamando al método Html.TextBox().

Tenga en cuenta que utilice los delimitadores de secuencia de comandos &lt;% = y %&gt; al llamar a las aplicaciones auxiliares de la Html.TextBox() y Html.Password(). Estas aplicaciones auxiliares de simplemente devuelven una cadena. Debe llamar a Response.Write () para representar la cadena en el explorador.

Uso de métodos auxiliares HTML es opcional. Facilitan la vida al reducir la cantidad de HTML y secuencias de comandos que necesita para escribir. La vista en el listado 5 representa la misma forma exacta que la vista en el listado 4 sin usar aplicaciones auxiliares HTML.

**Listado 5--\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

También tiene la opción de crear sus propias aplicaciones auxiliares HTML. Por ejemplo, puede crear un método auxiliar GridView() que muestra automáticamente un conjunto de registros de base de datos en una tabla HTML. Se explorará en este tema en el tutorial **crear aplicaciones auxiliares de HTML personalizado**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Usar datos de vista para pasar datos a una vista

Usar datos de la vista para pasar datos de un controlador a una vista. Piense en los datos de vista como un paquete que se envía por correo electrónico. Todos los datos transferidos desde un controlador a una vista deben enviarse con este paquete. Por ejemplo, el controlador en el listado 6 agrega un mensaje para ver los datos.

**Listado 6 - ProductController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

El controlador de propiedad ViewData representa una colección de pares de nombre y valor. En el listado 6, el método Index() agrega un elemento a la colección de datos de vista denominada mensaje con el valor de Hello World!. Cuando se devuelve la vista por el método Index(), los datos de vista se pasan automáticamente a la vista.

La vista en la lista 7 recupera el mensaje de los datos de vista y procesa el mensaje en el explorador.

**Listado 7--\Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

Tenga en cuenta que la vista aprovecha el método auxiliar de Html.Encode() HTML al representar el mensaje. La aplicación auxiliar HTML Html.Encode() codifica caracteres especiales como &lt; y &gt; en caracteres que son seguros mostrar en una página web. Cada vez que se representa el contenido que un usuario envía a un sitio Web, debe codificar el contenido para evitar ataques por inyección de código JavaScript.

(Ya hemos creado el mensaje nosotros mismos ProductController, no queremos t realmente se necesita codificar el mensaje. Sin embargo, es un buen hábito siempre llamar al método Html.Encode() al mostrar el contenido de recuperarse de los datos de vista en una vista.)

En la lista 7, aprovechamos de datos de vista que se va a pasar un mensaje de cadena simple de un controlador a una vista. También puede usar los datos de vista para pasar otros tipos de datos, como una colección de registros de base de datos, desde un controlador a una vista. Por ejemplo, si desea mostrar el contenido de la tabla de base de datos de productos en una vista, a continuación, deberá pasar la colección de base de datos en la vista registra datos.

También tiene la opción de pasar los datos de vista fuertemente tipada de un controlador a una vista. Se explorará en este tema en el tutorial **descripción fuertemente tipados vista de los datos y vistas**.

## <a name="summary"></a>Resumen

Este tutorial proporciona una breve introducción a las vistas de ASP.NET MVC, ver los datos y aplicaciones auxiliares HTML. En la primera sección, ha aprendido a agregar nuevas vistas al proyecto. Ha aprendido que debe agregar una vista a la carpeta correcta para poder llamarlo desde un controlador determinado. A continuación, analizamos el tema de las aplicaciones auxiliares HTML. Ha aprendido cómo las aplicaciones auxiliares HTML le permiten generar fácilmente el contenido HTML estándar. Por último, ha aprendido cómo sacar partido de los datos de vista para pasar datos de un controlador a una vista.

> [!div class="step-by-step"]
> [Siguiente](creating-custom-html-helpers-cs.md)
