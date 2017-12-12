---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: "Información general de enrutamiento de ASP.NET MVC (C#) | Documentos de Microsoft"
author: StephenWalther
description: "En este tutorial, Stephen Walther muestra cómo el marco de MVC de ASP.NET asigna las solicitudes del explorador a las acciones del controlador."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 714fd1939ffeba11b84a82e80193ecbbe4b12e09
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-routing-overview-c"></a>Información general de enrutamiento de ASP.NET MVC (C#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> En este tutorial, Stephen Walther muestra cómo el marco de MVC de ASP.NET asigna las solicitudes del explorador a las acciones del controlador.


En este tutorial, se le presenta una característica importante de cada aplicación de ASP.NET MVC llama *enrutamiento de ASP.NET*. El módulo de enrutamiento de ASP.NET es responsable de asignar las solicitudes entrantes a determinadas acciones de controlador MVC. Al final de este tutorial, comprenderá cómo la tabla de rutas estándar asigna las solicitudes a las acciones del controlador.

## <a name="using-the-default-route-table"></a>En la tabla de ruta predeterminada

Cuando se crea una nueva aplicación MVC de ASP.NET, la aplicación ya está configurada para usar el enrutamiento de ASP.NET. Enrutamiento ASP.NET está configurado en dos lugares.

En primer lugar, el enrutamiento de ASP.NET está habilitado en el archivo de configuración de la aplicación Web (archivo Web.config). Hay cuatro secciones en el archivo de configuración que son relevantes para el enrutamiento: la sección system.web.httpModules, la sección system.web.httpHandlers, la sección system.webserver.modules y la sección system.webserver.handlers. Tenga cuidado de no eliminar estas secciones porque sin estas secciones enrutamiento dejará de funcionar.

En segundo lugar y lo que es más importante, se crea una tabla de rutas en el archivo Global.asax de la aplicación. El archivo Global.asax es un archivo especial que contiene los controladores de eventos para eventos de ciclo de vida de aplicación de ASP.NET. La tabla de rutas se crea durante el evento de inicio de la aplicación.

El archivo en el listado 1 contiene el archivo Global.asax de manera predeterminada para una aplicación ASP.NET MVC.

**Lista 1 - Global.asax.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

Cuando se inicia primero una aplicación MVC, la aplicación\_se llama al método Start(). Este método, a su vez, llama al método de RegisterRoutes(). El método RegisterRoutes() crea la tabla de rutas.

La tabla de ruta predeterminada contiene una única ruta (denominada de forma predeterminada). La ruta predeterminada asigna el primer segmento de una dirección URL a un nombre de controlador, el segundo segmento de una dirección URL a una acción de controlador y el tercer segmento a un parámetro denominado **identificador**.

Imagine que escriba la dirección URL siguiente en la barra de direcciones del explorador web:

/ Principal/índice/3

La ruta predeterminada asigna esta dirección URL a los siguientes parámetros:

- controlador = inicio

- acción = índice

- Id. = 3

Cuando se solicita la dirección URL /Home/índice/3, se ejecuta el código siguiente:

HomeController.Index(3)

La ruta predeterminada incluye valores predeterminados para los tres parámetros. Si no proporciona un controlador, a continuación, el parámetro de controlador tiene como valor predeterminado el valor **inicio**. Si no proporciona una acción, el parámetro de acción tiene como valor predeterminado el valor **índice**. Por último, si no proporciona un identificador, el parámetro de identificador predeterminado es una cadena vacía.

Veamos algunos ejemplos de cómo la ruta predeterminada asigna las direcciones URL a las acciones del controlador. Imagine que escriba la dirección URL siguiente en la barra de direcciones del explorador:

/ Principal

Debido a los valores predeterminados de parámetros de ruta de forma predeterminada, al escribir esta dirección URL harán que el método Index() de la clase HomeController en el listado 2 para llamarlo.

**La lista 2 - HomeController.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

En el listado 2, la clase HomeController incluye un método denominado Index() que acepta un parámetro único denominado identificador. La dirección URL /Home hace que el método Index() a llamarse con una cadena vacía como el valor del parámetro de identificador.

Debido al modo en que el marco de MVC invoca las acciones de controlador, la dirección URL /Home también coincide con el método Index() de la clase HomeController en el listado 3.

**El listado 3 - HomeController.cs (acción del índice sin parámetros)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

El método Index() en el listado 3 no acepta ningún parámetro. La dirección URL /Home provocará que se llamará a este método de Index(). La dirección URL /Home/índice/3 también invoca este método (se omite el identificador).

La dirección URL /Home también coincide con el método Index() de la clase HomeController en el listado 4.

**Listado 4 - HomeController.cs (acción del índice con el parámetro acepta valores NULL)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

En el listado 4, el método Index() tiene un parámetro de número entero. Dado que el parámetro es un parámetro que acepta valores null (puede tener el valor Null), pueden llamarse el Index() sin provocar un error.

Por último, al invocar el método Index() en el listado 5 con la dirección URL /Home produce una excepción desde el parámetro Id *no es* un parámetro que acepta valores NULL. Si se intenta invocar el método Index() obtendrá el error mostrado en la figura 1.

**Listado 5 - HomeController.cs (acción del índice con el parámetro Id.)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


[![Invocar una acción de controlador que espera un valor de parámetro](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)

**Figura 01**: invocar una acción de controlador que espera un valor de parámetro ([haga clic aquí para ver la imagen a tamaño completo](asp-net-mvc-routing-overview-cs/_static/image2.png))


Dirección URL /Home/índice/3, por otro lado, funciona bien con la acción de controlador de índice en el listado 5. La solicitud /Home/Index/3 hace que el método Index() llamarlo con un parámetro de identificador que tiene el valor 3.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era para proporcionarle una breve introducción al enrutamiento de ASP.NET. Se examina la tabla de rutas predeterminadas que se obtiene con una nueva aplicación MVC de ASP.NET. Ha aprendido cómo la ruta predeterminada asigna las direcciones URL a las acciones del controlador.

>[!div class="step-by-step"]
[Siguiente](understanding-action-filters-cs.md)
