---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: "Llamar a API Web desde un Windows Phone 8 aplicación (C#) | Documentos de Microsoft"
author: rmcmurray
description: "Crear un escenario completo de extremo a extremo que consta de una aplicación de ASP.NET Web API que proporciona un catálogo de libros a una aplicación de Windows Phone 8."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 2025f31f369153b93cd293884880c97635fc8ab8
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2018
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="6ddeb-103">Llamar a API Web desde una aplicación de Windows Phone 8 (C#)</span><span class="sxs-lookup"><span data-stu-id="6ddeb-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>
====================
<span data-ttu-id="6ddeb-104">por [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="6ddeb-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="6ddeb-105">En este tutorial, obtendrá información sobre cómo crear un escenario completo de extremo a extremo que consta de una aplicación de ASP.NET Web API que proporciona un catálogo de libros a una aplicación de Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="6ddeb-106">Información general</span><span class="sxs-lookup"><span data-stu-id="6ddeb-106">Overview</span></span>

<span data-ttu-id="6ddeb-107">Los servicios rESTful como ASP.NET Web API simplifican la creación de aplicaciones basadas en HTTP para los desarrolladores mediante la abstracción de la arquitectura para aplicaciones de cliente y servidor.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="6ddeb-108">En lugar de crear un protocolo propietario basado en el socket para la comunicación, los desarrolladores de Web API basta publicar los métodos HTTP necesarios para su aplicación, (por ejemplo: GET, POST, PUT, DELETE), y los desarrolladores de aplicaciones de cliente solo deben consumir los métodos HTTP que son necesarios para su aplicación.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="6ddeb-109">En este tutorial to-end, obtendrá información sobre cómo usar Web API para crear los siguientes proyectos:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="6ddeb-110">En el [primera parte de este tutorial](#STEP1), creará una aplicación de ASP.NET Web API que admite todas las operaciones de creación, lectura, actualización y eliminación (CRUD) para administrar un catálogo de libros.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="6ddeb-111">Esta aplicación va a utilizar el [ejemplo de archivo XML (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) de MSDN.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="6ddeb-112">En el [segunda parte de este tutorial](#STEP2), creará una aplicación interactiva de Windows Phone 8 que recupera los datos de la aplicación de API Web.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="6ddeb-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="6ddeb-113">Prerequisites</span></span>

- <span data-ttu-id="6ddeb-114">Visual Studio 2013 con instalado el SDK de Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="6ddeb-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="6ddeb-115">Windows 8 o posterior en un sistema de 64 bits con Hyper-V instalado</span><span class="sxs-lookup"><span data-stu-id="6ddeb-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="6ddeb-116">Para obtener una lista de requisitos adicionales, consulte la *requisitos de sistema de* sección en la [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) página de descarga.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="6ddeb-117">Si va a probar la conectividad entre las API de Web y los proyectos de Windows Phone 8 en el sistema local, debe seguir las instrucciones que aparecen en la  *[conectar el emulador de Windows Phone 8 para las aplicaciones de la API de Web en una variable Local Equipo](https://go.microsoft.com/fwlink/?LinkId=324014)*  artículo que se va a configurar su entorno de pruebas.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="6ddeb-118">Paso 1: Crear el proyecto de librería de API de Web</span><span class="sxs-lookup"><span data-stu-id="6ddeb-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="6ddeb-119">El primer paso de este tutorial to-end es crear un proyecto de API Web que admite todas las operaciones CRUD; Tenga en cuenta que se agregará el proyecto de aplicación de Windows Phone para esta solución en [paso 2](#STEP2) de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="6ddeb-120">Abra **Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="6ddeb-121">Haga clic en **archivo**, a continuación, **nueva**y, a continuación, **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="6ddeb-122">Cuando el **nuevo proyecto** aparece el cuadro de diálogo, expanda **instalado**, a continuación, **plantillas**, a continuación, **Visual C#**y, a continuación, **Web**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>

    | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
    | --- |
    | <span data-ttu-id="6ddeb-123">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="6ddeb-123">Click image to expand</span></span> |
4. <span data-ttu-id="6ddeb-124">Resaltar **aplicación Web ASP.NET**, escriba **BookStore** para el nombre del proyecto y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="6ddeb-125">Cuando el **nuevo proyecto ASP.NET** aparece el cuadro de diálogo, seleccione la **API Web** plantilla y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>

    | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
    | --- |
    | <span data-ttu-id="6ddeb-126">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="6ddeb-126">Click image to expand</span></span> |
6. <span data-ttu-id="6ddeb-127">Cuando se abre el proyecto de API Web, quite el controlador de ejemplo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="6ddeb-128">Expanda el **controladores** carpeta en el Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="6ddeb-129">Haga clic en el **ValuesController.cs** de archivos y, a continuación, haga clic en **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="6ddeb-130">Haga clic en **Aceptar** cuando se le pida que confirme la eliminación.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="6ddeb-131">Agregar un archivo de datos XML al proyecto de Web API; Este archivo contiene el contenido del catálogo de librería:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

    1. <span data-ttu-id="6ddeb-132">Haga clic en el **aplicación\_datos** , a continuación, haga clic en la carpeta en el Explorador de soluciones, **agregar**y, a continuación, haga clic en **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
    2. <span data-ttu-id="6ddeb-133">Cuando el **Agregar nuevo elemento** aparece el cuadro de diálogo, resalte el **archivo XML** plantilla.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
    3. <span data-ttu-id="6ddeb-134">Asignar nombre al archivo **Books.xml**y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-134">Name the file **Books.xml**, and then click **Add**.</span></span>
    4. <span data-ttu-id="6ddeb-135">Cuando el **Books.xml** archivo se abre, reemplace el código en el archivo por el código XML del ejemplo **books.xml** archivo en MSDN:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
    5. <span data-ttu-id="6ddeb-136">Guarde y cierre el archivo XML.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-136">Save and close the XML file.</span></span>
8. <span data-ttu-id="6ddeb-137">Agregar el modelo de librería al proyecto Web API; Este modelo contiene la lógica de creación, lectura, actualización y eliminación (CRUD) para la aplicación de la librería:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

    1. <span data-ttu-id="6ddeb-138">Haga clic en el **modelos** , a continuación, haga clic en la carpeta en el Explorador de soluciones, **agregar**y, a continuación, haga clic en **clase**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
    2. <span data-ttu-id="6ddeb-139">Cuando el **Agregar nuevo elemento** aparece el cuadro de diálogo, un nombre al archivo de clase **BookDetails.cs**y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
    3. <span data-ttu-id="6ddeb-140">Cuando el **BookDetails.cs** archivo se abre, reemplace el código en el archivo con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
    4. <span data-ttu-id="6ddeb-141">Guarde y cierre el **BookDetails.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-141">Save and close the **BookDetails.cs** file.</span></span>
9. <span data-ttu-id="6ddeb-142">Agregue el controlador de librería al proyecto Web API:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-142">Add the bookstore controller to the Web API project:</span></span>

    1. <span data-ttu-id="6ddeb-143">Haga clic en el **controladores** , a continuación, haga clic en la carpeta en el Explorador de soluciones, **agregar**y, a continuación, haga clic en **controlador**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
    2. <span data-ttu-id="6ddeb-144">Cuando el **agregar scaffolding** aparece el cuadro de diálogo, resalte **Web API 2 controlador - vacío**y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
    3. <span data-ttu-id="6ddeb-145">Cuando el **Agregar controlador** se muestra el cuadro de diálogo, asigne el nombre del controlador **BooksController**y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
    4. <span data-ttu-id="6ddeb-146">Cuando el **BooksController.cs** archivo se abre, reemplace el código en el archivo con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
    5. <span data-ttu-id="6ddeb-147">Guarde y cierre el **BooksController.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-147">Save and close the **BooksController.cs** file.</span></span>
10. <span data-ttu-id="6ddeb-148">Genere la aplicación de API Web para comprobar si hay errores.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="6ddeb-149">Paso 2: Agregar el proyecto de catálogo de Windows Phone 8 Bookstore</span><span class="sxs-lookup"><span data-stu-id="6ddeb-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="6ddeb-150">El paso siguiente de este escenario de extremo a extremo es crear la aplicación de catálogo para Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="6ddeb-151">Esta aplicación va a utilizar el *Windows Phone Databound App* plantilla para la interfaz de usuario predeterminada y se utilizará la aplicación de API Web que creó en [paso 1](#STEP1) de este tutorial como el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="6ddeb-152">Haga clic en el **BookStore** solución en el en el Explorador de soluciones, haga clic en **agregar**y, a continuación, **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="6ddeb-153">Cuando el **nuevo proyecto** aparece el cuadro de diálogo, expanda **instalado**, a continuación, **Visual C#**y, a continuación, **de Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="6ddeb-154">Resaltar **Windows Phone Databound App**, escriba **BookCatalog** para el nombre y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="6ddeb-155">Agregue el paquete Json.NET NuGet para la **BookCatalog** proyecto:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="6ddeb-156">Haga clic en **referencias** para el **BookCatalog** del proyecto en el Explorador de soluciones y, a continuación, haga clic en **administrar paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="6ddeb-157">Cuando el **administrar paquetes de NuGet** aparece el cuadro de diálogo, expanda el **en línea** sección y resaltar **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="6ddeb-158">Escriba **Json.NET** en la búsqueda del campo y haga clic en el icono de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="6ddeb-159">Resaltar **Json.NET** en los resultados de búsqueda y, a continuación, haga clic en **instalar**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="6ddeb-160">Cuando haya finalizado la instalación, haga clic en **cerrar**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="6ddeb-161">Agregar el **BookDetails** modelo a la **BookCatalog** proyecto; contiene un modelo genérico de la clase de librería:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

    1. <span data-ttu-id="6ddeb-162">Haga clic en el **BookCatalog** del proyecto en el Explorador de soluciones, haga clic en **agregar**y, a continuación, haga clic en **nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
    2. <span data-ttu-id="6ddeb-163">Nombre de la nueva carpeta **modelos**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-163">Name the new folder **Models**.</span></span>
    3. <span data-ttu-id="6ddeb-164">Haga clic en el **modelos** , a continuación, haga clic en la carpeta en el Explorador de soluciones, **agregar**y, a continuación, haga clic en **clase**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
    4. <span data-ttu-id="6ddeb-165">Cuando el **Agregar nuevo elemento** aparece el cuadro de diálogo, un nombre al archivo de clase **BookDetails.cs**y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
    5. <span data-ttu-id="6ddeb-166">Cuando el **BookDetails.cs** archivo se abre, reemplace el código en el archivo con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
    6. <span data-ttu-id="6ddeb-167">Guarde y cierre el **BookDetails.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-167">Save and close the **BookDetails.cs** file.</span></span>
6. <span data-ttu-id="6ddeb-168">Actualización de la **MainViewModel.cs** clase para que incluya la funcionalidad para comunicarse con la aplicación de API Web de librería:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

    1. <span data-ttu-id="6ddeb-169">Expanda el **ViewModels** carpeta en el Explorador de soluciones y, a continuación, haga doble clic en el **MainViewModel.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
    2. <span data-ttu-id="6ddeb-170">Cuando el **MainViewModel.cs** archivo se abre, reemplace el código en el archivo con el siguiente; tenga en cuenta que debe actualizar el valor de la `apiUrl` constante con la dirección URL real de la API Web:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
    3. <span data-ttu-id="6ddeb-171">Guarde y cierre el **MainViewModel.cs** archivo.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-171">Save and close the **MainViewModel.cs** file.</span></span>
7. <span data-ttu-id="6ddeb-172">Actualización de la **MainPage.xaml** archivo para personalizar el nombre de aplicación:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

    1. <span data-ttu-id="6ddeb-173">Haga doble clic en el **MainPage.xaml** archivo en el Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
    2. <span data-ttu-id="6ddeb-174">Cuando el **MainPage.xaml** archivo se abre, busque las siguientes líneas de código:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
    3. <span data-ttu-id="6ddeb-175">Reemplace las líneas con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-175">Replace those lines with the following:</span></span> 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
    4. <span data-ttu-id="6ddeb-176">Guarde y cierre el **MainPage.xaml** archivo.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-176">Save and close the **MainPage.xaml** file.</span></span>
8. <span data-ttu-id="6ddeb-177">Actualización de la **DetailsPage.xaml** archivo para personalizar los elementos que se muestran:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

    1. <span data-ttu-id="6ddeb-178">Haga doble clic en el **DetailsPage.xaml** archivo en el Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
    2. <span data-ttu-id="6ddeb-179">Cuando el **DetailsPage.xaml** archivo se abre, busque las siguientes líneas de código:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
    3. <span data-ttu-id="6ddeb-180">Reemplace las líneas con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-180">Replace those lines with the following:</span></span> 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
    4. <span data-ttu-id="6ddeb-181">Guarde y cierre el **DetailsPage.xaml** archivo.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-181">Save and close the **DetailsPage.xaml** file.</span></span>
9. <span data-ttu-id="6ddeb-182">Genere la aplicación de Windows Phone para comprobar si hay errores.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="6ddeb-183">Paso 3: Probar la solución End-to-End</span><span class="sxs-lookup"><span data-stu-id="6ddeb-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="6ddeb-184">Como se mencionó en la *requisitos previos* proyectos de la sección de este tutorial, cuando esté probando la conectividad entre la API Web y Windows Phone 8 en el sistema local, debe seguir las instrucciones que aparecen en la  *[ Conectar el emulador de Windows Phone 8 a aplicaciones de API de Web en un equipo Local](https://go.microsoft.com/fwlink/?LinkId=324014)*  artículo que se va a configurar su entorno de pruebas.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="6ddeb-185">Una vez haya configurado el entorno de pruebas, debe establecer la aplicación de Windows Phone como proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="6ddeb-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="6ddeb-186">Para ello, resalte el **BookCatalog** aplicación en el Explorador de soluciones y, a continuación, haga clic en **establecer como proyecto de inicio**:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="6ddeb-187">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="6ddeb-187">Click image to expand</span></span> |

<span data-ttu-id="6ddeb-188">Cuando se presiona F5, Visual Studio iniciará tanto el Windows Phone emulador, que mostrará una &quot;espere&quot; mensaje mientras se recuperan los datos de aplicación a través de la API Web:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="6ddeb-189">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="6ddeb-189">Click image to expand</span></span> |

<span data-ttu-id="6ddeb-190">Si todo se realiza correctamente, debería ver el catálogo de muestra:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="6ddeb-191">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="6ddeb-191">Click image to expand</span></span> |

<span data-ttu-id="6ddeb-192">Si pulsa en cualquier título de libro, la aplicación mostrará la descripción del libro:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="6ddeb-193">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="6ddeb-193">Click image to expand</span></span> |

<span data-ttu-id="6ddeb-194">Si la aplicación no puede comunicarse con la API Web, se mostrará un mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="6ddeb-195">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="6ddeb-195">Click image to expand</span></span> |

<span data-ttu-id="6ddeb-196">Si pulsa en el mensaje de error, se mostrará los detalles adicionales sobre el error:</span><span class="sxs-lookup"><span data-stu-id="6ddeb-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
| --- |
| <span data-ttu-id="6ddeb-197">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="6ddeb-197">Click image to expand</span></span> |
