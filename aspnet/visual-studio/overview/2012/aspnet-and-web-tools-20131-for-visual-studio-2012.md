---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Notas de la versión de ASP.NET y Web Tools 2013.1 para Visual Studio 2012 | Microsoft Docs
author: microsoft
description: Este documento describe la versión de ASP.NET y Web Tools 2013.1 para Visual Studio 2012.
ms.author: aspnetcontent
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 33577dec7278694f932ac7eded84359baacf65bc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829106"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Notas de la versión de ASP.NET y Web Tools 2013.1 para Visual Studio 2012
====================
por [Microsoft](https://github.com/microsoft)

> Este documento describe la versión de ASP.NET y Web Tools 2013.1 para Visual Studio 2012.


## <a name="contents"></a>Contenido

- [Notas de instalación](#install)
- [Requisitos de software](#requirements)
- Nuevas características de ASP.NET y Web Tools 2013.1 para Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Templates](#templates) (Plantillas [C++])

        - [Plantilla de ASP.NET MVC 5](#mvc5template)
        - [Plantilla ASP.NET Web API 2](#apitemplate)
        - [Plantillas de elemento](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [Scaffolding de ASP.NET](#scaffold)
    - [Razor-Editor](#razor)
    - [NuGet 2.7](#nuget)
- Problemas conocidos y cambios importantes

    - [Scaffolding de ASP.NET](#issuescaffolding)

        - [MVC y Web API Scaffolding - 404 de HTTP, no se encontró](#404issue)
        - [Visual Studio Express 2012 para Web deja de funcionar después de agregar un elemento con scaffolding](#expressissue)
    - [3 de Razor de ASP.NET](#issuerazor)

        - [Ver archivo cshtml explorar con ni F5 produce un error de servidor](#browseissue)
        - [Tilde(~) y reescritura de Url](#rewriteissue)
    - [Templates](#templateissue) (Plantillas [C++])

<a id="install"></a>
## <a name="installation-notes"></a>Notas de instalación

[Instalar](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET y Web Tools 2013.1 para Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

Debe tener Visual Studio 2012 o Visual Studio Express 2012 para Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Nuevas características de ASP.NET y Web Tools 2013.1 para Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap

Al aplicar scaffolding a controladores de MVC 5 y vistas, se usa el marcado para las vistas [Bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Plantillas

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>Plantilla de ASP.NET MVC 5

Se ha agregado una nueva plantilla de MVC 5. Hace referencia a los paquetes de NuGet de MVC 5 más recientes, y puede usar el scaffolding para agregar controladores y vistas.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Plantilla ASP.NET Web API 2

Se ha agregado una nueva plantilla de Web API 2. Hace referencia a los paquetes de NuGet de Web API 2 más recientes, y puede usar el scaffolding para agregar controladores y vistas.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Plantillas de elemento

Hemos agregado nuevas plantillas de elemento para las vistas de MVC 5, Web Pages (Razor 3) y controladores Web API 2. Instalar los paquetes de NuGet relacionados al proyecto al agregar nuevos elementos.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Al aplicar la técnica scaffolding de un controlador de MVC o API Web mediante Entity Framework, usamos Framework 6. Para obtener más información acerca de Entity Framework, vea el [historial de versiones de Entity Framework](https://msdn.com/data/jj574253).

También puede descargar e instalar las herramientas de Entity Framework 6 para Visual Studio 2012. Consulte la [obtener Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding de ASP.NET

Scaffolding de ASP.NET es un marco de generación de código para aplicaciones Web ASP.NET. Resulta muy sencillo agregar código reutilizable para el proyecto que interactúa con un modelo de datos.

En versiones anteriores de Visual Studio, estaba limitado a los proyectos de MVC de ASP.NET scaffolding. Con esta actualización, ahora puede usar el scaffolding para un proyecto ASP.NET, incluidos formularios Web Forms. Esta actualización no admite la generación de páginas para un proyecto de formularios Web Forms, pero todavía puede usar scaffolding con formularios Web Forms mediante la adición de dependencias de MVC al proyecto. Se agregará compatibilidad para generar páginas de formularios Web Forms en una futura actualización.

Cuando se usa la técnica scaffolding, nos aseguramos de que todos los necesarios a las dependencias se instalan en el proyecto. Por ejemplo, si se inicia con un proyecto de formularios Web Forms de ASP.NET y, a continuación, usar el scaffolding para agregar un controlador Web API, los paquetes de NuGet necesarios y las referencias se agregan al proyecto automáticamente.

Para agregar scaffolding de MVC a un proyecto de formularios Web Forms, agregue un **nuevo elemento de scaffolding** y seleccione **dependencias de MVC 5** en la ventana de cuadro de diálogo. Hay dos opciones para scaffolding de MVC; Mínimo y completa. Si selecciona mínima, sólo los paquetes de NuGet y referencias de ASP.NET MVC se agregan al proyecto. Si selecciona la opción completa, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto de MVC.

Compatibilidad con controladores asincrónicos de scaffolding usa las nuevas características asincrónicas de Entity Framework 6.

Para obtener más información y tutoriales, consulte [información general sobre el Scaffolding de ASP.NET](../2013/aspnet-scaffolding-overview.md). Estos tutoriales muestra la técnica scaffolding con Visual Studio 2013, pero también son aplicables a ASP.NET y Web Tools 2013.1 para Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Razor-Editor

Con esta actualización, Visual Studio 2012 ahora es compatible con las herramientas de Razor 3 o editar.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 incluye un amplio conjunto de nuevas características que se describen en detalle en [notas de la versión de NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Esta versión de NuGet elimina la necesidad de los usuarios permitir explícitamente que NuGet para restaurar los paquetes que falten. Al instalar NuGet 2.7, los usuarios de forma implícita da su consentimiento para restaurar automáticamente los paquetes que falten. Los usuarios pueden optar explícitamente la restauración de paquetes a través de la configuración de NuGet en Visual Studio. Este cambio simplifica el funcionamiento de la restauración del paquete.

## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding de ASP.NET

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC y Web API Scaffolding - 404 de HTTP, no se encontró

Si se produce un error al agregar un elemento con scaffolding a un proyecto, es posible que el proyecto se quedará en un estado incoherente. Algunos de los cambios realizados a ser scaffolding se revertirá, pero otros cambios, como los paquetes de NuGet instalados, no se revertirán. Si se revierten los cambios de configuración de enrutamiento, los usuarios recibirán un error HTTP 404 cuando vaya a los elementos de scaffolding.

Para corregir este error para MVC, agregue un nuevo elemento con scaffolding y seleccionar las dependencias de MVC 5 (mínimo o completo). Este proceso se agregará todos los cambios necesarios al proyecto.

Para corregir este error para API Web:

1. Agregue la siguiente clase WebApiConfig al proyecto.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Configure WebApiConfig.Register en la aplicación\_Start (método) en Global.asax como sigue:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 para Web deja de funcionar después de agregar un elemento con scaffolding

Si Visual Studio Express 2012 para Web deja de funcionar después de agregar un elemento con scaffold con Entity Framework (por ejemplo, un controlador Web API 2 con acciones mediante Entity Framework), es posible que Visual Studio Express no se pudo cargar la imagen nativa de un ensamblado depende de System.Web.Extensions.

Para corregir este problema, configure Visual Studio Express para que funcione con la imagen MSIL de System.Web.Extensions:

1. Abra el símbolo del sistema en el modo de administrador.
2. Vaya a %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE o % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (para Windows de 64 bits).
3. Abra VWDExpress.exe.config en un editor de texto.
4. Agregue la siguiente línea en el &lt;configuración&gt;/&lt;en tiempo de ejecución&gt; elemento:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Reinicie Visual Studio Express 2012 para Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>3 de Razor de ASP.NET

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Ver archivo de cshtml withBrowse WithorF5causes un error de servidor

Al crear un proyecto de MVC 5 en Visual Studio 2012 (o abrir en el proyecto de Visual Studio 2012 una MVC 5 que se creó en Visual Studio 2013) e intenta ver un archivo cshtml mediante explorar con o F5, recibirá un error que indica - **Error de servidor en La aplicación '/'**. El servidor intenta navegar a `http://localhost:XXXX/Views/../XXXX.cshtml`

Para resolver este problema, cambie el **acción de inicio** establecer en el proyecto para **página específica**. No es necesario proporcionar un valor para la página.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Después de realizar este cambio, seleccionar F5 navega a la raíz de la aplicación (`http://localhost:XXXX`). Este comportamiento no es el mismo que el comportamiento de los proyectos de MVC 5 en Visual Studio 2013, donde el **página actual** configuración inicia la página abierta.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Tilde(~) y reescritura de Url

Después de actualizar a 3 de Razor de ASP.NET o ASP.NET MVC 5, la notación tilde(~) ya no funcionarán correctamente si está utilizando las operaciones de reescritura de dirección URL. La reescritura de URL afecta a la notación tilde(~) en los elementos HTML como &lt;A /&gt;, &lt;SCRIPT /&gt;, &lt;vínculo /&gt;, y como resultado la tilde ya no se asigna al directorio raíz.

Por ejemplo, si vuelve a escribir las solicitudes de **asp.net/content** a **asp.net**, el atributo href de &lt;un href = "~/content/" /&gt; se resuelve como **/content/ contenido /** en lugar de **/**. Para suprimir este cambio, puede establecer el **IIS\_WasUrlRewritten** contexto a false en cada página Web o en **aplicación\_BeginRequest** en Global.asax.

<a id="templateissue"></a>
### <a name="templates"></a>Plantillas

Cuando creas ASP.NET MVC proyectos con Visual Studio 2012 en Windows 8.1 o Windows Server 2012 R2, Visual Studio muestran un mensaje de error que indica "Configuración Web [url] para ASP.NET 4.5 no se pudo".

![error de configuración](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Vea este error porque Visual Studio 2012 no habilita la característica ASP.NET 4.5 cuando se instala en esas versiones de Windows. Para habilitar ASP.NET 4.5, realice los pasos descritos en [o desactivar las características de Windows Active](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![Activar o desactivar las características de Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Como alternativa, puede habilitar ASP.NET 4.5 a través de la línea de comandos.

1. Abra el símbolo del sistema en el modo de administrador.
2. Ejecute el siguiente comando para habilitar ASP.NET 4.5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
