---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Creación de una cadena de conexión y trabajar con SQL Server LocalDB | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 746101344832793b199d2b3f3dfcfcd4e3b9a8da
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578528"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Creación de una cadena de conexión y trabajar con SQL Server LocalDB
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Creación de una cadena de conexión y trabajar con SQL Server LocalDB

El `MovieDBContext` clase creó controla la tarea de conectarse a la base de datos y asignación `Movie` objetos a los registros de la base de datos. Una pregunta que podría preguntar, sin embargo, es cómo especificar que se conectará a la base de datos. Realmente no es necesario que especificar qué base de datos, Entity Framework de forma predeterminada utilizarán [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). En esta sección vamos a agregar explícitamente una cadena de conexión en el *Web.config* archivo de la aplicación.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) es una versión ligera de SQL Server Express Database Engine que se inicia a petición y se ejecuta en modo de usuario. LocalDB se ejecuta en un modo de ejecución especiales de SQL Server Express que le permite trabajar con bases de datos como *.mdf* archivos. Normalmente, los archivos de base de datos de LocalDB se mantienen en el *aplicación\_datos* carpeta de un proyecto web.

SQL Server Express no se recomienda para su uso en aplicaciones web de producción. LocalDB en particular no se recomienda para entornos de producción con una aplicación web porque no está diseñado para trabajar con IIS. Sin embargo, se puede migrar fácilmente una base de datos LocalDB a SQL Server o SQL Azure.

En Visual Studio 2017, LocalDB está instalado de forma predeterminada con Visual Studio.

De forma predeterminada, Entity Framework busca una cadena de conexión, el mismo nombre que la clase de contexto de objeto (`MovieDBContext` para este proyecto). Para obtener más información, consulte [cadenas de conexión de SQL Server para aplicaciones Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Abra la raíz de la aplicación *Web.config* archivo se muestra a continuación. (No el *Web.config* de archivos en el *vistas* carpeta.)

![](creating-a-connection-string/_static/image1.png)

Buscar el `<connectionStrings>` elemento:

![](creating-a-connection-string/_static/image2.png)

Agregue la siguiente cadena de conexión para el `<connectionStrings>` elemento en el *Web.config* archivo.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

El ejemplo siguiente muestra una parte de la *Web.config* archivo con la nueva cadena de conexión agregada:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Las dos cadenas de conexión son muy similares. La primera cadena de conexión se denomina `DefaultConnection` y se utiliza para la base de datos de pertenencia para controlar quién puede acceder a la aplicación. Ha agregado la cadena de conexión especifica una base de datos de LocalDB denominada *Movie.mdf* ubicado en el *aplicación\_datos* carpeta. Hemos no usar la base de datos de pertenencia en este tutorial, para obtener más información sobre la pertenencia, autenticación y seguridad, consulte mi tutorial [crear una aplicación ASP.NET MVC con la autenticación y base de datos SQL e implementar en Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

El nombre de la cadena de conexión debe coincidir con el nombre de la [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) clase.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Realmente no es necesario agregar el `MovieDBContext` cadena de conexión. Si no especifica una cadena de conexión, Entity Framework creará una base de datos LocalDB en el directorio de los usuarios con el nombre completo de la [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) clase (en este caso `MvcMovie.Models.MovieDBContext`). Puede asignar el nombre la base de datos que desee, siempre y cuando tenga el *. MDF* sufijo. Por ejemplo, podríamos denominar la base de datos *MyFilms.mdf*.

A continuación, creará un nuevo `MoviesController` clase que puede usar para mostrar los datos de la película y permitir a los usuarios crear nuevos anuncios de película.

> [!div class="step-by-step"]
> [Anterior](adding-a-model.md)
> [Siguiente](accessing-your-models-data-from-a-controller.md)
