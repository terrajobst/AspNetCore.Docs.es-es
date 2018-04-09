---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Agregar un modelo (C#) | Documentos de Microsoft
author: Rick-Anderson
description: 'Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7f9206ddd43b4a3b4f96240ee48b9414450da22
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model-c"></a>Agregar un modelo (C#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar por separado los requisitos previos mediante los siguientes vínculos:
> 
> - [Requisitos previos visuales Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)
> 
> Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con el código fuente de C# está disponible como acompañamiento de este tema. [Descargue la versión de C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si prefiere Visual Basic, cambie a la [versión de Visual Basic](../vb/adding-a-model.md) de este tutorial.


## <a name="adding-a-model"></a>Agregar un modelo

En esta sección agregará algunas clases para la administración de películas en una base de datos. Estas clases será la parte "modelo" de la aplicación de ASP.NET MVC.

Deberá usar una tecnología de acceso a datos de .NET Framework conocida como Entity Framework para definir y trabajar con estas clases de modelo. El Entity Framework (a menudo denominado EF) es compatible con un paradigma de desarrollo llama *Code First*. Código primero le permite crear objetos del modelo mediante la escritura de clases simples. (También conocido como son clases POCO, de "objetos CLR antiguos sin formato".) A continuación, puede hacer que la base de datos que se crea sobre la marcha de las clases, lo que permite a un flujo de trabajo de desarrollo muy limpio y rápido.

## <a name="adding-model-classes"></a>Agregar clases de modelo

En **el Explorador de soluciones**, haga clic con el *modelos* carpeta, seleccione **agregar**y, a continuación, seleccione **clase**.

![](adding-a-model/_static/image1.png)

Nombre de la *clase* "Película".

[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)

Agregue las siguientes propiedades de cinco a la `Movie` clase:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Vamos a usar la `Movie` clase para representar las películas de una base de datos. Cada instancia de un `Movie` objeto corresponderá a una fila en una tabla de base de datos y cada propiedad de la `Movie` clase se asignará a una columna en la tabla.

En el mismo archivo, agregue las siguientes `MovieDBContext` clase:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

El `MovieDBContext` clase representa el contexto de base de datos de película de Entity Framework, que controla capturar, almacenar y actualizar `Movie` instancias en una base de datos de clase. El `MovieDBContext` se deriva de la `DbContext` clase proporcionada por el marco de trabajo de la entidad base. Para obtener más información acerca de `DbContext` y `DbSet`, consulte [mejoras de productividad para Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Para poder hacer referencia a `DbContext` y `DbSet`, debe agregar la siguiente `using` instrucción en la parte superior del archivo:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

La sección completa *Movie.cs* archivo se muestra a continuación.

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Crear una cadena de conexión y trabajar con SQL Server Compact

El `MovieDBContext` clase creada controla la tarea de conectarse a la base de datos y asignación `Movie` objetos a los registros de base de datos. Una pregunta que podría preguntar, sin embargo, se muestra cómo especificar qué base de datos se conecta a. Llevará a cabo mediante la adición de información de conexión en el *Web.config* archivo de la aplicación.

Abrir la raíz de la aplicación *Web.config* archivo. (No el *Web.config* un archivo en el *vistas* carpeta.) La imagen siguiente se muestran ambos *Web.config* archivos; abrir el *Web.config* archivo con un círculo rojo.

![](adding-a-model/_static/image4.png)

### 

Agregue la siguiente cadena de conexión para el `<connectionStrings>` elemento en el *Web.config* archivo.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

En el ejemplo siguiente se muestra una parte de la *Web.config* archivo con la nueva cadena de conexión agregada:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Esta cantidad pequeña de código y XML es todo lo que necesario para escribir con el fin de representar y almacenar los datos de la película en una base de datos.

A continuación, vamos a compilar un nuevo `MoviesController` clase que puede usar para mostrar los datos de la película y permitir a los usuarios crear nuevos anuncios de película.

> [!div class="step-by-step"]
> [Anterior](adding-a-view.md)
> [Siguiente](accessing-your-models-data-from-a-controller.md)
