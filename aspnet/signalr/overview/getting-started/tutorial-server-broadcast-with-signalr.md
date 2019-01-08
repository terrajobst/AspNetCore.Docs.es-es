---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutorial: Difusión de servidores con SignalR 2 | Microsoft Docs'
author: tdykstra
description: Este tutorial muestra cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de difusión de servidor.
ms.author: riande
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a6014e604613492db91b2dc6f846c3c73d938d99
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099304"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>Tutorial: Servidor de difusión con SignalR 2

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Este tutorial muestra cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de difusión de servidor. Difusión de servidor significa que el servidor inicia a las comunicaciones enviadas a los clientes.

La aplicación que creará en este tutorial simula un tablero de cotizaciones, un escenario típico para la funcionalidad de difusión de servidor. Periódicamente, el servidor actualiza cotizaciones bursátiles aleatoriamente y difunde las actualizaciones a todos los clientes conectados. En el explorador, los números y símbolos en el **cambiar** y **%** columnas cambian dinámicamente en respuesta a las notificaciones desde el servidor. Si abre los exploradores adicionales a la misma dirección URL, todos ellos muestran los mismos datos y los mismos cambios a los datos simultáneamente.

![Creación de web](tutorial-server-broadcast-with-signalr/_static/image1.png)

En este tutorial, puede:

> [!div class="checklist"]
> * Crear el proyecto
> * Configurar el código de servidor
> * Examine el código de servidor
> * Configurar el código de cliente
> * Examine el código de cliente
> * Probar la aplicación
> * Habilite el registro

> [!IMPORTANT]
> Si no desea trabajar con los pasos de la creación de la aplicación, puede instalar el paquete SignalR.Sample en un nuevo proyecto de aplicación Web ASP.NET vacía. Si instala el paquete de NuGet sin necesidad de realizar los pasos de este tutorial, debe seguir las instrucciones de la *readme.txt* archivo. Para ejecutar el paquete que necesita agregar un inicio OWIN que llama a la clase el `ConfigureSignalR` método en el paquete instalado. Recibirá un error si no se agrega la clase de inicio OWIN. Consulte la [instalar el ejemplo StockTicker](#install-the-stockticker-sample) sección de este artículo.


## <a name="prerequisites"></a>Requisitos previos

 * [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con el **ASP.NET y desarrollo web** carga de trabajo.

## <a name="create-the-project"></a>Crear el proyecto

En esta sección se muestra cómo usar Visual Studio 2017 para crear una aplicación Web de ASP.NET vacía.

1. En Visual Studio, cree una aplicación Web ASP.NET.

    ![Creación de web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. En el **nueva aplicación Web de ASP.NET - SignalR.StockTicker** ventana, deje **vacía** seleccionado y seleccione **Aceptar**.

## <a name="set-up-the-server-code"></a>Configurar el código de servidor

En esta sección, se establece el código que se ejecuta en el servidor.

### <a name="create-the-stock-class"></a>Crear la clase de Stock

Empiece por crear la *Stock* clase que va a usar para almacenar y transmitir información sobre una acción de modelo.

1. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **clase**.

1. Nombre de la clase *Stock* y agréguelo al proyecto.

1. Reemplace el código en el *Stock.cs* archivo con este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Las dos propiedades que se va a configurar al crear las poblaciones son `Symbol` (por ejemplo, MSFT para Microsoft) y `Price`. Las demás propiedades dependen de cómo y cuándo establecer `Price`. La primera vez que establece `Price`, el valor se propague a `DayOpen`. Después de eso, al establecer `Price`, la aplicación calcula el `Change` y `PercentChange` los valores de propiedad según la diferencia entre `Price` y `DayOpen`.

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>Cree las clases StockTickerHub y StockTicker

Usará la API de concentrador SignalR para controlar la interacción con el cliente y el servidor. Un `StockTickerHub` clase que deriva la `SignalRHub` clase controlará recibir conexiones y las llamadas a métodos de los clientes. También deberá mantener datos de acciones y ejecutar un `Timer` objeto. La `Timer` objeto desencadenará periódicamente las actualizaciones de precios independientes de las conexiones de cliente. No se puede colocar estas funciones un `Hub` clase, porque los concentradores son transitorios. La aplicación crea un `Hub` instancia de la clase para cada tarea en el centro, como las conexiones y las llamadas desde el cliente al servidor. Por lo que el mecanismo que mantiene los datos de cotizaciones, actualiza los precios y difunde las actualizaciones de precios debe ejecutarse en una clase independiente. Le asigne el nombre de la clase `StockTicker`.

![Difusión de StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

Solo quiere una instancia de la `StockTicker` clase para ejecutar en el servidor, por lo que necesitará configurar una referencia de cada `StockTickerHub` instancia para el singleton `StockTicker` instancia. El `StockTicker` clase debe difundir a los clientes porque tiene los datos de acciones y desencadenadores de las actualizaciones, pero `StockTicker` no es un `Hub` clase. La `StockTicker` clase tiene que obtener una referencia al objeto de contexto de conexión de concentrador SignalR. También puede usar el objeto de contexto de conexión de SignalR para difundir a los clientes.

#### <a name="create-stocktickerhubcs"></a>Crear StockTickerHub.cs

1. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **nuevo elemento**.

1. En **Agregar nuevo elemento - SignalR.StockTicker**, seleccione **instalado** > **Visual C#**   >  **Web**  >  **SignalR** y, a continuación, seleccione **clase de concentrador de SignalR (v2)**.

1. Nombre de la clase *StockTickerHub* y agréguelo al proyecto.

    Este paso se crea el *StockTickerHub.cs* archivo de clase. Al mismo tiempo, agrega un conjunto de archivos de script y las referencias de ensamblado que es compatible con SignalR al proyecto.

1. Reemplace el código en el *StockTickerHub.cs* archivo con este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. Guarde el archivo.

La aplicación usa el [concentrador](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) clase para definir los métodos que se pueden llamar los clientes en el servidor. Definir un método: `GetAllStocks()`. Cuando un cliente se conecta inicialmente con el servidor, llamará a este método para obtener una lista de todas las acciones con el precio actual. Puede ejecutar de forma sincrónica y devolver el método `IEnumerable<Stock>` porque devuelve datos de la memoria.

Si el método tuvo que obtener los datos haciendo algo que implicaría espera, como una búsqueda de la base de datos o una llamada de servicio web, especificaría `Task<IEnumerable<Stock>>` como el valor devuelto para habilitar el procesamiento asincrónico. Para obtener más información, consulte [Guía de la API de ASP.NET SignalR Hubs - Server - cuándo se debe ejecutar de forma asincrónica](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

El `HubName` atributo especifica cómo la aplicación hará referencia el concentrador en el código de JavaScript en el cliente. El nombre predeterminado en el cliente si no usa este atributo, es una versión camelCase del nombre de clase, que en este caso sería `stockTickerHub`.

Como verá más adelante cuando se crea el `StockTicker` (clase), la aplicación crea una instancia singleton de esa clase en su estático `Instance` propiedad. Esa instancia singleton de `StockTicker` está en memoria independientemente de cuántos clientes conexión o desconexión. Esa instancia es lo que el `GetAllStocks()` método se usa para devolver información bursátil actual.

#### <a name="create-stocktickercs"></a>Crear StockTicker.cs

1. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **clase**.

1. Nombre de la clase *StockTicker* y agréguelo al proyecto.

1. Reemplace el código en el *StockTicker.cs* archivo con este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

Puesto que todos los subprocesos se ejecutará la misma instancia de código StockTicker, la clase StockTicker debe ser seguro para subprocesos.

### <a name="examine-the-server-code"></a>Examine el código de servidor

Si examina el código del servidor, le ayudará comprender el funcionamiento de la aplicación.

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>Almacenamiento de la instancia de singleton en un campo estático

El código inicializa estático `_instance` campo que respalda la `Instance` propiedad con una instancia de la clase. Dado que el constructor es privado, es la única instancia de la clase que se puede crear la aplicación. La aplicación usa [la inicialización diferida](/dotnet/framework/performance/lazy-initialization) para el `_instance` campo. No es por motivos de rendimiento. Es para asegurarse de que la creación de instancias es segura para subprocesos.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

Cada vez que un cliente se conecta al servidor, una nueva instancia de la clase StockTickerHub que se ejecuta en un subproceso independiente Obtiene la instancia de singleton StockTicker desde el `StockTicker.Instance` propiedad estática, como vimos anteriormente en el `StockTickerHub` clase.

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Almacenar datos de acciones en un elemento ConcurrentDictionary

El constructor inicializa el `_stocks` colección con datos de ejemplo stock, y `GetAllStocks` devuelve las existencias. Como vimos anteriormente, se devuelve esta colección de acciones por `StockTickerHub.GetAllStocks`, que es un método de servidor en el `Hub` clase que se pueden llamar los clientes.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

La colección de acciones se define como un [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) tipo de seguridad para subprocesos. Como alternativa, puede usar un [diccionario](https://msdn.microsoft.com/library/xfhwa508.aspx) de objetos y bloquear explícitamente el diccionario al realizar cambios en ella.

Para esta aplicación de ejemplo, Aceptar es para almacenar datos de la aplicación en la memoria y perder los datos cuando la aplicación se desecha el `StockTicker` instancia. En una aplicación real, podría funcionar con un almacén de datos back-end como una base de datos.

#### <a name="periodically-updating-stock-prices"></a>Actualizaciones periódicas de cotizaciones

El constructor se inicia una `Timer` objeto que llama periódicamente a los métodos que actualizan los precios de las acciones de forma aleatoria.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer` las llamadas `UpdateStockPrices`, qué pasa null en el parámetro state. Antes de actualizar los precios, la aplicación toma un bloqueo en el `_updateStockPricesLock` objeto. El código comprueba si otro subproceso ya se está actualizando los precios y, a continuación, llama a `TryUpdateStockPrice` en cada acción en la lista. El `TryUpdateStockPrice` método decide si debe cambiar el precio y cuánto para cambiarlo. Si cambia el precio, la aplicación llama a `BroadcastStockPrice` difundir el cambio de precio de las acciones a todos los clientes conectados.

El `_updatingStockPrices` marca designado [volátil](https://msdn.microsoft.com/library/x13ttww7.aspx) para asegurarse de que es seguro para subprocesos.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

En una aplicación real, el `TryUpdateStockPrice` método llamaría a un servicio web para buscar el precio. En este código, la aplicación utiliza un generador de números aleatorios para realizar cambios de forma aleatoria.

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Obtener el contexto de SignalR para que la clase StockTicker puede difundir a los clientes

Dado que los cambios de precio se originan aquí en el `StockTicker` objeto, es el objeto que necesita llamar a un `updateStockPrice` método en todos los clientes conectados. En un `Hub` (clase), que tiene una API para llamar a métodos de cliente, pero `StockTicker` no se deriva de la `Hub` clase y no tiene una referencia a ningún `Hub` objeto. Para difundir a los clientes conectados, la `StockTicker` clase tiene que obtener la instancia del contexto de SignalR para el `StockTickerHub` clase y usarlo para llamar a métodos en los clientes.

El código obtiene una referencia al contexto de SignalR cuando crea la instancia de la clase singleton, que hacen referencia a pasadas al constructor, y el constructor lo coloca en el `Clients` propiedad.

Hay dos razones por qué desea obtener el contexto de una sola vez: obtener el contexto es una tarea costosa y hacer que una vez que garantiza que la aplicación conserva el orden de los mensajes enviados a los clientes que prefiera.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

Obteniendo el `Clients` propiedad del contexto y ponerlo en la `StockTickerClient` propiedad le permite escribir código para llamar a los métodos de cliente que tiene el mismo aspecto como lo haría en un `Hub` clase. Por ejemplo, puede escribir difundir a todos los clientes `Clients.All.updateStockPrice(stock)`.

El `updateStockPrice` método al que está llamando en `BroadcastStockPrice` aún no existe. Se agregará más adelante al escribir código que se ejecuta en el cliente. Puede hacer referencia a `updateStockPrice` aquí porque `Clients.All` es dinámico, lo que significa que la aplicación evaluará la expresión en tiempo de ejecución. Cuando se ejecuta esta llamada al método, SignalR enviará el nombre del método y el valor del parámetro al cliente, y si el cliente tiene un método denominado `updateStockPrice`, llamará a ese método y pasar el valor del parámetro al la aplicación.

`Clients.All` significa que se envíe a todos los clientes. SignalR ofrece otras opciones para especificar qué clientes o grupos de clientes para enviar a. Para obtener más información, consulte [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registre la ruta de SignalR

El servidor necesita saber qué dirección URL para interceptar y dirigir a SignalR. Para ello, agregue una clase de inicio OWIN:

1. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **nuevo elemento**.

1. En **Agregar nuevo elemento - SignalR.StockTicker** seleccione **instalado** > **Visual C#**   >  **Web** y a continuación, seleccione **clase de inicio OWIN**.

1. Nombre de la clase *inicio* y seleccione **Aceptar**.

1. Reemplace el código predeterminado en el *Startup.cs* archivo con este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Ahora ha terminado la configuración, el código de servidor. En la siguiente sección, podrá configurar el cliente.

## <a name="set-up-the-client-code"></a>Configurar el código de cliente

En esta sección, se establece el código que se ejecuta en el cliente.

### <a name="create-the-html-page-and-javascript-file"></a>Crear la página HTML y archivos de JavaScript

La página HTML mostrará los datos y el archivo JavaScript va a organizar los datos.

#### <a name="create-stocktickerhtml"></a>Crear StockTicker.html

En primer lugar, agregará al cliente HTML.

1. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **página HTML**.

1. Nombre del archivo *StockTicker* y seleccione **Aceptar**.

1. Reemplace el código predeterminado en el *StockTicker.html* archivo con este código:

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    El código HTML crea una tabla con cinco columnas, una fila de encabezado y una fila de datos con una sola celda que abarca las cinco columnas. La fila de datos muestra "Cargando..." momentáneamente cuando se inicia la aplicación. Código de JavaScript quitará esa fila y agregar en su lugar de filas con datos recuperados del servidor de acciones.

    Especifican las etiquetas de script:

    * El archivo de script de jQuery.

    * El archivo de script básico de SignalR.

    * El archivo de script de proxy de SignalR.

    * Un archivo de script StockTicker que creará más adelante.

    La aplicación genera dinámicamente el archivo de script de proxy de SignalR. Especifica la dirección URL "/ signalr/hubs" y define los métodos de proxy para los métodos de la clase de Hub, en este caso, para `StockTickerHub.GetAllStocks`. Si lo prefiere, puede generar manualmente este archivo de JavaScript mediante [SignalR utilidades](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/). No olvide deshabilitar la creación dinámica de archivos en el `MapHubs` llamada al método.

1. En **el Explorador de soluciones**, expanda **Scripts**.

    Bibliotecas de scripts de jQuery y SignalR están visibles en el proyecto.

    > [!IMPORTANT]
    > El Administrador de paquetes instalará una versión posterior de los scripts de SignalR.

1. Actualizar las referencias de script en el bloque de código para que coincida con las versiones de los archivos de script en el proyecto.

1. En **el Explorador de soluciones**, haga clic en *StockTicker.html*y, a continuación, seleccione **establecer como página de inicio**.

#### <a name="create-stocktickerjs"></a>Crear StockTicker.js

Ahora cree el archivo de JavaScript.

1. En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **agregar** > **archivo JavaScript**.

1. Nombre del archivo *StockTicker* y seleccione **Aceptar**.

1. Agregue este código a la *StockTicker.js* archivo:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>Examine el código de cliente

Si examina el código de cliente, le permitirá aprender cómo interactúa el código de cliente con el código del servidor para que la aplicación funcione.

#### <a name="starting-the-connection"></a>A partir de la conexión

`$.connection` hace referencia a los servidores proxy de SignalR. El código obtiene una referencia al proxy para el `StockTickerHub` clase y lo coloca en el `ticker` variable. El nombre del proxy es el nombre que se ha establecido mediante la `HubName` atributo:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

Después de definir todas las variables y funciones, la última línea de código en el archivo inicializa la conexión de SignalR mediante una llamada a SignalR `start` función. El `start` función se ejecuta de forma asincrónica y devuelve un [jQuery aplazado objeto](http://api.jquery.com/category/deferred-object/). Puede llamar a la función done para especificar la función que se va a llamar cuando la aplicación finalice la acción asincrónica.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>Obtener todas las acciones

El `init` llamadas de función el `getAllStocks` funciones del servidor y usa la información que devuelve el servidor para actualizar la tabla stock. Tenga en cuenta que, de forma predeterminada, tiene que usar mayúsculas intercaladas en el cliente, incluso aunque el nombre del método con grafía pascal en el servidor. La regla camelCasing solo se aplica a los métodos, no objetos. Por ejemplo, hacer referencia a `stock.Symbol` y `stock.Price`, no `stock.symbol` o `stock.price`.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

En el `init` método, la aplicación crea HTML para una fila de tabla para cada objeto de acción recibido del servidor mediante una llamada a `formatStock` a las propiedades de formato de la `stock` de objetos y, a continuación, al llamar a `supplant` para reemplazar los marcadores de posición en el `rowTemplate` variable con el `stock` valores de propiedad del objeto. El HTML resultante, a continuación, se anexa a la tabla stock.

> [!NOTE]
> Se llama a `init` pasándolo como un `callback` función que se ejecuta después de la asincrónica `start` función finaliza. Si llama a `init` como una instrucción de JavaScript independiente después de llamar a `start`, la función produciría un error porque se ejecutaría inmediatamente sin esperar a la función de inicio a fin de establecer la conexión. En ese caso, el `init` función intentaría llamar a la `getAllStocks` funcionar antes de que la aplicación establece una conexión de servidor.

#### <a name="getting-updated-stock-prices"></a>Obtener cotizaciones bursátiles actualizadas

Cuando el servidor cambia el precio de una acción, llama a la `updateStockPrice` en los clientes conectados. La aplicación agrega la función a la propiedad de cliente de la `stockTicker` proxy para que esté disponible para las llamadas desde el servidor.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

El `updateStockPrice` formatos de función que un objeto estándar recibido del servidor en una tabla de filas del mismo modo que en el `init` función. En lugar de anexar la fila a la tabla, se busca la fila actual de la acción en la tabla y reemplaza esa fila por uno nuevo.

## <a name="test-the-application"></a>Probar la aplicación

Puede probar la aplicación para asegurarse de que se está trabajando. Verá todas las ventanas de explorador se muestran en la tabla de cotizaciones en directo con cotizaciones bursátiles fluctúa.

1. En la barra de herramientas activar **depuración de scripts** y, a continuación, seleccione el botón Reproducir para ejecutar la aplicación en modo de depuración.

    ![Captura de pantalla del usuario activar el modo de depuración y seleccione Reproducir.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    Abrirá una ventana del explorador muestra la **Live tabla Stock**. La tabla stock inicialmente muestra la línea "Cargando...", a continuación, tras un tiempo breve, la aplicación muestra los datos de acciones iniciales y, a continuación, iniciar los precios de las acciones cambiar.

1. Copie la dirección URL desde el explorador, abra dos otros exploradores y pegue las direcciones URL en las barras de dirección.

    La visualización del material inicial es el mismo que el primer explorador y los cambios se realizan simultáneamente.

1. Cierre todos los exploradores, abra un nuevo explorador y vaya a la misma dirección URL.

    El objeto singleton StockTicker siguiera ejecutándose en el servidor. El **Live tabla Stock** muestra que siguieron las poblaciones a cambiar. Cambiar las cifras no ve la tabla con cero inicial.

1. Cierre el explorador.

## <a name="enable-logging"></a>Habilite el registro

SignalR tiene una función de registro integrados que se puede habilitar en el cliente para ayudar a solucionar problemas. En esta sección, habilite el registro y vea los ejemplos que muestran cómo los registros de indican qué de los siguientes métodos de transporte usa SignalR:

* [WebSockets](http://en.wikipedia.org/wiki/WebSocket), admitido por IIS 8 y los exploradores actuales.

* [Eventos de servidor envió](http://en.wikipedia.org/wiki/Server-sent_events), admitido por los exploradores distintos de Internet Explorer.

* [Marco para siempre](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), admitido por Internet Explorer.

* [AJAX de sondeo prolongado](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), compatible con todos los exploradores.

En una conexión determinada, SignalR elige el mejor método de transporte que admiten el servidor y el cliente.

1. Abra *StockTicker.js*.

1. Agregue esta línea resaltada de código para habilitar el registro inmediatamente antes del código que inicializa la conexión al final del archivo:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. Presione **F5** para ejecutar el proyecto.

1. Abra la ventana de herramientas de desarrollador de su explorador y seleccione la consola para ver los registros. Es posible que deba actualizar la página para ver los registros de SignalR negocie el método de transporte para una conexión nueva.

    * Si está ejecutando Internet Explorer 10 en Windows 8 (IIS 8), el método de transporte es **WebSockets**.

    * Si está ejecutando Internet Explorer 10 en Windows 7 (IIS 7.5), el método de transporte es **iframe**.

    * Si está ejecutando el 19 de Firefox en Windows 8 (IIS 8), el método de transporte es **WebSockets**.

        > [!TIP]
        > En Firefox, instale el complemento Firebug para obtener una ventana de consola.

    * Si está ejecutando el 19 de Firefox en Windows 7 (IIS 7.5), el método de transporte es **servidor envió** eventos.

## <a name="install-the-stockticker-sample"></a>Instalar el ejemplo StockTicker

El [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) instala la aplicación StockTicker. El paquete NuGet incluye más características que la versión simplificada que ha creado desde cero. En esta sección del tutorial, instale el paquete NuGet y revise las nuevas características y el código que implementa en ellos.

> [!IMPORTANT]
> Si instala el paquete sin necesidad de realizar los pasos anteriores de este tutorial, debe agregar una clase de inicio OWIN al proyecto. Este paso explica en este archivo readme.txt del paquete NuGet.

### <a name="install-the-signalrsample-nuget-package"></a>Instale el paquete SignalR.Sample NuGet

1. En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Administrar paquetes NuGet**.

1. En **Administrador de paquetes de NuGet: SignalR.StockTicker**, seleccione **examinar**.

1. Desde **origen del paquete**, seleccione **nuget.org**.

1. Escriba *SignalR.Sample* en el cuadro de búsqueda y seleccione **Microsoft.AspNet.SignalR.Sample** > **instalar**.

1. En **el Explorador de soluciones**, expanda el *SignalR.Sample* carpeta.

    Instalar el paquete SignalR.Sample crea la carpeta y su contenido.

1. En el *SignalR.Sample* carpeta, haga clic en *StockTicker.html*y, a continuación, seleccione **establecer como página principal**.

    > [!NOTE]
    > Instalación de SignalR.Sample NuGet el paquete podría cambiar la versión de jQuery que tiene en su *Scripts* carpeta. El nuevo *StockTicker.html* archivo que instala el paquete en el *SignalR.Sample* carpeta estará sincronizado con la versión de jQuery que instala el paquete, pero si desea ejecutar el original *StockTicker.html* archivo nuevo, es posible que deba actualizar la referencia de jQuery en la etiqueta de script en primer lugar.

### <a name="run-the-application"></a>Ejecutar la aplicación

 La tabla que vio en la primera aplicación tenía características útiles. La aplicación de tablero de cotizaciones completo muestra las nuevas características: una ventana desplazable horizontal que muestra los datos de acciones y acciones que cambian de color tal y como se dividen y subiendo.

1. Presione **F5** para ejecutar la aplicación.

     Al ejecutar la aplicación por primera vez, el mercado"" es "closed" y verá una tabla estática y una ventana del indicador que no es el desplazamiento.

1. Seleccione **Open Market**.

    ![Captura de pantalla de los tableros de cotizaciones en directo.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * El **Live bolsa** cuadro comienza a desplazarse horizontalmente y se inicia el servidor para difundir periódicamente los cambios de precio de las acciones de forma aleatoria.

    * Cada vez que cambia de cotización, la aplicación actualiza la **Live tabla Stock** y el **tablero de cotizaciones en directo**.

    * Cuando cambie el precio de una acción es positivo, la aplicación muestra las existencias con un fondo verde.

    * Cuando el cambio es negativo, la aplicación muestra las existencias con un fondo rojo.

1. Seleccione **cerrar mercado**.

    * La tabla actualiza stop.

    * El teletipo detiene el desplazamiento.

1. Seleccione **restablecer**.

    * Se restablece todos los datos de acciones.

    * La aplicación restaura el estado inicial antes de los cambios de precio a trabajar.

1. Copie la dirección URL desde el explorador, abra dos otros exploradores y pegue las direcciones URL en las barras de dirección.

1. Verá los mismos datos que se actualiza dinámicamente al mismo tiempo en cada explorador.

1. Cuando se selecciona cualquiera de los controles, todos los exploradores responden la misma manera a la vez.

### <a name="live-stock-ticker-display"></a>Visualización de tablero de cotizaciones en directo

El **Live bolsa** presentación es una lista sin ordenar en un `<div>` elemento con formato en una sola línea, los estilos CSS. La aplicación se inicializa y actualiza el tablero del mismo modo que la tabla: reemplazando los marcadores de posición en una `<li>` cadena de plantilla y agregar dinámicamente la `<li>` elementos a la `<ul>` elemento. La aplicación incluye el desplazamiento con jQuery `animate` función para variar el margen izquierda de la lista sin ordenar dentro de la `<div>`.

#### <a name="signalrsample-stocktickerhtml"></a>SignalR.Sample StockTicker.html

El tablero de cotizaciones código HTML:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>SignalR.Sample StockTicker.css

El tablero de cotizaciones código CSS:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>SignalR.Sample SignalR.StockTicker.js

Desplácese por el código jQuery que hace:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Métodos adicionales en el servidor que el cliente puede llamar a

Para agregar flexibilidad a la aplicación, hay métodos adicionales que se puede llamar la aplicación.

#### <a name="signalrsample-stocktickerhubcs"></a>SignalR.Sample StockTickerHub.cs

La `StockTickerHub` clase define cuatro métodos adicionales que puede llamar el cliente:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

Las llamadas de aplicación `OpenMarket`, `CloseMarket`, y `Reset` en respuesta a los botones en la parte superior de la página. Muestra el patrón de un cliente desencadenar un cambio de estado que se propagaban inmediatamente a todos los clientes. Cada uno de estos métodos llama a un método el `StockTicker` clase que hace que el cambio de estado de mercado y, a continuación, transmite el estado nueva.

#### <a name="signalrsample-stocktickercs"></a>SignalR.Sample StockTicker.cs

En el `StockTicker` (clase), la aplicación mantiene el estado del mercado con un `MarketState` propiedad que devuelve un `MarketState` valor enum:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Cada uno de los métodos que cambian el estado de mercado hacerlo dentro de un bloqueo porque el `StockTicker` clase debe ser seguro para subprocesos:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Para asegurarse de que este código es seguro para subprocesos, el `_marketState` campo que respalda la `MarketState` propiedad designada `volatile`:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

El `BroadcastMarketStateChange` y `BroadcastMarketReset` métodos son similares al método BroadcastStockPrice que ya hemos visto, salvo que llaman a diferentes métodos definidos en el cliente:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Funciones adicionales en el cliente que el servidor puede llamar a

El `updateStockPrice` función controla ahora en la tabla y la presentación de tableros de cotizaciones y usa `jQuery.Color` parpadea colores verde y rojo.

Nuevas funciones en *SignalR.StockTicker.js* habilitar y deshabilitar los botones según el estado del mercado. Se iniciar o detener también el **Live bolsa** desplazamiento horizontal. Puesto que muchas de las funciones se agregan a `ticker.client`, la aplicación usa el [jQuery ampliar la función](http://api.jquery.com/jQuery.extend/) para agregarlos.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Programa de instalación de cliente adicionales después de establecer la conexión

Una vez que el cliente establece la conexión, tiene cierto trabajo adicional para hacer:

* Averiguar si el mercado está abierto o cerrado para llamar a la correspondiente `marketOpened` o `marketClosed` función.

* Adjunte las llamadas al método de servidor a los botones.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Los métodos de servidor no conectado con los botones hasta que una vez que la aplicación establece la conexión. Es por lo que el código no puede llamar a los métodos de servidor antes de estar disponibles.

## <a name="additional-resources"></a>Recursos adicionales

En este tutorial ha aprendido cómo programar una aplicación de SignalR que transmite mensajes desde el servidor a todos los clientes conectados. Ahora puede difundir mensajes de forma periódica y en respuesta a las notificaciones desde cualquier cliente. Puede usar el concepto de la instancia de singleton multiproceso para mantener el estado del servidor en escenarios de juego en línea multijugador. Para obtener un ejemplo, vea [ShootR juego basado en SignalR](https://github.com/NTaylorMullen/ShootR).

Para ver tutoriales que muestran escenarios de comunicación punto a punto, consulte [Introducción a SignalR](introduction-to-signalr.md) y [actualizar en tiempo real con SignalR](tutorial-high-frequency-realtime-with-signalr.md).

Para obtener más información acerca de SignalR, consulte los siguientes recursos:

* [SignalR de ASP.NET](../../index.md)
* [Proyecto de SignalR](http://signalr.net/)
* [SignalR GitHub y ejemplos](https://github.com/SignalR/SignalR)
* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, puede:

> [!div class="checklist"]
> * Crear el proyecto
> * Configurar el código de servidor
> * Examinar el código del servidor
> * Configurar el código de cliente
> * Examinar el código de cliente
> * Probar la aplicación
> * Registro habilitado

Avance al siguiente artículo para obtener información sobre cómo crear una aplicación web en tiempo real que usa ASP.NET SignalR 2.
> [!div class="nextstepaction"]
> [Crear aplicación web en tiempo real con SignalR](real-time-web-applications-with-signalr.md)