---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Simular Entity Framework cuando ASP.NET Web API 2 de pruebas unitarias | Microsoft Docs
author: Rick-Anderson
description: Esta guía y la aplicación muestran cómo crear pruebas unitarias para la aplicación Web API 2 que utiliza Entity Framework. Muestra cómo modificar el...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f1799b3f9d698053c397e57da3f33ff900ec4013
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021032"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Simular Entity Framework cuando ASP.NET Web API 2 de pruebas unitarias
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

[Descargue el proyecto completado](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Esta guía y la aplicación muestran cómo crear pruebas unitarias para la aplicación Web API 2 que utiliza Entity Framework. Muestra cómo modificar el controlador con scaffold para permitir pasar un objeto de contexto para las pruebas y cómo crear objetos de prueba que funcionan con Entity Framework.
>
> Para obtener una introducción a las pruebas unitarias con ASP.NET Web API, consulte [Unit Testing con ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).
>
> En este tutorial se da por supuesto que está familiarizado con los conceptos básicos de ASP.NET Web API. Para ver un tutorial introductorio, vea [Introducción a ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Web API 2

## <a name="in-this-topic"></a>En este tema

Este tema contiene las siguientes secciones:

- [Requisitos previos](#prereqs)
- [Descargue el código](#download)
- [Crear la aplicación con el proyecto de prueba unitaria](#appwithunittest)
- [Crear la clase del modelo](#modelclass)
- [Agregar el controlador](#controller)
- [Agregar la inserción de dependencias](#dependency)
- [Instalar paquetes de NuGet en el proyecto de prueba](#testpackages)
- [Crear el contexto de la prueba](#testcontext)
- [Crear pruebas](#tests)
- [Ejecutar pruebas](#runtests)

Si ya ha completado los pasos descritos en [Unit Testing con ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), puede ir a la sección [agregar el controlador](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Requisitos previos

Visual Studio 2017 Community, Professional o Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Descargue el código

Descargue el [proyecto completado](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). El proyecto descargable incluye código de prueba de unidad para este tema y la [unidad pruebas ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) tema.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Crear la aplicación con el proyecto de prueba unitaria

Puede crear un proyecto de prueba unitaria al crear la aplicación o agregar un proyecto de prueba unitaria a una aplicación existente. Este tutorial muestra cómo crear un proyecto de prueba unitaria al crear la aplicación.

Crear una nueva aplicación Web de ASP.NET denominada **StoreApp**.

En las ventanas de nuevo proyecto ASP.NET, seleccione la **vacía** plantilla y agregar carpetas y referencias centrales para API Web. Seleccione el **agregar pruebas unitarias** opción. El proyecto de prueba unitaria se denomina automáticamente **StoreApp.Tests**. Puede conservar este nombre.

![Crear proyecto de prueba unitaria](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Después de crear la aplicación, verá contiene dos proyectos: **StoreApp** y **StoreApp.Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Crear la clase del modelo

En el proyecto StoreApp, agregue un archivo de clase para el **modelos** carpeta denominada **Product.cs**. Reemplace el contenido del archivo con el código siguiente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Compila la solución.

<a id="controller"></a>
## <a name="add-the-controller"></a>Agregar el controlador

Haga clic en la carpeta Controllers y seleccione **agregar** y **nuevo elemento de scaffolding**. Seleccione el controlador de Web API 2 con acciones que usan Entity Framework.

![Agregar nuevo controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Establezca los siguientes valores:

- Nombre del controlador: **ProductController**
- Clase de modelo: **producto**
- Clase de contexto de datos: [seleccione **nuevo contexto de datos** botón que rellena los valores que se muestra a continuación]

![Especifique el controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Haga clic en **agregar** para crear el controlador con el código generado automáticamente. El código incluye métodos para crear, recuperar, actualizar y eliminar instancias de la clase de producto. El código siguiente muestra el método para agregar un producto. Tenga en cuenta que el método devuelve una instancia de **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult es una de las nuevas características de Web API 2, y simplifica el desarrollo de pruebas unitarias.

En la sección siguiente, personalizará el código generado para facilitar la transferencia de objetos de prueba al controlador.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Agregar la inserción de dependencias

Actualmente, la clase ProductController está codificado de forma rígida para utilizar una instancia de la clase StoreAppContext. Utilizará un patrón de inserción de dependencias para modificar la aplicación y quitar esa dependencia codificada de forma rígida. Al romper esta dependencia, puede pasar al realizar pruebas en un objeto ficticio.

Haga clic en el **modelos** carpeta y agregue una nueva interfaz denominada **IStoreAppContext**.

Reemplace el código por el siguiente código.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Abra el archivo StoreAppContext.cs y realice los siguientes cambios resaltados. Los cambios importantes a tener en cuenta son:

- Clase StoreAppContext implementa la interfaz IStoreAppContext
- Se implementa el método MarkAsModified


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Abra el archivo ProductController.cs. Cambie el código existente para que coincida con el código resaltado. Estos cambios romper la dependencia en StoreAppContext y permiten a otras clases pasar un objeto diferente de la clase de contexto. Este cambio permitirá pasar en un contexto de prueba durante las pruebas unitarias.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Hay un cambio más que debe realizar en ProductController. En el **PutProduct** método, reemplace la línea que establece el estado de la entidad a se modifica con una llamada al método MarkAsModified.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Compila la solución.

Ahora está listo para configurar el proyecto de prueba.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Instalar paquetes de NuGet en el proyecto de prueba

Cuando usas la plantilla vacía para crear una aplicación, el proyecto de prueba unitaria (StoreApp.Tests) no incluye los paquetes de NuGet instalados. Otras plantillas, como la plantilla Web API, incluyen algunos paquetes de NuGet en el proyecto de prueba unitaria. Para este tutorial, debe incluir el paquete de Entity Framework y el paquete de Microsoft ASP.NET Web API 2 Core al proyecto de prueba.

Haga clic en el proyecto StoreApp.Tests y seleccione **administrar paquetes de NuGet**. Debe seleccionar el proyecto StoreApp.Tests para agregar los paquetes a ese proyecto.

![administración de paquetes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

De los paquetes en línea, buscar e instalar el paquete de Entity Framework (versión 6.0 o posterior). Si parece que ya está instalado el paquete de Entity Framework, es posible que ha seleccionado el proyecto StoreApp en lugar del proyecto StoreApp.Tests.

![Agregar Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Buscar e instalar el paquete de Microsoft ASP.NET Web API 2 núcleos.

![instalar el paquete de core web api](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Cierre la ventana Administrar paquetes de NuGet.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Crear el contexto de la prueba

Agregue una clase denominada **TestDbSet** al proyecto de prueba. Esta clase actúa como clase base para el conjunto de datos de prueba. Reemplace el código por el siguiente código.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Agregue una clase denominada **TestProductDbSet** al proyecto de prueba que contiene el código siguiente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Agregue una clase denominada **TestStoreAppContext** y reemplace el código existente por el código siguiente.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Crear pruebas

De forma predeterminada, el proyecto de prueba incluye un archivo de prueba vacío denominado **UnitTest1.cs**. Este archivo muestra los atributos que se utiliza para crear métodos de prueba. Para este tutorial, puede eliminar este archivo, puesto que agregará una nueva clase de prueba.

Agregue una clase denominada **TestProductController** al proyecto de prueba. Reemplace el código por el siguiente código.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Ejecutar pruebas

Ahora está listo para ejecutar las pruebas. Todo el método que se marcan con el **TestMethod** se probará el atributo. Desde el **prueba** elemento de menú, ejecutar las pruebas.

![ejecución de pruebas](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Abra el **Explorador de pruebas** ventana y observe los resultados de las pruebas.

![resultados de pruebas](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
