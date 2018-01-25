---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: "Crear una aplicación de cliente de OData v4 (C#) | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 51a3c7b9c5b6525d6d82b9a45910f58b71268b7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="create-an-odata-v4-client-app-c"></a>Crear una aplicación de cliente de OData v4 (C#)
====================
por [Mike Wasson](https://github.com/MikeWasson)

En el tutorial anterior, creó un servicio básico de OData que admite operaciones CRUD. Ahora vamos a crear un cliente para el servicio.

Inicie una nueva instancia de Visual Studio y cree un nuevo proyecto de aplicación de consola. En el **nuevo proyecto** cuadro de diálogo, seleccione **instalado** &gt; **plantillas** &gt; **Visual C#** &gt; **Windows Desktop**y seleccione el **aplicación de consola** plantilla. Denomine el proyecto &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> También puede agregar la aplicación de consola a la misma solución de Visual Studio que contiene el servicio de OData.


## <a name="install-the-odata-client-code-generator"></a>Instalar el generador de código de cliente de OData

Desde el **herramientas** menú, seleccione **extensiones y actualizaciones**. Seleccione **en línea** &gt; **Galería de Visual Studio**. En el cuadro de búsqueda, busque &quot;generador de código de cliente de OData&quot;. Haga clic en **descargar** para instalar la extensión VSIX. Puede que deba reiniciar Visual Studio.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Ejecutar el servicio de OData localmente

Ejecute el proyecto ProductService desde Visual Studio. De forma predeterminada, Visual Studio inicia un explorador a la raíz de la aplicación. Tenga en cuenta el identificador URI; lo necesitará en el paso siguiente. Deje la aplicación en ejecución.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Si coloca ambos proyectos en la misma solución, asegúrese de ejecutar el proyecto ProductService sin depuración. En el paso siguiente, debe mantener el servicio se ejecuta mientras modifica el proyecto de aplicación de consola.


## <a name="generate-the-service-proxy"></a>Generar al Proxy de servicio

El proxy de servicio es una clase .NET que define los métodos para tener acceso al servicio de OData. El proxy convierte las llamadas de método en las solicitudes HTTP. Se creará la clase de proxy mediante la ejecución de un [plantilla T4](https://msdn.microsoft.com/library/bb126445.aspx).

Haga clic en el proyecto. Seleccione **agregar** &gt; **nuevo elemento**.

![](create-an-odata-v4-client-app/_static/image5.png)

En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **elementos de Visual C#** &gt; **código** &gt; **cliente de OData**. Nombre de la plantilla &quot;ProductClient.tt&quot;. Haga clic en **agregar** y haga clic en a través de la advertencia de seguridad.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

En este momento, obtendrá un error, que puede pasar por alto. Visual Studio ejecuta automáticamente la plantilla, pero la plantilla necesita algunos valores de configuración primero.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Abra el archivo ProductClient.odata.config. En el `Parameter` elemento, pegar el URI desde el proyecto ProductService (paso anterior). Por ejemplo:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Vuelva a ejecutar la plantilla. En el Explorador de soluciones, haga clic en el archivo de ProductClient.tt y seleccione **ejecutar herramienta personalizada**.

La plantilla crea un archivo de código denominado ProductClient.cs que define al servidor proxy. Cuando desarrolle la aplicación, si cambia el extremo de OData, ejecutar la plantilla de nuevo para actualizar al servidor proxy.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Usar al Proxy de servicio para llamar al servicio de OData

Abra el archivo Program.cs y reemplace el código reutilizable con lo siguiente.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Sustituya el valor de *serviceUri* con el URI del servicio de antes.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Cuando se ejecuta la aplicación, debe devolver el siguiente:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
