---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: "Habilitar la autenticación de Windows en Katana | Documentos de Microsoft"
author: MikeWasson
description: "Este artículo muestra cómo habilitar la autenticación de Windows en Katana. Incluye dos escenarios: usar IIS al host de Katana y usar HttpListener para autohospedaje Kat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: cc23a053fb1ba60ea84eca59e99f0e375fefc4cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="76d28-104">Habilitar la autenticación de Windows en Katana</span><span class="sxs-lookup"><span data-stu-id="76d28-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="76d28-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="76d28-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="76d28-106">Este artículo muestra cómo habilitar la autenticación de Windows en Katana.</span><span class="sxs-lookup"><span data-stu-id="76d28-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="76d28-107">Incluye dos escenarios: usar IIS al host de Katana y usar HttpListener para autohospedaje Katana en un proceso personalizado.</span><span class="sxs-lookup"><span data-stu-id="76d28-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="76d28-108">Gracias a Barry Dorrans, David Matson y Chris Ross de revisión de este artículo.</span><span class="sxs-lookup"><span data-stu-id="76d28-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="76d28-109">Katana es la implementación de Microsoft [OWIN](http://owin.org/), la interfaz Web abierta para. NET.</span><span class="sxs-lookup"><span data-stu-id="76d28-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="76d28-110">Puede leer una introducción a OWIN y Katana [aquí](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="76d28-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="76d28-111">La arquitectura OWIN consta de varios niveles:</span><span class="sxs-lookup"><span data-stu-id="76d28-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="76d28-112">Host: Administra el proceso en el que se ejecuta la canalización OWIN.</span><span class="sxs-lookup"><span data-stu-id="76d28-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="76d28-113">Servidor: Abre un socket de red y escucha las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="76d28-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="76d28-114">Middleware: Procesa la solicitud y respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="76d28-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="76d28-115">Katana actualmente proporciona dos servidores, los cuales admiten la autenticación integrada de Windows:</span><span class="sxs-lookup"><span data-stu-id="76d28-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="76d28-116">**Microsoft.Owin.Host.SystemWeb**.</span><span class="sxs-lookup"><span data-stu-id="76d28-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="76d28-117">Usa IIS con la canalización ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="76d28-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="76d28-118">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="76d28-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="76d28-119">Usa [System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="76d28-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="76d28-120">Este servidor actualmente es la opción predeterminada cuando se hospeda a sí mismo Katana.</span><span class="sxs-lookup"><span data-stu-id="76d28-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="76d28-121">Katana no actualmente proporciona middleware de OWIN para la autenticación de Windows, porque esta funcionalidad ya está disponible en los servidores.</span><span class="sxs-lookup"><span data-stu-id="76d28-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>


## <a name="windows-authentication-in-iis"></a><span data-ttu-id="76d28-122">Autenticación de Windows en IIS</span><span class="sxs-lookup"><span data-stu-id="76d28-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="76d28-123">Con Microsoft.Owin.Host.SystemWeb, simplemente puede habilitar la autenticación de Windows en IIS.</span><span class="sxs-lookup"><span data-stu-id="76d28-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="76d28-124">Empecemos creando una nueva aplicación de ASP.NET, mediante la plantilla de proyecto de "Aplicación Web ASP.NET vacía".</span><span class="sxs-lookup"><span data-stu-id="76d28-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="76d28-125">A continuación, agregar paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="76d28-125">Next, add NuGet packages.</span></span> <span data-ttu-id="76d28-126">Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="76d28-126">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="76d28-127">En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="76d28-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="76d28-128">Ahora agregue una clase denominada `Startup` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="76d28-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="76d28-129">Que es todo lo que necesita para crear una aplicación "Hello world" para OWIN, que se ejecuta en IIS.</span><span class="sxs-lookup"><span data-stu-id="76d28-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="76d28-130">Presione F5 para depurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="76d28-130">Press F5 to debug the application.</span></span> <span data-ttu-id="76d28-131">Debería ver "¡Hello World!"</span><span class="sxs-lookup"><span data-stu-id="76d28-131">You should see "Hello World!"</span></span> <span data-ttu-id="76d28-132">en la ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="76d28-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="76d28-133">A continuación, se habilitará la autenticación de Windows en IIS Express.</span><span class="sxs-lookup"><span data-stu-id="76d28-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="76d28-134">Desde el **vista** menú, seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="76d28-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="76d28-135">Haga clic en el nombre del proyecto en el Explorador de soluciones para ver las propiedades del proyecto.</span><span class="sxs-lookup"><span data-stu-id="76d28-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="76d28-136">En el **propiedades** ventana, establezca **autenticación anónima** a **deshabilitado** y establecer **autenticación de Windows** a  **Habilitado**.</span><span class="sxs-lookup"><span data-stu-id="76d28-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="76d28-137">Al ejecutar la aplicación desde Visual Studio, IIS Express requiere credenciales de Windows del usuario.</span><span class="sxs-lookup"><span data-stu-id="76d28-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="76d28-138">Puede ver mediante el uso de [Fiddler](http://fiddler2.com/home) o HTTP otra herramienta de depuración.</span><span class="sxs-lookup"><span data-stu-id="76d28-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="76d28-139">Este es un ejemplo de respuesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="76d28-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="76d28-140">Los encabezados WWW-Authenticate en esta respuesta indican que el servidor admite la [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocolo, que se usa Kerberos o NTLM.</span><span class="sxs-lookup"><span data-stu-id="76d28-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="76d28-141">Más adelante, cuando se implementa la aplicación en un servidor, realice [estos pasos](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) para habilitar la autenticación de Windows en IIS en ese servidor.</span><span class="sxs-lookup"><span data-stu-id="76d28-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="76d28-142">Autenticación de Windows en HttpListener</span><span class="sxs-lookup"><span data-stu-id="76d28-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="76d28-143">Si usas Microsoft.Owin.Host.HttpListener para auto-hospedar Katana, puede habilitar la autenticación de Windows directamente en el **HttpListener** instancia.</span><span class="sxs-lookup"><span data-stu-id="76d28-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="76d28-144">En primer lugar, cree una nueva aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="76d28-144">First, create a new console application.</span></span> <span data-ttu-id="76d28-145">A continuación, agregar paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="76d28-145">Next, add NuGet packages.</span></span> <span data-ttu-id="76d28-146">Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="76d28-146">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="76d28-147">En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="76d28-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="76d28-148">Ahora agregue una clase denominada `Startup` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="76d28-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="76d28-149">Esta clase implementa el mismo ejemplo de "Hola a todos" de antes, pero también establece la autenticación de Windows como el esquema de autenticación.</span><span class="sxs-lookup"><span data-stu-id="76d28-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="76d28-150">Dentro de la `Main` funcionar, iniciar la canalización OWIN:</span><span class="sxs-lookup"><span data-stu-id="76d28-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="76d28-151">Puede enviar una solicitud de Fiddler para confirmar que la aplicación utiliza la autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="76d28-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="76d28-152">Temas relacionados</span><span class="sxs-lookup"><span data-stu-id="76d28-152">Related Topics</span></span>

[<span data-ttu-id="76d28-153">Información general del proyecto Katana</span><span class="sxs-lookup"><span data-stu-id="76d28-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="76d28-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="76d28-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx)

[<span data-ttu-id="76d28-155">Descripción de la autenticación de formularios OWIN en MVC 5</span><span class="sxs-lookup"><span data-stu-id="76d28-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
