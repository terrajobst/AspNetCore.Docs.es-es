---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Trabajar con SSL en API Web | Microsoft Docs
author: MikeWasson
description: Muestra cómo usar SSL con ASP.NET Web API, incluido el uso de certificados de cliente SSL.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4e14607f11fcd376b4ceca4bf7862259abe015e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396635"
---
<a name="working-with-ssl-in-web-api"></a><span data-ttu-id="ef3dd-103">Trabajar con SSL en Web API</span><span class="sxs-lookup"><span data-stu-id="ef3dd-103">Working with SSL in Web API</span></span>
====================
<span data-ttu-id="ef3dd-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ef3dd-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ef3dd-105">Varios esquemas de autenticación habituales no son seguras a través de HTTP sin formato.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="ef3dd-106">En concreto, la autenticación básica y autenticación mediante formularios envían credenciales no cifradas.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="ef3dd-107">Para ser seguro, estos esquemas de autenticación *debe* utilizar SSL.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="ef3dd-108">Además, se pueden usar certificados de cliente SSL para autenticar a los clientes.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="ef3dd-109">Habilitación de SSL en el servidor</span><span class="sxs-lookup"><span data-stu-id="ef3dd-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="ef3dd-110">Para configurar SSL en IIS 7 o posterior:</span><span class="sxs-lookup"><span data-stu-id="ef3dd-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="ef3dd-111">Crear u obtener un certificado.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-111">Create or get a certificate.</span></span> <span data-ttu-id="ef3dd-112">Para las pruebas, puede crear un certificado autofirmado.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="ef3dd-113">Agregar un enlace HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="ef3dd-114">Para obtener más información, consulte [cómo configurar SSL en IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="ef3dd-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="ef3dd-115">Para realizar pruebas locales, puede habilitar SSL en IIS Express de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="ef3dd-116">En la ventana Propiedades, establezca **SSL habilitado** a **True**.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="ef3dd-117">Tenga en cuenta el valor de **dirección URL de SSL**; use esta dirección URL para probar las conexiones HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="ef3dd-118">Exigir SSL en un controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="ef3dd-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="ef3dd-119">Si tiene un enlace HTTP y HTTPS, los clientes aún pueden usar HTTP para acceder al sitio.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="ef3dd-120">Puede permitir que algunos recursos estén disponibles a través de HTTP, mientras que otros recursos requieren SSL.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="ef3dd-121">En ese caso, utilice un filtro de acción para requerir SSL para los recursos protegidos.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="ef3dd-122">El código siguiente muestra un filtro de autenticación de API Web que busca SSL:</span><span class="sxs-lookup"><span data-stu-id="ef3dd-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="ef3dd-123">Agregue este filtro para las acciones de API Web que requieran SSL:</span><span class="sxs-lookup"><span data-stu-id="ef3dd-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="ef3dd-124">Certificados de cliente SSL</span><span class="sxs-lookup"><span data-stu-id="ef3dd-124">SSL Client Certificates</span></span>

<span data-ttu-id="ef3dd-125">SSL proporciona autenticación mediante certificados de infraestructura de clave pública.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="ef3dd-126">El servidor debe proporcionar un certificado para autenticar el servidor al cliente.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="ef3dd-127">Es menos común para el cliente proporcionar un certificado al servidor, pero se trata de una opción para autenticar los clientes.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="ef3dd-128">Para usar certificados de cliente con SSL, necesita una forma de distribuir los certificados autofirmados a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="ef3dd-129">Para muchos tipos de aplicaciones, esto no será una buena experiencia del usuario, pero en algunos entornos (por ejemplo, enterprise) puede ser factible.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="ef3dd-130">Ventajas</span><span class="sxs-lookup"><span data-stu-id="ef3dd-130">Advantages</span></span> | <span data-ttu-id="ef3dd-131">Desventajas</span><span class="sxs-lookup"><span data-stu-id="ef3dd-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="ef3dd-132">-Credenciales de certificado son más seguras que el nombre de usuario/contraseña.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="ef3dd-133">-SSL proporciona un canal seguro completando, con autenticación, integridad del mensaje y el cifrado de mensajes.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="ef3dd-134">-Debe obtener y administrar certificados PKI.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="ef3dd-135">-La plataforma de cliente debe admitir los certificados de cliente SSL.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="ef3dd-136">Para configurar IIS para que acepte certificados de cliente, abra el Administrador de IIS y realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="ef3dd-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="ef3dd-137">Haga clic en el nodo de sitio en la vista de árbol.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="ef3dd-138">Haga doble clic en el **configuración SSL** característica en el panel central.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="ef3dd-139">En **certificados de cliente**, seleccione una de estas opciones:</span><span class="sxs-lookup"><span data-stu-id="ef3dd-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="ef3dd-140">**Aceptar**: IIS aceptará un certificado desde el cliente, pero no requiere una.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="ef3dd-141">**Requerir**: requieren un certificado de cliente.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="ef3dd-142">(Para habilitar esta opción, también debe seleccionar "Requerir SSL")</span><span class="sxs-lookup"><span data-stu-id="ef3dd-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="ef3dd-143">También puede establecer estas opciones en el archivo ApplicationHost.config:</span><span class="sxs-lookup"><span data-stu-id="ef3dd-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="ef3dd-144">El **SslNegotiateCert** marca significa IIS aceptará un certificado desde el cliente, pero no requiere una (equivalente a la opción "Aceptar" en el Administrador de IIS).</span><span class="sxs-lookup"><span data-stu-id="ef3dd-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="ef3dd-145">Para solicitar un certificado, establezca el **SslRequireCert** marca.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="ef3dd-146">Para las pruebas, también puede establecer estas opciones en IIS Express, en el applicationhost local. Archivo de configuración, ubicado en "Documents\IISExpress\config".</span><span class="sxs-lookup"><span data-stu-id="ef3dd-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="ef3dd-147">Creación de un certificado de cliente de prueba</span><span class="sxs-lookup"><span data-stu-id="ef3dd-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="ef3dd-148">Con fines de prueba, puede usar [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) para crear un certificado de cliente.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-148">For testing purposes, you can use [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) to create a client certificate.</span></span> <span data-ttu-id="ef3dd-149">En primer lugar, cree una entidad emisora raíz de prueba:</span><span class="sxs-lookup"><span data-stu-id="ef3dd-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="ef3dd-150">Makecert le solicitará que escriba una contraseña para la clave privada.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="ef3dd-151">A continuación, agregue el certificado a la prueba del servidor "de confianza raíz almacén entidades de certificación", como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="ef3dd-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="ef3dd-152">Abra MMC.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-152">Open MMC.</span></span>
2. <span data-ttu-id="ef3dd-153">En **archivo**, seleccione **Add/Remove Snap-In**.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="ef3dd-154">Seleccione **cuenta de equipo**.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="ef3dd-155">Seleccione **ordenador** y complete el asistente.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="ef3dd-156">En el panel de navegación, expanda el nodo de "Entidades de certificación de raíz de confianza".</span><span class="sxs-lookup"><span data-stu-id="ef3dd-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="ef3dd-157">En el **acción** menú, elija **todas las tareas**y, a continuación, haga clic en **importación** para iniciar el Asistente para importación de certificados.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="ef3dd-158">Busque el archivo de certificado, TempCA.cer.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="ef3dd-159">Haga clic en **abierto**, a continuación, haga clic en **siguiente** y complete el asistente.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="ef3dd-160">(Se le indicará que vuelva a escribir la contraseña.)</span><span class="sxs-lookup"><span data-stu-id="ef3dd-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="ef3dd-161">Ahora cree un certificado de cliente que está firmado por el primer certificado:</span><span class="sxs-lookup"><span data-stu-id="ef3dd-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="ef3dd-162">Uso de certificados de cliente en API Web</span><span class="sxs-lookup"><span data-stu-id="ef3dd-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="ef3dd-163">En el servidor, puede obtener el certificado de cliente mediante una llamada a [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) en el mensaje de solicitud.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="ef3dd-164">El método devuelve null si no hay ningún certificado de cliente.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="ef3dd-165">De lo contrario, devuelve un **X509Certificate2** instancia.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="ef3dd-166">Utilice este objeto para obtener información del certificado, como el emisor y el asunto.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="ef3dd-167">A continuación, puede usar esta información para la autenticación o autorización.</span><span class="sxs-lookup"><span data-stu-id="ef3dd-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
