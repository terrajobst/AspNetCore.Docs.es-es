---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Creación de un controlador (C#) | Documentos de Microsoft
author: StephenWalther
description: En este tutorial, Stephen Walther muestra cómo puede agregar un controlador a una aplicación de ASP.NET MVC.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 86966f1064d09419e2102542c6d14c4162d153e4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-controller-c"></a>Creación de un controlador (C#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> En este tutorial, Stephen Walther muestra cómo puede agregar un controlador a una aplicación de ASP.NET MVC.


El objetivo de este tutorial es explicar cómo se puede crear controladores de MVC de ASP.NET nuevo. Obtenga información acerca de cómo crear controladores mediante la opción de menú Agregar controlador de Visual Studio y mediante la creación de un archivo de clase a mano.

### <a name="using-the-add-controller-menu-option"></a>Mediante el agregar la opción de menú de controlador

Es la manera más fácil de crear un nuevo controlador haga clic en la carpeta de controladores en la ventana Explorador de soluciones de Visual Studio y seleccione la **agregar, controlador** opción de menú (consulte la figura 1). Al seleccionar esta opción de menú se abre la **Agregar controlador** cuadro de diálogo (consulte la figura 2).


[![El cuadro de diálogo nuevo proyecto](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)

**Figura 01**: agregar un nuevo controlador ([haga clic aquí para ver la imagen a tamaño completo](creating-a-controller-cs/_static/image2.png))


[![El cuadro de diálogo nuevo proyecto](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)

**Figura 02**: cuadro de diálogo de agregar el controlador ([haga clic aquí para ver la imagen a tamaño completo](creating-a-controller-cs/_static/image4.png))


Tenga en cuenta que la primera parte del nombre del controlador se resalta en el **Agregar controlador** cuadro de diálogo. Cada nombre de controlador debe finalizar con el sufijo *controlador*. Por ejemplo, puede crear un controlador denominado *ProductController* pero no un controlador denominado *producto*.


Si creas un controlador que falta el *controlador* sufijo, a continuación, no podrá invocar el controlador. No lo hace, he perdí horas y horas de mi vida después de realizar este error.


**Lista 1 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

Siempre debe crear los controladores en la carpeta de controladores. En caso contrario, podrá ser infringir las convenciones de ASP.NET MVC y otros desarrolladores tendrán más dificultades a la hora Descripción de la aplicación.

### <a name="scaffolding-action-methods"></a>Métodos de acción de scaffolding

Cuando se crea un controlador, tiene la opción para generar automáticamente los métodos de acción de creación, actualización y detalles (consulte la figura 3). Si selecciona esta opción, se genera la clase de controlador en el listado 2.


[![Creación automática de los métodos de acción](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)

**Figura 03**: creación automática de los métodos de acción ([haga clic aquí para ver la imagen a tamaño completo](creating-a-controller-cs/_static/image6.png))


**La lista 2 - Controllers\CustomerController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

Estos métodos generados son métodos de código auxiliar. Debe agregar la lógica real para crear, actualizar y mostrar detalles de un cliente. Sin embargo, los métodos de código auxiliar proporcionan con un punto de partida "nice".

### <a name="creating-a-controller-class"></a>Crear una clase de controlador

El controlador de MVC de ASP.NET es simplemente una clase. Si lo prefiere, puede omitir el scaffolding de controlador adecuado de Visual Studio y cree una clase de controlador a mano. Siga estos pasos:

1. Haga clic en la carpeta Controllers y seleccione la opción de menú **agregar, nuevo elemento** y seleccione la **clase** plantilla (consulte la figura 4).
2. Nombre de la nueva clase PersonController.cs y haga clic en el **agregar** botón.
3. Modificar el archivo resultante de la clase para que la clase hereda de la clase base de System.Web.Mvc.Controller (consulte el listado 3).


[![Crear una nueva clase](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)

**Figura 04**: crear una nueva clase ([haga clic aquí para ver la imagen a tamaño completo](creating-a-controller-cs/_static/image8.png))


**El listado 3 - Controllers\PersonController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

El controlador en el listado 3 expone una acción denominada Index() que devuelve la cadena "¡Hello World!". Puede invocar esta acción de controlador, ejecute la aplicación y solicita una dirección URL similar al siguiente:

`http://localhost:40071/Person`

> [!NOTE]
> 
> El servidor de desarrollo de ASP.NET usa un número de puerto aleatorio (por ejemplo, 40071). Cuando escriba una dirección URL para invocar un controlador, debe proporcionar el número de puerto adecuado. Puede determinar el número de puerto desplazando el puntero del mouse sobre el icono para el servidor de desarrollo de ASP.NET en el área de notificación de Windows (parte inferior derecha de la pantalla).
> 
> [!div class="step-by-step"]
> [Anterior](adding-dynamic-content-to-a-cached-page-cs.md)
> [Siguiente](creating-an-action-cs.md)
