---
uid: signalr/overview/advanced/dependency-injection
title: Inserción de dependencias en SignalR | Microsoft Docs
author: MikeWasson
description: Las versiones de software usan en este tema Visual Studio 2013 .NET 4.5 SignalR las versiones anteriores de la versión 2 de este tema para obtener información acerca de las versiones anteriores de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 0b4276d25c999c2a78864a856f7f3de233c9ac87
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397765"
---
<a name="dependency-injection-in-signalr"></a>Inserción de dependencias en SignalR
====================
por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software que se usa en este tema
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Versión 2 de SignalR
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema.
> 
> Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


Inserción de dependencias es una manera de quitar las dependencias codificadas de forma rígida entre objetos, facilitando la tarea para reemplazar las dependencias de un objeto, ya sea por pruebas (con objetos ficticios) o para cambiar el comportamiento de tiempo de ejecución. Este tutorial muestra cómo realizar la inserción de dependencias en concentradores de SignalR. También muestra cómo usar contenedores de IoC con SignalR. Un contenedor de IoC es un marco general para la inserción de dependencias.

## <a name="what-is-dependency-injection"></a>¿Qué es la inserción de dependencias?

Omitir esta sección si ya está familiarizado con la inserción de dependencias.

*Inserción de dependencias* (DI) es un patrón donde los objetos no son responsables de crear sus propias dependencias. Este es un ejemplo sencillo para motivar DI. Suponga que tiene un objeto que se debe registrar los mensajes. Podría definir una interfaz de registro:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

En el objeto, puede crear un `ILogger` para registrar los mensajes:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Esto funciona, pero no es el mejor diseño. Si desea reemplazar `FileLogger` con otra `ILogger` implementación, tendrá que modificar `SomeComponent`. Si suponemos que una gran cantidad de otros objetos utilizan `FileLogger`, deberá cambiar todas ellas. O si decide realizar `FileLogger` un singleton, también tendrá que realizar cambios en toda la aplicación.

Un mejor enfoque es "Insertar" un `ILogger` en el objeto, por ejemplo, mediante un argumento de constructor:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Ahora, el objeto no es responsable de seleccionarlos `ILogger` a usar. Puede swich `ILogger` implementaciones sin cambiar los objetos que dependen de él.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Este patrón se denomina [inserción del constructor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Otro patrón es la inserción del establecedor, donde establece la dependencia a través de un método establecedor o una propiedad.

## <a name="simple-dependency-injection-in-signalr"></a>Inserción de dependencias simple en SignalR

Considere la posibilidad de la aplicación de Chat del tutorial [Introducción a SignalR](../getting-started/tutorial-getting-started-with-signalr.md). Esta es la clase de hub desde la aplicación:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Suponga que desea almacenar los mensajes de chat en el servidor antes de enviarlos. Es posible que defina una interfaz que abstrae esta funcionalidad y usar DI para insertar la interfaz en el `ChatHub` clase.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

El único problema es que una aplicación de SignalR no crea directamente hubs; SignalR crea automáticamente. De forma predeterminada, SignalR espera una clase de concentrador para tener un constructor sin parámetros. Sin embargo, fácilmente puede registrar una función para crear instancias de concentrador y usar esta función para realizar la inserción de dependencias. Registre la función mediante una llamada a **GlobalHost.DependencyResolver.Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Ahora SignalR invocará esta función anónima cada vez que necesita para crear un `ChatHub` instancia.

## <a name="ioc-containers"></a>Contenedores de IoC

El código anterior está bien para los casos más sencillos. Pero todavía tenía que escribir lo siguiente:

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

En una aplicación compleja con muchas dependencias, debe escribir una gran cantidad de este código de "conexión". Este código puede ser difícil de mantener, especialmente si se anidan las dependencias. También es difícil realizar pruebas unitarias.

Una solución consiste en usar un contenedor de IoC. Un contenedor de IoC es un componente de software que se encarga de administrar las dependencias. Registrar tipos con el contenedor y, a continuación, usar el contenedor para crear objetos. El contenedor calcula automáticamente las relaciones de dependencia. Muchos de los contenedores de IoC también permiten controlar aspectos como la duración del objeto y el ámbito.

> [!NOTE]
> "IoC" es el acrónimo "inversión de control", que es un patrón general que llama un marco de trabajo en el código de la aplicación. Un contenedor de IoC construye los objetos, que "invierte" el flujo de control normal.


## <a name="using-ioc-containers-in-signalr"></a>Uso de contenedores de IoC en SignalR

La aplicación de Chat probablemente es demasiado simple para beneficiarse de un contenedor de IoC. En su lugar, echemos un vistazo a la [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) ejemplo.

El ejemplo StockTicker define dos clases principales:

- `StockTickerHub`: La clase de hub, que administra las conexiones de cliente.
- `StockTicker`: Un singleton que contiene los precios de las acciones y los actualiza periódicamente.

`StockTickerHub` contiene una referencia a la `StockTicker` singleton, mientras que `StockTicker` contiene una referencia a la **IHubConnectionContext** para el `StockTickerHub`. Usa esta interfaz para comunicarse con `StockTickerHub` instancias. (Para obtener más información, consulte [difusión de servidores con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)

Podemos usar un contenedor de IoC ahorró un poco de estas dependencias. En primer lugar, vamos a simplificar la `StockTickerHub` y `StockTicker` clases. En el siguiente código, he comentado las partes que no es necesario.

Quite el constructor sin parámetros de `StockTickerHub`. En su lugar, usaremos siempre DI para crear el centro.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

Para StockTicker, quite la instancia de singleton. Más adelante, vamos a usar el contenedor de IoC para controlar la duración de StockTicker. Además, hacer público el constructor.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

A continuación, podemos refactorizar el código mediante la creación de una interfaz para `StockTicker`. Vamos a usar esta interfaz para desacoplar el `StockTickerHub` desde el `StockTicker` clase.

Visual Studio hace que este tipo de refactorización sencilla. Abra el archivo StockTicker.cs, haga doble clic en el `StockTicker` declaración de clase y seleccione **refactorizar** ... **Extraer interfaz**.

![](dependency-injection/_static/image1.png)

En el **Extraer interfaz** cuadro de diálogo, haga clic en **seleccionar todo**. Deje los demás valores predeterminados. Haga clic en **Aceptar**.

![](dependency-injection/_static/image2.png)

Visual Studio crea una nueva interfaz denominada `IStockTicker`y también cambia `StockTicker` derivar `IStockTicker`.

Abra el archivo IStockTicker.cs y cambiar la interfaz a **pública**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

En el `StockTickerHub` clase, cambie las dos instancias de `StockTicker` a `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Creación de un `IStockTicker` interfaz no es estrictamente necesaria, pero quise mostrarle cómo inserción de dependencias puede ayudar a reducir el acoplamiento entre componentes de la aplicación.

## <a name="add-the-ninject-library"></a>Agregue la biblioteca Ninject

Hay muchos contenedores de IoC de código abierto para. NET. Para este tutorial, utilizaré [Ninject](http://www.ninject.org/). (Incluyen otras bibliotecas populares [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), y [StructureMap ](http://docs.structuremap.net).)

Use el Administrador de paquetes NuGet para instalar el [Ninject biblioteca](https://nuget.org/packages/Ninject/3.0.1.10). En Visual Studio, desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca** | **Package Manager Console**. En la ventana de consola de administrador de paquetes, escriba el siguiente comando:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Reemplace a la resolución de dependencia de SignalR

Para usar Ninject en SignalR, cree una clase que derive de **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Esta clase invalida el **GetService** y **GetServices** métodos de **DefaultDependencyResolver**. SignalR se llama a estos métodos para crear varios objetos en tiempo de ejecución, incluidas las instancias del concentrador, así como varios servicios utilizados internamente por SignalR.

- El **GetService** método crea una instancia única de un tipo. Invalide este método para llamar el kernel Ninject **TryGet** método. Si el método devuelve null, revertir a la resolución predeterminada.
- El **GetServices** método crea una colección de objetos de un tipo especificado. Invalide este método para concatenar los resultados de Ninject con los resultados de la resolución predeterminada.

## <a name="configure-ninject-bindings"></a>Configurar enlaces Ninject

Ahora vamos a usar Ninject para declarar los enlaces de tipo.

Abra la clase de la aplicación Startup.cs (ya sea creados manualmente según las instrucciones del paquete de `readme.txt`, o que se creó mediante la adición de autenticación a su proyecto). En el `Startup.Configuration` método, cree el contenedor Ninject, que llama Ninject el *kernel*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Cree una instancia de la resolución de dependencia personalizada:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Crear un enlace para `IStockTicker` como sigue:

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

Este código indica que dos cosas. En primer lugar, siempre que la aplicación necesita un `IStockTicker`, el kernel debe crear una instancia de `StockTicker`. Segundo, el `StockTicker` clase debe ser un creado como un objeto singleton. Ninject creará una instancia del objeto y devolver la misma instancia para cada solicitud.

Crear un enlace para **IHubConnectionContext** como sigue:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Este creatres código una función anónima que devuelve un **IHubConnection**. El **WhenInjectedInto** método indica Ninject para usar esta función sólo cuando se crea `IStockTicker` instancias. El motivo es que crea SignalR **IHubConnectionContext** instancias internamente, y no queremos invalidar cómo los crea SignalR. Esta función solo se aplica a nuestro `StockTicker` clase.

Pasar la resolución de dependencia en el **MapSignalR** método mediante la adición de una configuración de concentrador:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Actualice el método Startup.ConfigureSignalR en la clase de inicio del ejemplo con el nuevo parámetro:

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Ahora va a utilizar el solucionador especificado en SignalR **MapSignalR**, en lugar de la resolución predeterminada.

Esta es la lista de código completa `Startup.Configuration`.

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

Para ejecutar la aplicación StockTicker en Visual Studio, presione F5. En la ventana del explorador, vaya a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

La aplicación tiene exactamente la misma funcionalidad que antes. (Para obtener una descripción, consulte [difusión de servidores con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Que no hemos cambiado el comportamiento; acaba de crear el código más fácil de probar, mantener y evolucionar.
