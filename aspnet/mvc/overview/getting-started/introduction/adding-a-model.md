---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Agregar un modelo | Documentos de Microsoft
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 79f136257119a8600a65e8d7c5f6e99cb9abceae
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2018
---
<a name="adding-a-model"></a>Agregar un modelo
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

En esta sección agregará algunas clases para la administración de películas en una base de datos. Estas clases será la &quot;modelo&quot; forma parte de la aplicación de ASP.NET MVC.

Deberá usar una tecnología de acceso a datos de .NET Framework conocida como la [Entity Framework](https://docs.microsoft.com/ef/) para definir y trabajar con estas clases de modelo. El Entity Framework (a menudo denominado EF) es compatible con un paradigma de desarrollo llama *Code First*. Código primero le permite crear objetos del modelo mediante la escritura de clases simples. (Estas son también conocido como las clases, desde &quot;los objetos CLR antiguos sin formato.&quot;) A continuación, puede hacer que la base de datos que se crea sobre la marcha de las clases, lo que permite a un flujo de trabajo de desarrollo muy limpio y rápido. Si es necesario crear primero la base de datos, puede seguir este tutorial para aprender acerca del desarrollo de aplicaciones MVC y EF. A continuación, puede seguir Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, que cubre el primer enfoque de base de datos.

## <a name="adding-model-classes"></a>Agregar clases de modelo

En **el Explorador de soluciones**, haga clic con el *modelos* carpeta, seleccione **agregar**y, a continuación, seleccione **clase**.

![](adding-a-model/_static/image1.png)

Escriba el *clase* nombre &quot;película&quot;.

Agregue las siguientes propiedades de cinco a la `Movie` clase:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Vamos a usar la `Movie` clase para representar las películas de una base de datos. Cada instancia de un `Movie` objeto corresponderá a una fila en una tabla de base de datos y cada propiedad de la `Movie` clase se asignará a una columna en la tabla.

Nota: Para poder usar System.Data.Entity y la clase relacionada, debe instalar la [paquete NuGet de Entity Framework](https://www.nuget.org/packages/EntityFramework/). Siga el vínculo para obtener más instrucciones.

En el mismo archivo, agregue las siguientes `MovieDBContext` clase:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

El `MovieDBContext` clase representa el contexto de base de datos de película de Entity Framework, que controla capturar, almacenar y actualizar `Movie` instancias en una base de datos de clase. El `MovieDBContext` se deriva de la `DbContext` clase proporcionada por el marco de trabajo de la entidad base.

Para poder hacer referencia a `DbContext` y `DbSet`, debe agregar la siguiente `using` instrucción en la parte superior del archivo:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Puede hacerlo agregando manualmente el uso de la instrucción, o se puede mantener el mouse sobre las líneas onduladas rojas, haga clic en `Show potential fixes` y haga clic en`using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Nota: Algunos sin usar `using` instrucciones se han quitado. Visual Studio mostrará dependencias no usadas en gris. Puede quitar dependencias no usadas por el mouse sobre las dependencias de gris, haga clic en `Show potential fixes` y haga clic en **quitar instrucciones using no usadas.**

![](adding-a-model/_static/image3.png)

Por último, hemos agregado un modelo (M en MVC). En la siguiente sección, trabajará con la cadena de conexión de base de datos.

>[!div class="step-by-step"]
[Anterior](adding-a-view.md)
[Siguiente](creating-a-connection-string.md)
