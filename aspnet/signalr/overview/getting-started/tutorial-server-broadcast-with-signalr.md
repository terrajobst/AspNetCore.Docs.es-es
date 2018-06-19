---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutorial: Servidor difusión con SignalR 2 | Documentos de Microsoft'
author: tdykstra
description: Este tutorial muestra cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de difusión de servidor. Difusión de servidor significa que esa común...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: de4ccb4f0865e250fa0d78a9707fe5129c78e764
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876818"
---
<a name="tutorial-server-broadcast-with-signalr-2"></a>Tutorial: Servidor difusión con SignalR 2
====================
por [Tom Dykstra](https://github.com/tdykstra), [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de difusión de servidor. Difusión de servidor significa que se inician las comunicaciones enviadas a los clientes por el servidor. Este escenario requiere un enfoque de programación diferente que en los escenarios de peer-to-peer como las aplicaciones de chat, en el que se inician las comunicaciones enviadas a los clientes por uno o varios de los clientes.
> 
> La aplicación que creará en este tutorial simula una cotización bursátil, un escenario típico para la funcionalidad de difusión de servidor.
> 
> En este tema se escribió originalmente por Patrick Fletcher.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versión 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Uso de Visual Studio 2012 con este tutorial
> 
> 
> Para usar Visual Studio 2012 con este tutorial, realice lo siguiente:
> 
> - Actualización de su [el Administrador de paquetes](http://docs.nuget.org/docs/start-here/installing-nuget) a la versión más reciente.
> - Instalar el [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx).
> - El instalador de plataforma Web, buscar e instalar **2013.1 de herramientas Web para Visual Studio 2012 y ASP.NET**. Esto instalará como plantillas de Visual Studio para las clases de SignalR **concentrador**.
> - Algunas plantillas (como **clase de inicio de OWIN**) no estará disponible; para ello, utilice un archivo de clase en su lugar.
> 
> 
> ## <a name="tutorial-versions"></a>Versiones de tutoriales
> 
> Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Información general

En este tutorial, podrá crear una aplicación de cotización bursátil que sea representativo de aplicaciones en tiempo real en el que desea periódicamente "Insertar" o difusión, las notificaciones desde el servidor a todos los clientes conectados. En la primera parte de este tutorial, creará una versión simplificada de la aplicación desde el principio. En el resto del tutorial, podrá instalar un paquete de NuGet que contiene características adicionales y revisar el código para estas características.

La aplicación que compilará en la primera parte de este tutorial muestra una cuadrícula con datos de acciones.

![Versión inicial de indicador de cotizaciones](tutorial-server-broadcast-with-signalr/_static/image1.png)

Periódicamente el servidor aleatoriamente actualiza cotizaciones e inserta las actualizaciones a todos los clientes conectados. En el explorador los números y símbolos en el **cambiar** y **%** columnas cambian dinámicamente en respuesta a las notificaciones desde el servidor. Si abre exploradores adicionales a la misma dirección URL, todos ellos muestran los mismos datos y los mismos cambios a los datos al mismo tiempo.

Este tutorial contiene las siguientes secciones:

- [Requisitos previos](#prerequisites)
- [Crear el proyecto](#createproject)
- [Configurar el código de servidor](#server)
- [Configurar el código de cliente](#client)
- [Probar la aplicación](#test)
- [Habilitar el registro](#enablelogging)
- [Instalar y revise el ejemplo completo de indicador de cotizaciones](#fullsample)
- [Pasos siguientes](#nextsteps)

El [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) paquete NuGet instala una aplicación de cotización bursátil simulado de ejemplo en un proyecto de Visual Studio.

> [!NOTE]
> Si no desea trabajar a través de los pasos necesarios para compilar la aplicación, puede instalar el paquete de SignalR.Sample en un nuevo proyecto de aplicación Web ASP.NET vacía. Si instala el paquete de NuGet sin realizar los pasos de este tutorial, **debe seguir las instrucciones que aparecen en el archivo readme.txt**. Para ejecutar el paquete que se debe agregar una clase de inicio OWIN que llama al método ConfigureSignalR en el paquete instalado. Recibirá un error si no se agrega la clase de inicio OWIN.


<a id="prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que tiene instalado en el equipo de Visual Studio 2013. Si no tiene Visual Studio, consulte [descarga ASP.NET](https://www.asp.net/downloads) para obtener el libre Visual Studio 2013 Express.

<a id="createproject"></a>

## <a name="create-the-project"></a>Crear el proyecto

1. Desde el **archivo** menú, haga clic en **nuevo proyecto**.
2. En el **nuevo proyecto** cuadro de diálogo, expanda **C#** en **plantillas** y seleccione **Web**.
3. Seleccione el **aplicación Web ASP.NET vacía** plantilla, el nombre del proyecto *SignalR.StockTicker*y haga clic en **Aceptar**.

    ![Cuadro de diálogo Nuevo proyecto](tutorial-server-broadcast-with-signalr/_static/image2.png)
4. En el **ASP.NET nueva** ventana del proyecto, deje **vacía** seleccionada y haga clic en **crear proyecto**.

    ![Cuadro de diálogo nuevo proyecto de ASP](tutorial-server-broadcast-with-signalr/_static/image3.png)

<a id="server"></a>

## <a name="set-up-the-server-code"></a>Configurar el código de servidor

En esta sección Configurar el código que se ejecuta en el servidor.

### <a name="create-the-stock-class"></a>Crear la clase de cotizaciones

Empiece por crear la clase del modelo de material que va a utilizar para almacenar y transmitir información sobre una población.

1. Crear un nuevo archivo de clase en la carpeta del proyecto, asígnele el nombre *Stock.cs*y, a continuación, reemplace el código de plantilla con el código siguiente:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Las dos propiedades que se configuran al crear las existencias son el símbolo (por ejemplo, MSFT de Microsoft) y el precio. Las demás propiedades dependen de cómo y cuándo establecer precio. La primera vez que establece el precio, el valor obtiene propaga a DayOpen. Horas posteriores al establecer precio, el cambio y se calculan los valores de propiedad CambioPorcentual se basan en la diferencia entre el precio y DayOpen.

### <a name="create-the-stockticker-and-stocktickerhub-classes"></a>Crear las clases de indicador de cotizaciones y StockTickerHub

Deberá usar la API de concentrador de SignalR para controlar la interacción del servidor al cliente. Una clase StockTickerHub que se deriva de la clase de concentrador de SignalR controlará recibir conexiones y llamadas a métodos de los clientes. También debe mantener los datos de acciones y ejecutar un objeto de temporizador para desencadenar periódicamente las actualizaciones de precio, independientemente de las conexiones de cliente. No se puede colocar estas funciones en una clase base de datos central, porque las instancias de base de datos central son transitorias. Se crea una instancia de la clase de base de datos central para cada operación en el concentrador, por ejemplo, las conexiones y las llamadas desde el cliente al servidor. Por lo que el mecanismo que mantiene los datos de acciones, las actualizaciones de los precios y difunde las actualizaciones de precios debe ejecutarse en una clase independiente, que se le asigna un nombre de indicador de cotizaciones.

![Difusión de indicador de cotizaciones](tutorial-server-broadcast-with-signalr/_static/image5.png)

Desea que sólo una instancia de la clase de indicador de cotizaciones para que se ejecute en el servidor, por lo que necesitará configurar una referencia de cada instancia de StockTickerHub a la instancia singleton del indicador de cotizaciones. La clase de indicador de cotizaciones debe ser capaz de transmitir a los clientes porque tiene los datos de acciones y desencadenadores de las actualizaciones, pero indicador de cotizaciones no es una clase de base de datos central. Por lo tanto, la clase de indicador de cotizaciones debe obtener una referencia al objeto de contexto de conexión de concentrador de SignalR. A continuación, puede usar el objeto de contexto de conexión de SignalR para difundirlo a los clientes.

1. En **el Explorador de soluciones**, haga clic en el proyecto y haga clic en **agregar | Clase de concentrador de SignalR (v2)**.
2. Nombre del nuevo centro de *StockTickerHub.cs*y, a continuación, haga clic en **agregar**. Paquetes de SignalR NuGet se agregará al proyecto.
3. Reemplace el código de plantilla con el código siguiente:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

    El [concentrador](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) clase se utiliza para definir métodos que se pueden llamar los clientes en el servidor. Define un método: `GetAllStocks()`. Cuando un cliente se conecta inicialmente con el servidor, llamará a este método para obtener una lista de todas las existencias con sus precios actuales. Puede ejecutar de forma sincrónica y devolver el método `IEnumerable<Stock>` porque está devolviendo datos de la memoria. Si el método tuvo que obtener los datos por hacer algo que implicaría espera, por ejemplo, una búsqueda de la base de datos o una llamada de servicio web, debe especificar `Task<IEnumerable<Stock>>` como el valor devuelto para habilitar el procesamiento asincrónico. Para obtener más información, consulte [Guía de API de concentradores de ASP.NET SignalR - Server - cuándo se debe ejecutar de forma asincrónica](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

    El atributo HubName especifica cómo hacer referencia a la central en el código de JavaScript en el cliente. El nombre predeterminado en el cliente si no utiliza este atributo es una versión de grafía de camel del nombre de clase, que en este caso sería stockTickerHub.

    Como verá más adelante cuando se crea la clase de indicador de cotizaciones, se crea una instancia de singleton de la clase en la propiedad de instancia estática. Que instancia singleton del indicador de cotizaciones permanece en memoria independientemente de cuántos clientes conectan o desconexión y esa instancia es lo que usa el método GetAllStocks para devolver información bursátil actual.
4. Crear un nuevo archivo de clase en la carpeta del proyecto, asígnele el nombre *StockTicker.cs*y, a continuación, reemplace el código de plantilla con el código siguiente:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

    Puesto que varios subprocesos se ejecutará la misma instancia de código del indicador de cotizaciones, la clase de indicador de cotizaciones debe ser seguro para subprocesos.

    ### <a name="storing-the-singleton-instance-in-a-static-field"></a>Almacenar la instancia de singleton en un campo estático

    El código inicializa el método estático \_campo de instancia que respalda la propiedad de instancia con una instancia de la clase y esta es la única instancia de la clase que se pueden crear, porque el constructor está marcado como privado. [La inicialización diferida](https://msdn.microsoft.com/library/dd997286.aspx) se usa para la \_campo de instancia, no por motivos de rendimiento pero para asegurarse de que la creación de instancias es seguro para subprocesos.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

    Cada vez que un cliente se conecta al servidor, una nueva instancia de la clase StockTickerHub que se ejecuta en un subproceso independiente Obtiene la instancia de singleton de indicador de cotizaciones desde la propiedad estática StockTicker.Instance, tal y como se vio anteriormente en la clase StockTickerHub.

    ### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Almacenar datos de acciones de ConcurrentDictionary

    El constructor inicializa la \_colección las existencias con algunos datos de acciones de ejemplo y GetAllStocks devuelve las existencias. Como se vio anteriormente, esta colección de cotizaciones se devuelve a su vez por StockTickerHub.GetAllStocks que es un método de servidor en la clase de base de datos central que pueden llamar los clientes.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

    La colección de las existencias se define como un [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) tipo de seguridad para subprocesos. Como alternativa, podría utilizar un [diccionario](https://msdn.microsoft.com/library/xfhwa508.aspx) objeto y bloquear explícitamente el diccionario cuando realiza cambios en él.

    Para esta aplicación de ejemplo, es correcto para almacenar datos de la aplicación en la memoria y perder los datos cuando se elimina la instancia de indicador de cotizaciones. En una aplicación real podría funcionar con un almacén de datos back-end como una base de datos.

    ### <a name="periodically-updating-stock-prices"></a>Actualizaciones periódicas de cotizaciones

    El constructor se inicia un objeto de temporizador que llama periódicamente a los métodos que actualizan los precios de las acciones de forma aleatoria.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

    Se llama a UpdateStockPrices por el temporizador, que se pasa null en el parámetro state. Antes de actualizar los precios, se aplica un bloqueo el \_updateStockPricesLock objeto. El código comprueba si otro subproceso ya está actualizando los precios y, a continuación, llama a TryUpdateStockPrice en cada acción en la lista. El método TryUpdateStockPrice decide si debe cambiar el precio de las acciones y la cantidad para cambiarlo. Si se cambia el precio de las acciones, se llama BroadcastStockPrice para el cambio de precio de las acciones de difusión a todos los clientes conectados.

    El \_updatingStockPrices marca está marcada como [volátiles](https://msdn.microsoft.com/library/x13ttww7.aspx) para asegurarse de que el acceso a él es seguro para subprocesos.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

    En una aplicación real, el método TryUpdateStockPrice llamaría a un servicio web para buscar el precio; en este código se utiliza un generador de números aleatorios para realizar cambios de forma aleatoria.

    ### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Obtener el contexto de SignalR para que la clase de indicador de cotizaciones puede difundir a los clientes

    Dado que los cambios de precio aquí se originan en el objeto de indicador de cotizaciones, este es el objeto que se debe llamar a un método de updateStockPrice en todos los clientes conectados. En una clase de base de datos central tiene una API para llamar a métodos de cliente, pero no se deriva de la clase de base de datos central de indicador de cotizaciones y no tiene una referencia a cualquier objeto de base de datos central. Por lo tanto, para difundir a los clientes conectados, la clase de indicador de cotizaciones tiene que obtener la instancia de contexto de SignalR para la clase StockTickerHub y usarlo para llamar a métodos en los clientes.

    El código obtiene una referencia al contexto de SignalR cuando crea la instancia de clase singleton, que hacen referencia a pasa al constructor, y el constructor lo coloca en la propiedad de los clientes.

    Hay dos razones por las que obtener el contexto de una sola vez: obtener el contexto es una operación costosa, y se asegura de obtener de una vez que se conserva el orden deseado de mensajes enviados a los clientes.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

    Obtener la propiedad de los clientes del contexto y escribirlos en la propiedad StockTickerClient le permite escribir código para llamar a los métodos de cliente que tenga la misma apariencia como lo haría en una clase de base de datos central. Por ejemplo, puede escribir Clients.All.updateStockPrice(stock) para difundir a todos los clientes.

    El método updateStockPrice al que se llama en BroadcastStockPrice no existe todavía; se agregará más tarde cuando se escribe código que se ejecuta en el cliente. Puede hacer referencia a updateStockPrice aquí porque Clients.All es dinámico, lo que significa que la expresión se evaluará en tiempo de ejecución. Cuando se ejecuta esta llamada al método, SignalR enviará el nombre del método y el valor del parámetro al cliente y si el cliente tiene un método denominado updateStockPrice, se llamará a ese método y el valor del parámetro se pasará a él.

    Clients.All significa enviar a todos los clientes. SignalR proporciona otras opciones para especificar qué clientes o grupos de clientes para enviar a. Para obtener más información, consulte [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registrar la ruta de SignalR

El servidor necesita saber qué dirección URL para interceptar y dirigir a SignalR. Para hacer que vas a agregar y clase de inicio OWIN.

1. En **el Explorador de soluciones**, haga clic en el proyecto y, a continuación, haga clic en **agregar | Clase de inicio OWIN**. Nombre de la clase **Startup.cs**.
2. Reemplace el código de **Startup.cs** con lo siguiente.

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Ahora ha completado al configurar el código de servidor. En la siguiente sección podrá configurar el cliente.

<a id="client"></a>

## <a name="set-up-the-client-code"></a>Configurar el código de cliente

1. Crear un nuevo archivo HTML en la carpeta del proyecto y asígnele el nombre *StockTicker.html*.
2. Reemplace el código de plantilla con el código siguiente.

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html)]

    El código HTML crea una tabla con 5 columnas, una fila de encabezado y una fila de datos con una sola celda que abarca todas las 5 columnas. La fila de datos muestra "Cargando..." y solo se mostrará en breve cuando se inicia la aplicación. Código de JavaScript quitará esa fila y agregar en sus lugar filas con cotizaciones datos recuperados desde el servidor.

    Las etiquetas de script especifican el archivo de script de jQuery, el archivo de script de SignalR core, el archivo de script de SignalR servidores proxy y un archivo de script de indicador de cotizaciones que creará más adelante. El archivo de script de proxy de SignalR, que especifica la dirección URL "/ signalr/concentradores", se genera dinámicamente y define los métodos de proxy para los métodos de la clase base de datos central, en este caso para StockTickerHub.GetAllStocks. Si lo prefiere, puede generar este archivo JavaScript manualmente mediante [utilidades de SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) y deshabilitar la creación dinámica de archivos en la llamada al método MapHubs.
3. > [!IMPORTANT]
   > Asegúrese de que el archivo JavaScript hace referencia en *StockTicker.html* son correctos. Es decir, asegúrese de que la versión de jQuery en la etiqueta de script (1.10.2 en el ejemplo) es la misma que la versión de jQuery en el proyecto *Scripts* carpeta y asegúrese de que la versión de SignalR en la etiqueta de script es el mismo que el objeto de SignalR versión del proyecto *Scripts* carpeta. Si es necesario, cambie los nombres de archivo en las etiquetas de script.
4. En **el Explorador de soluciones**, haga clic en *StockTicker.html*y, a continuación, haga clic en **establecer como página de inicio**.
5. Crear un nuevo archivo de JavaScript en la carpeta del proyecto y asígnele el nombre *StockTicker.js*...
6. Reemplace el código de plantilla con el código siguiente:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

    $.connection hace referencia a los servidores proxy de SignalR. El código obtiene una referencia al servidor proxy para la clase StockTickerHub y lo coloca en la variable de tableros de cotizaciones. El nombre del proxy es el nombre que se establece mediante el atributo [HubName]:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

    Después de que se definen todas las variables y funciones, la última línea de código en el archivo inicializa la conexión de SignalR mediante una llamada a la función de inicio de SignalR. La función de inicio se ejecuta de forma asincrónica y devuelve un [jQuery diferida objeto](http://api.jquery.com/category/deferred-object/), lo cual significa que puede llamar a la función listo para especificar la función que se llamará cuando se complete la operación asincrónica...

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

    La función init llama a la función getAllStocks en el servidor y utiliza la información devuelta por el servidor para actualizar la tabla stock. Tenga en cuenta que de forma predeterminada, tendrá que usar la grafía camel en el cliente aunque el nombre del método pascal de mayúsculas y minúsculas en el servidor. La regla de grafía camel solo se aplica a los métodos, no objetos. Por ejemplo, consulte existencias. Símbolos y existencias. Precio, no stock.symbol o stock.price.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

    Si desea utilizar la grafía pascal en el cliente, o si desea utilizar un nombre de método completamente diferentes, puede decorar el método de concentrador con el atributo HubMethodName del mismo modo representativo de la propia clase de base de datos central con el atributo HubName.

    En el método init, HTML para una fila de la tabla se crea para cada objeto de acción recibida desde el servidor que realiza la llamada formatStock a las propiedades de formato del objeto estándar y, a continuación, mediante una llamada a suplantar (que se define en la parte superior de *StockTicker.js*) para reemplazar los marcadores de posición en la variable rowTemplate con los valores de propiedad de objeto estándar. El HTML resultante, a continuación, se anexa a la tabla stock.

    Puede llamar a init pasarlo como una función de devolución de llamada que se ejecuta una vez completada la función de inicio asincrónica. Si se llama a init como una instrucción de JavaScript independiente después de llamar a Inicio, la función produciría un error porque se ejecutaría inmediatamente sin esperar a la función de inicio a fin de establecer la conexión. En ese caso, la función init intentaría llamar a la función getAllStocks antes de establece la conexión al servidor.

    Cuando el servidor cambia el precio de la acción, llama a la updateStockPrice en los clientes conectados. La función se agrega a la propiedad de cliente del proxy de indicador de cotizaciones para ponerlo a disposición de las llamadas desde el servidor.

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

    La función updateStockPrice da formato a un objeto de cotizaciones recibido del servidor en una fila de la tabla del mismo modo que la función init. Sin embargo, en lugar de anexar la fila a la tabla, busca la fila actual de la acción en la tabla y se reemplaza esa fila con una nueva.

<a id="test"></a>

## <a name="test-the-application"></a>Probar la aplicación

1. Presione F5 para ejecutar la aplicación en modo de depuración.

    La tabla stock muestra inicialmente la "Cargando..." línea, a continuación, tras un breve retraso que se muestran los datos de cotizaciones iniciales y, a continuación, los precios de cotización podrán cambiar.

    ![Carga](tutorial-server-broadcast-with-signalr/_static/image6.png)

    ![Tabla stock inicial](tutorial-server-broadcast-with-signalr/_static/image7.png)

    ![Tabla stock recibir los cambios de servidor](tutorial-server-broadcast-with-signalr/_static/image8.png)
2. Copie la dirección URL de la barra de direcciones del explorador y péguela en una o varias ventanas de explorador nueva.

    La visualización del material inicial es el mismo que el primer explorador y cambios se realizan de manera simultánea.
3. Cierre todos los exploradores y abrir una nueva ventana del explorador y, a continuación, vaya a la misma dirección URL.

    El objeto singleton de indicador de cotizaciones ha ido ejecutar en el servidor, por lo que la presentación de la tabla stock muestra que las existencias siguieron cambiar. (No se ve la tabla inicial con cero cambiar figuras.)
4. Cierre el explorador.

<a id="enablelogging"></a>

## <a name="enable-logging"></a>Habilite el registro

SignalR tiene una función de registro integrados que se puede habilitar en el cliente para ayudar en la solución de problemas. En esta sección Habilitar el registro y vea los ejemplos que muestran cómo registros indican que, de los siguientes métodos de transporte, use SignalR:

- [WebSockets](http://en.wikipedia.org/wiki/WebSocket), compatible con IIS 8 y los exploradores actuales.
- [Eventos de servidor envió](http://en.wikipedia.org/wiki/Server-sent_events), compatible con los exploradores que no sea Internet Explorer.
- [Marco para siempre](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), compatible con Internet Explorer.
- [Tiempo de sondeo de AJAX](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), compatible con todos los exploradores.

En cada conexión, SignalR elige el mejor método de transporte que admiten el servidor y el cliente.

1. Abra *StockTicker.js* y agregue una línea de código para habilitar el registro inmediatamente antes del código que inicializa la conexión al final del archivo:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js)]
2. Presione F5 para ejecutar el proyecto.
3. Abra la ventana de herramientas de desarrollador de su explorador y seleccione la consola para ver los registros. Es posible que deba actualizar la página para ver los registros de Signalr negociar el método de transporte para una nueva conexión.

    Si está ejecutando Internet Explorer 10 en Windows 8 (IIS 8), el método de transporte es WebSockets.

    ![Consola de Internet Explorer 10 IIS 8](tutorial-server-broadcast-with-signalr/_static/image9.png)

    Si está ejecutando Internet Explorer 10 en Windows 7 (IIS 7.5), el método de transporte es iframe.

    ![Internet Explorer 10 consola, IIS 7.5](tutorial-server-broadcast-with-signalr/_static/image10.png)

    En Firefox, instale el complemento Firebug para obtener una ventana de consola. Si está ejecutando Firefox 19 en Windows 8 (IIS 8), el método de transporte es WebSockets.

    ![Firefox 19 IIS 8 Websockets](tutorial-server-broadcast-with-signalr/_static/image11.png)

    Si está ejecutando Firefox 19 en Windows 7 (IIS 7.5), el método de transporte es eventos enviados por el servidor.

    ![La consola de IIS 7.5 Firefox 19](tutorial-server-broadcast-with-signalr/_static/image12.png)

<a id="fullsample"></a>

## <a name="install-and-review-the-full-stockticker-sample"></a>Instalar y revise el ejemplo completo de indicador de cotizaciones

La aplicación de indicador de cotizaciones que se instala mediante el [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) paquete de NuGet incluye más características que la versión simplificada que acaba de crear desde cero. En esta sección del tutorial, instale el paquete de NuGet y revise las nuevas características y el código que implementa. Si instala el paquete sin tener que realizar los pasos anteriores de este tutorial, debe agregar una clase de inicio OWIN para el proyecto. Este paso se explica en el archivo readme.txt del paquete de NuGet.

### <a name="install-the-signalrsample-nuget-package"></a>Instale el paquete SignalR.Sample NuGet

1. En **el Explorador de soluciones**, haga clic en el proyecto y haga clic en **administrar paquetes de NuGet**.
2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **en línea**, escriba *SignalR.Sample* en el **buscar en línea** cuadro y, a continuación, haga clic en  **Instalar** en el **SignalR.Sample** paquete.

    ![Instalar el paquete de SignalR.Sample](tutorial-server-broadcast-with-signalr/_static/image13.png)
3. En **el Explorador de soluciones**, expanda la *SignalR.Sample* carpeta a la que se creó al instalar el paquete SignalR.Sample.
4. En el *SignalR.Sample* carpeta, haga clic en *StockTicker.html*y, a continuación, haga clic en **establecer como página principal**.

    > [!NOTE]
    > Instalación de SignalR.Sample NuGet el paquete podría cambiar la versión de jQuery que tiene en su *Scripts* carpeta. El nuevo *StockTicker.html* archivo que el paquete se instala en el *SignalR.Sample* carpeta se sincronizarán con la versión de jQuery que instala el paquete, pero si desea ejecutar el original *StockTicker.html* archivo nuevo, es posible que deba actualizar la referencia de jQuery en la etiqueta de script en primer lugar.

### <a name="run-the-application"></a>Ejecutar la aplicación

1. Presione F5 para ejecutar la aplicación.

    Además de la cuadrícula que vio anteriormente, la aplicación completa cotizaciones muestra una ventana desplazable horizontal que muestra los mismos datos de cotizaciones. Al ejecutar la aplicación por primera vez, el "mercado" es "closed" y verá una cuadrícula estática y una ventana de tableros de cotizaciones que no es desplazable.

    ![Inicio de la pantalla de indicador de cotizaciones](tutorial-server-broadcast-with-signalr/_static/image14.png)

    Al hacer clic en **Open Market**, **Live bolsa** cuadro empieza a desplazarse horizontalmente, y el servidor se inicia para difundir periódicamente los cambios de precio de las acciones de forma aleatoria. Cada vez que un precio de las acciones cambia tanto el **Live tabla Stock** cuadrícula y el **Live bolsa** cuadro se actualizan. Al cambio de precio de la acción es positivo, las existencias se muestran con un fondo verde, y cuando el cambio es negativo, el material se muestra con un fondo rojo.

    ![Aplicación de indicador de cotizaciones, mercado abrir](tutorial-server-broadcast-with-signalr/_static/image15.png)

    El **mercado cerrar** botón deja de los cambios y el desplazamiento de tableros de cotizaciones y el **restablecer** botón restablece todos los datos de acciones al estado inicial antes de iniciar cambios de precio. Si abre más ventanas del explorador y vaya a la misma dirección URL, vea los mismos datos que se actualiza dinámicamente a la vez en cada explorador. Al hacer clic en uno de los botones, todos los exploradores responden igual al mismo tiempo.

### <a name="live-stock-ticker-display"></a>Presentación de bolsa en vivo

El **Live bolsa** presentación es una lista sin ordenar en un elemento div que tiene el formato en una sola línea con estilos CSS. Los tableros de cotizaciones se inicializa y actualiza la misma manera que la tabla: reemplazando los marcadores de posición en una &lt;li&gt; cadena de plantilla y agregar dinámicamente la &lt;li&gt; elementos a la &lt;ul&gt; elemento. El desplazamiento se realiza mediante la función animar jQuery para variar el margen izquierda de la lista sin ordenar dentro de la div.

La cotización bursátil HTML:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

La cotización bursátil CSS:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

Desplácese por el código de jQuery que permite:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Métodos adicionales en el servidor que el cliente puede llamar a

La clase StockTickerHub define cuatro métodos adicionales para que el cliente puede llamar:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

En respuesta a los botones en la parte superior de la página se denominan OpenMarket, CloseMarket y restablecimiento. Muestra el patrón de un cliente, lo que desencadenaría un cambio en un estado que se propaga inmediatamente a todos los clientes. Cada uno de estos métodos llama a un método en la clase de indicador de cotizaciones que afecta el estado de mercado cambiar y, a continuación, transmite el nuevo estado.

En la clase de indicador de cotizaciones, se mantiene el estado del mercado mediante una propiedad MarketState que devuelve un valor de enumeración MarketState:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Cada uno de los métodos que cambian el estado de mercado hacerlo dentro de un bloque de bloqueo ya que la clase de indicador de cotizaciones debe ser seguro para subprocesos:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Para asegurarse de que este código es seguro para subprocesos, el \_marketState campo que respalda la propiedad MarketState está marcado como volátil,

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Los métodos BroadcastMarketStateChange y BroadcastMarketReset son similares al método BroadcastStockPrice que ya ha visto, salvo que llaman diferentes métodos definidos en el cliente:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Funciones adicionales en el cliente que el servidor puede llamar a

La función updateStockPrice ahora controla la cuadrícula y la presentación de tableros de cotizaciones y usa jQuery.Color parpadee colores rojos y verdes.

Nuevas funciones en *SignalR.StockTicker.js* habilitar y deshabilitar los botones basados en estado en el mercado, y detener o iniciar el desplazamiento horizontal de tableros de cotizaciones ventana. Puesto que se han agregado varias funciones para ticker.client, el [jQuery extender función](http://api.jquery.com/jQuery.extend/) se usa para agregarlos.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Programa de instalación de cliente adicionales después de establecer la conexión

Una vez que el cliente establece la conexión, tiene algún trabajo adicional para hacer: averiguar si el mercado está abierta o cerrada para llamar a la función de marketClosed o marketOpened adecuado y asociar las llamadas de método de servidor a los botones.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Los métodos de servidor no son conectados los botones hasta que una vez establecida la conexión, por lo que el código no se intenta llamar a los métodos de servidor para que estén disponibles.

<a id="nextsteps"></a>

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido cómo programar una aplicación de SignalR que transmite mensajes desde el servidor a todos los clientes conectados de forma periódica y en respuesta a las notificaciones desde cualquier cliente. El patrón del uso de una instancia de singleton multiproceso para mantener el estado de servidor también se puede usar también en escenarios de juegos de varios jugadores en línea. Para obtener un ejemplo, vea [el juego de ShootR que se basa en SignalR](https://github.com/NTaylorMullen/ShootR).

Para ver los tutoriales que muestran escenarios de comunicación punto a punto, vea [Getting Started with SignalR](introduction-to-signalr.md) y [actualizar en tiempo real con SignalR](tutorial-high-frequency-realtime-with-signalr.md).

Para obtener información sobre conceptos de desarrollo de SignalR más avanzados, visite los siguientes sitios de código fuente de SignalR y recursos:

- [ASP.NET SignalR](../../index.md)
- [Proyecto de SignalR](http://signalr.net/)
- [SignalR Github y ejemplos](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

Para ver un tutorial sobre cómo implementar una aplicación de SignalR en Azure, consulte [utilizando SignalR con las aplicaciones Web en el servicio de aplicación de Azure](../deployment/using-signalr-with-azure-web-sites.md). Para obtener información detallada sobre cómo implementar un proyecto web de Visual Studio en un sitio Web de Windows Azure, consulte [crear una aplicación web ASP.NET en el servicio de aplicación de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
