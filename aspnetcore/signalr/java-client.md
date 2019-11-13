---
title: ASP.NET Core SignalR cliente de Java
author: mikaelm12
description: Aprenda a usar el cliente de Java de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/java-client
ms.openlocfilehash: d7143b2c22ecdc4e68f484aa4c244e1c520beae0
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963784"
---
# <a name="aspnet-core-opno-locsignalr-java-client"></a><span data-ttu-id="00c96-103">ASP.NET Core SignalR cliente de Java</span><span class="sxs-lookup"><span data-stu-id="00c96-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="00c96-104">Por [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="00c96-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="00c96-105">El cliente de Java permite la conexión a un servidor de ASP.NET Core SignalR desde código Java, incluidas las aplicaciones Android.</span><span class="sxs-lookup"><span data-stu-id="00c96-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="00c96-106">Al igual que el [cliente JavaScript](xref:signalr/javascript-client) y el [cliente .net](xref:signalr/dotnet-client), el cliente Java permite recibir y enviar mensajes a un centro en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="00c96-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="00c96-107">El cliente de Java está disponible en ASP.NET Core 2,2 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="00c96-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="00c96-108">La aplicación de consola de Java de ejemplo a la que se hace referencia en este artículo usa el cliente de Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="00c96-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="00c96-109">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="00c96-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-opno-locsignalr-java-client-package"></a><span data-ttu-id="00c96-110">Instalación del paquete de cliente de SignalR Java</span><span class="sxs-lookup"><span data-stu-id="00c96-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="00c96-111">El archivo jar *signalr-1.0.0* permite que los clientes se conecten a SignalR hubs.</span><span class="sxs-lookup"><span data-stu-id="00c96-111">The *signalr-1.0.0* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="00c96-112">Para buscar el número de versión del archivo JAR más reciente, consulte los resultados de la [búsqueda de Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span><span class="sxs-lookup"><span data-stu-id="00c96-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="00c96-113">Si usa Gradle, agregue la siguiente línea a la sección `dependencies` del archivo *Build. Gradle* :</span><span class="sxs-lookup"><span data-stu-id="00c96-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

<span data-ttu-id="00c96-114">Si usa Maven, agregue las siguientes líneas dentro del elemento `<dependencies>` del archivo *archivo pom. XML* :</span><span class="sxs-lookup"><span data-stu-id="00c96-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="00c96-115">Conexión a un concentrador</span><span class="sxs-lookup"><span data-stu-id="00c96-115">Connect to a hub</span></span>

<span data-ttu-id="00c96-116">Para establecer un `HubConnection`, se debe usar el `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="00c96-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="00c96-117">La dirección URL del concentrador y el nivel de registro se pueden configurar al compilar una conexión.</span><span class="sxs-lookup"><span data-stu-id="00c96-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="00c96-118">Configure las opciones necesarias llamando a cualquiera de los métodos `HubConnectionBuilder` antes de `build`.</span><span class="sxs-lookup"><span data-stu-id="00c96-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="00c96-119">Inicie la conexión con `start`.</span><span class="sxs-lookup"><span data-stu-id="00c96-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="00c96-120">Llamar a métodos de Hub desde el cliente</span><span class="sxs-lookup"><span data-stu-id="00c96-120">Call hub methods from client</span></span>

<span data-ttu-id="00c96-121">Una llamada a `send` invoca un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="00c96-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="00c96-122">Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `send`.</span><span class="sxs-lookup"><span data-stu-id="00c96-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> <span data-ttu-id="00c96-123">Si usa Azure SignalR Service en modo sin *servidor*, no puede llamar a métodos de concentrador desde un cliente.</span><span class="sxs-lookup"><span data-stu-id="00c96-123">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="00c96-124">Para obtener más información, consulte la [documentación del servicio deSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="00c96-124">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="00c96-125">Llamar a métodos de cliente desde el concentrador</span><span class="sxs-lookup"><span data-stu-id="00c96-125">Call client methods from hub</span></span>

<span data-ttu-id="00c96-126">Use `hubConnection.on` para definir métodos en el cliente a los que puede llamar el concentrador.</span><span class="sxs-lookup"><span data-stu-id="00c96-126">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="00c96-127">Defina los métodos después de la compilación, pero antes de iniciar la conexión.</span><span class="sxs-lookup"><span data-stu-id="00c96-127">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="00c96-128">Agregar registro</span><span class="sxs-lookup"><span data-stu-id="00c96-128">Add logging</span></span>

<span data-ttu-id="00c96-129">El cliente de Java de SignalR usa la biblioteca [SLF4J](https://www.slf4j.org/) para el registro.</span><span class="sxs-lookup"><span data-stu-id="00c96-129">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="00c96-130">Se trata de una API de registro de alto nivel que permite a los usuarios de la biblioteca elegir su propia implementación de registro específica mediante la incorporación de una dependencia de registro específica.</span><span class="sxs-lookup"><span data-stu-id="00c96-130">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="00c96-131">En el fragmento de código siguiente se muestra cómo usar `java.util.logging` con el cliente de Java SignalR.</span><span class="sxs-lookup"><span data-stu-id="00c96-131">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="00c96-132">Si no configura el registro en las dependencias, SLF4J carga un registrador de no operaciones predeterminado con el siguiente mensaje de ADVERTENCIA:</span><span class="sxs-lookup"><span data-stu-id="00c96-132">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="00c96-133">Esto se puede omitir sin ningún riesgo.</span><span class="sxs-lookup"><span data-stu-id="00c96-133">This can safely be ignored.</span></span>

## <a name="android-development-notes"></a><span data-ttu-id="00c96-134">Notas de desarrollo de Android</span><span class="sxs-lookup"><span data-stu-id="00c96-134">Android development notes</span></span>

<span data-ttu-id="00c96-135">Con respecto a Android SDK compatibilidad con las características del cliente de SignalR, tenga en cuenta los siguientes elementos al especificar la versión de Android SDK de destino:</span><span class="sxs-lookup"><span data-stu-id="00c96-135">With regards to Android SDK compatibility for the SignalR client features, consider the following items when specifying your target Android SDK version:</span></span>

* <span data-ttu-id="00c96-136">El cliente de Java de SignalR se ejecutará en el nivel de API de Android 16 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="00c96-136">The SignalR Java Client will run on Android API Level 16 and later.</span></span>
* <span data-ttu-id="00c96-137">La conexión a través del servicio Azure SignalR requerirá el nivel de API de Android 20 y versiones posteriores, ya que el [servicio azure SignalR](/azure/azure-signalr/signalr-overview) requiere TLS 1,2 y no es compatible con conjuntos de cifrado basados en SHA-1.</span><span class="sxs-lookup"><span data-stu-id="00c96-137">Connecting through the Azure SignalR Service will require Android API Level 20 and later because the [Azure SignalR Service](/azure/azure-signalr/signalr-overview) requires TLS 1.2 and doesn't support SHA-1-based cipher suites.</span></span> <span data-ttu-id="00c96-138">Android [ha agregado compatibilidad con los conjuntos de cifrado SHA-256 (y versiones posteriores)](https://developer.android.com/reference/javax/net/ssl/SSLSocket) en el nivel de API 20.</span><span class="sxs-lookup"><span data-stu-id="00c96-138">Android [added support for SHA-256 (and above) cipher suites](https://developer.android.com/reference/javax/net/ssl/SSLSocket) in API Level 20.</span></span>

## <a name="configure-bearer-token-authentication"></a><span data-ttu-id="00c96-139">Configurar la autenticación de token de portador</span><span class="sxs-lookup"><span data-stu-id="00c96-139">Configure bearer token authentication</span></span>

<span data-ttu-id="00c96-140">En el cliente de SignalR Java, puede configurar un token de portador que se usará para la autenticación proporcionando un "generador de tokens de acceso" a [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="00c96-140">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an "access token factory" to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="00c96-141">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) para [proporcionar una](https://github.com/ReactiveX/RxJava) [> de cadena de\<única](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="00c96-141">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="00c96-142">Con una llamada a [Single. defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), puede escribir lógica para generar tokens de acceso para el cliente.</span><span class="sxs-lookup"><span data-stu-id="00c96-142">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a><span data-ttu-id="00c96-143">Limitaciones conocidas</span><span class="sxs-lookup"><span data-stu-id="00c96-143">Known limitations</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="00c96-144">Solo se admite el protocolo JSON.</span><span class="sxs-lookup"><span data-stu-id="00c96-144">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="00c96-145">No se admiten la reserva de transporte ni el transporte de eventos enviados del servidor.</span><span class="sxs-lookup"><span data-stu-id="00c96-145">Transport fallback and the Server Sent Events transport aren't supported.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="00c96-146">Solo se admite el protocolo JSON.</span><span class="sxs-lookup"><span data-stu-id="00c96-146">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="00c96-147">Solo se admite el transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="00c96-147">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="00c96-148">Todavía no se admite la transmisión por secuencias.</span><span class="sxs-lookup"><span data-stu-id="00c96-148">Streaming isn't supported yet.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="00c96-149">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="00c96-149">Additional resources</span></span>

* [<span data-ttu-id="00c96-150">Referencia de API de Java</span><span class="sxs-lookup"><span data-stu-id="00c96-150">Java API reference</span></span>](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* <span data-ttu-id="00c96-151">[Documentación sin servidor del servicio SignalR de Azure](/azure/azure-signalr/signalr-concept-serverless-development-config)</span><span class="sxs-lookup"><span data-stu-id="00c96-151">[Azure SignalR Service serverless documentation](/azure/azure-signalr/signalr-concept-serverless-development-config)</span></span>
