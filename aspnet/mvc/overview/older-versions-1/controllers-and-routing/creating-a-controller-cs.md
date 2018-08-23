---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Creación de un controlador (C#) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther demuestra cómo puede agregar un controlador a una aplicación ASP.NET MVC.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 6fe880775a5d477aefcf0fe6cedb3e719760ed90
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837989"
---
<a name="creating-a-controller-c"></a>Creación de un controlador (C#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> En este tutorial, Stephen Walther demuestra cómo puede agregar un controlador a una aplicación ASP.NET MVC.


El objetivo de este tutorial es explicar cómo se puede crear controladores de MVC de ASP.NET nuevo. Aprenda a crear controladores mediante la opción de menú Agregar controlador de Visual Studio y mediante la creación de un archivo de clase a mano.

### <a name="using-the-add-controller-menu-option"></a>Mediante el agregar la opción de menú del controlador

Es la manera más fácil de crear un nuevo controlador secundario en la carpeta controladores en la ventana Explorador de soluciones de Visual Studio y seleccione el **Add, controlador** opción de menú (consulte la figura 1). Al seleccionar esta opción de menú se abre el **Agregar controlador** cuadro de diálogo (consulte la figura 2).


[![El cuadro de diálogo nuevo proyecto](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)

**Figura 01**: agregar un nuevo controlador ([haga clic aquí para ver imagen en tamaño completo](creating-a-controller-cs/_static/image2.png))


[![El cuadro de diálogo nuevo proyecto](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)

**Figura 02**: cuadro de diálogo de agregar el controlador ([haga clic aquí para ver imagen en tamaño completo](creating-a-controller-cs/_static/image4.png))


Tenga en cuenta que la primera parte del nombre del controlador se resalta en el **Agregar controlador** cuadro de diálogo. Cada nombre de controlador debe finalizar con el sufijo *controlador*. Por ejemplo, puede crear un controlador denominado *ProductController* pero no un controlador denominado *producto*.


Si crea un controlador que falta el *controlador* sufijo, a continuación, no podrá invocar el controlador. No hace esto, he en vano incontables horas de mi vida después cometer este error.


**Listado 1 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

Siempre debe crear los controladores en la carpeta Controllers. En caso contrario, deberá se infringen las convenciones de ASP.NET MVC y otros desarrolladores tendrán un tiempo más difícil comprender la aplicación.

### <a name="scaffolding-action-methods"></a>Métodos de acción de scaffolding

Cuando se crea un controlador, tiene la opción para generar automáticamente los métodos de acción Create, Update y los detalles (consulte la figura 3). Si selecciona esta opción, se genera la clase de controlador en el listado 2.


[![Creación automática de los métodos de acción](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)

**Figura 03**: creación automática de los métodos de acción ([haga clic aquí para ver imagen en tamaño completo](creating-a-controller-cs/_static/image6.png))


**Listado 2 - Controllers\CustomerController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

Estos métodos generados son los métodos auxiliares. Debe agregar la lógica real para crear, actualizar y mostrar los detalles de un cliente. Sin embargo, los métodos de código auxiliar proporcionan un buen punto de partida.

### <a name="creating-a-controller-class"></a>Creación de una clase de controlador

El controlador de MVC de ASP.NET es simplemente una clase. Si lo prefiere, puede omitir el scaffolding de controlador adecuado de Visual Studio y crear una clase de controlador a mano. Siga estos pasos:

1. Haga clic en la carpeta Controllers y seleccione la opción de menú **agregar, nuevo elemento** y seleccione el **clase** plantilla (consulte la figura 4).
2. Nombre de la nueva clase PersonController.cs y haga clic en el **agregar** botón.
3. Modifique el archivo resultante de la clase para que la clase hereda de la clase base System.Web.Mvc.Controller (consulte el listado 3).


[![Crear una nueva clase](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)

**Figura 04**: crear una nueva clase ([haga clic aquí para ver imagen en tamaño completo](creating-a-controller-cs/_static/image8.png))


**Listado 3 - Controllers\PersonController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

El controlador en el listado 3 expone una acción denominada Index() que devuelve la cadena "Hello World!". Puede invocar esta acción de controlador mediante la ejecución de la aplicación y solicitar una dirección URL similar al siguiente:

`http://localhost:40071/Person`

> [!NOTE]
> 
> El servidor de desarrollo de ASP.NET utiliza un número de puerto aleatorio (por ejemplo, 40071). Cuando escriba una dirección URL para invocar un controlador, deberá proporcionar el número de puerto de la derecha. Puede determinar el número de puerto desplazando el puntero del mouse sobre el icono para el servidor de desarrollo de ASP.NET en el área de notificación de Windows (esquina inferior derecha de la pantalla).
> 
> [!div class="step-by-step"]
> [Anterior](adding-dynamic-content-to-a-cached-page-cs.md)
> [Siguiente](creating-an-action-cs.md)
