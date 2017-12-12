---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: "Crear una cadena de conexión y trabajar con SQL Server LocalDB | Documentos de Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 41f1f30d86406580ab9fc7278a94d9c291913f9a
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2017
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Crear una cadena de conexión y trabajar con SQL Server LocalDB
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Crear una cadena de conexión y trabajar con SQL Server LocalDB

El `MovieDBContext` clase creada controla la tarea de conectarse a la base de datos y asignación `Movie` objetos a los registros de base de datos. Una pregunta que podría preguntar, sin embargo, se muestra cómo especificar qué base de datos se conecta a. Realmente no es necesario que especificar qué base de datos, Entity Framework de forma predeterminada utilizarán [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). En esta sección vamos a agregar explícitamente una cadena de conexión en el *Web.config* archivo de la aplicación.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) es una versión ligera de SQL Server Express Database Engine que se inicia a petición y se ejecuta en modo de usuario. LocalDB se ejecuta en un modo especial de ejecución de SQL Server Express que le permite trabajar con bases de datos como *.mdf* archivos. Normalmente, los archivos de base de datos de LocalDB se mantienen en la *aplicación\_datos* carpeta de un proyecto web.

SQL Server Express no se recomienda para su uso en aplicaciones web de producción. LocalDB en particular no se recomienda para entornos de producción con una aplicación web porque no está diseñado para trabajar con IIS. Sin embargo, se puede migrar fácilmente una base de datos de LocalDB a SQL Server o SQL Azure.

En Visual Studio 2017 LocalDB se instala de forma predeterminada con Visual Studio.

De forma predeterminada, Entity Framework buscará en una cadena de conexión que el mismo nombre que la clase de contexto de objeto (`MovieDBContext` para este proyecto). Para obtener más información, consulte [cadenas de conexión de SQL Server para las aplicaciones Web ASP.NET](https://msdn.microsoft.com/en-us/library/jj653752.aspx).

Abrir la raíz de la aplicación *Web.config* archivo se muestra a continuación. (No el *Web.config* un archivo en el *vistas* carpeta.)

![](creating-a-connection-string/_static/image1.png)

Buscar el `<connectionStrings>` elemento:

![](creating-a-connection-string/_static/image2.png)

Agregue la siguiente cadena de conexión para el `<connectionStrings>` elemento en el *Web.config* archivo.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

En el ejemplo siguiente se muestra una parte de la *Web.config* archivo con la nueva cadena de conexión agregada:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Las dos cadenas de conexión son muy similares. La primera cadena de conexión se denomina `DefaultConnection` y se utiliza para la base de datos de pertenencia para controlar quién puede tener acceso a la aplicación. Ha agregado la cadena de conexión especifica una base de datos de LocalDB denominado *Movie.mdf* ubicado en el *aplicación\_datos* carpeta. Nos no usar la base de datos de pertenencia en este tutorial, para obtener más información sobre la pertenencia, autenticación y seguridad, vea el tutorial [crear una aplicación de MVC de ASP.NET con la autenticación y la base de datos SQL e implementar al servicio de aplicaciones de Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

El nombre de la cadena de conexión debe coincidir con el nombre de la [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=vs.103).aspx) clase.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Realmente no es necesario agregar el `MovieDBContext` cadena de conexión. Si no se especifica una cadena de conexión, Entity Framework creará una base de datos de LocalDB en el directorio de los usuarios con el nombre completo de la [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=vs.103).aspx) clase (en este caso `MvcMovie.Models.MovieDBContext`). Se puede asignar un nombre la base de datos que desee, siempre y cuando tenga el *. MDF* sufijo. Por ejemplo, se puede nombrar según la base de datos *MyFilms.mdf*.

A continuación, vamos a compilar un nuevo `MoviesController` clase que puede usar para mostrar los datos de la película y permitir a los usuarios crear nuevos anuncios de película.

>[!div class="step-by-step"]
[Anterior](adding-a-model.md)
[Siguiente](accessing-your-models-data-from-a-controller.md)
