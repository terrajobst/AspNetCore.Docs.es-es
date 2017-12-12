---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Agregar un modelo (VB) | Documentos de Microsoft
author: Rick-Anderson
description: "Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: efc18dd71e29d12dacc6cf84a1d3c7f7e92f520d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-vb"></a>Agregar un modelo (VB)
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar por separado los requisitos previos mediante los siguientes vínculos:
> 
> - [Requisitos previos visuales Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)
> 
> Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con el código fuente VB.NET está disponible como acompañamiento de este tema. [Descargue la versión VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [versión de C#](../cs/adding-a-model.md) de este tutorial.


## <a name="adding-a-model"></a>Agregar un modelo

En esta sección agregará algunas clases para la administración de películas en una base de datos. Estas clases será la parte "modelo" de la aplicación de ASP.NET MVC.

Deberá usar una tecnología de acceso a datos de .NET Framework conocida como Entity Framework para definir y trabajar con estas clases de modelo. El Entity Framework (a menudo denominado EF) es compatible con un paradigma de desarrollo llama *Code First*. Código primero le permite crear objetos del modelo mediante la escritura de clases simples. (También conocido como son clases POCO, de "objetos CLR antiguos sin formato".) A continuación, puede hacer que la base de datos que se crea sobre la marcha de las clases, lo que permite a un flujo de trabajo de desarrollo muy limpio y rápido.

## <a name="adding-model-classes"></a>Agregar clases de modelo

En **el Explorador de soluciones**, haga clic con el *modelos* carpeta, seleccione **agregar**y, a continuación, seleccione **clase**.

![](adding-a-model/_static/image1.png)

Nombre de la clase "Película".

Agregue las siguientes propiedades de cinco a la `Movie` clase:

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Vamos a usar la `Movie` clase para representar las películas de una base de datos. Cada instancia de un `Movie` objeto corresponderá a una fila en una tabla de base de datos y cada propiedad de la `Movie` clase se asignará a una columna en la tabla.

En el mismo archivo, agregue las siguientes `MovieDBContext` clase:

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

El `MovieDBContext` clase representa el contexto de base de datos de película de Entity Framework, que controla capturar, almacenar y actualizar `Movie` instancias en una base de datos de clase. El `MovieDBContext` se deriva de la `DbContext` clase proporcionada por el marco de trabajo de la entidad base. Para obtener más información acerca de `DbContext` y `DbSet`, consulte [mejoras de productividad para Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Para poder hacer referencia a `DbContext` y `DbSet`, debe agregar la siguiente `imports` instrucción en la parte superior del archivo:

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

La sección completa *Movie.vb* archivo se muestra a continuación.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Crear una cadena de conexión y trabajar con SQL Server Compact

El `MovieDBContext` clase creada controla la tarea de conectarse a la base de datos y asignación `Movie` objetos a los registros de base de datos. Una pregunta que podría preguntar, sin embargo, se muestra cómo especificar qué base de datos se conecta a. Llevará a cabo mediante la adición de información de conexión en el *Web.config* archivo de la aplicación.

Abrir la raíz de la aplicación *Web.config* archivo. (No el *Web.config* un archivo en el *vistas* carpeta.) La imagen siguiente se muestran ambos *Web.config* archivos; abrir el *Web.config* archivo con un círculo rojo.

![](adding-a-model/_static/image2.png)

## 

Agregue la siguiente cadena de conexión para el `<connectionStrings>` elemento en el *Web.config* archivo.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

En el ejemplo siguiente se muestra una parte de la *Web.config* archivo con la nueva cadena de conexión agregada:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Esta cantidad pequeña de código y XML es todo lo que necesario para escribir con el fin de representar y almacenar los datos de la película en una base de datos.

A continuación, vamos a compilar un nuevo `MoviesController` clase que puede usar para mostrar los datos de la película y permitir a los usuarios crear nuevos anuncios de película.

>[!div class="step-by-step"]
[Anterior](adding-a-view.md)
[Siguiente](accessing-your-models-data-from-a-controller.md)
