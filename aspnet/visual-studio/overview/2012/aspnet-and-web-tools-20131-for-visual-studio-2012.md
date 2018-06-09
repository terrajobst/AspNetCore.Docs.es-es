---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Notas de la versión de ASP.NET y herramientas Web 2013.1 para Visual Studio 2012 | Documentos de Microsoft
author: microsoft
description: Este documento describe la versión de ASP.NET y 2013.1 de herramientas Web para Visual Studio 2012.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: c11e2ef9c33b0cae1f196690533094ce1c342da5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036432"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="8b210-103">Notas de la versión de ASP.NET y herramientas Web 2013.1 para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="8b210-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>
====================
<span data-ttu-id="8b210-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="8b210-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="8b210-105">Este documento describe la versión de ASP.NET y 2013.1 de herramientas Web para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8b210-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>


## <a name="contents"></a><span data-ttu-id="8b210-106">Contenido</span><span class="sxs-lookup"><span data-stu-id="8b210-106">Contents</span></span>

- [<span data-ttu-id="8b210-107">Notas sobre la instalación</span><span class="sxs-lookup"><span data-stu-id="8b210-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="8b210-108">Requisitos de software</span><span class="sxs-lookup"><span data-stu-id="8b210-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="8b210-109">Nuevas características de ASP.NET y herramientas Web 2013.1 para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="8b210-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="8b210-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="8b210-110">Bootstrap</span></span>](#bootstrap)
    - <span data-ttu-id="8b210-111">[Templates](#templates) (Plantillas [C++])</span><span class="sxs-lookup"><span data-stu-id="8b210-111">[Templates](#templates)</span></span>

        - [<span data-ttu-id="8b210-112">Plantilla de MVC de ASP.NET 5</span><span class="sxs-lookup"><span data-stu-id="8b210-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="8b210-113">Plantilla ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="8b210-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="8b210-114">Plantillas de elementos</span><span class="sxs-lookup"><span data-stu-id="8b210-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="8b210-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8b210-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="8b210-116">Scaffolding de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8b210-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="8b210-117">Editor de Razor</span><span class="sxs-lookup"><span data-stu-id="8b210-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="8b210-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="8b210-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="8b210-119">Problemas conocidos y los cambios recientes</span><span class="sxs-lookup"><span data-stu-id="8b210-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="8b210-120">Scaffolding de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8b210-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="8b210-121">MVC y Web API Scaffolding - 404 de HTTP, no se encontró</span><span class="sxs-lookup"><span data-stu-id="8b210-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="8b210-122">Visual Studio Express 2012 for Web deja de funcionar después de agregar un elemento con scaffolding</span><span class="sxs-lookup"><span data-stu-id="8b210-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="8b210-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="8b210-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="8b210-124">Ver archivo cshtml explorar con ni F5 provoca un error de servidor</span><span class="sxs-lookup"><span data-stu-id="8b210-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="8b210-125">Tilde(~) y reescritura de dirección Url</span><span class="sxs-lookup"><span data-stu-id="8b210-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - <span data-ttu-id="8b210-126">[Templates](#templateissue) (Plantillas [C++])</span><span class="sxs-lookup"><span data-stu-id="8b210-126">[Templates](#templateissue)</span></span>

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="8b210-127">Notas sobre la instalación</span><span class="sxs-lookup"><span data-stu-id="8b210-127">Installation Notes</span></span>

<span data-ttu-id="8b210-128">[Instalar](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) Web ASP.NET y herramientas 2013.1 para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8b210-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="8b210-129">Requisitos de software</span><span class="sxs-lookup"><span data-stu-id="8b210-129">Software Requirements</span></span>

<span data-ttu-id="8b210-130">Debe tener Visual Studio 2012 o Visual Studio Express 2012 para Web.</span><span class="sxs-lookup"><span data-stu-id="8b210-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="8b210-131">Nuevas características de ASP.NET y herramientas Web 2013.1 para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="8b210-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="8b210-132">bootstrap</span><span class="sxs-lookup"><span data-stu-id="8b210-132">Bootstrap</span></span>

<span data-ttu-id="8b210-133">Cuando se aplique la técnica scaffolding MVC 5 controladores y vistas, se utiliza el marcado de las vistas de [arranque](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="8b210-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="8b210-134">Plantillas</span><span class="sxs-lookup"><span data-stu-id="8b210-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="8b210-135">Plantilla de MVC de ASP.NET 5</span><span class="sxs-lookup"><span data-stu-id="8b210-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="8b210-136">Hemos agregado una nueva plantilla de MVC 5.</span><span class="sxs-lookup"><span data-stu-id="8b210-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="8b210-137">Hace referencia a los paquetes de NuGet de MVC 5 más recientes, y puede usar el scaffolding para agregar controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="8b210-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="8b210-138">Plantilla ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="8b210-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="8b210-139">Hemos agregado una nueva plantilla de API Web 2.</span><span class="sxs-lookup"><span data-stu-id="8b210-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="8b210-140">Hace referencia a los paquetes de NuGet para Web API 2 más recientes, y puede usar el scaffolding para agregar controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="8b210-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="8b210-141">Plantillas de elementos</span><span class="sxs-lookup"><span data-stu-id="8b210-141">Item Templates</span></span>

<span data-ttu-id="8b210-142">Hemos agregado nuevas plantillas de elemento para vistas de MVC 5, las páginas Web (Razor 3) y controladores de API Web 2.</span><span class="sxs-lookup"><span data-stu-id="8b210-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="8b210-143">Instalar los paquetes de NuGet relacionados al proyecto al agregar nuevos elementos.</span><span class="sxs-lookup"><span data-stu-id="8b210-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="8b210-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8b210-144">Entity Framework 6</span></span>

<span data-ttu-id="8b210-145">Cuando se aplique la técnica scaffolding un controlador de MVC o Web API con Entity Framework, usamos Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8b210-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="8b210-146">Para obtener más información acerca de Entity Framework, vea la [historial de versiones de Entity Framework](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="8b210-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="8b210-147">También puede descargar e instalar las herramientas de Entity Framework 6 para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8b210-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="8b210-148">Consulte la [obtener Entity Framework](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="8b210-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="8b210-149">Scaffolding de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8b210-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="8b210-150">La técnica de Scaffolding de ASP.NET es un marco de trabajo de generación de código para aplicaciones Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8b210-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="8b210-151">Resulta muy sencillo agregar código reutilizable en el proyecto que interactúa con un modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="8b210-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="8b210-152">En versiones anteriores de Visual Studio, scaffolding estaba limitado a los proyectos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8b210-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="8b210-153">Con esta actualización, ahora puede usar el scaffolding para cualquier proyecto de ASP.NET, incluidos los formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="8b210-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="8b210-154">Esta actualización no admite la generación de páginas para un proyecto de formularios Web Forms, pero todavía puede usar scaffolding con formularios Web Forms mediante la adición de las dependencias de MVC al proyecto.</span><span class="sxs-lookup"><span data-stu-id="8b210-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="8b210-155">Se agregará compatibilidad para generar páginas de formularios Web Forms en una futura actualización.</span><span class="sxs-lookup"><span data-stu-id="8b210-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="8b210-156">Cuando se utiliza la técnica scaffolding, es asegurarse de que las necesarias están instaladas las dependencias en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="8b210-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="8b210-157">Por ejemplo, si empieza con un proyecto de formularios Web Forms de ASP.NET y, a continuación, usar el scaffolding para agregar un controlador de API Web, los paquetes de NuGet necesarios y las referencias se agregan al proyecto automáticamente.</span><span class="sxs-lookup"><span data-stu-id="8b210-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="8b210-158">Para agregar scaffolding de MVC a un proyecto de formularios Web Forms, agregue un **nuevo elemento de scaffolding** y seleccione **las dependencias de MVC 5** en el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="8b210-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="8b210-159">Hay dos opciones de scaffolding MVC; Mínimo y completa.</span><span class="sxs-lookup"><span data-stu-id="8b210-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="8b210-160">Si selecciona mínimo, solo los paquetes de NuGet y referencias para ASP.NET MVC se agregan al proyecto.</span><span class="sxs-lookup"><span data-stu-id="8b210-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="8b210-161">Si selecciona la opción completa, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto de MVC.</span><span class="sxs-lookup"><span data-stu-id="8b210-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="8b210-162">Compatibilidad con scaffolding controladores asincrónicos usa las nuevas características asincrónicas de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8b210-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="8b210-163">Para obtener más información y ver tutoriales, vea [información general sobre la técnica Scaffolding de ASP.NET](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8b210-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="8b210-164">Estos tutoriales muestran scaffolding con Visual Studio 2013, pero también son aplicables a ASP.NET y 2013.1 de herramientas Web para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8b210-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="8b210-165">Editor de Razor</span><span class="sxs-lookup"><span data-stu-id="8b210-165">Razor Editor</span></span>

<span data-ttu-id="8b210-166">Con esta actualización, Visual Studio 2012 ahora admite herramientas de Razor 3 o editar.</span><span class="sxs-lookup"><span data-stu-id="8b210-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="8b210-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="8b210-167">NuGet 2.7</span></span>

<span data-ttu-id="8b210-168">NuGet 2.7 incluye un amplio conjunto de nuevas características que se describen en detalle en [notas de la versión 2.7 NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="8b210-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="8b210-169">Esta versión de NuGet elimina la necesidad de los usuarios permitir explícitamente NuGet para restaurar los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="8b210-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="8b210-170">Al instalar NuGet 2.7, los usuarios consentir implícitamente a restaurar automáticamente los paquetes que falten.</span><span class="sxs-lookup"><span data-stu-id="8b210-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="8b210-171">Los usuarios pueden excluirse explícitamente restauración del paquete a través de la configuración de NuGet en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8b210-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="8b210-172">Este cambio simplifica el funcionamiento de la restauración del paquete.</span><span class="sxs-lookup"><span data-stu-id="8b210-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="8b210-173">Problemas conocidos y los cambios recientes</span><span class="sxs-lookup"><span data-stu-id="8b210-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="8b210-174">Scaffolding de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8b210-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="8b210-175">MVC y Web API Scaffolding - 404 de HTTP, no se encontró</span><span class="sxs-lookup"><span data-stu-id="8b210-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="8b210-176">Si se produce un error al agregar un elemento con scaffolding para un proyecto, es posible que el proyecto se quedará en un estado incoherente.</span><span class="sxs-lookup"><span data-stu-id="8b210-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="8b210-177">Algunos de los cambios realizados pueden scaffolding se revertirá pero otros cambios, como los paquetes de NuGet instalados, no se revertirá.</span><span class="sxs-lookup"><span data-stu-id="8b210-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="8b210-178">Si se deshacen los cambios de configuración de enrutamiento, los usuarios recibirán un error HTTP 404 cuando vaya a elementos de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8b210-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="8b210-179">Para corregir este error para MVC, agregar un nuevo elemento con scaffolding y seleccionar las dependencias de MVC 5 (básica o completa).</span><span class="sxs-lookup"><span data-stu-id="8b210-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="8b210-180">Este proceso agregará todos los cambios necesarios al proyecto.</span><span class="sxs-lookup"><span data-stu-id="8b210-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="8b210-181">Para corregir este error para la API Web:</span><span class="sxs-lookup"><span data-stu-id="8b210-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="8b210-182">Agregue la siguiente clase WebApiConfig al proyecto.</span><span class="sxs-lookup"><span data-stu-id="8b210-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="8b210-183">Configure WebApiConfig.Register en la aplicación\_Start (método) en Global.asax, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="8b210-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="8b210-184">Visual Studio Express 2012 for Web deja de funcionar después de agregar un elemento con scaffolding</span><span class="sxs-lookup"><span data-stu-id="8b210-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="8b210-185">Si Visual Studio Express 2012 para Web deja de funcionar después de agregar el elemento con scaffolding con Entity Framework (por ejemplo, Web API 2 controlador con acciones que usan Entity Framework), es posible que Visual Studio Express no se pudo cargar la imagen nativa de un ensamblado dependiente de System.Web.Extensions.</span><span class="sxs-lookup"><span data-stu-id="8b210-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="8b210-186">Para corregir este problema, configure Visual Studio Express para trabajar con la imagen de MSIL de System.Web.Extensions:</span><span class="sxs-lookup"><span data-stu-id="8b210-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="8b210-187">Abra el símbolo del sistema en el modo de administrador.</span><span class="sxs-lookup"><span data-stu-id="8b210-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="8b210-188">Vaya a %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE o % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (para Windows de 64 bits).</span><span class="sxs-lookup"><span data-stu-id="8b210-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="8b210-189">Abra VWDExpress.exe.config en un editor de texto.</span><span class="sxs-lookup"><span data-stu-id="8b210-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="8b210-190">Agregue la siguiente línea en el &lt;configuración&gt;/&lt;en tiempo de ejecución&gt; elemento:</span><span class="sxs-lookup"><span data-stu-id="8b210-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="8b210-191">Reinicie Visual Studio Express 2012 para Web.</span><span class="sxs-lookup"><span data-stu-id="8b210-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="8b210-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="8b210-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a><span data-ttu-id="8b210-193">Ver cshtml archivo withBrowse WithorF5causes un error de servidor</span><span class="sxs-lookup"><span data-stu-id="8b210-193">Viewing cshtml file withBrowse WithorF5causes a server error</span></span>

<span data-ttu-id="8b210-194">Al crear un proyecto de MVC 5 en Visual Studio 2012 (o abrir en el proyecto de Visual Studio 2012 un 5 de MVC que creó en Visual Studio 2013) e intenta ver un archivo cshtml mediante explorar con o F5, recibirá un error que indica - **Error de servidor en La aplicación '/'**.</span><span class="sxs-lookup"><span data-stu-id="8b210-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="8b210-195">El servidor intenta desplazarse a `http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="8b210-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="8b210-196">Para resolver este problema, cambie la **acción de inicio** en el proyecto **página específica**.</span><span class="sxs-lookup"><span data-stu-id="8b210-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="8b210-197">No es necesario proporcionar un valor para la página.</span><span class="sxs-lookup"><span data-stu-id="8b210-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="8b210-198">Después de realizar este cambio, al seleccionar F5 navega a la raíz de la aplicación (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="8b210-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="8b210-199">Este comportamiento no es el mismo que el comportamiento para los proyectos de MVC 5 en Visual Studio 2013, donde el **página actual** configuración inicia la página abierta.</span><span class="sxs-lookup"><span data-stu-id="8b210-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="8b210-200">Tilde(~) y reescritura de dirección Url</span><span class="sxs-lookup"><span data-stu-id="8b210-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="8b210-201">Después de actualizar a ASP.NET Razor 3 o 5 de MVC de ASP.NET, la notación de tilde(~) ya no funcionen correctamente si utilizas una dirección URL de escribir de nuevo.</span><span class="sxs-lookup"><span data-stu-id="8b210-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="8b210-202">La reescritura de direcciones URL afecta a la notación de tilde(~) en los elementos HTML como &lt;A /&gt;, &lt;secuencia de comandos /&gt;, &lt;vínculo /&gt;, y así la tilde ya no se asigna al directorio raíz.</span><span class="sxs-lookup"><span data-stu-id="8b210-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="8b210-203">Por ejemplo, si vuelve a escribir las solicitudes de **asp.net/content** a **asp.net**, el atributo href de &lt;A href = "~/content/" /&gt; se resuelve como **/content/ contenido /** en lugar de **/**.</span><span class="sxs-lookup"><span data-stu-id="8b210-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="8b210-204">Para suprimir este cambio, puede establecer la **IIS\_WasUrlRewritten** contexto en false en cada página Web o en **aplicación\_BeginRequest** en Global.asax.</span><span class="sxs-lookup"><span data-stu-id="8b210-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="8b210-205">Plantillas</span><span class="sxs-lookup"><span data-stu-id="8b210-205">Templates</span></span>

<span data-ttu-id="8b210-206">Cuando creas ASP.NET MVC proyectos con Visual Studio 2012 en Windows 8.1 o Windows Server 2012 R2, Visual Studio muestran un mensaje de error que indica "Configuración Web [url] para ASP.NET 4.5 no se pudo".</span><span class="sxs-lookup"><span data-stu-id="8b210-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![error de configuración](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="8b210-208">Verá este error porque Visual Studio 2012 no habilita la característica ASP.NET 4.5 cuando se instala en esas versiones de Windows.</span><span class="sxs-lookup"><span data-stu-id="8b210-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="8b210-209">Para habilitar ASP.NET 4.5, siga los pasos descritos en [o desactivar las características de Windows Active](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="8b210-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![Activar o desactivar las características de Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="8b210-211">Como alternativa, puede habilitar ASP.NET 4.5 a través de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="8b210-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="8b210-212">Abra el símbolo del sistema en el modo de administrador.</span><span class="sxs-lookup"><span data-stu-id="8b210-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="8b210-213">Ejecute el comando siguiente para habilitar ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="8b210-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
