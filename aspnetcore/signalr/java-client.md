---
title: Cliente ASP.NET Core SignalR Java
author: mikaelm12
description: Obtenga información sobre cómo usar al cliente de Java de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 10/18/2018
uid: signalr/java-client
ms.openlocfilehash: 646118c78d5d38b44b89d399cd06a5332a11d064
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207776"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="159ec-103">Cliente ASP.NET Core SignalR Java</span><span class="sxs-lookup"><span data-stu-id="159ec-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="159ec-104">Por [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="159ec-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="159ec-105">El cliente de Java que permite conectarse a un servidor de ASP.NET Core SignalR desde el código de Java, incluidas las aplicaciones Android.</span><span class="sxs-lookup"><span data-stu-id="159ec-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="159ec-106">Al igual que el [cliente JavaScript](xref:signalr/javascript-client) y el [cliente .NET](xref:signalr/dotnet-client), el cliente de Java que le permite recibir y enviar mensajes a un concentrador en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="159ec-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="159ec-107">El cliente de Java está disponible en ASP.NET Core 2.2 y posteriores.</span><span class="sxs-lookup"><span data-stu-id="159ec-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="159ec-108">En este artículo hace referencia a la aplicación de consola de Java de ejemplo usa al cliente de SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="159ec-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="159ec-109">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="159ec-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="159ec-110">Instale el paquete de cliente de SignalR Java</span><span class="sxs-lookup"><span data-stu-id="159ec-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="159ec-111">El *signalr-1.0.0-preview3-35501* archivo JAR permite a los clientes para conectarse a los concentradores de SignalR.</span><span class="sxs-lookup"><span data-stu-id="159ec-111">The *signalr-1.0.0-preview3-35501* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="159ec-112">Para buscar el número de versión de archivo JAR más reciente, consulte el [los resultados de búsqueda de Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span><span class="sxs-lookup"><span data-stu-id="159ec-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="159ec-113">Si usa Gradle, agregue la siguiente línea a la `dependencies` sección de su *build.gradle* archivo:</span><span class="sxs-lookup"><span data-stu-id="159ec-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

<span data-ttu-id="159ec-114">Si con Maven, agregue las siguientes líneas dentro de la `<dependencies>` elemento de su *pom.xml* archivo:</span><span class="sxs-lookup"><span data-stu-id="159ec-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="159ec-115">Conectarse a un concentrador</span><span class="sxs-lookup"><span data-stu-id="159ec-115">Connect to a hub</span></span>

<span data-ttu-id="159ec-116">Para establecer un `HubConnection`, el `HubConnectionBuilder` se debe usar.</span><span class="sxs-lookup"><span data-stu-id="159ec-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="159ec-117">El nivel de registro y la dirección URL del centro se puede configurar durante la compilación de una conexión.</span><span class="sxs-lookup"><span data-stu-id="159ec-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="159ec-118">Configurar las opciones necesarias mediante una llamada a cualquiera de los `HubConnectionBuilder` métodos antes de `build`.</span><span class="sxs-lookup"><span data-stu-id="159ec-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="159ec-119">Iniciar la conexión con `start`.</span><span class="sxs-lookup"><span data-stu-id="159ec-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="159ec-120">Llamar a métodos de concentrador de cliente</span><span class="sxs-lookup"><span data-stu-id="159ec-120">Call hub methods from client</span></span>

<span data-ttu-id="159ec-121">Una llamada a `send` invoca un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="159ec-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="159ec-122">Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `send`.</span><span class="sxs-lookup"><span data-stu-id="159ec-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="159ec-123">Llamar a métodos cliente desde el concentrador</span><span class="sxs-lookup"><span data-stu-id="159ec-123">Call client methods from hub</span></span>

<span data-ttu-id="159ec-124">Use `hubConnection.on` para definir métodos en el cliente que se puede llamar la central.</span><span class="sxs-lookup"><span data-stu-id="159ec-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="159ec-125">Definir los métodos después de compilar, pero antes de iniciar la conexión.</span><span class="sxs-lookup"><span data-stu-id="159ec-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="159ec-126">Agregue un registro</span><span class="sxs-lookup"><span data-stu-id="159ec-126">Add logging</span></span>

<span data-ttu-id="159ec-127">El cliente de SignalR Java usa el [SLF4J](https://www.slf4j.org/) biblioteca para el registro.</span><span class="sxs-lookup"><span data-stu-id="159ec-127">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="159ec-128">Es una API de alto nivel de registro que permite a los usuarios de la biblioteca elegir su propia implementación de registro específico al incorporar una dependencia de registro específico.</span><span class="sxs-lookup"><span data-stu-id="159ec-128">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="159ec-129">El fragmento de código siguiente muestra cómo usar `java.util.logging` con el cliente de SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="159ec-129">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="159ec-130">Si no configura el registro de las dependencias, SLF4J carga un registrador de ninguna operación de forma predeterminada con el mensaje de advertencia siguiente:</span><span class="sxs-lookup"><span data-stu-id="159ec-130">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="159ec-131">Esto puede pasar por alto.</span><span class="sxs-lookup"><span data-stu-id="159ec-131">This can safely be ignored.</span></span>

## <a name="known-limitations"></a><span data-ttu-id="159ec-132">Limitaciones conocidas</span><span class="sxs-lookup"><span data-stu-id="159ec-132">Known limitations</span></span>

<span data-ttu-id="159ec-133">Se trata de una versión preliminar del cliente de Java.</span><span class="sxs-lookup"><span data-stu-id="159ec-133">This is a preview release of the Java client.</span></span> <span data-ttu-id="159ec-134">No se admiten algunas características:</span><span class="sxs-lookup"><span data-stu-id="159ec-134">Some features aren't supported:</span></span>

* <span data-ttu-id="159ec-135">Se admite solo el protocolo JSON.</span><span class="sxs-lookup"><span data-stu-id="159ec-135">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="159ec-136">Se admite sólo el transporte de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="159ec-136">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="159ec-137">Transmisión por secuencias no se admite todavía.</span><span class="sxs-lookup"><span data-stu-id="159ec-137">Streaming isn't supported yet.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="159ec-138">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="159ec-138">Additional resources</span></span>

* [<span data-ttu-id="159ec-139">Referencia de API de Java</span><span class="sxs-lookup"><span data-stu-id="159ec-139">Java API reference</span></span>](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
