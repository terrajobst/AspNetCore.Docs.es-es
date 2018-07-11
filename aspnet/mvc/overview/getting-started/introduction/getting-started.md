---
uid: mvc/overview/getting-started/introduction/getting-started
title: Introducción a ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versión actualizada de este tutorial está disponible aquí con Visual Studio 2015. El nuevo tutorial usa ASP.NET Core MVC 6, que proporciona muchas improvem...'
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 85d5d3292ff99ade6995c710e2728c41255def4c
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38211753"
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="0f621-104">Introducción a ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="0f621-104">Getting Started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="0f621-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0f621-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 <span data-ttu-id="0f621-106">Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web de ASP.NET MVC 5 con [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="0f621-106">This tutorial will teach you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> <span data-ttu-id="0f621-107">Origen final para el tutorial se encuentra en [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span><span class="sxs-lookup"><span data-stu-id="0f621-107">Final Source for tutorial located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span></span>


 <span data-ttu-id="0f621-108">En este tutorial se escribió por [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , y [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="0f621-108">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>

 <span data-ttu-id="0f621-109">Necesita una cuenta de Azure para implementar esta aplicación en Azure:</span><span class="sxs-lookup"><span data-stu-id="0f621-109">You need an Azure account to deploy this app to Azure:</span></span>

 - <span data-ttu-id="0f621-110">También puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtenga créditos puede usar para probar los servicios de Azure de pago e incluso después de que se usan hasta, puede mantener la cuenta y usar servicios gratuitos de Azure.</span><span class="sxs-lookup"><span data-stu-id="0f621-110">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
 - <span data-ttu-id="0f621-111">También puede [activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -su suscripción a MSDN le proporciona crédito todos los meses que puede usar para servicios de Azure de pago.</span><span class="sxs-lookup"><span data-stu-id="0f621-111">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>


## <a name="getting-started"></a><span data-ttu-id="0f621-112">Introducción</span><span class="sxs-lookup"><span data-stu-id="0f621-112">Getting Started</span></span>

<span data-ttu-id="0f621-113">Comience por instalar y ejecutar [Visual Studio 2017](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="0f621-113">Start by installing and running [Visual Studio 2017](https://www.visualstudio.com/).</span></span>

<span data-ttu-id="0f621-114">Visual Studio es un entorno de desarrollo integrado o IDE.</span><span class="sxs-lookup"><span data-stu-id="0f621-114">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="0f621-115">Al igual que utiliza Microsoft Word para escribir documentos, usará un IDE para crear aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="0f621-115">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="0f621-116">En Visual Studio, hay una lista en la parte inferior muestra distintas opciones disponibles para usted.</span><span class="sxs-lookup"><span data-stu-id="0f621-116">In Visual Studio there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="0f621-117">También hay un menú que proporciona otra manera de realizar las tareas en el IDE.</span><span class="sxs-lookup"><span data-stu-id="0f621-117">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="0f621-118">(Por ejemplo, en lugar de seleccionar **nuevo proyecto** desde el **iniciar** página, puede usar el menú y seleccione **archivo** &gt; **denuevoproyecto**.)</span><span class="sxs-lookup"><span data-stu-id="0f621-118">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a><span data-ttu-id="0f621-119">Crear su primera aplicación</span><span class="sxs-lookup"><span data-stu-id="0f621-119">Creating Your First Application</span></span>

<span data-ttu-id="0f621-120">Haga clic en **nuevo proyecto**, a continuación, seleccione Visual C# a la izquierda y, a continuación, **Web** y, a continuación, seleccione **aplicación Web ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="0f621-120">Click **New Project**, then select Visual C# on the left, then **Web** and then select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="0f621-121">Nombre del proyecto "MvcMovie" y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0f621-121">Name your project "MvcMovie" and then click **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="0f621-122">En el **nuevo proyecto ASP.NET** cuadro de diálogo, haga clic en **MVC** y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0f621-122">In the **New ASP.NET Project** dialog, click **MVC** and then click **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="0f621-123">Visual Studio usa una plantilla predeterminada para el proyecto de ASP.NET MVC que acaba de crear, por lo que tiene ahora una aplicación de trabajo sin hacer nada!</span><span class="sxs-lookup"><span data-stu-id="0f621-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="0f621-124">Se trata de un simple "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="0f621-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="0f621-125">proyecto y, de un buen lugar para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0f621-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="0f621-126">Haga clic en F5 para iniciar la depuración.</span><span class="sxs-lookup"><span data-stu-id="0f621-126">Click F5 to start debugging.</span></span> <span data-ttu-id="0f621-127">F5 hace que Visual Studio iniciar [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) y ejecutar la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="0f621-127">F5 causes Visual Studio to start [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and run your web app.</span></span> <span data-ttu-id="0f621-128">A continuación, Visual Studio inicia un explorador y abre la página principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0f621-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="0f621-129">Tenga en cuenta que la barra de direcciones del explorador dice `localhost:port#` y no algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0f621-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="0f621-130">Eso es porque `localhost` siempre apunta a su propio equipo local, que en este caso, se ejecuta la aplicación que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="0f621-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="0f621-131">Cuando Visual Studio se ejecuta un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="0f621-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="0f621-132">En la imagen siguiente, el número de puerto es 1234.</span><span class="sxs-lookup"><span data-stu-id="0f621-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="0f621-133">Al ejecutar la aplicación, verá un número de puerto diferente.</span><span class="sxs-lookup"><span data-stu-id="0f621-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="0f621-134">Desde el comienzo esta plantilla predeterminada proporciona páginas principal, contactos y sobre.</span><span class="sxs-lookup"><span data-stu-id="0f621-134">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="0f621-135">No se muestra la imagen anterior el **inicio**, **sobre** y **póngase en contacto con** vínculos.</span><span class="sxs-lookup"><span data-stu-id="0f621-135">The image above doesn't show the **Home**, **About** and **Contact** links.</span></span> <span data-ttu-id="0f621-136">Según el tamaño de la ventana del explorador, es posible que deba haga clic en el icono de navegación para ver estos vínculos.</span><span class="sxs-lookup"><span data-stu-id="0f621-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)  

<span data-ttu-id="0f621-137">La aplicación también proporciona compatibilidad para registrar e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="0f621-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="0f621-138">El siguiente paso es cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0f621-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="0f621-139">Cierre la aplicación de ASP.NET MVC y vamos a cambiar algo de código.</span><span class="sxs-lookup"><span data-stu-id="0f621-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="0f621-140">Para obtener una lista de tutoriales actuales, consulte [MVC artículos recomendado](../mvc-learning-sequence.md).</span><span class="sxs-lookup"><span data-stu-id="0f621-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="0f621-141">Consulte esta aplicación se ejecuta en Azure</span><span class="sxs-lookup"><span data-stu-id="0f621-141">See this App Running on Azure</span></span>

<span data-ttu-id="0f621-142">¿Desea ver el sitio terminado que se ejecuta como una aplicación web en directo?</span><span class="sxs-lookup"><span data-stu-id="0f621-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="0f621-143">Puede implementar una versión completa de la aplicación en su cuenta de Azure, simplemente haga clic en el botón siguiente.</span><span class="sxs-lookup"><span data-stu-id="0f621-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="0f621-144">Necesita una cuenta de Azure para implementar esta solución en Azure.</span><span class="sxs-lookup"><span data-stu-id="0f621-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="0f621-145">Si no dispone de una cuenta, tiene las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="0f621-145">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="0f621-146">[Abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtiene crédito puede usar para probar los servicios de Azure de pago e incluso después de que se usan hasta, puede mantener la cuenta y usar servicios gratuitos de Azure.</span><span class="sxs-lookup"><span data-stu-id="0f621-146">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="0f621-147">[Activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -su suscripción a MSDN le proporciona crédito todos los meses que puede usar para servicios de Azure de pago.</span><span class="sxs-lookup"><span data-stu-id="0f621-147">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0f621-148">Siguiente</span><span class="sxs-lookup"><span data-stu-id="0f621-148">Next</span></span>](adding-a-controller.md)
