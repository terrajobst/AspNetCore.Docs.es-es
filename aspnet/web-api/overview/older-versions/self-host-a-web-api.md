---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Autohospedar ASP.NET Web API 1 (C#) | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API no requiere IIS. Puede probarlo internamente una API web en su propio proceso de host. Este tutorial muestra cómo hospedar una API web dentro de una aplicación de consola...
ms.author: riande
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: cac0d5aeaf49f45051d062935e0e9207ce59c7eb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829125"
---
<a name="self-host-aspnet-web-api-1-c"></a>Autohospedar ASP.NET Web API 1 (C#)
====================
por [Mike Wasson](https://github.com/MikeWasson)

> ASP.NET Web API no requiere IIS. Puede probarlo internamente una API web en su propio proceso de host. Este tutorial muestra cómo hospedar una API web dentro de una aplicación de consola.
> 
> **Las nuevas aplicaciones deben usar OWIN para autohospedaje API Web.** Consulte [Use OWIN para autohospedaje de ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Web API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Crear el proyecto de aplicación de consola

Inicie Visual Studio y seleccione **nuevo proyecto** desde el **iniciar** página. O bien, en el **archivo** menú, seleccione **New** y, a continuación, **proyecto**.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo. En **Visual C#**, seleccione **Windows**. En la lista de plantillas de proyecto, seleccione **aplicación de consola**. Denomine el proyecto &quot;SelfHost&quot; y haga clic en **Aceptar**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Establezca la plataforma de destino (Visual Studio 2010)

Si utiliza Visual Studio 2010, cambie la plataforma de destino a .NET Framework 4.0. (De forma predeterminada, los destinos de la plantilla de proyecto la [perfil de .net Framework Client](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

En el Explorador de soluciones, haga clic en el proyecto y seleccione **propiedades**. En el **.NET framework de destino** lista desplegable Mostrar, cambiar la plataforma de destino a .NET Framework 4.0. Cuando se le pida para aplicar el cambio, haga clic en **Sí**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Instalar a Administrador de paquetes de NuGet

El Administrador de paquetes de NuGet es la manera más fácil de agregar los ensamblados de la API Web a un proyecto que no sean ASP.NET.

Para comprobar si está instalado el Administrador de paquetes de NuGet, haga clic en el **herramientas** menú en Visual Studio. Si ve un elemento de menú denominado **Administrador de paquetes de biblioteca**, tendrá que Administrador de paquetes de NuGet.

Para instalar el Administrador de paquetes de NuGet:

1. Inicie Visual Studio.
2. Desde el **herramientas** menú, seleccione **extensiones y actualizaciones**.
3. En el **extensiones y actualizaciones** cuadro de diálogo, seleccione **Online**.
4. Si no ve "Administrador de paquetes de NuGet", escriba "Administrador de paquetes de nuget" en el cuadro de búsqueda.
5. Seleccione el Administrador de paquetes de NuGet y haga clic en **descargar**.
6. Una vez finalizada la descarga, se le pedirá que instale.
7. Una vez finalizada la instalación, se le podrían reiniciar Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Agregue el paquete de API NuGet Web

Después de instalar Administrador de paquetes de NuGet, agregue el paquete de Web API Self al proyecto.

1. Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**. *Tenga en cuenta*: si no se ve este menú de elemento, asegúrese de que ese administrador de paquetes de NuGet instalado correctamente.
2. Seleccione **administrar paquetes de NuGet para la solución...**
3. En el **administrar paquetes de NuGet** cuadro de diálogo, seleccione **Online**.
4. En el cuadro de búsqueda, escriba &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. Seleccione el paquete de Host automático de ASP.NET Web API y haga clic en **instalar**.
6. Una vez instalado el paquete, haga clic en **cerrar** para cerrar el cuadro de diálogo.

> [!NOTE]
> Asegúrese de instalar el paquete denominado Microsoft.AspNet.WebApi.SelfHost, no AspNetWebApi.SelfHost.


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Crear el modelo y controlador

Este tutorial usa las mismas clases de modelo y controlador como el [Introducción](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.

Agregue una clase pública denominada `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Agregue una clase pública denominada `ProductsController`. Derive esta clase de **System.Web.Http.ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Para obtener más información sobre el código de este controlador, consulte el [Introducción](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial. Este controlador define tres acciones GET:

| Identificador URI | Descripción |
| --- | --- |
| productos/api / | Obtener una lista de todos los productos. |
| /API/productos/*Id.* | Obtener un producto por identificador. |
| / API/productos /? categoría =*categoría* | Obtener una lista de productos por categoría. |

## <a name="host-the-web-api"></a>Hospedar la API Web

Abra el archivo Program.cs y agregue las siguientes instrucciones using:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Agregue el código siguiente a la **programa** clase.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(Opcional) Agregar una reserva de Namespace de direcciones URL de HTTP

Esta aplicación escucha a `http://localhost:8080/`. De forma predeterminada, escucha en una dirección HTTP determinada requiere privilegios de administrador. Al ejecutar el tutorial, por lo tanto, puede obtener este error: "HTTP no pudo registrar la dirección URL http://+:8080/" hay dos formas de evitar este error:

- Ejecutar Visual Studio con permisos de administrador con privilegios elevados, o
- Utilice Netsh.exe para conceder los permisos de cuenta para reservar la dirección URL.

Para usar Netsh.exe, abra un símbolo del sistema con privilegios de administrador y escriba el comando siguiente: comando siguiente:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

donde *máquina\nombredeusuario* es su cuenta de usuario.

Cuando haya terminado de autohospedaje, asegúrese de eliminar la reserva:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Llame a la API Web desde una aplicación cliente (C#)

Vamos a escribir una aplicación de consola simple que llama a la API web.

Agregue un nuevo proyecto de aplicación de consola a la solución:

- En el Explorador de soluciones, haga clic en la solución y seleccione **Agregar nuevo proyecto**.
- Crear una nueva aplicación de consola denominada &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Use el Administrador de paquetes de NuGet para agregar el paquete de bibliotecas de núcleo de ASP.NET Web API:

- En el menú Herramientas, seleccione **Administrador de paquetes de biblioteca**.
- Seleccione **administrar paquetes de NuGet para la solución...**
- En el **administrar paquetes de NuGet** cuadro de diálogo, seleccione **Online**.
- En el cuadro de búsqueda, escriba &quot;Microsoft.AspNet.WebApi.Client&quot;.
- Seleccione el paquete de bibliotecas de cliente de Microsoft ASP.NET Web API y haga clic en **instalar**.

Agregue una referencia en ClientApp al proyecto SelfHost:

- En el Explorador de soluciones, haga clic en el proyecto ClientApp.
- Seleccione **Agregar referencia**.
- En el **Administrador de referencias** cuadro de diálogo, en **solución**, seleccione **proyectos**.
- Seleccione el proyecto SelfHost.
- Haga clic en **Aceptar**.

![](self-host-a-web-api/_static/image6.png)

Abra el archivo Client/Program.cs. Agregue el siguiente **mediante** instrucción:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Agregar una variable static **HttpClient** instancia:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Agregue los métodos siguientes para obtener una lista de todos los productos, un producto por el identificador de la lista y lista de productos por categoría.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Cada uno de estos métodos sigue el mismo patrón:

1. Llame a **HttpClient.GetAsync** para enviar una solicitud GET al URI adecuado.
2. Llame a **HttpResponseMessage.EnsureSuccessStatusCode**. Este método produce una excepción si el estado de respuesta HTTP es un código de error.
3. Llame a **ReadAsAsync&lt;T&gt;**  para deserializar un tipo CLR de la respuesta HTTP. Este método es un método de extensión definido en **System.Net.Http.HttpContentExtensions**.

El **GetAsync** y **ReadAsAsync** métodos son asincrónicos. Devuelven **tarea** objetos que representan la operación asincrónica. Obteniendo el **resultado** propiedad bloquea el subproceso hasta que se complete la operación.

Para obtener más información sobre el uso de HttpClient, incluida la forma de realizar llamadas sin bloqueo, consulte [una llamada a una Web API desde un cliente .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Antes de llamar a estos métodos, establezca la propiedad BaseAddress en la instancia de HttpClient para "`http://localhost:8080`". Por ejemplo:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Esto debería mostrar lo siguiente. (Recuerde que se ejecute la aplicación SelfHost primero).

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
