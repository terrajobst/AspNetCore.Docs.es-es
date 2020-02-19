---
title: Modelos de hospedaje de Blazor de ASP.NET Core
author: guardrex
description: Comprenda Blazor los modelos de hospedaje de webassembly y Blazor Server.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: 54be0e032a60c69880f428e52f9d778032385dc5
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447053"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a>Modelos de hospedaje de Blazor de ASP.NET Core

Por [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor es un marco de trabajo web diseñado para ejecutar el lado cliente en el explorador en un entorno de tiempo de ejecución .NET basado en [Webassembly](https://webassembly.org/)( *Blazor webassembly*) o en el servidor en ASP.net Core ( *Blazor Server*). Independientemente del modelo de hospedaje, los modelos de aplicación y de componente *son los mismos*.

Para crear un proyecto para los modelos de hospedaje descritos en este artículo, consulte <xref:blazor/get-started>.

Para obtener una configuración avanzada, consulte <xref:blazor/hosting-model-configuration>.

## <a name="opno-locblazor-webassembly"></a>Blazor WebAssembly

El modelo de hospedaje principal de Blazor se está ejecutando en el lado cliente en el explorador de webassembly. La aplicación Blazor, sus dependencias y el entorno de ejecución de .NET se descargan en el explorador. La aplicación se ejecuta directamente en el subproceso de interfaz de usuario del explorador. Las actualizaciones de la interfaz de usuario y el control de eventos se producen dentro del mismo proceso. Los recursos de la aplicación se implementan como archivos estáticos en un servidor web o servicio capaz de servir contenido estático a los clientes.

![[! Operador. Webassembly NO-LOC (increíble)]: [! Operador. NO-LOC (increíble): la aplicación se ejecuta en un subproceso de interfaz de usuario dentro del explorador.](hosting-models/_static/blazor-webassembly.png)

Para crear una aplicación Blazor mediante el modelo de hospedaje del lado cliente, use la plantilla de **aplicación de WebassembleBlazor** ([dotnet New blazorwasm](/dotnet/core/tools/dotnet-new)).

Después de seleccionar la plantilla de **aplicaciónBlazor Webassembly** , tiene la opción de configurar la aplicación para usar un back-end de ASP.net Core activando la casilla **ASP.net Core hospedado** ([dotnet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)). La aplicación ASP.NET Core sirve a la aplicación de Blazor a los clientes. La aplicación Blazor webassembly puede interactuar con el servidor a través de la red mediante llamadas API Web o [SignalR](xref:signalr/introduction) (<xref:tutorials/signalr-blazor-webassembly>).

Las plantillas incluyen el script de `blazor.webassembly.js` que controla:

* Descarga del tiempo de ejecución de .NET, la aplicación y las dependencias de la aplicación.
* Inicialización del tiempo de ejecución para ejecutar la aplicación.

El modelo de hospedaje de webassembly de Blazor ofrece varias ventajas:

* No hay ninguna dependencia de servidor .NET. La aplicación funciona totalmente después de descargarla en el cliente.
* Los recursos de cliente y las capacidades se aprovechan por completo.
* El trabajo se descarga del servidor al cliente.
* No es necesario un servidor Web ASP.NET Core para hospedar la aplicación. Los escenarios de implementación sin servidor son posibles (por ejemplo, para dar servicio a la aplicación desde una red CDN).

Hay desventajas para Blazor hospedaje de webassembly:

* La aplicación está restringida a las capacidades del explorador.
* Se requiere hardware y software de cliente compatible (por ejemplo, compatibilidad con webassembly).
* El tamaño de descarga es mayor y las aplicaciones tardan más tiempo en cargarse.
* La compatibilidad con las herramientas y el tiempo de ejecución de .NET es menos madura. Por ejemplo, existen limitaciones en [.net Standard](/dotnet/standard/net-standard) la compatibilidad y la depuración.

## <a name="opno-locblazor-server"></a>Servidor de Blazor

Con el modelo de hospedaje de servidor de Blazor, la aplicación se ejecuta en el servidor desde una aplicación ASP.NET Core. Las actualizaciones de la interfaz de usuario, el control de eventos y las llamadas de JavaScript se controlan mediante una conexión [SignalR](xref:signalr/introduction).

![El explorador interactúa con la aplicación (hospedada dentro de una aplicación ASP.NET Core) en el servidor a través de un [! Operador. Conexión NO-LOC (Signalr)].](hosting-models/_static/blazor-server.png)

Para crear una aplicación de Blazor con el modelo de hospedaje de Blazor Server, use la plantilla de **aplicación de servidor** de ASP.net CoreBlazor ([dotnet New blazorserver](/dotnet/core/tools/dotnet-new)). La aplicación ASP.NET Core hospeda la aplicación de Blazor Server y crea el punto de conexión de SignalR donde se conectan los clientes.

La aplicación ASP.NET Core hace referencia a la clase `Startup` de la aplicación que se va a agregar:

* Servicios del lado servidor.
* La aplicación a la canalización de control de solicitudes.

El script de `blazor.server.js`&dagger; establece la conexión del cliente. Es responsabilidad de la aplicación conservar y restaurar el estado de la aplicación según sea necesario (por ejemplo, en caso de que se pierda una conexión de red).

El modelo de hospedaje de Blazor Server ofrece varias ventajas:

* El tamaño de la descarga es significativamente menor que una aplicación Blazor webassembly y la aplicación se carga mucho más rápido.
* La aplicación aprovecha al máximo las funcionalidades del servidor, incluido el uso de cualquier API compatible con .NET Core.
* .NET Core en el servidor se usa para ejecutar la aplicación, por lo que las herramientas de .NET existentes, como la depuración, funcionan según lo previsto.
* Se admiten clientes ligeros. Por ejemplo, las aplicaciones de Blazor Server funcionan con los exploradores que no admiten webassembly y en los dispositivos con restricción de recursos.
* La base de .NET/C# Code de la aplicación, incluido el código de componente de la aplicación, no se sirve a los clientes.

Hay desventajas para Blazor servidor que hospeda:

* Normalmente existe una mayor latencia. Cada interacción del usuario implica un salto de red.
* No hay compatibilidad sin conexión. Si se produce un error en la conexión de cliente, la aplicación deja de funcionar.
* La escalabilidad es desafiante para aplicaciones con muchos usuarios. El servidor debe administrar varias conexiones de cliente y controlar el estado del cliente.
* Se requiere un servidor ASP.NET Core para atender la aplicación. Los escenarios de implementación sin servidor no son posibles (por ejemplo, para dar servicio a la aplicación desde una red CDN).

&dagger;el script de `blazor.server.js` se sirve desde un recurso incrustado en el marco de ASP.NET Core Shared.

### <a name="comparison-to-server-rendered-ui"></a>Comparación con la interfaz de usuario representada por el servidor

Una manera de comprender las aplicaciones de Blazor Server es comprender cómo difiere de los modelos tradicionales para representar la interfaz de usuario en ASP.NET Core aplicaciones con vistas de Razor o Razor Pages. Ambos modelos usan el lenguaje Razor para describir el contenido HTML, pero difieren significativamente en el modo en que se representa el marcado.

Cuando se representa una página o vista de Razor, cada línea de código Razor emite HTML en forma de texto. Después de la representación, el servidor desecha la instancia de la página o la vista, incluido cualquier Estado que se haya producido. Cuando se produce otra solicitud para la página, por ejemplo, cuando se produce un error en la validación del servidor y se muestra el Resumen de la validación:

* La página completa se vuelve a representar en el texto HTML de nuevo.
* La página se envía al cliente.

Una aplicación Blazor se compone de elementos reutilizables de la interfaz de usuario denominada *componentes*. Un componente contiene C# código, marcado y otros componentes. Cuando se representa un componente, Blazor genera un gráfico de los componentes incluidos de forma similar a una Document Object Model HTML o XML (DOM). Este gráfico incluye el estado del componente contenido en propiedades y campos. Blazor evalúa el gráfico de componentes para generar una representación binaria del marcado. El formato binario puede ser:

* Se convierte en texto HTML (durante la representación previa&dagger;).
* Se usa para actualizar de forma eficaz el marcado durante la representación normal.

&dagger;la *representación* previa &ndash; el componente de Razor solicitado se compila en el servidor en HTML estático y se envía al cliente, donde se representa al usuario. Una vez realizada la conexión entre el cliente y el servidor, los elementos estáticos prerepresentados del componente se reemplazan por elementos interactivos. La representación previa hace que la aplicación tenga más capacidad de respuesta al usuario.

Una actualización de la interfaz de usuario en Blazor se desencadena mediante:

* Interacción con el usuario, como la selección de un botón.
* Desencadenadores de aplicaciones, como un temporizador.

El gráfico se representará y se calculará *una diferencia de interfaz de* usuario (diferencia). Esta diferencia es el conjunto más pequeño de ediciones DOM necesarias para actualizar la interfaz de usuario en el cliente. La diferencia se envía al cliente en un formato binario y se aplica mediante el explorador.

Un componente se desecha una vez que el usuario sale de él en el cliente. Mientras que un usuario interactúa con un componente, el estado del componente (servicios, recursos) debe mantenerse en la memoria del servidor. Dado que el servidor puede mantener el estado de muchos componentes al mismo tiempo, el agotamiento de la memoria es un problema que se debe solucionar. Para obtener instrucciones sobre cómo crear una aplicación de Blazor Server para garantizar el mejor uso de la memoria del servidor, consulte <xref:security/blazor/server>.

### <a name="circuits"></a>Circuitos

Una aplicación Blazor Server se basa en [ASP.NET Core SignalR](xref:signalr/introduction). Cada cliente se comunica con el servidor a través de una o varias conexiones SignalR llamadas un *circuito*. Un circuito es la abstracción de Blazoren SignalR conexiones que pueden tolerar interrupciones de red temporales. Cuando un cliente de Blazor ve que la conexión de SignalR está desconectada, intenta volver a conectarse al servidor mediante una nueva conexión de SignalR.

Cada pantalla del explorador (pestaña del explorador o iframe) que está conectada a una aplicación de Blazor Server usa una conexión SignalR. Esta es otra diferencia importante en comparación con las aplicaciones típicas representadas por el servidor. En una aplicación representada por el servidor, la apertura de la misma aplicación en varias pantallas del explorador normalmente no se traduce en demandas de recursos adicionales en el servidor. En una aplicación de Blazor Server, cada pantalla del explorador requiere un circuito independiente e instancias independientes del estado del componente que administra el servidor.

Blazor considera cerrar una pestaña del explorador o desplazarse a una dirección URL externa como terminación *correcta* . En el caso de una terminación correcta, el circuito y los recursos asociados se liberan inmediatamente. Un cliente también puede desconectarse de manera no correcta, por ejemplo, debido a una interrupción de la red. Blazor Server almacena los circuitos desconectados durante un intervalo configurable para permitir que el cliente vuelva a conectarse.

### <a name="ui-latency"></a>Latencia de IU

La latencia de la interfaz de usuario es el tiempo que tarda una acción iniciada en el momento en que se actualiza la interfaz de usuario. Los valores más pequeños para la latencia de la interfaz de usuario son imprescindibles para que una aplicación pueda responder a un usuario. En una aplicación de Blazor Server, cada acción se envía al servidor, se procesa y se devuelve una diferencia de la interfaz de usuario. Por lo tanto, la latencia de la interfaz de usuario es la suma de la latencia de red y la latencia del servidor en el procesamiento de la acción.

En el caso de una aplicación de línea de negocio que está limitada a una red corporativa privada, el efecto en las percepciones de usuario de latencia debido a la latencia de red suele ser imperceptibles. En el caso de una aplicación implementada a través de Internet, la latencia puede ser apreciable para los usuarios, especialmente si los usuarios están ampliamente distribuidos geográficamente.

El uso de memoria también puede contribuir a la latencia de la aplicación. El aumento del uso de memoria da como resultado la recolección frecuente de elementos no utilizados o la paginación de memoria en el disco, y ambos degradan el rendimiento de la aplicación y, por consiguiente, aumentan la latencia Para más información, consulte <xref:security/blazor/server>.

las aplicaciones de Blazor Server deben optimizarse para minimizar la latencia de la interfaz de usuario al reducir la latencia de red y el uso de memoria. Para obtener información acerca de cómo medir la latencia de red, consulte <xref:host-and-deploy/blazor/server#measure-network-latency>. Para obtener más información sobre SignalR y Blazor, vea:

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a>Conexión al servidor

las aplicaciones de Blazor Server requieren una conexión de SignalR activa con el servidor. Si se pierde la conexión, la aplicación intenta volver a conectarse al servidor. Siempre que el estado del cliente todavía esté en la memoria, la sesión del cliente se reanudará sin perder el estado.

Una aplicación de Blazor Server se representa en respuesta a la primera solicitud de cliente, que configura el estado de la interfaz de usuario en el servidor. Cuando el cliente intenta crear una conexión SignalR, el cliente debe volver a conectarse al mismo servidor. las aplicaciones de Blazor Server que usan más de un servidor back-end deben implementar *sesiones permanentes* para conexiones SignalR.

Se recomienda usar [Azure SignalR Service](/azure/azure-signalr) para las aplicaciones Blazor Server. El servicio permite el escalado vertical de una aplicación Blazor Server a un gran número de conexiones SignalR simultáneas. Las sesiones permanentes están habilitadas para el servicio Azure SignalR estableciendo la opción de `ServerStickyMode` o el valor de configuración del servicio en `Required`. Para más información, consulte <xref:host-and-deploy/blazor/server#signalr-configuration>.

Cuando se usa IIS, las sesiones permanentes se habilitan con el enrutamiento de solicitud de aplicaciones. Para más información, vea [Equilibrio de carga HTTP mediante el enrutamiento de solicitud de aplicaciones](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:blazor/get-started>
* <xref:signalr/introduction>
* <xref:blazor/hosting-model-configuration>
* <xref:tutorials/signalr-blazor-webassembly>
