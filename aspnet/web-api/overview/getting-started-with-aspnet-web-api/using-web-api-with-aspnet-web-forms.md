---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Uso de API Web con ASP.NET Web Forms | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: 91811031b937d3ca05888ce8d4f28bcc33537c76
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400884"
---
<a name="using-web-api-with-aspnet-web-forms"></a>Uso de API Web con ASP.NET Web Forms
====================
por [Mike Wasson](https://github.com/MikeWasson)

Aunque ASP.NET Web API se empaqueta con ASP.NET MVC, es fácil agregar API Web a una aplicación de formularios Web Forms de ASP.NET tradicional. Este tutorial le guiará a través de los pasos.

## <a name="overview"></a>Información general

Para usar la API Web en una aplicación de formularios Web Forms, hay dos pasos principales:

- Agregar un controlador de API Web que se deriva de la **ApiController** clase.
- Agregar una tabla de rutas para el **aplicación\_iniciar** método.

## <a name="create-a-web-forms-project"></a>Crear un proyecto de Web Forms

Inicie Visual Studio y seleccione **nuevo proyecto** desde el **iniciar** página. O bien, en el **archivo** menú, seleccione **New** y, a continuación, **proyecto**.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo. En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación de ASP.NET Web Forms**. Escriba un nombre para el proyecto y haga clic en **Aceptar**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Crear el modelo y controlador

Este tutorial usa las mismas clases de modelo y controlador como el [Introducción](tutorial-your-first-web-api.md) tutorial.

En primer lugar, agregue una clase de modelo. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **Agregar clase**. Nombre de la clase de producto y agregue la siguiente implementación:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

A continuación, agregue un controlador Web API al proyecto., un *controlador* es el objeto que controla las solicitudes HTTP para API Web.

En **el Explorador de soluciones**, haga clic en el proyecto. Seleccione **Agregar nuevo elemento**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

En **plantillas instaladas**, expanda **Visual C#** y seleccione **Web**. A continuación, en la lista de plantillas, seleccione **clase de controlador de Web API**. Asigne al controlador el "ProductsController" y haga clic en **agregar**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

El **Agregar nuevo elemento** asistente creará un archivo denominado ProductsController.cs. Elimine los métodos que incluye el asistente y agregue los métodos siguientes:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Para obtener más información sobre el código de este controlador, consulte el [Introducción](tutorial-your-first-web-api.md) tutorial.

## <a name="add-routing-information"></a>Agregar información de enrutamiento

A continuación, vamos a agregar una ruta URI así que esa URI del formulario &quot;/API/productos/&quot; se enrutan al controlador.

En **el Explorador de soluciones**, haga doble clic en Global.asax para abrir el archivo de código subyacente Global.asax.cs. Agregue el siguiente **mediante** instrucción.

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

A continuación, agregue el código siguiente a la **aplicación\_iniciar** método:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Para obtener más información acerca de las tablas de enrutamientos, consulte [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Agregar cliente AJAX

Eso es todo lo que necesita para crear una API web que pueden tener acceso los clientes. Ahora vamos a agregar una página HTML que usa jQuery para llamar a la API.

Asegúrese de que la página maestra (por ejemplo, *Site.Master*) incluye un `ContentPlaceHolder` con `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Abra el archivo Default.aspx. Reemplace el texto reutilizable que se encuentra en la sección de contenido principal, como se muestra:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

A continuación, agregue una referencia al archivo de código fuente de jQuery en la `HeaderContent` sección:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Nota: Puede agregar fácilmente la referencia de script arrastrando y colocando el archivo de **el Explorador de soluciones** en la ventana del editor de código.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

A continuación de la etiqueta de script de jQuery, agregue el siguiente bloque de script:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Cuando se carga el documento, este script realiza una solicitud de AJAX para &quot;api/products&quot;. La solicitud devuelve una lista de productos en formato JSON. El script agrega la información del producto a la tabla HTML.

Al ejecutar la aplicación, debe ver así:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
