---
title: Cliente ASP.NET Core SignalR Java
author: mikaelm12
description: Obtenga información sobre cómo usar al cliente de Java de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 06/27/2019
uid: signalr/java-client
ms.openlocfilehash: eea1dfb7d8afcd34c0dacd8315ad196d7235c9f7
ms.sourcegitcommit: 6d9cf728465cdb0de1037633a8b7df9a8989cccb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67463271"
---
# <a name="aspnet-core-signalr-java-client"></a>Cliente ASP.NET Core SignalR Java

Por [Mikael Mengistu](https://twitter.com/MikaelM_12)

El cliente de Java que permite conectarse a un servidor de ASP.NET Core SignalR desde el código de Java, incluidas las aplicaciones Android. Al igual que el [cliente JavaScript](xref:signalr/javascript-client) y el [cliente .NET](xref:signalr/dotnet-client), el cliente de Java que le permite recibir y enviar mensajes a un concentrador en tiempo real. El cliente de Java está disponible en ASP.NET Core 2.2 y posteriores.

En este artículo hace referencia a la aplicación de consola de Java de ejemplo usa al cliente de SignalR Java.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Instale el paquete de cliente de SignalR Java

El *signalr 1.0.0* archivo JAR permite a los clientes para conectarse a los concentradores de SignalR. Para buscar el número de versión de archivo JAR más reciente, consulte el [los resultados de búsqueda de Maven](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

Si usa Gradle, agregue la siguiente línea a la `dependencies` sección de su *build.gradle* archivo:

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

Si con Maven, agregue las siguientes líneas dentro de la `<dependencies>` elemento de su *pom.xml* archivo:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Conectarse a un concentrador

Para establecer un `HubConnection`, el `HubConnectionBuilder` se debe usar. El nivel de registro y la dirección URL del centro se puede configurar durante la compilación de una conexión. Configurar las opciones necesarias mediante una llamada a cualquiera de los `HubConnectionBuilder` métodos antes de `build`. Iniciar la conexión con `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>Llamar a métodos de concentrador de cliente

Una llamada a `send` invoca un método de concentrador. Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> Si usa Azure SignalR Service en *modo sin servidor*, no puede llamar a métodos de concentrador desde un cliente. Para obtener más información, consulte el [documentación de SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>Llamar a métodos cliente desde el concentrador

Use `hubConnection.on` para definir métodos en el cliente que se puede llamar la central. Definir los métodos después de compilar, pero antes de iniciar la conexión.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>Agregue un registro

El cliente de SignalR Java usa el [SLF4J](https://www.slf4j.org/) biblioteca para el registro. Es una API de alto nivel de registro que permite a los usuarios de la biblioteca elegir su propia implementación de registro específico al incorporar una dependencia de registro específico. El fragmento de código siguiente muestra cómo usar `java.util.logging` con el cliente de SignalR Java.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Si no configura el registro de las dependencias, SLF4J carga un registrador de ninguna operación de forma predeterminada con el mensaje de advertencia siguiente:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Esto puede pasar por alto.

## <a name="android-development-notes"></a>Notas de desarrollo de Android

Con respecto a la compatibilidad con el SDK de Android para las características de cliente de SignalR, tenga en cuenta los siguientes elementos cuando se especifica la versión del SDK de Android de destino:

* El cliente de Java de SignalR se ejecutará en el nivel de API Android 16 y versiones posteriores.
* Conexión a través de Azure SignalR Service requerirá el nivel de API de Android 20 y versiones posterior porque la [Azure SignalR Service](/azure/azure-signalr/signalr-overview) requiere TLS 1.2 y no es compatible con conjuntos de cifrado basado en SHA-1. Android [agrega compatibilidad con SHA-256 (y versiones posteriores) conjuntos de cifrado](https://developer.android.com/reference/javax/net/ssl/SSLSocket) en el nivel de API 20.

## <a name="configure-bearer-token-authentication"></a>Configurar la autenticación de token de portador

En el cliente de SignalR Java, puede configurar un token de portador a usar para la autenticación mediante un "generador de tokens de acceso" para el [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) para proporcionar una [RxJava](https://github.com/ReactiveX/RxJava) [único\<cadena >](http://reactivex.io/documentation/single.html). Con una llamada a [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), puede escribir la lógica para generar tokens de acceso para el cliente.

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a>Limitaciones conocidas

::: moniker range=">= aspnetcore-3.0"

* Se admite solo el protocolo JSON.
* No se admiten el transporte de reserva y el transporte de eventos enviados del servidor.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Se admite solo el protocolo JSON.
* Se admite sólo el transporte de WebSockets.
* Transmisión por secuencias no se admite todavía.

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* [Referencia de API de Java](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* [Documentación sin servidor de Azure SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config)
