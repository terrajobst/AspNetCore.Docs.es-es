---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Agregar un modelo | Documentos de Microsoft
author: Rick-Anderson
description: "Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 1d066e4bab866a2195647f43aa886279fee941db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model"></a>Agregar un modelo
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestra más características.


En esta sección agregará algunas clases para la administración de películas en una base de datos. Estas clases será la &quot;modelo&quot; forma parte de la aplicación ASP.NET MVC.

Deberá usar una tecnología de acceso a datos de .NET Framework conocida como la [Entity Framework](https://msdn.microsoft.com/en-us/library/bb399572(VS.110).aspx) para definir y trabajar con estas clases de modelo. El Entity Framework (a menudo denominado EF) es compatible con un paradigma de desarrollo llama *Code First*. Código primero le permite crear objetos del modelo mediante la escritura de clases simples. (Estas son también conocido como las clases, desde &quot;los objetos CLR antiguos sin formato.&quot;) A continuación, puede hacer que la base de datos que se crea sobre la marcha de las clases, lo que permite a un flujo de trabajo de desarrollo muy limpio y rápido.

## <a name="adding-model-classes"></a>Agregar clases de modelo

En **el Explorador de soluciones**, haga clic con el *modelos* carpeta, seleccione **agregar**y, a continuación, seleccione **clase**.

![](adding-a-model/_static/image1.png)

Escriba el *clase* nombre &quot;película&quot;.

Agregue las siguientes propiedades de cinco a la `Movie` clase:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Vamos a usar la `Movie` clase para representar las películas de una base de datos. Cada instancia de un `Movie` objeto corresponderá a una fila en una tabla de base de datos y cada propiedad de la `Movie` clase se asignará a una columna en la tabla.

En el mismo archivo, agregue las siguientes `MovieDBContext` clase:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

El `MovieDBContext` clase representa el contexto de base de datos de película de Entity Framework, que controla capturar, almacenar y actualizar `Movie` instancias en una base de datos de clase. El `MovieDBContext` se deriva de la `DbContext` clase proporcionada por el marco de trabajo de la entidad base.

Para poder hacer referencia a `DbContext` y `DbSet`, debe agregar la siguiente `using` instrucción en la parte superior del archivo:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

La sección completa *Movie.cs* archivo se muestra a continuación. (Varias instrucciones using que no son necesarios se han quitado.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Crear una cadena de conexión y trabajar con SQL Server LocalDB

El `MovieDBContext` clase creada controla la tarea de conectarse a la base de datos y asignación `Movie` objetos a los registros de base de datos. Una pregunta que podría preguntar, sin embargo, se muestra cómo especificar qué base de datos se conecta a. Llevará a cabo mediante la adición de información de conexión en el *Web.config* archivo de la aplicación.

Abrir la raíz de la aplicación *Web.config* archivo. (No el *Web.config* un archivo en el *vistas* carpeta.) Abra la *Web.config* archivo destacado en rojo.

![](adding-a-model/_static/image2.png)

Agregue la siguiente cadena de conexión para el `<connectionStrings>` elemento en el *Web.config* archivo.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

En el ejemplo siguiente se muestra una parte de la *Web.config* archivo con la nueva cadena de conexión agregada:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Esta cantidad pequeña de código y XML es todo lo que necesario para escribir con el fin de representar y almacenar los datos de la película en una base de datos.

A continuación, vamos a compilar un nuevo `MoviesController` clase que puede usar para mostrar los datos de la película y permitir a los usuarios crear nuevos anuncios de película.

>[!div class="step-by-step"]
[Anterior](adding-a-view.md)
[Siguiente](accessing-your-models-data-from-a-controller.md)
