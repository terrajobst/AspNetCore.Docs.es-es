---
uid: signalr/overview/advanced/dependency-injection
title: "Inserción de dependencias en SignalR | Documentos de Microsoft"
author: MikeWasson
description: "Versiones de software emplea en este tema Visual Studio 2013 .NET 4.5 SignalR las versiones anteriores de la versión 2 de este tema para obtener información acerca de las versiones anteriores de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-signalr"></a>Inserción de dependencias en SignalR
====================
por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software que se usa en este tema
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versión 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema
> 
> Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


Inserción de dependencias es una manera de quitar las dependencias codificadas de forma rígida entre objetos, lo que facilita para sustituir las dependencias de un objeto, ya sea para realizar pruebas (con objetos ficticios) o para cambiar el comportamiento de tiempo de ejecución. Este tutorial muestra cómo realizar la inserción de dependencias en concentradores de SignalR. También muestra cómo utilizar los contenedores de IoC con SignalR. Un contenedor de IoC es un marco de trabajo general para la inyección de dependencia.

## <a name="what-is-dependency-injection"></a>¿Qué es la inyección de dependencia?

Omitir esta sección si ya está familiarizado con la inserción de dependencias.

*Inyección de dependencia* (DI) es un modelo donde los objetos no son responsables de crear sus propias dependencias. Este es un ejemplo simple para motivar a DI. Suponga que tiene un objeto que deba registrar los mensajes. Puede definir una interfaz de registro:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

En el objeto, puede crear un `ILogger` para registrar los mensajes:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Esto funciona, pero no es el mejor diseño. Si desea reemplazar `FileLogger` con otro `ILogger` implementación, tendrá que modificar `SomeComponent`. Suponiendo que un lote de otros objetos `FileLogger`, deberá cambiar todas ellas. O si decide hacer `FileLogger` un singleton, que también necesite realizar cambios en toda la aplicación.

Es un enfoque más adecuado para "Insertar" un `ILogger` en el objeto, por ejemplo, mediante un argumento de constructor:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Ahora, el objeto no es responsable de seleccionar que `ILogger` a usar. También puede swich `ILogger` implementaciones sin cambiar los objetos que dependen de él.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Este patrón se denomina [inyección de constructor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Otro patrón es la inserción de establecedor, donde establecer la dependencia a través de un método de establecedor o propiedad.

## <a name="simple-dependency-injection-in-signalr"></a>Inyección de dependencia simple en SignalR

Considere la posibilidad de la aplicación de Chat del tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md). Esta es la clase de base de datos central de esa aplicación:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Suponga que desea almacenar los mensajes de chat en el servidor antes de enviarlos. Puede definir una interfaz que abstrae esta funcionalidad y usar DI para insertar la interfaz en la `ChatHub` clase.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

El único problema es que una aplicación de SignalR no crea directamente de los centros; SignalR las creará automáticamente. De forma predeterminada, SignalR espera que una clase de concentrador para tener un constructor sin parámetros. Sin embargo, fácilmente puede registrar una función para crear instancias de base de datos central y usar esta función para realizar DI. Registre la función mediante una llamada a **GlobalHost.DependencyResolver.Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Ahora SignalR invocará esta función anónima cuando sea necesario crear un `ChatHub` instancia.

## <a name="ioc-containers"></a>Contenedores de IoC

El código anterior es correcto para los casos más sencillos. Pero todavía tenía que escribir lo siguiente:

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

En una aplicación compleja con muchas dependencias, deberá escribir una gran cantidad de este código de "conexión". Este código puede ser difícil de mantener, especialmente si se anidan las dependencias. También es difícil realizar pruebas unitarias.

Una solución consiste en usar un contenedor de IoC. Un contenedor de IoC es un componente de software que se encarga de administrar las dependencias. Registrar tipos con el contenedor y, a continuación, utilice el contenedor para crear objetos. El contenedor imagina automáticamente las relaciones de dependencia. Muchos de los contenedores de IoC también le permiten controlar aspectos como la duración de los objetos y el ámbito.

> [!NOTE]
> "IoC" es el acrónimo "de inversión de control", que es un patrón general donde un marco de trabajo llama al código de la aplicación. Un contenedor de IoC construye los objetos para usted, que "invierte" el flujo de control normal.


## <a name="using-ioc-containers-in-signalr"></a>Uso de contenedores de IoC en SignalR

La aplicación de Chat probablemente es demasiado simple para beneficiarse de un contenedor de IoC. En su lugar, echemos un vistazo a la [indicador de cotizaciones](http://nuget.org/packages/microsoft.aspnet.signalr.sample) ejemplo.

El ejemplo de indicador de cotizaciones define dos clases principales:

- `StockTickerHub`: La clase de base de datos central, que administra las conexiones de cliente.
- `StockTicker`: Un singleton que contiene los precios de las acciones y los actualiza periódicamente.

`StockTickerHub`contiene una referencia a la `StockTicker` singleton, mientras que `StockTicker` contiene una referencia a la **IHubConnectionContext** para el `StockTickerHub`. Usa esta interfaz para comunicarse con `StockTickerHub` instancias. (Para obtener más información, consulte [Server difusión con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)

Podemos usar un contenedor de IoC para resolver estas dependencias un poco. En primer lugar, vamos a simplificar la `StockTickerHub` y `StockTicker` clases. En el código siguiente, he comentadas las partes que no es necesario.

Quite el constructor sin parámetros de `StockTickerHub`. En su lugar, usaremos siempre DI para crear el concentrador.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

Para el indicador de cotizaciones, quite la instancia de singleton. Más adelante, vamos a usar el contenedor de IoC para controlar la duración de indicador de cotizaciones. Además, marque el constructor público.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

A continuación, se puede refactorizar el código mediante la creación de una interfaz para `StockTicker`. Vamos a usar esta interfaz para desacoplar el `StockTickerHub` desde el `StockTicker` clase.

Visual Studio hace que este tipo de refactorización la forma más fácil. Abra el archivo StockTicker.cs, haga doble clic en el `StockTicker` declaración de clase y seleccione **refactorizar** ... **Extraer interfaz**.

![](dependency-injection/_static/image1.png)

En el **Extraer interfaz** cuadro de diálogo, haga clic en **seleccionar todo**. Deje los demás valores predeterminados. Haga clic en **Aceptar**.

![](dependency-injection/_static/image2.png)

Visual Studio crea una nueva interfaz denominada `IStockTicker`y también se cambia `StockTicker` pueden derivar `IStockTicker`.

Abra el archivo IStockTicker.cs y cambiar la interfaz a **público**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

En el `StockTickerHub` clase, cambie las dos instancias de `StockTicker` a `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Crear un `IStockTicker` interfaz no es estrictamente necesaria, pero quise mostrarle cómo DI puede ayudar a reducir el acoplamiento entre los componentes de la aplicación.

## <a name="add-the-ninject-library"></a>Agregue la biblioteca de Ninject

Hay muchos contenedores de IoC de código abierto para. NET. En este tutorial, utilizará [Ninject](http://www.ninject.org/). (Incluyen otras bibliotecas populares [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), y [StructureMap ](http://docs.structuremap.net).)

Use el Administrador de paquetes de NuGet para instalar el [Ninject biblioteca](https://nuget.org/packages/Ninject/3.0.1.10). En Visual Studio, desde la **herramientas** menú, seleccione **Administrador de paquetes de biblioteca** | **Package Manager Console**. En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Reemplace a la resolución de dependencia de SignalR

Para utilizar Ninject en SignalR, cree una clase que deriva de **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Esta clase reemplaza la **GetService** y **GetServices** métodos de **DefaultDependencyResolver**. SignalR llama a estos métodos para crear varios objetos en tiempo de ejecución, incluidas las instancias de base de datos central, así como varios servicios utilizados internamente por SignalR.

- El **GetService** método crea una única instancia de un tipo. Invalide este método para llamar el kernel Ninject **TryGet** método. Si el método devuelve null, revertir a la resolución predeterminada.
- El **GetServices** método crea una colección de objetos de un tipo especificado. Invalide este método para concatenar los resultados de Ninject con los resultados de la resolución predeterminada.

## <a name="configure-ninject-bindings"></a>Configurar los enlaces de Ninject

Ahora vamos a usar Ninject para declarar enlaces de tipo.

Abra la clase de la aplicación Startup.cs (uno creado manualmente según las instrucciones de paquete de `readme.txt`, o que se creó mediante la adición de autenticación para el proyecto). En el `Startup.Configuration` (método), crear el contenedor Ninject, que llama a Ninject el *kernel*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Cree una instancia de la resolución de dependencia personalizados:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Cree un enlace para `IStockTicker` como se indica a continuación:

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

Este código está diciendo dos cosas. En primer lugar, siempre que la aplicación necesita una `IStockTicker`, el kernel debe crearse una instancia de `StockTicker`. Segundo, la `StockTicker` clase debe ser otra creada como un objeto singleton. Ninject creará una instancia del objeto y devolver la misma instancia para cada solicitud.

Cree un enlace para **IHubConnectionContext** como se indica a continuación:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Este creatres código una función anónima que devuelve un **IHubConnection**. El **WhenInjectedInto** método indica Ninject para usar esta función sólo cuando se crea `IStockTicker` instancias. La razón es que crea SignalR **IHubConnectionContext** instancias internamente, y no desea reemplazar el modo en que crea SignalR. Esta función solo se aplica a nuestro `StockTicker` clase.

Pasar la resolución de dependencia en el **MapSignalR** método mediante la adición de una configuración de base de datos central:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Actualice el método Startup.ConfigureSignalR en la clase de inicio del ejemplo con el nuevo parámetro:

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Ahora va a utilizar el solucionador especificado en SignalR **MapSignalR**, en lugar de la resolución predeterminada.

Esta es la lista de código completa `Startup.Configuration`.

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

Presione F5 para ejecutar la aplicación de indicador de cotizaciones en Visual Studio. En la ventana del explorador, vaya a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

La aplicación tiene exactamente la misma funcionalidad que antes. (Para obtener una descripción, consulte [Server difusión con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) No hemos cambiamos el comportamiento; acaba de crear más fácil probar, mantener y desarrollar el código.
