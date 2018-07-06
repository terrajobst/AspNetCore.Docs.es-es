---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutorial: Difusión de servidores con SignalR 2 | Microsoft Docs'
author: tdykstra
description: Este tutorial muestra cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de difusión de servidor. Difusión de servidor significa que común...
ms.author: aspnetcontent
ms.date: 10/13/2014
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0e86fbea9c5668e20fce7a494c76c52f9c089c09
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820702"
---
<a name="tutorial-server-broadcast-with-signalr-2"></a>Tutorial: Difusión de servidores con SignalR 2
====================
por [Tom Dykstra](https://github.com/tdykstra), [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de difusión de servidor. Difusión de servidor significa que se inician las comunicaciones enviadas a los clientes por el servidor. Este escenario requiere un método de programación distinto a escenarios punto a punto, como aplicaciones de chat, en el que se inician las comunicaciones enviadas a los clientes por uno o varios de los clientes.
> 
> La aplicación que creará en este tutorial simula un tablero de cotizaciones, un escenario típico para la funcionalidad de difusión de servidor.
> 
> En este tema se escribió originalmente por Patrick Fletcher.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Versión 2 de SignalR
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Uso de Visual Studio 2012 con este tutorial
> 
> 
> Para usar Visual Studio 2012 con este tutorial, realice lo siguiente:
> 
> - Actualización de su [Administrador de paquetes](http://docs.nuget.org/docs/start-here/installing-nuget) a la versión más reciente.
> - Instalar el [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx).
> - En el instalador de plataforma Web, buscar e instalar **ASP.NET y Web Tools 2013.1 para Visual Studio 2012**. Este modo instalará como plantillas de Visual Studio para las clases de SignalR **concentrador**.
> - Algunas plantillas (como **clase de inicio OWIN**) no estará disponible; para ello, use un archivo de clase en su lugar.
> 
> 
> ## <a name="tutorial-versions"></a>Versiones de tutoriales
> 
> Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Información general

En este tutorial, podrá crear una aplicación de cotización bursátil que sea representativo de aplicaciones en tiempo real en el que desee "Insertar" periódicamente o difundir notificaciones desde el servidor a todos los clientes conectados. En la primera parte de este tutorial, creará una versión simplificada de la aplicación desde cero. En el resto del tutorial, deberá instalar un paquete de NuGet que contiene características adicionales y revisar el código para dichas características.

La aplicación que va a compilar en la primera parte de este tutorial muestra una cuadrícula con datos de acciones.

![Versión inicial de StockTicker](tutorial-server-broadcast-with-signalr/_static/image1.png)

Periódicamente el servidor de forma aleatoria actualiza cotizaciones bursátiles e inserta las actualizaciones en todos los clientes conectados. En el explorador los números y símbolos en el **cambiar** y **%** columnas cambian dinámicamente en respuesta a las notificaciones desde el servidor. Si abre los exploradores adicionales a la misma dirección URL, todos ellos muestran los mismos datos y los mismos cambios a los datos simultáneamente.

Este tutorial contiene las siguientes secciones:

- [Requisitos previos](#prerequisites)
- [Crear el proyecto](#createproject)
- [Configurar el código de servidor](#server)
- [Configurar el código de cliente](#client)
- [Probar la aplicación](#test)
- [Habilitar el registro](#enablelogging)
- [Instalar y revise el ejemplo StockTicker completo](#fullsample)
- [Pasos siguientes](#nextsteps)

El [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) paquete NuGet instala una aplicación de tablero de cotizaciones de ejemplo simulada en un proyecto de Visual Studio.

> [!NOTE]
> Si no desea trabajar con los pasos de la creación de la aplicación, puede instalar el paquete SignalR.Sample en un nuevo proyecto de aplicación Web ASP.NET vacía. Si instala el paquete de NuGet sin realizar los pasos de este tutorial, **debe seguir las instrucciones que aparecen en el archivo readme.txt**. Para ejecutar el paquete que tiene que agregar una clase de inicio OWIN que se llama al método ConfigureSignalR en el paquete instalado. Recibirá un error si no se agrega la clase de inicio OWIN.


<a id="prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que haya instalado en el equipo de Visual Studio 2013. Si no tiene Visual Studio, consulte [descargas de ASP.NET](https://www.asp.net/downloads) para obtener la versión gratuita Visual Studio 2013 Express.

<a id="createproject"></a>

## <a name="create-the-project"></a>Crear el proyecto

1. Desde el **archivo** menú, haga clic en **nuevo proyecto**.
2. En el **nuevo proyecto** cuadro de diálogo, expanda **C#** en **plantillas** y seleccione **Web**.
3. Seleccione el **aplicación Web ASP.NET vacía** plantilla, el nombre del proyecto *SignalR.StockTicker*y haga clic en **Aceptar**.

    ![Cuadro de diálogo Nuevo proyecto](tutorial-server-broadcast-with-signalr/_static/image2.png)
4. En el **ASP.NET nueva** ventana del proyecto, deje **vacía** seleccionado y haga clic en **crear proyecto**.

    ![Cuadro de diálogo nuevo proyecto de ASP](tutorial-server-broadcast-with-signalr/_static/image3.png)

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Configurar el código de servidor

En esta sección se establece el código que se ejecuta en el servidor.

### <a name="create-the-stock-class"></a>Crear la clase de Stock

Empiece por crear la clase de modelo de Stock que va a usar para almacenar y transmitir información sobre una acción.

1. Cree un nuevo archivo de clase en la carpeta del proyecto, asígnele el nombre *Stock.cs*y, a continuación, reemplace el código de plantilla con el código siguiente:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Las dos propiedades que se va a configurar al crear las poblaciones son el símbolo (por ejemplo, MSFT para Microsoft) y el precio. Las demás propiedades dependen de cómo y cuándo establecer precios. La primera vez que se establece el precio, el valor obtiene propaga a DayOpen. Posteriormente, al establecer el cambio de precio y se calculan los valores de propiedad CambioPorcentual según la diferencia entre el precio y DayOpen.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Cree las clases StockTicker y StockTickerHub

Usará la API de concentrador SignalR para controlar la interacción con el cliente y el servidor. Una clase StockTickerHub que se deriva de la clase de concentrador SignalR controlará recibir conexiones y las llamadas a métodos de los clientes. También necesita mantener los datos de cotizaciones y ejecutar un objeto de temporizador para desencadenar periódicamente las actualizaciones de precios, independientemente de las conexiones de cliente. No se puede colocar estas funciones en una clase de Hub, porque las instancias de concentrador son transitorias. Se crea una instancia de la clase de concentrador para cada operación en el centro, como las conexiones y las llamadas desde el cliente al servidor. Por lo que el mecanismo que mantiene los datos de cotizaciones, actualiza los precios y difunde las actualizaciones de precios debe ejecutarse en una clase independiente, que le nombre StockTicker.

![Difusión de StockTicker](tutorial-server-broadcast-with-signalr/_static/image5.png)

Solo quiere una instancia de la clase StockTicker para ejecutarse en el servidor, por lo que necesitará configurar una referencia de cada instancia StockTickerHub a la instancia de singleton StockTicker. La clase StockTicker tiene que ser capaz de difusión a los clientes porque tiene los datos de cotizaciones y desencadena las actualizaciones, pero StockTicker no es una clase de concentrador. Por lo tanto, la clase StockTicker debe obtener una referencia al objeto de contexto de conexión de concentrador SignalR. También puede usar el objeto de contexto de conexión de SignalR para difundir a los clientes.

1. En **el Explorador de soluciones**, haga clic en el proyecto y haga clic en **Add | Clase de concentrador SignalR (v2)**.
2. Nombre del nuevo centro *StockTickerHub.cs*y, a continuación, haga clic en **agregar**. Paquetes de SignalR NuGet se agregará al proyecto.
3. Reemplace el código de plantilla con el código siguiente:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

    El [concentrador](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) clase se utiliza para definir los métodos que se pueden llamar los clientes en el servidor. Define un método: `GetAllStocks()`. Cuando un cliente se conecta inicialmente con el servidor, llamará a este método para obtener una lista de todas las acciones con el precio actual. Puede ejecutar de forma sincrónica y devolver el método `IEnumerable<Stock>` porque devuelve datos de la memoria. Si el método tuvo que obtener los datos haciendo algo que implicaría espera, por ejemplo, una búsqueda de la base de datos o una llamada de servicio web, especificaría `Task<IEnumerable<Stock>>` como el valor devuelto para habilitar el procesamiento asincrónico. Para obtener más información, consulte [Guía de la API de ASP.NET SignalR Hubs - Server - cuándo se debe ejecutar de forma asincrónica](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

    El atributo HubName especifica cómo hacer referencia a la central en el código de JavaScript en el cliente. El nombre predeterminado en el cliente si no usa este atributo es una versión de mayúsculas y minúsculas camel del nombre de clase, que en este caso sería stockTickerHub.

    Como verá más adelante al crear la clase StockTicker, se crea una instancia singleton de esa clase en su propiedad de instancia estática. Que la instancia singleton de StockTicker permanece en memoria independientemente de cuántos clientes conexión o desconexión y esa instancia es lo que usa el método GetAllStocks para devolver información bursátil actual.
4. Cree un nuevo archivo de clase en la carpeta del proyecto, asígnele el nombre *StockTicker.cs*y, a continuación, reemplace el código de plantilla con el código siguiente:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

    Puesto que varios subprocesos a ejecutar la misma instancia de código StockTicker, la clase StockTicker debe ser seguro para subprocesos.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Almacenamiento de la instancia de singleton en un campo estático

    El código inicializa estático \_campo de instancia que respalda la propiedad de instancia con una instancia de la clase y esta es la única instancia de la clase que se puede crear, porque el constructor está marcado como privado. [La inicialización diferida](https://msdn.microsoft.com/library/dd997286.aspx) se usa para el \_campo de instancia, no por motivos de rendimiento, pero para asegurarse de que la creación de instancias es seguro para subprocesos.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

    Cada vez que un cliente se conecta al servidor, una nueva instancia de la clase StockTickerHub que se ejecuta en un subproceso independiente Obtiene la instancia de singleton StockTicker desde la propiedad estática StockTicker.Instance, como se vio anteriormente en la clase StockTickerHub.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Almacenar datos de acciones en un elemento ConcurrentDictionary

    El constructor inicializa el \_colección de acciones con algunos datos de ejemplo stock y GetAllStocks devuelve las existencias. Como vimos anteriormente, esta colección de acciones a su vez devuelve StockTickerHub.GetAllStocks que es un método de servidor en la clase de concentrador que se pueden llamar los clientes.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

    La colección de acciones se define como un [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) tipo de seguridad para subprocesos. Como alternativa, puede usar un [diccionario](https://msdn.microsoft.com/library/xfhwa508.aspx) de objetos y bloquear explícitamente el diccionario al realizar cambios en ella.

    Para esta aplicación de ejemplo, es correcto para almacenar datos de la aplicación en la memoria y perder los datos cuando se elimina la instancia de StockTicker. En una aplicación real podría funcionar con un almacén de datos back-end como una base de datos.

    ### <a name="periodically-updating-stock-prices"></a>Actualizaciones periódicas de cotizaciones

    El constructor se inicia un objeto de temporizador que llama periódicamente a los métodos que actualizan los precios de las acciones de forma aleatoria.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

    UpdateStockPrices lo llama el temporizador, que se pasa null en el parámetro state. Antes de actualizar los precios, se toma un bloqueo en el \_updateStockPricesLock objeto. El código comprueba si otro subproceso ya se está actualizando los precios y, a continuación, llama a TryUpdateStockPrice en cada acción en la lista. El método TryUpdateStockPrice decide si debe cambiar el precio y cuánto para cambiarlo. Si se cambia el precio de las acciones, se llama BroadcastStockPrice para el cambio de precio de las acciones de difusión a todos los clientes conectados.

    El \_updatingStockPrices marca está marcado como [volátil](https://msdn.microsoft.com/library/x13ttww7.aspx) para asegurarse de que el acceso a él es seguro para subprocesos.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

    En una aplicación real, el método TryUpdateStockPrice llamaría a un servicio web para buscar el precio; en este código utiliza un generador de números aleatorios para realizar cambios de forma aleatoria.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Obtener el contexto de SignalR para que la clase StockTicker puede difundir a los clientes

    Dado que los cambios de precio aquí se originan en el objeto StockTicker, esto es el objeto que se debe llamar a un método updateStockPrice en todos los clientes conectados. En una clase de Hub tiene una API para llamar a métodos de cliente, pero StockTicker no se deriva de la clase de concentrador y no tiene una referencia a cualquier objeto de concentrador. Por lo tanto, para difundir a los clientes conectados, la clase StockTicker debe obtener la instancia del contexto de SignalR para la clase StockTickerHub y usarlo para llamar a métodos en los clientes.

    El código obtiene una referencia al contexto de SignalR cuando crea la instancia de la clase singleton, que hacen referencia a pasadas al constructor, y el constructor lo coloca en la propiedad de los clientes.

    Hay dos razones por qué desea obtener el contexto de una sola vez: obtener el contexto es una operación costosa, y garantiza la obtención de una vez que se conserva el orden deseado de los mensajes enviados a los clientes.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

    Obtener la propiedad de los clientes del contexto y lo coloca en la propiedad StockTickerClient le permite escribir código para llamar a los métodos de cliente que tiene el mismo aspecto y como lo haría en una clase de concentrador. Por ejemplo, puede escribir Clients.All.updateStockPrice(stock) para difundir a todos los clientes.

    El método updateStockPrice que está llamando en BroadcastStockPrice que no exista; se agregará más adelante al escribir código que se ejecuta en el cliente. Puede hacer referencia a updateStockPrice aquí porque Clients.All es dinámico, lo que significa que la expresión se evaluará en tiempo de ejecución. Cuando se ejecuta esta llamada al método, SignalR enviará el nombre del método y el valor del parámetro al cliente y si el cliente tiene un método denominado updateStockPrice, se llamará a ese método y el valor del parámetro se pasará a él.

    Clients.All significa que enviar a todos los clientes. SignalR ofrece otras opciones para especificar qué clientes o grupos de clientes para enviar a. Para obtener más información, consulte [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registre la ruta de SignalR

El servidor necesita saber qué dirección URL para interceptar y dirigir a SignalR. Para ello, agregue una clase de inicio OWIN:

1. En **el Explorador de soluciones**, haga clic en el proyecto y, a continuación, haga clic en **Add | Clase de inicio OWIN**. Nombre de la clase **Startup.cs**.
2. Reemplace el código de **Startup.cs** con lo siguiente.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Ahora ha completado la configuración el código del servidor. En la sección siguiente a configurar el cliente.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Configurar el código de cliente

1. Crear un nuevo archivo HTML en la carpeta del proyecto y asígnele el nombre *StockTicker.html*.
2. Reemplace el código de plantilla con el código siguiente.

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html)]

    El código HTML crea una tabla con 5 columnas, una fila de encabezado y una fila de datos con una sola celda que abarca las 5 columnas. La fila de datos muestra "Cargando..." y solo se mostrarán momentáneamente cuando se inicia la aplicación. Código de JavaScript quitará esa fila y agregar en su lugar de filas con datos recuperados del servidor de acciones.

    Las etiquetas de script especifican el archivo de script de jQuery, el archivo de script de SignalR core, el archivo de script de proxy de SignalR y un archivo de script StockTicker que creará más adelante. El archivo de script de proxy de SignalR, que especifica la dirección URL "/ signalr/hubs", se genera dinámicamente y define los métodos de proxy para los métodos de la clase de Hub, en este caso para StockTickerHub.GetAllStocks. Si lo prefiere, puede generar manualmente este archivo de JavaScript mediante [SignalR utilidades](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) y deshabilite la creación dinámica de archivos en la llamada al método MapHubs.
3. > [!IMPORTANT]
   > Asegúrese de que el archivo JavaScript hace referencia en *StockTicker.html* son correctos. Es decir, asegúrese de que la versión de jQuery en la etiqueta de script (1.10.2 en el ejemplo) es la misma que la versión de jQuery en su proyecto *Scripts* carpeta y asegúrese de que la versión de SignalR en la etiqueta de script es el mismo que el objeto de SignalR versión del proyecto *Scripts* carpeta. Si es necesario, cambie los nombres de archivo en las etiquetas de script.
4. En **el Explorador de soluciones**, haga clic en *StockTicker.html*y, a continuación, haga clic en **establecer como página de inicio**.
5. Cree un nuevo archivo JavaScript en la carpeta del proyecto y denomínelo *StockTicker.js*...
6. Reemplace el código de plantilla con el código siguiente:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

    $.connection hace referencia a los servidores proxy de SignalR. El código obtiene una referencia al proxy para la clase StockTickerHub y lo coloca en la variable de tableros de cotizaciones. El nombre del proxy es el nombre que se ha establecido mediante el atributo [HubName]:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

    Después de que se definen todas las variables y funciones, la última línea de código en el archivo inicializa la conexión de SignalR mediante una llamada a la función de inicio de SignalR. La función de inicio se ejecuta de forma asincrónica y devuelve un [jQuery aplazado objeto](http://api.jquery.com/category/deferred-object/), lo que significa que puede llamar a la función done para especificar la función que se va a llamar cuando se complete la operación asincrónica...

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

    La función init llama a la función getAllStocks en el servidor y usa la información que devuelve el servidor para actualizar la tabla stock. Tenga en cuenta que de forma predeterminada, tiene que usar la grafía camel en el cliente aunque el nombre del método con grafía pascal en el servidor. La regla de grafía camel solo se aplica a los métodos, no objetos. Por ejemplo, consulte existencias. Símbolo de y acciones. Precio, no stock.symbol o stock.price.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

    Si desea utilizar las mayúsculas y minúsculas de pascal en el cliente, o si desea utilizar un nombre de método completamente diferente, podría decorar el método de concentrador con el atributo HubMethodName del mismo modo decorar la clase de Hub con el atributo HubName.

    En el método init, HTML para una fila de tabla se crea para cada objeto de acción recibida desde el servidor formatStock que realiza la llamada a las propiedades de formato del objeto estándar y, a continuación, al llamar a suplantar (que se define en la parte superior de *StockTicker.js*) para reemplazar los marcadores de posición en la variable rowTemplate con los valores de propiedad de objeto estándar. El HTML resultante, a continuación, se anexa a la tabla stock.

    Se llama a init pasándolo como una función de devolución de llamada que se ejecuta una vez finalizada la función de inicio asincrónica. Si se llama a init como una instrucción de JavaScript independiente después de llamar al inicio, la función produciría un error porque se ejecutaría inmediatamente sin esperar a la función de inicio a fin de establecer la conexión. En ese caso, la función init intentaría llamar a la función getAllStocks antes de establece la conexión al servidor.

    Cuando el servidor cambia el precio de una acción, llama a la updateStockPrice en los clientes conectados. La función se agrega a la propiedad de cliente del proxy stockTicker para ponerlo a disposición de las llamadas desde el servidor.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

    La función updateStockPrice da formato a un objeto estándar recibido del servidor en una fila de tabla de la misma forma que la función init. Sin embargo, en lugar de anexar la fila a la tabla, busca la fila actual de la acción en la tabla y reemplaza esa fila por uno nuevo.

<a id="test"></a>

## <a name="test-the-application"></a>Probar la aplicación

1. Presione F5 para ejecutar la aplicación en modo de depuración.

    La tabla stock muestra inicialmente la "Cargando..." línea, a continuación, tras una breve demora que se muestran los datos de stock iniciales y, a continuación, los precios de cotización iniciar un cambio.

    ![Carga](tutorial-server-broadcast-with-signalr/_static/image6.png)

    ![Tabla inicial de stock](tutorial-server-broadcast-with-signalr/_static/image7.png)

    ![Tabla stock recibir los cambios de servidor](tutorial-server-broadcast-with-signalr/_static/image8.png)
2. Copie la dirección URL de la barra de direcciones del explorador y péguela en una o varias ventanas de explorador nueva.

    La visualización del material inicial es el mismo que el primer explorador y los cambios se realizan simultáneamente.
3. Cierre todos los exploradores y abra un nuevo explorador, vaya a la misma dirección URL.

    El objeto singleton StockTicker ha continuado ejecutar en el servidor, por lo que la presentación de la tabla stock muestra que siguieron las poblaciones a cambiar. (No se ve en la tabla con cero inicial cambiar cifras.)
4. Cierre el explorador.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Habilite el registro

SignalR tiene una función de registro integrados que se puede habilitar en el cliente para ayudar a solucionar problemas. En esta sección Habilitar el registro y vea los ejemplos que muestran cómo los registros de indican qué de los siguientes métodos de transporte usa SignalR:

- [WebSockets](http://en.wikipedia.org/wiki/WebSocket), admitido por IIS 8 y los exploradores actuales.
- [Eventos de servidor envió](http://en.wikipedia.org/wiki/Server-sent_events), admitido por los exploradores distintos de Internet Explorer.
- [Marco para siempre](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), admitido por Internet Explorer.
- [AJAX de sondeo prolongado](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), compatible con todos los exploradores.

En una conexión determinada, SignalR elige el mejor método de transporte que admiten el servidor y el cliente.

1. Abra *StockTicker.js* y agregue una línea de código para habilitar el registro inmediatamente antes del código que inicializa la conexión al final del archivo:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js)]
2. Presione F5 para ejecutar el proyecto.
3. Abra la ventana de herramientas de desarrollador de su explorador y seleccione la consola para ver los registros. Es posible que deba actualizar la página para ver los registros de Signalr negocie el método de transporte para una conexión nueva.

    Si está ejecutando Internet Explorer 10 en Windows 8 (IIS 8), el método de transporte es WebSockets.

    ![Consola de IE 10 IIS 8](tutorial-server-broadcast-with-signalr/_static/image9.png)

    Si está ejecutando Internet Explorer 10 en Windows 7 (IIS 7.5), el método de transporte es iframe.

    ![Consola IE 10, IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image10.png)

    En Firefox, instale el complemento Firebug para obtener una ventana de consola. Si está ejecutando el 19 de Firefox en Windows 8 (IIS 8), el método de transporte es WebSockets.

    ![Firefox 19 IIS 8 Websockets](tutorial-server-broadcast-with-signalr/_static/image11.png)

    Si está ejecutando el 19 de Firefox en Windows 7 (IIS 7.5), el método de transporte es eventos enviados por el servidor.

    ![Consola de IIS 7.5 Firefox 19](tutorial-server-broadcast-with-signalr/_static/image12.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Instalar y revise el ejemplo StockTicker completo

La aplicación StockTicker instalado por el [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) paquete NuGet incluye más características que la versión simplificada que acaba de crear desde cero. En esta sección del tutorial, instale el paquete NuGet y revise las nuevas características y el código que implementa en ellos. Si instala el paquete sin necesidad de realizar los pasos anteriores de este tutorial, debe agregar una clase de inicio OWIN al proyecto. Este paso se explica en el archivo readme.txt del paquete NuGet.

### <a name="install-the-signalrsample-nuget-package"></a>Instale el paquete SignalR.Sample NuGet

1. En **el Explorador de soluciones**, haga clic en el proyecto y haga clic en **administrar paquetes de NuGet**.
2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **Online**, escriba *SignalR.Sample* en el **buscar en línea** cuadro y, a continuación, haga clic en  **Instalar** en el **SignalR.Sample** paquete.

    ![Instalar paquete SignalR.Sample](tutorial-server-broadcast-with-signalr/_static/image13.png)
3. En **el Explorador de soluciones**, expanda el *SignalR.Sample* carpeta que se creó al instalar el paquete SignalR.Sample.
4. En el *SignalR.Sample* carpeta, haga clic en *StockTicker.html*y, a continuación, haga clic en **establecer como página principal**.

    > [!NOTE]
    > Instalación de SignalR.Sample NuGet el paquete podría cambiar la versión de jQuery que tiene en su *Scripts* carpeta. El nuevo *StockTicker.html* archivo que instala el paquete en el *SignalR.Sample* carpeta estará sincronizado con la versión de jQuery que instala el paquete, pero si desea ejecutar el original *StockTicker.html* archivo nuevo, es posible que deba actualizar la referencia de jQuery en la etiqueta de script en primer lugar.

### <a name="run-the-application"></a>Ejecutar la aplicación

1. Presione F5 para ejecutar la aplicación.

    Además de la cuadrícula que vio anteriormente, la aplicación de tablero de cotizaciones completa muestra una ventana de desplazamiento horizontal que muestra los mismos datos de stock. Al ejecutar la aplicación por primera vez, el mercado"" es "closed" y verá una cuadrícula estática y una ventana del indicador que no es el desplazamiento.

    ![Inicio de la pantalla de StockTicker](tutorial-server-broadcast-with-signalr/_static/image14.png)

    Al hacer clic en **Open Market**, **Live bolsa** cuadro comienza a desplazarse horizontalmente y se inicia el servidor para difundir periódicamente los cambios de precio de las acciones de forma aleatoria. Cada vez que una cotización cambia tanto el **Live tabla Stock** cuadrícula y el **tablero de cotizaciones en directo** cuadro se actualizan. Cuando cambie el precio de una acción es positivo, se muestra el paquete de acciones con un fondo verde, y cuando el cambio es negativo, se muestra el paquete de acciones con un fondo rojo.

    ![Mercado de aplicación StockTicker, abrir](tutorial-server-broadcast-with-signalr/_static/image15.png)

    El **mercado cerrar** botón deja de los cambios y el desplazamiento del tablero y el **restablecer** botón restablece todos los datos de acciones al estado inicial antes de iniciar los cambios de precio. Si abre más ventanas del explorador y vaya a la misma dirección URL, verá los mismos datos que se actualiza dinámicamente al mismo tiempo en cada explorador. Al hacer clic en uno de los botones, todos los exploradores responden la misma manera a la vez.

### <a name="live-stock-ticker-display"></a>Visualización de tablero de cotizaciones en directo

El **Live bolsa** presentación es una lista sin ordenar en un elemento div que tiene el formato en una sola línea con los estilos CSS. El teletipo se inicializa y se actualiza la misma manera que la tabla: reemplazando los marcadores de posición en una &lt;li&gt; cadena de plantilla y agregar dinámicamente la &lt;li&gt; elementos a la &lt;ul&gt; elemento. El desplazamiento se realiza mediante la función animar jQuery para variar el margen izquierda de la lista sin ordenar en el elemento div.

El tablero de cotizaciones HTML:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

El tablero de cotizaciones CSS:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

Desplácese por el código jQuery que hace:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Métodos adicionales en el servidor que el cliente puede llamar a

La clase StockTickerHub define cuatro métodos adicionales que puede llamar el cliente:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

En respuesta a los botones en la parte superior de la página se denominan OpenMarket, CloseMarket y restablecimiento. Muestra el patrón de un cliente desencadenar un cambio de estado que se propaga inmediatamente a todos los clientes. Cada uno de estos métodos llama a un método en la clase StockTicker que afecte el estado de mercado cambiar y, a continuación, transmite el estado nueva.

En la clase StockTicker, se mantiene el estado del mercado por una propiedad MarketState que devuelve un valor de enumeración MarketState:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Cada uno de los métodos que cambian el estado de mercado hacerlo dentro de un bloqueo porque la clase StockTicker debe ser seguro para subprocesos:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Para asegurarse de que este código es seguro para subprocesos, el \_marketState campo que respalda la propiedad MarketState se marca como volátil,

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Los métodos BroadcastMarketStateChange y BroadcastMarketReset son similares al método BroadcastStockPrice que ya hemos visto, salvo que llaman a diferentes métodos definidos en el cliente:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Funciones adicionales en el cliente que el servidor puede llamar a

La función updateStockPrice ahora controla la cuadrícula y la presentación de tableros de cotizaciones y usa jQuery.Color parpadea colores verde y rojo.

Nuevas funciones en *SignalR.StockTicker.js* habilitar y deshabilitar los botones basados en estado de mercado, y detener o iniciar el desplazamiento horizontal de teletipo ventana. Puesto que se agregan varias funciones para ticker.client, el [jQuery ampliar la función](http://api.jquery.com/jQuery.extend/) se usa para agregarlos.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Programa de instalación de cliente adicionales después de establecer la conexión

Una vez que el cliente establece la conexión, tiene cierto trabajo adicional para hacer: averiguar si el mercado está abierto o cerrado con el fin de llamar a la función de marketClosed o marketOpened adecuado y asociar las llamadas al método de servidor a los botones.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Los métodos de servidor no son conecté los botones hasta que una vez establecida la conexión, para que el código no se puede tratar de llamar a los métodos de servidor para que estén disponibles.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido cómo programar una aplicación de SignalR que transmite mensajes desde el servidor a todos los clientes conectados tanto de forma periódica en respuesta a las notificaciones desde cualquier cliente. El patrón de uso de una instancia singleton de multiproceso para mantener el estado del servidor también puede usarse también en escenarios de juego en línea multijugador. Para obtener un ejemplo, vea [ShootR juego basado en SignalR](https://github.com/NTaylorMullen/ShootR).

Para ver tutoriales que muestran escenarios de comunicación punto a punto, consulte [Introducción a SignalR](introduction-to-signalr.md) y [actualizar en tiempo real con SignalR](tutorial-high-frequency-realtime-with-signalr.md).

Para obtener información sobre los conceptos más avanzados de desarrollo de SignalR, visite los sitios siguientes para recursos y código fuente de SignalR:

- [SignalR de ASP.NET](../../index.md)
- [Proyecto de SignalR](http://signalr.net/)
- [SignalR Github y ejemplos](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

Para ver un tutorial sobre cómo implementar una aplicación de SignalR en Azure, consulte [usando SignalR con Web Apps en Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Para obtener información detallada sobre cómo implementar un proyecto web de Visual Studio a un sitio Web de Windows Azure, consulte [crear una aplicación web ASP.NET en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
