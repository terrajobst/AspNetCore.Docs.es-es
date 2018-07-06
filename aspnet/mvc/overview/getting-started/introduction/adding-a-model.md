---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Adición de un modelo | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 2d99a9c553ba79922cb0c5e852709ab97ea2e22b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818468"
---
<a name="adding-a-model"></a>Adición de un modelo
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

En esta sección agregará algunas clases para administrar películas en una base de datos. Estas clases serán el &quot;modelo&quot; forma parte de la aplicación ASP.NET MVC.

Deberá usar una tecnología de acceso a datos de .NET Framework conocida como la [Entity Framework](https://docs.microsoft.com/ef/) para definir y trabajar con estas clases de modelo. El Entity Framework (a menudo denominado EF) es compatible con un paradigma de desarrollo llama *Code First*. Código primero permite crear objetos de modelo mediante la escritura de clases simple. (También conocido como son las clases POCO, de &quot;los objetos CLR antiguos sin formato.&quot;) A continuación, puede tener la base de datos que se crean sobre la marcha de las clases, lo que permite un flujo de trabajo de desarrollo muy limpio y rápido. Si es necesario crear primero la base de datos, puede seguir este tutorial para aprender sobre el desarrollo de aplicaciones MVC y EF. A continuación, puede seguir Tom Fizmakens [Scaffolding de ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, que cubre el primer enfoque de base de datos.

## <a name="adding-model-classes"></a>Agregar clases de modelo

En **el Explorador de soluciones**, haga clic en el *modelos* carpeta, seleccione **agregar**y, a continuación, seleccione **clase**.

![](adding-a-model/_static/image1.png)

Escriba el *clase* nombre &quot;película&quot;.

Agregue las siguientes cinco propiedades para el `Movie` clase:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Vamos a usar la `Movie` clase para representar las películas en una base de datos. Cada instancia de un `Movie` objeto corresponderá a una fila en una tabla de base de datos y cada propiedad de la `Movie` clase se asignará a una columna en la tabla.

Nota: Para poder usar la clase relacionada y System.Data.Entity, deberá instalar el [paquete NuGet de Entity Framework](https://www.nuget.org/packages/EntityFramework/). Siga el vínculo para obtener más instrucciones.

En el mismo archivo, agregue la siguiente `MovieDBContext` clase:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

El `MovieDBContext` clase representa el contexto de base de datos de películas de Entity Framework, que se encarga de capturar, almacenar y actualizar `Movie` instancias en una base de datos de clase. El `MovieDBContext` deriva el `DbContext` proporcionado por Entity Framework de clase base.

Para poder hacer referencia a `DbContext` y `DbSet`, deberá agregar lo siguiente `using` instrucción en la parte superior del archivo:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Puede hacerlo agregando manualmente el uso de la instrucción, o bien puede mantener el mouse sobre las líneas onduladas rojas, haga clic en `Show potential fixes` y haga clic en `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Nota: Algunos sin usar `using` instrucciones se han quitado. Visual Studio mostrará las dependencias no usadas en gris. Puede quitar las dependencias no usadas manteniendo el mouse sobre las dependencias de gris, haga clic en `Show potential fixes` y haga clic en **quitar instrucciones using no usadas.**

![](adding-a-model/_static/image3.png)

Por último, agregamos un modelo (M en MVC). En la siguiente sección trabajará con la cadena de conexión de base de datos.

> [!div class="step-by-step"]
> [Anterior](adding-a-view.md)
> [Siguiente](creating-a-connection-string.md)
