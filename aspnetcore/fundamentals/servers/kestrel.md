---
title: "Implementación del servidor web Kestrel en ASP.NET Core"
author: tdykstra
description: "Este artículo presenta Kestrel, el servidor web multiplataforma para ASP.NET Core basado en libuv."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>Introducción a la implementación del servidor web Kestrel en ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) y [Stephen Halter](https://twitter.com/halter73)

Kestrel es un [servidor web multiplataforma para ASP.NET Core](index.md) basado en [libuv](https://github.com/libuv/libuv), una biblioteca de E/S asincrónica y multiplataforma. Kestrel es el servidor web que se incluye de forma predeterminada en las plantillas de proyecto de ASP.NET Core. 

Kestrel admite las siguientes características:

  * HTTPS
  * Actualización opaca para habilitar [WebSockets](https://github.com/aspnet/websockets)
  * Sockets de Unix para alto rendimiento detrás de Nginx 

Kestrel admite todas las plataformas y versiones que sean compatibles con .NET Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Vea o descargue el código de ejemplo para 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Vea o descargue el código de ejemplo para 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Cuándo usar Kestrel con un proxy inverso

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

Se necesita un proxy inverso para las implementaciones de borde (expuestas al tráfico de Internet) por motivos de seguridad. Las versiones 1.x de Kestrel no tienen un complemento completo de defensas frente a los ataques. Esto incluye (aunque no de forma limitada) los tiempos de espera, los límites de tamaño y los límites de conexiones simultáneas correspondientes.

---

Un escenario en el que se requiere un proxy inverso sería cuando varias aplicaciones comparten el mismo puerto y dirección IP, y se ejecutan en un solo servidor. Esto no funciona con Kestrel directamente, porque Kestrel no admite compartir la misma dirección IP y puerto entre varios procesos. Al configurar Kestrel para escuchar en un puerto, se controla todo el tráfico de ese puerto, independientemente del encabezado de host. Después, debe reenviar a Kestrel un proxy inverso que pueda compartir puertos en una única dirección IP y puerto.

Aunque no sea necesario un servidor proxy inverso, puede ser útil usar uno por otras razones:

* Puede limitar el área de superficie expuesta.
* Proporciona una capa de configuración y defensa adicional opcional.
* Es posible que se integre mejor con la infraestructura existente.
* Simplifica el equilibrio de carga y la configuración SSL. Solo el servidor proxy inverso requiere un certificado SSL y dicho servidor puede comunicarse con los servidores de aplicaciones en la red interna mediante HTTP sin formato.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Cómo usar Kestrel en aplicaciones ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

El paquete [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) se incluye en el [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

Las plantillas de proyecto de ASP.NET Core usan Kestrel de forma predeterminada. En *Program.cs*, el código de plantilla llama a `CreateDefaultBuilder`, que a su vez llama a [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) en segundo plano.

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

Si tiene que configurar las opciones de Kestrel, llame a `UseKestrel` en *Program.cs*, tal como se muestra en este ejemplo:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Instale el paquete NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).

Llame al método de extensión [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) en `WebHostBuilder` en el método `Main` y especifique cualquier [opción de Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) que necesite, como se muestra en la sección siguiente.

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Opciones de Kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

El servidor web Kestrel tiene opciones de configuración de restricción que son especialmente útiles en las implementaciones con conexión a Internet. Estos son algunos de los límites que puede establecer:

- Las conexiones máximas de cliente
- El tamaño máximo del cuerpo de solicitud
- La velocidad mínima de los datos del cuerpo de solicitud.

Establezca estas restricciones y otras en la propiedad `Limits` de la clase [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs). La propiedad `Limits` contiene una instancia de la clase [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs). 

**Conexiones máximas de cliente**

El número máximo de conexiones de TCP abiertas simultáneas que se pueden establecer para toda la aplicación con este código:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

Hay un límite independiente para las conexiones que se han actualizado desde HTTP o HTTPS a otro protocolo (por ejemplo, en una solicitud de WebSockets). Cuando se actualiza una conexión, no se cuenta con respecto al límite de `MaxConcurrentConnections`. 

El número máximo de conexiones es ilimitado de forma predeterminada (null).

**Tamaño máximo del cuerpo de la solicitud**

El tamaño máximo predeterminado del cuerpo de la solicitud es 30 000 000 bytes, que es aproximadamente 28,6 MB. 

La manera recomendada de invalidar el límite de una aplicación ASP.NET Core MVC es usar el atributo [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) en un método de acción:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Este es un ejemplo que muestra cómo configurar la restricción para toda la aplicación y todas las solicitudes:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

Puede invalidar la configuración en una solicitud específica de middleware:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
Se produce una excepción si intenta configurar el límite de una solicitud después de que la aplicación haya empezado a leer la solicitud. Hay una propiedad `IsReadOnly` que indica si la propiedad `MaxRequestBodySize` tiene el estado de solo lectura, lo que significa que es demasiado tarde para configurar el límite.

**Velocidad mínima de los datos del cuerpo de la solicitud**

Kestrel comprueba cada segundo si los datos entran a la velocidad especificada en bytes por segundo. Si la velocidad está por debajo del mínimo, se agota el tiempo de espera de la conexión. El período de gracia es la cantidad de tiempo que Kestrel da al cliente para aumentar su velocidad de envío hasta el mínimo; no se comprueba la velocidad durante ese tiempo. Este período de gracia permite evitar que se interrumpan las conexiones que inicialmente están enviando datos a una velocidad lenta debido a un inicio lento de TCP.

La velocidad mínima predeterminada es 240 bytes por segundo, con un período de gracia de 5 segundos.

También se aplica una velocidad mínima a la respuesta. El código para establecer el límite de solicitudes y el límite de respuestas es el mismo, salvo que tienen `RequestBody` o `Response` en los nombres de propiedad y de interfaz. 

Este es un ejemplo que muestra cómo configurar las velocidades de datos mínimas en *Program.cs*:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

Puede configurar las velocidades por solicitud de middleware:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

Para saber más sobre otras opciones de Kestrel, vea estas clases:

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para más información sobre las opciones de Kestrel, vea [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) (Clase KestrelServerOptions).

---

### <a name="endpoint-configuration"></a>Configuración de punto de conexión

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

De forma predeterminada, ASP.NET Core enlaza a `http://localhost:5000`. Configure los prefijos de URL y los puertos en los que escuchará Kestrel mediante una llamada a los métodos `Listen` o `ListenUnixSocket` en `KestrelServerOptions`. (`UseUrls`, el argumento de línea de comandos `urls` y la variable de entorno ASPNETCORE_URLS también funcionan pero tienen las limitaciones que se indican [más adelante en este artículo](#useurls-limitations)).

**Enlazar a un socket TCP**

El método `Listen` se enlaza a un socket TCP y una lambda de opciones permite configurar un certificado SSL:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

Observe cómo este ejemplo configura SSL para un punto de conexión determinado mediante el uso de [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs). Puede usar la misma API para configurar otras opciones de Kestrel para puntos de conexión determinados.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**Enlazar a un socket de Unix**

Puede escuchar en un socket de Unix para mejorar el rendimiento con Nginx, tal como se muestra en este ejemplo:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**Puerto 0**

Si especifica el número de puerto 0, Kestrel se enlaza de forma dinámica a un puerto disponible. En el ejemplo siguiente se muestra cómo determinar qué puerto Kestrel está realmente enlazado a un runtime:

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**Limitaciones de UseUrls**

Puede configurar puntos de conexión mediante una llamada al método `UseUrls` o usando el argumento de línea de comandos `urls` o la variable de entorno ASPNETCORE_URLS. Estos métodos son útiles si quiere que el código funcione con servidores distintos de Kestrel. Pero tenga en cuenta estas limitaciones:

* No se puede usar SSL con estos métodos.
* Si usa los métodos `Listen` y `UseUrls`, los puntos de conexión `Listen` invalidan los puntos de conexión `UseUrls`.

**Configuración de puntos de conexión para IIS**

Si usa IIS, los enlaces de URL para IIS invalidan todos los enlaces que se establecen mediante una llamada a `Listen` o `UseUrls`. Para más información, vea [Introduction to ASP.NET Core Module](aspnet-core-module.md) (Introducción al módulo ASP.NET Core).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

De forma predeterminada, ASP.NET Core enlaza a `http://localhost:5000`. Puede configurar los prefijos de URL y los puertos en los que escuchará Kestrel mediante el método de extensión `UseUrls`, el argumento de línea de comandos `urls` o el sistema de configuración de ASP.NET Core. Para más información sobre estos métodos, vea [Hosting](../../fundamentals/hosting.md) (Hospedaje en ASP.NET Core). Para más información sobre el funcionamiento de enlace de URL cuando se usa IIS como proxy inverso, vea [ASP.NET Core Module](aspnet-core-module.md) (Introducción al módulo ASP.NET Core). 

---

### <a name="url-prefixes"></a>Prefijos de URL

Si se llama a `UseUrls` o se usa el argumento de línea de comandos `urls` o una variable de entorno ASPNETCORE_URLS, los prefijos de URL pueden tener cualquiera de estos formatos. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Solo son válidos los prefijos de URL HTTP; Kestrel no admite SSL al configurar enlaces de URL mediante el uso de `UseUrls`.

* Dirección IPv4 con número de puerto

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 es un caso especial que enlaza a todas las direcciones IPv4.


* Dirección IPv6 con número de puerto

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [::] es el equivalente de IPv4 0.0.0.0 para IPv6.


* Nombre de host con número de puerto

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Los nombres de host * y + no son especiales. Todo lo que no sea una dirección IP o un "localhost" reconocido se enlazará a todas las direcciones IP de IPv6 y IPv4. Si tiene que enlazar distintos nombres de host a distintas aplicaciones ASP.NET Core en el mismo puerto, use [HTTP.sys](httpsys.md) o un servidor proxy inverso, como IIS, Nginx o Apache.

* Nombre de "localhost" con el número de puerto o la IP de bucle invertido con el número de puerto

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Cuando se especifica `localhost`, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6. Si el puerto solicitado lo está usando otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar. Si ninguna de estas interfaces de bucle invertido está disponible por cualquier otra razón (normalmente porque no se admite IPv6), Kestrel registra una advertencia. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* Dirección IPv4 con número de puerto

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 es un caso especial que enlaza a todas las direcciones IPv4.


* Dirección IPv6 con número de puerto

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [::] es el equivalente de IPv4 0.0.0.0 para IPv6.


* Nombre de host con número de puerto

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Los nombres de host \* y + no son especiales. Todo lo que no sea una dirección IP o un "localhost" reconocido se enlaza a todas las direcciones IP de IPv6 e IPv4. Si tiene que enlazar distintos nombres de host a distintas aplicaciones ASP.NET Core en el mismo puerto, use [WebListener](weblistener.md) o un servidor proxy inverso, como IIS, Nginx o Apache.

* Nombre de "localhost" con el número de puerto o la IP de bucle invertido con el número de puerto

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Cuando se especifica `localhost`, Kestrel intenta enlazar a las interfaces de bucle invertido de IPv4 e IPv6. Si el puerto solicitado lo está usando otro servicio en cualquier interfaz de bucle invertido, Kestrel no se puede iniciar. Si ninguna de estas interfaces de bucle invertido está disponible por cualquier otra razón (normalmente porque no se admite IPv6), Kestrel registra una advertencia. 

* Socket de UNIX

  ```
  http://unix:/run/dan-live.sock  
  ```

**Puerto 0**

Si especifica el número de puerto 0, Kestrel se enlaza de forma dinámica a un puerto disponible. El enlace al puerto 0 está permitido para cualquier nombre de host o IP excepto para el nombre `localhost`.

En el ejemplo siguiente se muestra cómo determinar qué puerto Kestrel está realmente enlazado a un runtime:

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**Prefijos de URL para SSL**

No olvide incluir los prefijos de URL con `https:` si se llama al método de extensión `UseHttps`, tal y como se muestra aquí.

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
> HTTP y HTTPS no se pueden hospedar en el mismo puerto.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, vea los siguientes recursos:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Aplicación de ejemplo para 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Código fuente de Kestrel](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Aplicación de ejemplo para 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Código fuente de Kestrel](https://github.com/aspnet/KestrelHttpServer)

---
