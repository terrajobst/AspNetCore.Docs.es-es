---
title: "Implementación de servidor web kestrel en ASP.NET Core"
author: tdykstra
description: "Presenta Kestrel, el servidor web de multiplataforma para ASP.NET Core según libuv."
keywords: "Núcleo de ASP.NET, Kestrel, libuv, prefijos de dirección url"
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 560bd66f-7dd0-4e68-b5fb-f31477e4b785
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 451c36fc9095b6e076e5287c992b6163205c523b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>Introducción a la implementación de servidor web Kestrel en ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), y [Stephen Halter](https://twitter.com/halter73)

Kestrel es una plataforma de entre [servidor web de ASP.NET Core](index.md) basado en [libuv](https://github.com/libuv/libuv), una biblioteca de E/S asincrónica de multiplataforma. Kestrel es el servidor web que se incluye de forma predeterminada en las plantillas de proyecto de ASP.NET Core. 

Kestrel admite las siguientes características:

  * HTTPS
  * Utilizada para habilitar la actualización opaca [WebSockets](https://github.com/aspnet/websockets)
  * Sockets de UNIX de alto rendimiento detrás de Nginx 

Kestrel es compatible con todas las plataformas y versiones que admite .NET Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Ver o descargar el código de ejemplo para 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Ver o descargar el código de ejemplo para 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Cuándo utilizar Kestrel con un proxy inverso

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Puede usar Kestrel por sí solo o con un *servidor proxy inverso*, como IIS, Nginx o Apache. Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.

![Kestrel se comunica directamente con Internet sin ningún servidor proxy inverso](kestrel/_static/kestrel-to-internet2.png)

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

También se puede usar cualquiera de las configuraciones (con o sin servidor proxy inverso) si Kestrel está expuesto solo a una red interna.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Si la aplicación acepta solicitudes únicamente de una red interna, puede usar Kestrel por sí mismo.

![Kestrel se comunica directamente con la red interna](kestrel/_static/kestrel-to-internal.png)

Si expone la aplicación a Internet, debe usar IIS, Nginx o Apache como *servidor proxy inverso*. Un servidor proxy inverso recibe las solicitudes HTTP de Internet y las reenvía a Kestrel después de un control preliminar.

![Kestrel se comunica indirectamente con Internet a través de un servidor proxy inverso, como IIS, Nginx o Apache](kestrel/_static/kestrel-to-internet.png)

Un proxy inverso es necesario para las implementaciones de borde (expuestas al tráfico de Internet) por motivos de seguridad. Las versiones 1.x de Kestrel no tienen un complemento completo de defensas frente a los ataques. Esto incluye, pero no se limita a los tiempos de espera adecuado, límites de tamaño y los límites de conexiones simultáneas.

---

Un escenario que requiere a un proxy inverso es cuando tiene varias aplicaciones que comparten la misma dirección IP y puerto que se ejecuta en un solo servidor. Esto no funciona con Kestrel directamente, porque Kestrel no es compatible con la misma dirección IP y puerto entre varios procesos de uso compartido. Al configurar Kestrel para escuchar en un puerto, controla todo el tráfico de ese puerto independientemente del encabezado de host. A continuación, debe reenviar un proxy inverso que puedan compartir puertos Kestrel en una única dirección IP y puerto.

Incluso si un servidor proxy inverso no es necesario, mediante uno podría ser una buena elección por otras razones:

* Puede limitar la superficie expuesta.
* Proporciona un nivel de configuración y una defensa adicional opcional.
* Es posible que integren mejor con la infraestructura existente.
* Esta característica simplifica el equilibrio de carga y la configuración SSL. Solo el servidor proxy inverso requiere un certificado SSL y que el servidor puede comunicarse con los servidores de aplicaciones en la red interna mediante HTTP sin formato.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Cómo usar Kestrel en aplicaciones de ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

El [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paquete se incluye en el [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

Plantillas de proyecto de ASP.NET Core usan Kestrel de forma predeterminada. En *Program.cs*, las llamadas de código de plantilla `CreateDefaultBuilder`, que llama [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) en segundo plano.

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Si tiene que configurar las opciones de Kestrel, llame a `UseKestrel` en *Program.cs* tal como se muestra en el ejemplo siguiente:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Instalar el [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) paquete NuGet.

Llame a la [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) método de extensión `WebHostBuilder` en su `Main` método especifica cualquier [opciones Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) que necesite, como se muestra en la sección siguiente.

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Opciones de kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

El servidor de web Kestrel tiene opciones de configuración de restricción que son especialmente útiles en las implementaciones con conexión a Internet. Estos son algunos de los límites que puede establecer:

- Las conexiones máximas de cliente
- El tamaño máximo del cuerpo de solicitud
- La velocidad mínima de los datos del cuerpo de solicitud.

Establezca estas restricciones y otras personas en la `Limits` propiedad de la [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) clase. El `Limits` propiedad contiene una instancia de la [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) clase. 

**Conexiones de cliente máximo**

El número máximo de conexiones simultáneas de TCP abiertos se puede establecer para toda la aplicación con el código siguiente:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

Hay un límite independiente para las conexiones que se han actualizado desde HTTP o HTTPS a otro protocolo (por ejemplo, en una solicitud de WebSockets). Cuando se actualiza una conexión, no cuentan con la `MaxConcurrentConnections` límite. 

El número máximo de conexiones es un número ilimitado de forma predeterminada (null).

**Tamaño del cuerpo de solicitud máximo**

El tamaño predeterminado del cuerpo de solicitud máximo es: 30.000.000 bytes, que es aproximadamente 28,6 MB. 

La manera recomendada para invalidar el límite de una aplicación de MVC de ASP.NET Core es usar el [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) atributo en un método de acción:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Este es un ejemplo que muestra cómo configurar la restricción para toda la aplicación, todas las solicitudes:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

Puede invalidar la configuración en una solicitud específica de middleware:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
Se produce una excepción si intenta configurar el límite de una solicitud después de que la aplicación se ha empezado la lectura de la solicitud. Hay un `IsReadOnly` propiedad que indica si la `MaxRequestBodySize` propiedad está en estado de solo lectura, lo que significa es demasiado tarde para configurar el límite.

**Velocidad de datos de cuerpo de solicitud mínima**

Kestrel comprueba cada segundo si datos próximas novedades en a la velocidad especificada en bytes por segundo. Si la velocidad se sitúa por debajo del mínimo, la conexión se agotó el tiempo. El período de gracia es la cantidad de tiempo que Kestrel da al cliente para aumentar su velocidad de envío hasta el mínimo; no se comprueba la tasa durante ese tiempo. El período de gracia le ayuda a evitar las conexiones que inicialmente están enviando datos a una velocidad lento debido a TCP inicio lento.

El porcentaje mínimo predeterminado es 240 bytes por segundo, con un período de gracia de 5 segundos.

Una velocidad mínima también se aplica a la respuesta. El código para establecer el límite de solicitudes y el límite de respuesta es el mismo salvo tener `RequestBody` o `Response` en los nombres de propiedad y la interfaz. 

Este es un ejemplo que muestra cómo configurar los tipos de datos mínimo en *Program.cs*:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

Puede configurar las tarifas por solicitud de middleware:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

Para obtener información sobre otras opciones de Kestrel, vea las clases siguientes:

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para obtener información acerca de las opciones de Kestrel, consulte [KestrelServerOptions clase](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).

---

### <a name="endpoint-configuration"></a>Configuración de extremo

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

De forma predeterminada, ASP.NET Core enlaza a `http://localhost:5000`. Configurar los prefijos de dirección URL y los puertos para Kestrel para que escuche en mediante una llamada a `Listen` o `ListenUnixSocket` métodos en `KestrelServerOptions`. (`UseUrls`, `urls` argumento de línea de comandos y la variable de entorno ASPNETCORE_URLS también funcionan pero tienen las limitaciones se indicó [más adelante en este artículo](#useurls-limitations).)

**Enlazar a un socket TCP**

El `Listen` método se enlaza a un socket de TCP, y una lambda de opciones le permite configurar un certificado SSL:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Observe cómo este ejemplo configura SSL para un extremo determinado mediante el uso de [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs). Puede utilizar las mismas API para configurar otras opciones de Kestrel para extremos determinados.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**Enlazar a un socket de Unix**

Puede escuchar en un socket de Unix para mejorar el rendimiento con Nginx, tal como se muestra en este ejemplo:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**Puerto 0**

Si especifica el número de puerto 0, Kestrel se enlaza de forma dinámica a un puerto disponible. En el ejemplo siguiente se muestra cómo determinar qué puerto Kestrel realmente enlazado en tiempo de ejecución:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**Limitaciones de UseUrls**

Puede configurar los puntos de conexión mediante una llamada a la `UseUrls` método o utilizando el `urls` argumento de línea de comandos o la variable de entorno ASPNETCORE_URLS. Estos métodos son útiles si desea que su código para trabajar con servidores distintos Kestrel. Sin embargo, tenga en cuenta estas limitaciones:

* No se puede utilizar SSL con estos métodos.
* Si se usan los la `Listen` método y `UseUrls`, el `Listen` extremos invalidación el `UseUrls` puntos de conexión.

**Configuración de extremo para IIS**

Si usa IIS, los enlaces de dirección URL para IIS invalidan todos los enlaces que se establecen mediante una llamada a `Listen` o `UseUrls`. Para obtener más información, consulte [Introducción a ASP.NET Core módulo](aspnet-core-module.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

De forma predeterminada, ASP.NET Core enlaza a `http://localhost:5000`. Puede configurar los prefijos de dirección URL y los puertos para Kestrel para que escuche en mediante el uso de la `UseUrls` método de extensión, el `urls` argumento de línea de comandos o el sistema de configuración de ASP.NET Core. Para obtener más información acerca de estos métodos, consulte [hospedaje](../../fundamentals/hosting.md). Para obtener información acerca del funcionamiento de enlace de dirección URL cuando se usa IIS como un proxy inverso, consulte [módulo principal de ASP.NET](aspnet-core-module.md). 

---

### <a name="url-prefixes"></a>Prefijos de dirección URL

Si se llama a `UseUrls` o usar el `urls` argumento de línea de comandos o una variable de entorno de ASPNETCORE_URLS, los prefijos de dirección URL pueden ser de cualquiera de los formatos siguientes. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Prefijos de dirección URL de HTTP solo son válidos; Kestrel no admite SSL al configurar los enlaces de dirección URL mediante `UseUrls`.

* Dirección IPv4 con número de puerto

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 es un caso especial que se enlaza a todas las direcciones IPv4.


* Dirección IPv6 con número de puerto

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [::] es el equivalente de IPv6 de IPv4 0.0.0.0.


* Nombre de host con el número de puerto

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Hospedar nombres, *, y + no son especiales. Todo lo que no es una dirección IP o el "localhost" reconocido se enlazará a todas las direcciones IP de IPv6 y IPv4. Si tiene que enlazar nombres de host diferente para distintas aplicaciones ASP.NET Core en el mismo puerto, use [HTTP.sys](httpsys.md) o un servidor proxy inverso, como IIS, Nginx o Apache.

* Nombre de "Localhost" con la dirección IP de bucle invertido o de número de puerto con el número de puerto

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Cuando `localhost` se especifica, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6. Si el puerto solicitado está en uso por otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar. Si ninguna de estas interfaces de bucle invertido no está disponible por cualquier otra razón (mayoría normalmente porque no se admite IPv6), Kestrel registra una advertencia. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* Dirección IPv4 con número de puerto

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 es un caso especial que se enlaza a todas las direcciones IPv4.


* Dirección IPv6 con número de puerto

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [::] es el equivalente de IPv6 de IPv4 0.0.0.0.


* Nombre de host con el número de puerto

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Los nombres, de host \*, y + no son especiales. Todo lo que no es una dirección IP o el "localhost" reconocido enlaza a todas las direcciones IP de IPv6 y IPv4. Si tiene que enlazar nombres de host diferente para distintas aplicaciones ASP.NET Core en el mismo puerto, use [WebListener](weblistener.md) o un servidor proxy inverso, como IIS, Nginx o Apache.

* Nombre de "Localhost" con la dirección IP de bucle invertido o de número de puerto con el número de puerto

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Cuando `localhost` se especifica, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6. Si el puerto solicitado está en uso por otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar. Si ninguna de estas interfaces de bucle invertido no está disponible por cualquier otra razón (mayoría normalmente porque no se admite IPv6), Kestrel registra una advertencia. 

* Sockets de UNIX

  ```
  http://unix:/run/dan-live.sock  
  ```

**Puerto 0**

Si especifica el número de puerto 0, Kestrel se enlaza de forma dinámica a un puerto disponible. Enlace de puerto 0 se permite para cualquier nombre de host o una dirección IP excepto `localhost` nombre.

En el ejemplo siguiente se muestra cómo determinar qué puerto Kestrel realmente enlazado en tiempo de ejecución:

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**Prefijos de dirección URL de SSL**

No olvide incluir los prefijos de dirección URL con `https:` si se llama a la `UseHttps` método de extensión, tal y como se muestra a continuación.

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> HTTP y HTTPS no se puede hospedar en el mismo puerto.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes recursos:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Aplicación de ejemplo de 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Código fuente de kestrel](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Aplicación de ejemplo para 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Código fuente de kestrel](https://github.com/aspnet/KestrelHttpServer)

---
