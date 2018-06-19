---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: Cómo actualizar un MVC de ASP.NET 4 y proyecto de API para ASP.NET MVC 5 y Web API 2 Web | Documentos de Microsoft
author: Rick-Anderson
description: MVC de ASP.NET 5 y API Web 2 aportan una serie de características nuevas, como ruta de atributo, filtros de autenticación y mucho más.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f61502933a5ba92896ee97cef9cff915fe23831d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874735"
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>Cómo actualizar un ASP.NET MVC 4 y el proyecto de API Web MVC de ASP.NET 5 y Web API 2
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> MVC de ASP.NET 5 y API Web 2 aportan una serie de características nuevas, como ruta de atributo, filtros de autenticación y mucho más. Vea [ https://www.asp.net/vnext ](https://www.asp.net/core) para obtener más detalles.
> 
> Este tutorial le ayudará con los pasos necesarios para actualizar la aplicación a la versión más reciente.  
> 
> > [!NOTE]
> > Vea [ASP.NET y herramientas Web para Visual Studio 2013 notas](../../../visual-studio/overview/2013/release-notes.md) para obtener información sobre cambios principales en MVC 4 y API Web a la versión siguiente.
> 
>   
> 
> En este artículo se escribió Youngjune Hong y Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>Pasos de actualización

1. Copia de seguridad del proyecto. En este tutorial, deberá realizar cambios en el archivo de proyecto, configuración de paquetes y archivos web.config.
2. Para actualizar a través de API Web a API Web 2, en global.asax, cambie:

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   por

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. Asegúrese de que todos los paquetes que utilizan los proyectos son compatibles con MVC 5 y API Web 2. La siguiente tabla se muestran MVC 4 y Web API relacionadas con los paquetes que deben cambiarse. Si tiene un paquete que depende de uno de los paquetes que se enumeran a continuación, póngase en contacto con los publicadores para obtener las versiones más recientes que son compatibles con MVC 5 y API Web 2. Si tiene el código fuente de esos paquetes, debe volver a compilar con los nuevos ensamblados de MVC 5 y API Web 2.   

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
    | Microsoft.Net.Http | 2.0. x. | 2.2.x. |
    | Microsoft.Data.OData | 5.2. x | 5.6.x |
    | System.Spatial | 5.2. x | 5.6.x |
    | Microsoft.Data.Edm | 5.2. x | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | <o:p> </o:p> | Quitada |
    | Microsoft.AspNet.WebPages.Administration | <o:p> </o:p> | Quitada |
    | Microsoft-Web-Helpers | <o:p> </o:p> | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Aplicaciones auxiliares de Web de Microsoft se ha reemplazado por Microsoft.AspNet.WebHelpers. Debe quitar el paquete antiguo en primer lugar y, a continuación, instale el paquete más reciente.   
    >   
    > No hay ninguna compatibilidad de versiones cruzada entre paquetes principales de ASP.NET. Por ejemplo, MVC 5 es compatible con solo 3 Razor y no 2 de Razor.
4. Abra el proyecto en Visual Studio 2013.
5. Quite alguno de los siguientes paquetes de NuGet de ASP.NET que están instalados. Se quitan estas mediante la consola de administrador de paquete (PMC). Para abrir el PMC, seleccione la **herramientas** menú y, a continuación, seleccione **Administrador de paquetes de biblioteca,** , a continuación, seleccione **Package Manager Console**. El proyecto no puede incluir todos ellos.

    1. `Microsoft.AspNet.WebPages.Administration`  
   Este paquete normalmente se agrega al actualizar desde MVC 3 a MVC 4. Para quitarlo, ejecute el siguiente comando en el PMC:  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   Este paquete se ha cambiado el nombre `Microsoft.AspNet.WebHelpers`. Para quitarlo, ejecute el siguiente comando en el PMC:  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   Este paquete contiene una solución alternativa para un error en MVC 4 que se ha solucionado en MVC 5. Para quitarlo, ejecute el siguiente comando en el PMC:  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. Actualizar todos los paquetes de NuGet de ASP.NET mediante el PMC. En el PMC, ejecute el siguiente comando:  
    `Update-Package`  
   El `Update-Package` comando sin ningún parámetro actualizará todos los paquetes. Puede actualizar paquetes individualmente mediante el argumento de ID. Para obtener más información sobre el comando de actualización, ejecute `get-help update-package` .

## <a name="update-the-application-webconfig-file"></a>Actualizar la aplicación *web.config* archivo

Asegúrese de realizar estos cambios en la aplicación *web.config* archivo, no el *web.config* un archivo en el *vistas* carpeta.

Busque la `<runtime>/<assemblyBinding>` sección y realizar los siguientes cambios:

1. En los elementos con el atributo de nombre "System.Web.Mvc", cambie el número de versión de "4.0.0.0" a "5.0.0.0". (Dos cambios en ese elemento).
2. En elementos con el atributo name &quot;System.Web.Helpers "y &quot;System.Web.WebPages&quot; cambiar el número de versión de"2.0.0.0"a"3.0.0.0". Se producirá cuatro cambios, dos en cada uno de los elementos.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. Busque la `<appSettings>` sección y actualizar la webpages:version de 2.0.0.0.0 a la versión 3.0.0.0 tal y como se muestra a continuación:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. Quitar los niveles de confianza que no sea completo. Por ejemplo:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>Actualización de la *web.config* archivos en la carpeta vistas

Si la aplicación está utilizando áreas, también debe actualizar cada uno *web.config* un archivo en el *vistas* subcarpeta de cada carpeta de área.

1. Actualizar todos los elementos que contienen "System.Web.Mvc" de la versión "4.0.0.0" a la versión "5.0.0.0".  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. Actualizar todos los elementos que contienen "System.Web.WebPages.Razor" de la versión "2.0.0.0" a la versión "3.0.0.0". Si esta sección contiene "System.Web.WebPages", actualizar los elementos de la versión "2.0.0.0" a la versión "3.0.0.0"  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. Si ha quitado el `Microsoft-Web-Helpers` Instalar paquete NuGet en un paso anterior, `Microsoft.AspNet.WebHelpers` con el siguiente comando en el PMC:  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. Si su aplicación usa el [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) método, agregue lo siguiente a la *Web.config* archivo.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>Pasos finales

Compilar y probar la aplicación.

Quite el tipo de proyecto de MVC 4 GUID de los archivos de proyecto.

1. En el Explorador de soluciones, haga clic en el nombre del proyecto y, a continuación, seleccione **descargar el proyecto**.
2. Haga clic en el proyecto y seleccione Editar nombredelproyecto.csproj.
3. Busque la `ProjectTypeGuids` elemento y, a continuación, GUID, del proyecto de MVC 4 quitar `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.
4. Guarde y cierre el archivo de proyecto abierto.
5. Haga clic en el proyecto y seleccione **recargar el proyecto**.
