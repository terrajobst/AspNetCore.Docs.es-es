---
title: Cliente ASP.NET Core SignalR Java
author: mikaelm12
description: Obtenga información sobre cómo usar al cliente de Java de ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: 0eba59a05ea6fd3fed46fcab86ac20caf40ebb65
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "46482923"
---
# <a name="aspnet-core-signalr-java-client"></a>Cliente de SignalR Java de ASP.NET Core

Por [Mikael Mengistu](https://twitter.com/MikaelM_12)

El cliente de Java que permite conectarse a un servidor de ASP.NET Core SignalR desde el código de Java, incluidas las aplicaciones Android. Al igual que el [cliente JavaScript](xref:signalr/javascript-client) y el [cliente .NET](xref:signalr/dotnet-client), el cliente de Java que le permite recibir y enviar mensajes a un concentrador en tiempo real. El cliente de Java está disponible en ASP.NET Core 2.2 y posteriores.

En este artículo hace referencia a la aplicación de consola de Java de ejemplo usa al cliente de SignalR Java.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>Instale el paquete de cliente de SignalR Java

El *signalr-0.1.0-preview2-35174* archivo JAR permite a los clientes para conectarse a los concentradores de SignalR. Para buscar el número de versión de archivo JAR más reciente, consulte el [los resultados de búsqueda de Maven](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).

Si usa Gradle, agregue la siguiente línea a la `dependencies` sección de su *build.gradle* archivo:

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview2-35174'
```

Si con Maven, agregue las siguientes líneas dentro de la `<dependencies>` elemento de su *pom.xml* archivo:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Conectarse a un concentrador

Para establecer un `HubConnection`, el `HubConnectionBuilder` se debe usar. El nivel de registro y la dirección URL del centro se puede configurar durante la compilación de una conexión. Configurar las opciones necesarias mediante una llamada a cualquiera de los `HubConnectionBuilder` métodos antes de `build`. Iniciar la conexión con `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a>Llamar a métodos de concentrador de cliente

Una llamada a `send` invoca un método de concentrador. Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a>Llamar a métodos cliente desde el concentrador

Use `hubConnection.on` para definir métodos en el cliente que se puede llamar la central. Definir los métodos después de compilar, pero antes de iniciar la conexión.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a>Limitaciones conocidas

Se trata de una versión preliminar del cliente de Java. Hay muchas características que aún no se admiten. Para las versiones futuras, están trabajando los huecos siguientes:

* Solo los tipos primitivos se pueden aceptar como parámetros y tipos de valor devuelto.
* Las API son sincrónicas.
* Solo el tipo de llamada de "Envío" se admite en este momento. No se admiten "Invocar" y la transmisión por secuencias de valores devueltos.
* Se admite solo el protocolo JSON.
* Se admite sólo el transporte de WebSockets.

## <a name="additional-resources"></a>Recursos adicionales

* [Referencia de la API de Java](/java/api/com.microsoft.aspnet.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
