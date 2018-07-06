---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: Cómo actualizar a ASP.NET MVC 4 y Web de proyecto de API a ASP.NET MVC 5 y Web API 2 | Microsoft Docs
author: Rick-Anderson
description: ASP.NET MVC 5 y Web API 2 poner un sinfín de características nuevas, como el enrutamiento mediante atributos, los filtros de autenticación y mucho más.
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 8a50c188a2283af46789739e710be69159799695
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826749"
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>Cómo actualizar un ASP.NET MVC 4 y el proyecto de API Web a ASP.NET MVC 5 y Web API 2
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> ASP.NET MVC 5 y Web API 2 poner un sinfín de características nuevas, como el enrutamiento mediante atributos, los filtros de autenticación y mucho más. Consulte [ https://www.asp.net/vnext ](https://www.asp.net/core) para obtener más detalles.
> 
> En este tutorial le ayudará con los pasos necesarios para actualizar la aplicación a la versión más reciente.  
> 
> > [!NOTE]
> > Consulte [ASP.NET and Web Tools para Visual Studio 2013 notas](../../../visual-studio/overview/2013/release-notes.md) para obtener información sobre cambios importantes respecto a MVC 4 y Web API a la siguiente versión.
> 
>   
> 
> En este artículo se escribió por Youngjune Hong y Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>Pasos de actualización

1. Copia de seguridad del proyecto. En este tutorial, deberá realizar cambios en el archivo de proyecto, configuración de paquete y los archivos web.config.
2. Para actualizar desde API Web a Web API 2, en global.asax, cambie:

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   por

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. Asegúrese de que todos los paquetes que usan los proyectos son compatibles con MVC 5 y Web API 2. La siguiente tabla se muestran los 4 MVC y Web API relacionadas con los paquetes que deben cambiarse. Si tiene un paquete que depende de uno de los paquetes que se enumeran a continuación, póngase en contacto con los publicadores para obtener las versiones más recientes que son compatibles con MVC 5 y Web API 2. Si tiene el código fuente para esos paquetes, debe volver a compilar con los nuevos ensamblados de MVC 5 y Web API 2.   

    | **Id. de paquete** | **Versión anterior** | **Nueva versión** |
    | --- | --- | --- |
    | Microsoft.AspNet.Razor | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.WebData | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.OAuth | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.Mvc | 4.0 | 5.0.0 |
    | Microsoft.AspNet.Mvc.Facebook | 4.0 | 5.0.0 |
    | Microsoft.AspNet.WebApi.Core | 4.0 | 5.0.0 |
    | Microsoft.AspNet.WebApi.SelfHost | 4.0 | 5.0.0 |
    | Microsoft.AspNet.WebApi.Client | 4.0 | 5.0.0 |
    | Microsoft.AspNet.WebApi.OData | 4.0 | 5.0.0 |
    | Microsoft.AspNet.WebApi | 4.0 | 5.0.0 |
    | Microsoft.AspNet.WebApi.WebHost | 4.0 | 5.0.0 |
    | Microsoft.AspNet.WebApi.Tracing | 4.0 | 5.0.0 |
    | Microsoft.AspNet.WebApi.HelpPage | 4.0 | 5.0.0 |
    | Microsoft.Net.Http | 2.0. x. | 2.2. x. |
    | Microsoft.Data.OData | 5.2. x | 5.6 |
    | System.Spatial | 5.2. x | 5.6 |
    | Microsoft.Data.Edm | 5.2. x | 5.6 |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | < o:p >< / o:p > | Quitada |
    | Microsoft.AspNet.WebPages.Administration | < o:p >< / o:p > | Quitada |
    | Aplicaciones auxiliares de Web de Microsoft | < o:p >< / o:p > | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Aplicaciones auxiliares de Web de Microsoft se ha reemplazado por Microsoft.AspNet.WebHelpers. Debe quitar el paquete antiguo en primer lugar y, a continuación, instale el paquete más reciente.   
    >   
    > No hay ninguna compatibilidad de versiones entre los paquetes principales de ASP.NET. Por ejemplo, MVC 5 es compatible con solo Razor 3 y 2 de Razor no.
4. Abra el proyecto en Visual Studio 2013.
5. Quitar cualquiera de los siguientes paquetes de NuGet de ASP.NET que se instalan. Quitará ellas mediante la consola de administrador de paquetes (PMC). Para abrir la PMC, seleccione el **herramientas** menú y, a continuación, seleccione **Administrador de paquetes de biblioteca,** , a continuación, seleccione **Package Manager Console**. El proyecto no puede incluir todos ellos.

    1. `Microsoft.AspNet.WebPages.Administration`  
   Normalmente, este paquete se agrega al actualizar de MVC 3 a MVC 4. Para quitarlo, ejecute el siguiente comando en la PMC:  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   Este paquete se ha pasado a llamarse `Microsoft.AspNet.WebHelpers`. Para quitarlo, ejecute el siguiente comando en la PMC:  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   Este paquete contiene una solución alternativa para un error en MVC 4 que se ha corregido en MVC 5. Para quitarlo, ejecute el siguiente comando en la PMC:  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. Actualizar todos los paquetes de NuGet de ASP.NET con la PMC. En la PMC, ejecute el siguiente comando:  
    `Update-Package`  
   El `Update-Package` comando sin ningún parámetro actualizará todos los paquetes. Puede actualizar los paquetes individualmente mediante el argumento de identificador. Para obtener más información acerca del comando de actualización, ejecute `get-help update-package` .

## <a name="update-the-application-webconfig-file"></a>Actualizar la aplicación *web.config* archivo

Asegúrese de realizar estos cambios en la aplicación *web.config* archivo, no el *web.config* de archivos en el *vistas* carpeta.

Busque el `<runtime>/<assemblyBinding>` sección y realizar los cambios siguientes:

1. En los elementos con el atributo de nombre "System.Web.Mvc", cambie el número de versión de "4.0.0.0" a "5.0.0.0". (Dos cambios en ese elemento).
2. En los elementos con el atributo de nombre &quot;System.Web.Helpers "y &quot;System.Web.WebPages&quot; cambiar el número de versión de"2.0.0.0"a"3.0.0.0". Se producirá cuatro cambios, dos en cada uno de los elementos.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. Busque el `<appSettings>` sección y actualizar el webpages:version desde 2.0.0.0.0 a 3.0.0.0, tal como se muestra a continuación:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. Quitar los niveles de confianza que no sea completo. Por ejemplo:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>Actualización de la *web.config* archivos bajo la carpeta de vistas

Si la aplicación está utilizando áreas, también deberá actualizar cada *web.config* de archivos en el *vistas* subcarpeta de cada carpeta de área.

1. Actualizar todos los elementos que contienen "System.Web.Mvc" de la versión "4.0.0.0" a la versión "5.0.0.0".  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. Actualizar todos los elementos que contienen "System.Web.WebPages.Razor" de la versión "2.0.0.0" a la versión "3.0.0.0". Si esta sección contiene "System.Web.WebPages", actualizar esos elementos de la versión "2.0.0.0" versión "3.0.0.0"  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. Si ha quitado el `Microsoft-Web-Helpers` Instalar paquete NuGet en un paso anterior, `Microsoft.AspNet.WebHelpers` con el siguiente comando en la PMC:  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. Si su aplicación usa el [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) método, agregue lo siguiente a la *Web.config* archivo.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>Pasos finales

Compilar y probar la aplicación.

Quitar el tipo de proyecto de MVC 4 GUID de los archivos del proyecto.

1. En el Explorador de soluciones, haga clic en el nombre del proyecto y, a continuación, seleccione **descargar el proyecto**.
2. Haga clic en el proyecto y seleccione Editar nombredelproyecto.csproj.
3. Busque el `ProjectTypeGuids` elemento y, a continuación, quitar el MVC 4 proyecto GUID, `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.
4. Guarde y cierre el archivo de proyecto abierto.
5. Haga clic en el proyecto y seleccione **recargar el proyecto**.
