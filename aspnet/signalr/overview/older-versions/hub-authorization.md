---
uid: signalr/overview/older-versions/hub-authorization
title: Autenticación y autorización para los concentradores de SignalR (SignalR 1.x) | Documentos de Microsoft
author: pfletcher
description: En este tema se describe cómo restringir qué usuarios o roles pueden tener acceso a métodos de concentrador.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: d73ab6c9091556a62e5d9475baf67a18e305585f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042880"
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="ef5ac-103">Autenticación y autorización para los concentradores de SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="ef5ac-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>
====================
<span data-ttu-id="ef5ac-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ef5ac-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ef5ac-105">En este tema se describe cómo restringir qué usuarios o roles pueden tener acceso a métodos de concentrador.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>


## <a name="overview"></a><span data-ttu-id="ef5ac-106">Información general</span><span class="sxs-lookup"><span data-stu-id="ef5ac-106">Overview</span></span>

<span data-ttu-id="ef5ac-107">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="ef5ac-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="ef5ac-108">Autorizar el atributo</span><span class="sxs-lookup"><span data-stu-id="ef5ac-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="ef5ac-109">Requerir autenticación para todos los centros</span><span class="sxs-lookup"><span data-stu-id="ef5ac-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="ef5ac-110">Autorización personalizada</span><span class="sxs-lookup"><span data-stu-id="ef5ac-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="ef5ac-111">Pasar información de autenticación a los clientes</span><span class="sxs-lookup"><span data-stu-id="ef5ac-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="ef5ac-112">Opciones de autenticación para los clientes de .NET</span><span class="sxs-lookup"><span data-stu-id="ef5ac-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="ef5ac-113">Cookie de autenticación de formularios</span><span class="sxs-lookup"><span data-stu-id="ef5ac-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="ef5ac-114">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="ef5ac-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="ef5ac-115">Encabezado de conexión</span><span class="sxs-lookup"><span data-stu-id="ef5ac-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="ef5ac-116">Certificate</span><span class="sxs-lookup"><span data-stu-id="ef5ac-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="ef5ac-117">Autorizar el atributo</span><span class="sxs-lookup"><span data-stu-id="ef5ac-117">Authorize attribute</span></span>

<span data-ttu-id="ef5ac-118">SignalR proporciona el [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atributo para especificar qué usuarios o roles tienen acceso a un concentrador o un método.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-118">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="ef5ac-119">Este atributo se encuentra en la `Microsoft.AspNet.SignalR` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="ef5ac-120">Aplicar el `Authorize` atributo a un concentrador o determinados métodos en un concentrador.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="ef5ac-121">Al aplicar el `Authorize` atributo a una clase de base de datos central, el requisito de autorización especificado se aplica a todos los métodos en el concentrador.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="ef5ac-122">A continuación, se muestran los distintos tipos de requisitos de autorización que se pueden aplicar.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="ef5ac-123">Sin el `Authorize` atributo, todos los métodos públicos en el concentrador están disponibles para un cliente que está conectado al concentrador.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="ef5ac-124">Si ha definido un rol denominado "Admin" en la aplicación web, puede especificar que sólo los usuarios con ese rol pueden tener acceso a un concentrador con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="ef5ac-125">O bien, puede especificar que un concentrador contiene un método que está disponible para todos los usuarios y un segundo método solo está disponible para los usuarios autenticados, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="ef5ac-126">Los siguientes ejemplos referentes a los escenarios de autorización diferentes:</span><span class="sxs-lookup"><span data-stu-id="ef5ac-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="ef5ac-127">`[Authorize]`: solo los usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="ef5ac-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="ef5ac-128">`[Authorize(Roles = "Admin,Manager")]`: solo autenticados los usuarios de los roles especificados</span><span class="sxs-lookup"><span data-stu-id="ef5ac-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="ef5ac-129">`[Authorize(Users = "user1,user2")]`: solo autenticados los usuarios con los nombres de usuario especificado</span><span class="sxs-lookup"><span data-stu-id="ef5ac-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="ef5ac-130">`[Authorize(RequireOutgoing=false)]`: solo los usuarios autenticados pueden invocar el concentrador, pero las llamadas desde el servidor a los clientes no están limitadas por la autorización, como por ejemplo, cuando solo ciertos usuarios pueden enviar un mensaje, pero todas las demás pueden recibir el mensaje.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="ef5ac-131">La propiedad RequireOutgoing solo puede aplicarse al concentrador completo, no en los métodos de usuarios en el concentrador.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="ef5ac-132">Cuando RequireOutgoing no está establecido en false, solo los usuarios que cumplan los requisitos de autorización se llaman desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="ef5ac-133">Requerir autenticación para todos los centros</span><span class="sxs-lookup"><span data-stu-id="ef5ac-133">Require authentication for all hubs</span></span>

<span data-ttu-id="ef5ac-134">Puede requerir la autenticación para todos los concentradores y métodos de concentrador en la aplicación mediante una llamada a la [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) método cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="ef5ac-135">Puede utilizar este método cuando tiene varios concentradores y desea aplicar un requisito de autenticación para todas ellas.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="ef5ac-136">Con este método, no puede especificar el rol, un usuario o autorización saliente.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="ef5ac-137">Solo se puede especificar que está restringido a los usuarios autenticados acceso a los métodos de concentrador.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="ef5ac-138">Sin embargo, todavía puede aplicar el atributo Authorize a concentradores o métodos para especificar requisitos adicionales.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="ef5ac-139">Cualquier requisito que especifique en los atributos se aplica además del requisito de autenticación básico.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="ef5ac-140">En el ejemplo siguiente se muestra un archivo Global.asax que restringe todos los métodos de concentrador a los usuarios autenticados.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="ef5ac-141">Si se llama a la `RequireAuthentication()` método se ha procesado una solicitud de SignalR, SignalR producirá un `InvalidOperationException` excepción.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="ef5ac-142">Esta excepción se produce porque no se puede agregar un módulo a la HubPipeline después de que se ha invocado la canalización.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="ef5ac-143">El ejemplo anterior muestra que realiza la llamada la `RequireAuthentication` método en el `Application_Start` método que se ejecuta una vez antes de tratar la primera solicitud.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="ef5ac-144">Autorización personalizada</span><span class="sxs-lookup"><span data-stu-id="ef5ac-144">Customized authorization</span></span>

<span data-ttu-id="ef5ac-145">Si necesita personalizar cómo se determina la autorización, puede crear una clase que deriva de `AuthorizeAttribute` e invalide el [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) método.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="ef5ac-146">Se llama a este método para que cada solicitud para determinar si el usuario está autorizado para completar la solicitud.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="ef5ac-147">En el método invalidado, proporcione la lógica necesaria para su escenario de autorización.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="ef5ac-148">En el ejemplo siguiente se muestra cómo aplicar la autorización a través de la identidad basada en notificaciones.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="ef5ac-149">Pasar información de autenticación a los clientes</span><span class="sxs-lookup"><span data-stu-id="ef5ac-149">Pass authentication information to clients</span></span>

<span data-ttu-id="ef5ac-150">Debe usar la información de autenticación en el código que se ejecuta en el cliente.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="ef5ac-151">Pasar la información necesaria al llamar a los métodos en el cliente.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="ef5ac-152">Por ejemplo, un método de aplicación de chat podría pasar como parámetro el nombre de usuario de la persona que publica un mensaje, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="ef5ac-153">O bien, puede crear un objeto para representar la información de autenticación y pasar ese objeto como un parámetro, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="ef5ac-154">No debe pasar nunca Id. de conexión de un cliente a otros clientes, cuando un usuario malintencionado podría utilizarlo para imitar una solicitud de ese cliente.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="ef5ac-155">Opciones de autenticación para los clientes de .NET</span><span class="sxs-lookup"><span data-stu-id="ef5ac-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="ef5ac-156">Si tiene un cliente. NET, como una aplicación de consola que interactúa con un concentrador que está limitado a los usuarios autenticados, puede pasar las credenciales de autenticación en una cookie, el encabezado de conexión o un certificado.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="ef5ac-157">Los ejemplos de esta sección muestran cómo utilizar los distintos métodos para autenticar a un usuario.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="ef5ac-158">No son totalmente funcionales aplicaciones SignalR.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="ef5ac-159">Para obtener más información acerca de los clientes de .NET con SignalR, consulte [Guía de la API de concentradores - cliente .NET](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="ef5ac-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="ef5ac-160">Cookie</span><span class="sxs-lookup"><span data-stu-id="ef5ac-160">Cookie</span></span>

<span data-ttu-id="ef5ac-161">Cuando el cliente .NET interactúa con un concentrador que utiliza la autenticación de formularios de ASP.NET, debe establecer manualmente la cookie de autenticación en la conexión.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="ef5ac-162">Agregue la cookie a la `CookieContainer` propiedad en el [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) objeto.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="ef5ac-163">En el ejemplo siguiente se muestra una aplicación de consola que recupera una cookie de autenticación desde una página web y agrega dicha cookie para la conexión.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="ef5ac-164">La dirección URL `https://www.contoso.com/RemoteLogin` en los puntos de ejemplo a una página web que debe crear.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="ef5ac-165">La página podría recuperar el nombre de usuario registrado y la contraseña y se intentan iniciar sesión en el usuario con las credenciales.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="ef5ac-166">La aplicación de consola envía las credenciales que se www.contoso.com/RemoteLogin que podría hacer referencia a una página vacía que contiene el siguiente archivo de código subyacente.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="ef5ac-167">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="ef5ac-167">Windows authentication</span></span>

<span data-ttu-id="ef5ac-168">Al utilizar la autenticación de Windows, puede pasar las credenciales del usuario actual mediante el [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) propiedad.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="ef5ac-169">Establezca las credenciales para la conexión con el valor de la DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="ef5ac-170">Encabezado de conexión</span><span class="sxs-lookup"><span data-stu-id="ef5ac-170">Connection header</span></span>

<span data-ttu-id="ef5ac-171">Si la aplicación no utiliza cookies, puede pasar información de usuario en el encabezado de conexión.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="ef5ac-172">Por ejemplo, puede pasar un token en el encabezado de conexión.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="ef5ac-173">A continuación, en el concentrador, debe comprobar el token del usuario.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="ef5ac-174">Certificado</span><span class="sxs-lookup"><span data-stu-id="ef5ac-174">Certificate</span></span>

<span data-ttu-id="ef5ac-175">Puede pasar un certificado de cliente para comprobar que el usuario.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="ef5ac-176">Agregar el certificado al crear la conexión.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="ef5ac-177">En el ejemplo siguiente se muestra solamente cómo agregar un certificado de cliente para la conexión; no se muestra la aplicación de consola completa.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="ef5ac-178">Usa el [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) clase que proporciona varias formas de crear el certificado.</span><span class="sxs-lookup"><span data-stu-id="ef5ac-178">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
