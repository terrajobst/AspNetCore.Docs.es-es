---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Creación de una aplicación de cliente de OData v4 (C#) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 4d62a64006e2a500e0379419dbebe7ddff16fba5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828531"
---
<a name="create-an-odata-v4-client-app-c"></a>Creación de una aplicación de cliente de OData v4 (C#)
====================
por [Mike Wasson](https://github.com/MikeWasson)

En el tutorial anterior, creó un servicio básico de OData que admite operaciones CRUD. Ahora vamos a crear un cliente para el servicio.

Inicie una nueva instancia de Visual Studio y cree un nuevo proyecto de aplicación de consola. En el **nuevo proyecto** cuadro de diálogo, seleccione **instalado** &gt; **plantillas** &gt; **Visual C#** &gt; **Windows Desktop**y seleccione el **aplicación de consola** plantilla. Denomine el proyecto &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> También puede agregar la aplicación de consola a la misma solución de Visual Studio que contiene el servicio de OData.


## <a name="install-the-odata-client-code-generator"></a>Instalar el generador de código de cliente de OData

Desde el **herramientas** menú, seleccione **extensiones y actualizaciones**. Seleccione **Online** &gt; **Galería de Visual Studio**. En el cuadro de búsqueda, busque &quot;generador de código de cliente de OData&quot;. Haga clic en **descargar** para instalar la extensión VSIX. Puede que deba reiniciar Visual Studio.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Ejecuta el servicio de OData localmente

Ejecute el proyecto ProductService desde Visual Studio. De forma predeterminada, Visual Studio inicia un explorador en la raíz de la aplicación. Tenga en cuenta el identificador URI; lo necesitará en el paso siguiente. Deje la aplicación en ejecución.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Si coloca ambos proyectos en la misma solución, asegúrese de ejecutar el proyecto ProductService sin depuración. En el paso siguiente, deberá mantener el servicio se está ejecutando mientras modifica el proyecto de aplicación de consola.


## <a name="generate-the-service-proxy"></a>Generar al Proxy de servicio

El proxy de servicio es una clase .NET que define los métodos para acceder al servicio de OData. El proxy traduce llamadas de método en las solicitudes HTTP. Creará la clase de proxy mediante la ejecución de un [plantilla T4](https://msdn.microsoft.com/library/bb126445.aspx).

Haga clic en el proyecto. Seleccione **agregar** &gt; **nuevo elemento**.

![](create-an-odata-v4-client-app/_static/image5.png)

En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **elementos de Visual C#** &gt; **código** &gt; **cliente de OData**. Nombre de la plantilla &quot;ProductClient.tt&quot;. Haga clic en **agregar** y haga clic en a través de la advertencia de seguridad.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

En este momento, obtendrá un error, que puede pasar por alto. Visual Studio ejecuta automáticamente la plantilla, pero necesita algunos valores de configuración de la plantilla de primera.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Abra el archivo ProductClient.odata.config. En el `Parameter` elemento, pegue el URI desde el proyecto ProductService (paso anterior). Por ejemplo:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Vuelva a ejecutar la plantilla. En el Explorador de soluciones, haga clic el archivo ProductClient.tt y seleccione **ejecutar herramienta personalizada**.

La plantilla crea un archivo de código denominado ProductClient.cs que define al proxy. Al desarrollar la aplicación, si cambia el punto de conexión de OData, ejecutar la plantilla de nuevo para actualizar al servidor proxy.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Usar al Proxy de servicio para llamar al servicio de OData

Abra el archivo Program.cs y reemplace el código reutilizable con lo siguiente.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Reemplace el valor de *serviceUri* con el URI del servicio de antes.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Al ejecutar la aplicación, debe mostrar lo siguiente:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
