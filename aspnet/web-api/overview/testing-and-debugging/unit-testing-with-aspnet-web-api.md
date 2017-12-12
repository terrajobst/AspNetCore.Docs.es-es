---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: ASP.NET Web API 2 de pruebas unitarias | Documentos de Microsoft
author: tfitzmac
description: "Esta guía y la aplicación muestran cómo crear pruebas unitarias simple para la aplicación de API Web 2. Este tutorial muestra cómo incluir un proj de prueba unitaria..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 13211ee4543e17a4bfb2f83495f4041880f37df2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-aspnet-web-api-2"></a>ASP.NET Web API 2 de pruebas unitarias
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Esta guía y la aplicación muestran cómo crear pruebas unitarias simple para la aplicación de API Web 2. Este tutorial muestra cómo incluir un proyecto de prueba unitaria en la solución y escribir métodos de prueba que comprobación los valores devueltos de un método de controlador.
> 
> Este tutorial se da por supuesto que está familiarizado con los conceptos básicos de ASP.NET Web API. Para un tutorial introductorio, vea [Introducción a ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> Las pruebas unitarias en este tema son intencionadamente limitadas a escenarios de datos simple. Para escenarios más avanzados de datos de pruebas unitarias, vea [simulación Entity Framework cuando unidad pruebas ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2


## <a name="in-this-topic"></a>En este tema

Este tema contiene las siguientes secciones:

- [Requisitos previos](#prereqs)
- [Descargar código](#download)
- [Crear aplicaciones con el proyecto de prueba unitaria](#appwithunittest)

    - [Agregar proyecto de prueba unitaria al crear la aplicación](#whencreate)
    - [Agregar proyecto de prueba unitaria a una aplicación existente](#addtoexisting)
- [Configurar la aplicación Web API 2](#setupproject)
- [Instalar paquetes de NuGet en el proyecto de prueba](#testpackages)
- [Crear pruebas](#tests)
- [Ejecutar pruebas](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Requisitos previos

Visual Studio 2017 Community, Professional o Enterprise.

<a id="download"></a>
## <a name="download-code"></a>Descargar código

Descargue el [proyecto completado](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). El proyecto descargable incluye código de prueba de unidad de este tema y la [simulación Entity Framework cuando ASP.NET Web API de pruebas de unidad](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) tema.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Crear aplicaciones con el proyecto de prueba unitaria

Puede crear un proyecto de prueba unitaria al crear la aplicación o agregar un proyecto de prueba unitaria a una aplicación existente. Este tutorial muestra ambos métodos para crear un proyecto de prueba unitaria. Para seguir este tutorial, puede utilizar cualquier enfoque.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Agregar proyecto de prueba unitaria al crear la aplicación

Crear una nueva aplicación Web de ASP.NET denominada **StoreApp**.

![Crear proyecto](unit-testing-with-aspnet-web-api/_static/image1.png)

En la ventana nuevo proyecto ASP.NET, seleccione la **vacía** plantilla y agregar carpetas y principales referencias de API Web. Seleccione el **agregar pruebas unitarias** opción. El proyecto de prueba unitaria se denomina automáticamente **StoreApp.Tests**. Puede conservar este nombre.

![Crear proyecto de prueba unitaria](unit-testing-with-aspnet-web-api/_static/image2.png)

Después de crear la aplicación, verá que contiene dos proyectos.

![dos proyectos](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Agregar proyecto de prueba unitaria a una aplicación existente

Si no ha creado el proyecto de prueba unitaria al crear la aplicación, puede agregar uno en cualquier momento. Por ejemplo, supongamos que ya tiene una aplicación denominada StoreApp y desea agregar pruebas unitarias. Para agregar un proyecto de prueba unitaria, haga clic en la solución y seleccione **agregar** y **nuevo proyecto**.

![Agregar nuevo proyecto a la solución](unit-testing-with-aspnet-web-api/_static/image4.png)

Seleccione **prueba** en el panel izquierdo y seleccione **proyecto de prueba unitaria** para el tipo de proyecto. Denomine el proyecto **StoreApp.Tests**.

![Agregar proyecto de prueba unitaria](unit-testing-with-aspnet-web-api/_static/image5.png)

Verá el proyecto de prueba unitaria en la solución.

En el proyecto de prueba unitaria, agregue una referencia de proyecto al proyecto original.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Configurar la aplicación Web API 2

En el proyecto StoreApp, agregue un archivo de clase para la **modelos** carpeta denominada **Product.cs**. Reemplace el contenido del archivo con el código siguiente.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Compile la solución.

Haga clic en la carpeta Controllers y seleccione **agregar** y **nuevo elemento de scaffolding**. Seleccione **vacía de Web API 2 controlador -**.

![Agregar nuevo controlador](unit-testing-with-aspnet-web-api/_static/image6.png)

Establece el nombre del controlador en **SimpleProductController**y haga clic en **agregar**.

![especificar controlador](unit-testing-with-aspnet-web-api/_static/image7.png)

Reemplace el código existente por el siguiente código. Para simplificar este ejemplo, los datos se almacenan en una lista en lugar de una base de datos. La lista definida en esta clase representa los datos de producción. Tenga en cuenta que el controlador incluye un constructor que toma como parámetro una lista de objetos de producto. Este constructor permite pasar datos de prueba cuando pruebas unitarias. El controlador también incluye dos **async** métodos para mostrar los métodos asincrónicos de pruebas unitarias. Estos métodos asincrónicos se implementaron mediante una llamada a **Task.FromResult** minimizar el código extraño, pero normalmente los métodos incluiría las operaciones que consumen muchos recursos.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

El método GetProduct devuelve una instancia de la **IHttpActionResult** interfaz. IHttpActionResult es una de las nuevas características de API Web 2, y simplifica el desarrollo de pruebas unitarias. Clases que implementan la interfaz IHttpActionResult se encuentran en el [System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx) espacio de nombres. Estas clases representan las posibles respuestas de una solicitud de acción y corresponden a los códigos de estado HTTP.

Compile la solución.

Ahora está listo para configurar el proyecto de prueba.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Instalar paquetes de NuGet en el proyecto de prueba

Cuando usas la plantilla vacía para crear una aplicación, el proyecto de prueba unitaria (StoreApp.Tests) no incluye los paquetes de NuGet instalados. Otras plantillas, como la plantilla API Web, incluyen algunos paquetes de NuGet en el proyecto de prueba unitaria. Para este tutorial, debe incluir el paquete de Microsoft ASP.NET Web API 2 principal para el proyecto de prueba.

Haga clic en el proyecto StoreApp.Tests y seleccione **administrar paquetes de NuGet**. Debe seleccionar el proyecto StoreApp.Tests para agregar los paquetes a ese proyecto.

![Administrar paquetes](unit-testing-with-aspnet-web-api/_static/image8.png)

Buscar e instalar el paquete de Microsoft ASP.NET Web API 2 Core.

![instalar el paquete de web api core](unit-testing-with-aspnet-web-api/_static/image9.png)

Cierre la ventana Administrar paquetes de NuGet.

<a id="tests"></a>
## <a name="create-tests"></a>Crear pruebas

De forma predeterminada, el proyecto de prueba incluye un archivo de prueba vacío denominado UnitTest1.cs. Este archivo muestra los atributos que se utiliza para crear métodos de prueba. Para las pruebas unitarias, puede usar este archivo o crear su propio archivo.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

Para este tutorial, creará su propia clase de prueba. Puede eliminar el archivo UnitTest1.cs. Agregue una clase denominada **TestSimpleProductController.cs**y reemplace el código con el código siguiente.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Ejecutar pruebas

Ahora está listo para ejecutar las pruebas. Todos los del método que se marcan con la **TestMethod** se probará el atributo. Desde el **prueba** elemento de menú, ejecutar las pruebas.

![ejecución de pruebas](unit-testing-with-aspnet-web-api/_static/image11.png)

Abra la **Explorador de pruebas** ventana y observe los resultados de las pruebas.

![resultados de pruebas](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Resumen

Ha completado este tutorial. Los datos en este tutorial se ha simplificado deliberadamente para centrarse en las condiciones de pruebas unitarias. Para escenarios más avanzados de datos de pruebas unitarias, vea [simulación Entity Framework cuando unidad pruebas ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
