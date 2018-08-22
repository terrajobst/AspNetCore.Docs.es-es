---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Introducción a OWIN y Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: f400ec887bdee123084f582d18016fb5ed3f2165
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829620"
---
<a name="getting-started-with-owin-and-katana"></a>Introducción a OWIN y Katana
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Interfaz Web abierta para .NET (OWIN)](http://owin.org/) define una abstracción entre los servidores web de .NET y aplicaciones web. Al desacoplar el servidor web de la aplicación, OWIN resulta más fácil crear middleware para el desarrollo web. NET. Asimismo, OWIN facilita a las aplicaciones web de puerto a otros hosts&#8212;por ejemplo, un hospedaje autosuficiente en un servicio de Windows u otro proceso.

OWIN es una especificación de propiedad de la Comunidad, no es una implementación. El proyecto Katana es un conjunto de componentes de OWIN de código abierto desarrollado por Microsoft. Para obtener una descripción general de OWIN y Katana, consulte [una visión general del proyecto Katana](an-overview-of-project-katana.md). En este artículo, saltará derecha en el código para empezar a trabajar.

Este tutorial se usa [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), pero también puede usar Visual Studio 2012. Algunos de los pasos son diferentes en Visual Studio 2012, que tenga en cuenta a continuación.

## <a name="host-owin-in-iis"></a>Hospedar OWIN en IIS

En esta sección, se hospedarán OWIN en IIS. Esta opción le ofrece la flexibilidad y capacidad de composición de una canalización de OWIN junto con el conjunto de características para adultos de IIS. Con esta opción, se ejecuta la aplicación OWIN en la canalización de solicitudes ASP.NET.

En primer lugar, cree un nuevo proyecto de aplicación Web ASP.NET. (En Visual Studio 2012, utilice el tipo de proyecto de aplicación Web ASP.NET vacía).

![](getting-started-with-owin-and-katana/_static/image1.png)

En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el **vacía** plantilla.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Agregar paquetes de NuGet

A continuación, agregue los paquetes de NuGet necesarios. Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**. En la ventana de consola de administrador de paquetes, escriba el siguiente comando:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Agregue una clase de inicio

A continuación, agregue una clase de inicio OWIN. En el Explorador de soluciones, haga clic en el proyecto y seleccione **agregar**, a continuación, seleccione **nuevo elemento**. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **clase Owin Startup**. Para obtener más información sobre la configuración de la clase de inicio, consulte [detección de clase de inicio de OWIN](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Agregue el código siguiente al método `Startup1.Configuration`:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Este código agrega un simple fragmento de middleware a la canalización OWIN, que se implementa como una función que recibe un **Microsoft.Owin.IOwinContext** instancia. Cuando el servidor recibe una solicitud HTTP, la canalización de OWIN invoca el middleware. El middleware establece el tipo de contenido para la respuesta y escribe el cuerpo de respuesta.

> [!NOTE]
> La plantilla de clase de inicio OWIN está disponible en Visual Studio 2013. Si utiliza Visual Studio 2012, solo tiene que agregar una nueva clase vacía denominada `Startup1`y pegue el código siguiente:


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Ejecutar la aplicación

Presione F5 para comenzar la depuración. Visual Studio abrirá una ventana del explorador a `http://localhost:*port*/`. La página debe tener el siguiente aspecto:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>Autohospedaje de OWIN en una aplicación de consola

Es fácil convertir esta aplicación de hospedaje de IIS para el autohospedaje en un proceso personalizado. Con el alojamiento en IIS, IIS actúa como el servidor HTTP y que el proceso que hospeda el servicio. Con el tipo alojado, crea el proceso de la aplicación y utiliza el **HttpListener** clase como el servidor HTTP.

En Visual Studio, cree una nueva aplicación de consola. En la ventana de consola de administrador de paquetes, escriba el siguiente comando:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Agregar un `Startup1` clase a partir de la parte 1 de este tutorial al proyecto. No es necesario modificar esta clase.

Implementar la aplicación `Main` método tal como se indica a continuación.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Al ejecutar la aplicación de consola, el servidor empieza a escuchar `http://localhost:9000`. Si navega a esta dirección en un explorador web, verá la página "Hello world".

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Agregar diagnósticos OWIN

El paquete Microsoft.Owin.Diagnostics contiene middleware que detecta las excepciones no controladas y muestra una página HTML con los detalles del error. Funciones de esta página de forma similar a la página de error ASP.NET que a veces se denomina el "[pantalla amarilla de la muerte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Al igual que el YSOD, la página de error de Katana es útil durante el desarrollo, pero es una buena práctica para deshabilitarlo en modo de producción.

Para instalar el paquete de diagnóstico en el proyecto, escriba el siguiente comando en la ventana de consola de administrador de paquetes:

`install-package Microsoft.Owin.Diagnostics –Pre`

Cambie el código en su `Startup1.Configuration` método como sigue:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Ahora utilice CTRL + F5 para ejecutar la aplicación sin depuración, para que Visual Studio no se interrumpirá en la excepción. La aplicación comporta igual que antes, hasta que vaya a `http://localhost/fail`, momento en que la aplicación produce la excepción. El middleware de la página de error se detecte la excepción y mostrar una página HTML con información sobre el error. Puede hacer clic en las pestañas para ver la pila de cadena de consulta, cookies, encabezado de solicitud y las variables de entorno OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Pasos siguientes

- 
  [Detección de la clase de inicio OWIN](owin-startup-class-detection.md)
- [Use OWIN para autohospedaje de ASP.NET Web API](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Use OWIN para autohospedaje de SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
