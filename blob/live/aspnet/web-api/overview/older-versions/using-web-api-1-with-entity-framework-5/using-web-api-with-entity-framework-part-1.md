---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: "Parte 1: Información general y crear el proyecto | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: d76efa2e95c95c91045c7f631040dfff3d4afd2c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="c99a8-102">Parte 1: Información general y crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="c99a8-102">Part 1: Overview and Creating the Project</span></span>
====================
<span data-ttu-id="c99a8-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c99a8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c99a8-104">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="c99a8-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="c99a8-105">Entity Framework es un marco de trabajo de asignación relacional de objetos.</span><span class="sxs-lookup"><span data-stu-id="c99a8-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="c99a8-106">Los objetos de dominio en el código se asigna a las entidades de una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="c99a8-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="c99a8-107">En general, no tiene que preocuparse de la capa de base de datos, dado que Entity Framework se encarga de ello automáticamente.</span><span class="sxs-lookup"><span data-stu-id="c99a8-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="c99a8-108">El código manipula los objetos y cambios se mantienen en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="c99a8-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="c99a8-109">Acerca del Tutorial</span><span class="sxs-lookup"><span data-stu-id="c99a8-109">About the Tutorial</span></span>

<span data-ttu-id="c99a8-110">En este tutorial, creará una aplicación de almacenamiento simple.</span><span class="sxs-lookup"><span data-stu-id="c99a8-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="c99a8-111">Hay dos partes principales para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c99a8-111">There are two main parts to the application.</span></span> <span data-ttu-id="c99a8-112">Los usuarios normales pueden ver productos y crear pedidos:</span><span class="sxs-lookup"><span data-stu-id="c99a8-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="c99a8-113">Los administradores pueden crear, eliminar o editar productos:</span><span class="sxs-lookup"><span data-stu-id="c99a8-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="c99a8-114">Obtendrá información de conocimientos</span><span class="sxs-lookup"><span data-stu-id="c99a8-114">Skills You'll Learn</span></span>

<span data-ttu-id="c99a8-115">Esto es lo aprenderá:</span><span class="sxs-lookup"><span data-stu-id="c99a8-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="c99a8-116">Cómo usar Entity Framework con ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="c99a8-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="c99a8-117">Describe cómo usar knockout.js para crear una interfaz de usuario de cliente dinámico.</span><span class="sxs-lookup"><span data-stu-id="c99a8-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="c99a8-118">Cómo usar la autenticación de formularios con API Web para autenticar a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="c99a8-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="c99a8-119">Aunque este tutorial es independiente entre sí, debe leer primero los siguientes tutoriales:</span><span class="sxs-lookup"><span data-stu-id="c99a8-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="c99a8-120">Su primer ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="c99a8-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="c99a8-121">Crear una API Web que admita operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="c99a8-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="c99a8-122">Algunos conocimientos de [ASP.NET MVC](../../../../mvc/index.md) también es útil.</span><span class="sxs-lookup"><span data-stu-id="c99a8-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="c99a8-123">Información general</span><span class="sxs-lookup"><span data-stu-id="c99a8-123">Overview</span></span>

<span data-ttu-id="c99a8-124">En un nivel alto, ésta es la arquitectura de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="c99a8-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="c99a8-125">ASP.NET MVC genera las páginas HTML para el cliente.</span><span class="sxs-lookup"><span data-stu-id="c99a8-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="c99a8-126">ASP.NET Web API expone operaciones CRUD en los datos (productos y pedidos).</span><span class="sxs-lookup"><span data-stu-id="c99a8-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="c99a8-127">Entity Framework traduce los modelos de C# usados por la API Web en entidades de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c99a8-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="c99a8-128">El diagrama siguiente muestra cómo se representan los objetos de dominio en varias capas de la aplicación: la capa de base de datos, el modelo de objetos y, por último, el formato, que se utiliza para transmitir datos al cliente a través de HTTP.</span><span class="sxs-lookup"><span data-stu-id="c99a8-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="c99a8-129">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c99a8-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="c99a8-130">Puede crear el proyecto de tutorial con Visual Web Developer Express o la versión completa de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c99a8-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="c99a8-131">Desde el **iniciar** página, haga clic en **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="c99a8-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="c99a8-132">En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo.</span><span class="sxs-lookup"><span data-stu-id="c99a8-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="c99a8-133">En **Visual C#**, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="c99a8-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="c99a8-134">En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="c99a8-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="c99a8-135">Denomine el proyecto "ProductStore" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c99a8-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="c99a8-136">En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione **aplicación de Internet** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c99a8-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="c99a8-137">La plantilla de "Aplicación de Internet" crea una aplicación de ASP.NET MVC que admita la autenticación de formularios.</span><span class="sxs-lookup"><span data-stu-id="c99a8-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="c99a8-138">Si se ejecuta la aplicación ahora, ya tiene algunas características:</span><span class="sxs-lookup"><span data-stu-id="c99a8-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="c99a8-139">Pueden registrar los nuevos usuarios, haga clic en el vínculo "Register" en la esquina superior derecha.</span><span class="sxs-lookup"><span data-stu-id="c99a8-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="c99a8-140">Los usuarios registrados pueden iniciar sesión, haga clic en el vínculo "Iniciar sesión".</span><span class="sxs-lookup"><span data-stu-id="c99a8-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="c99a8-141">Información de pertenencia se conserva en una base de datos que se crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="c99a8-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="c99a8-142">Para obtener más información acerca de la autenticación de formularios en ASP.NET MVC, consulte [Tutorial: mediante la autenticación de formularios en ASP.NET MVC](https://msdn.microsoft.com/en-us/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="c99a8-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/en-us/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="c99a8-143">Actualizar el archivo CSS</span><span class="sxs-lookup"><span data-stu-id="c99a8-143">Update the CSS File</span></span>

<span data-ttu-id="c99a8-144">Este paso es cosmético, pero hará que las páginas de representar como las capturas de pantalla anterior.</span><span class="sxs-lookup"><span data-stu-id="c99a8-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="c99a8-145">En el Explorador de soluciones, expanda la carpeta de contenido y abra el archivo denominado Site.css.</span><span class="sxs-lookup"><span data-stu-id="c99a8-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="c99a8-146">Agregue los siguientes estilos CSS:</span><span class="sxs-lookup"><span data-stu-id="c99a8-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

>[!div class="step-by-step"]
[<span data-ttu-id="c99a8-147">Siguiente</span><span class="sxs-lookup"><span data-stu-id="c99a8-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
