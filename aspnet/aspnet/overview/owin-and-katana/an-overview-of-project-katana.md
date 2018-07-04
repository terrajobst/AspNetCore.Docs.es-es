---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Información general del proyecto Katana | Microsoft Docs
author: howarddierking
description: El marco de ASP.NET ha existido durante más de diez años, y la plataforma ha permitido el desarrollo de innumerables sitios Web y servicios. Como aplicación Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/30/2013
ms.topic: article
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 212594b503bfc17d3faa6df03e01402d2862522f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390193"
---
<a name="an-overview-of-project-katana"></a>Información general del proyecto Katana
====================
por [Howard Dierking](https://github.com/howarddierking)

> El marco de ASP.NET ha existido durante más de diez años, y la plataforma ha permitido el desarrollo de innumerables sitios Web y servicios. Tal y como han evolucionado las estrategias de desarrollo de aplicaciones Web, el marco de trabajo ha sido capaz de evolucionar en el paso con tecnologías como ASP.NET MVC y ASP.NET Web API. Como el desarrollo de aplicaciones Web tiene el siguiente paso evolutivo en el mundo de la informática en nube, proyecto [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) proporciona el conjunto de componentes para aplicaciones de ASP.NET, lo que les permite ser flexible y portátil, subyacente ligero y proporcionar un mejor rendimiento, dicho de otro modo, el proyecto [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) en la nube optimiza las aplicaciones ASP.NET.


## <a name="why-katana--why-now"></a>¿Por qué Katana: ¿por qué ahora?

 Sin tener en cuenta si uno está debatiendo un producto para desarrolladores de framework o por el usuario final, es importante comprender las motivaciones subyacentes para crear el producto y en este paso incluye saber que se creó el producto para. ASP.NET se creó originalmente con dos clientes en mente.   
  
**El primer grupo de clientes era que los programadores de ASP clásicos.** En el momento, ASP fue una de las principales tecnologías para crear aplicaciones y sitios Web dinámicos, controlada por datos mediante entrelazamiento de marcado y el script del lado servidor. El tiempo de ejecución ASP había proporcionado el script del lado servidor con un conjunto de objetos que abstrae los aspectos básicos de protocolo HTTP subyacente y el servidor Web y proporciona acceso a otros servicios tal administración de Estados de sesión y aplicación, almacenar en caché, etcetera. Aunque eficaz, las aplicaciones ASP clásicas se convirtió en un desafío de administrar a medida que aumentó de tamaño y complejidad. Esto fue principalmente debido a la falta de estructura que se encuentra en entornos junto con la duplicación de código resultante de la intercalación de marcado y código de script. Con el fin de aprovechar los puntos fuertes de ASP clásico además de satisfacer algunos de sus desafíos, ASP.NET aprovechó la organización de código proporcionada por los lenguajes orientados a objetos de .NET Framework mientras conserva también el modelo de programación del lado servidor a qué ASP clásico había crecido a los desarrolladores acostumbrados.

**El segundo grupo de clientes de destino para ASP.NET era que los desarrolladores de aplicaciones de negocio de Windows.** A diferencia de los desarrolladores ASP clásicos, que estaban acostumbrados a escribir el marcado HTML y el código para generar marcado HTML más, los desarrolladores de formularios Windows Forms (por ejemplo, los desarrolladores de VB6 antes que ellos) estaba acostumbrado a una experiencia en tiempo de diseño que incluye un elemento canvas y un amplio conjunto de usuario controles de interfaz. La primera versión de ASP.NET: también conocido como "Web Forms" proporciona una experiencia similar en tiempo de diseño junto con un modelo de evento del lado servidor para los componentes de interfaz de usuario y un conjunto de características de infraestructura (por ejemplo, ViewState) para crear una experiencia de desarrollador coherente entre el cliente y la programación del lado del servidor. Formularios Web Forms hid eficazmente la naturaleza sin estado de la Web en un modelo de evento con estado que estaba familiar para los desarrolladores de formularios Windows Forms.

### <a name="challenges-raised-by-the-historical-model"></a>Estos desafíos mediante el modelo histórico

**El resultado final fue un maduro y con muchas características de tiempo de ejecución y el modelo de programación del desarrollador.** Sin embargo, con la que variedad de características incluye dos retos importantes. En primer lugar, el marco de trabajo fue **monolítica**, con lógicamente distintas unidades de funcionalidad estrechamente acoplados en el mismo ensamblado System.Web.dll (por ejemplo, los objetos de núcleo HTTP con el marco de formularios Web). En segundo lugar, se incluyó como parte de .NET Framework más grande, lo que significaba que ASP.NET la **era de tiempo entre las versiones del orden de los años.** Esto dificultaba ASP.NET para mantenerse al día con todos los cambios que se producen en rápida evolución de desarrollo Web. Por último, se acopla System.Web.dll propio de varias maneras diferentes para un opción de hospedaje específico: Internet Information Services (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Pasos evolutivos: ASP.NET MVC y ASP.NET Web API

Y una gran cantidad de cambios estaba sucediendo en el desarrollo Web. Aplicaciones Web cada vez más que se desarrollaron como una serie de pequeños, centran componentes en lugar de marcos de trabajo grandes. El número de componentes, así como la frecuencia con que se publicaron se aumenta a un ritmo nunca más rápido. Resultaba evidente que marcos de trabajo para obtener, desacoplado y delimitará en lugar de mayor y más completos, por lo tanto, requiere la adaptación al ritmo de la Web la **equipo ASP.NET tardó varios pasos evolutivos para habilitar ASP.NET como una familia de componentes Web conectables, en lugar de un solo marco**.

Uno de los primeros cambios era el aumento de la popularidad del conocido modelo de diseño model-view-controller (MVC) gracias a los marcos de desarrollo Web como Ruby on Rails. Este estilo de creación de aplicaciones Web le asignó el desarrollador mayor control sobre el marcado de su aplicación al tiempo que se mantiene la separación de marcado y la lógica empresarial, que fue uno de los puntos de venta iniciales para ASP.NET. Para satisfacer la demanda de este estilo de desarrollo de aplicaciones Web, Microsoft ha adoptado la oportunidad para colocarse mejor para el futuro por **desarrollando ASP.NET MVC fuera de banda** (y no lo incluido en .NET Framework). ASP.NET MVC se publicó como una descarga independiente. Esto daba al equipo de ingeniería de la flexibilidad para entregar actualizaciones de mucha más frecuencia que había sido posible anteriormente.

Otro cambio importante en el desarrollo de aplicaciones Web fue el cambio de páginas Web dinámicas generados por el servidor a la inicial del marcado estático con secciones dinámicas de la página que se genera a partir de script de cliente comunicarse **con las API Web back-end a través de Las solicitudes AJAX**. Este cambio de arquitectura ayudó a impulsar el auge de las API Web y el desarrollo del marco ASP.NET Web API. Como en el caso de ASP.NET MVC, la versión de ASP.NET Web API proporciona otra oportunidad para desarrollar aún más como un marco más modular ASP.NET. El equipo de ingeniería aprovechó la oportunidad y **compilan ASP.NET Web API de modo que no tuviera ninguna dependencia en cualquiera de los tipos de marco principal se encuentra en System.Web.dll**. Esto habilita dos cosas: en primer lugar, significa que podría evolucionar API Web de ASP.NET de forma totalmente independiente (y puede seguir iterar rápidamente porque se entrega a través de NuGet). En segundo lugar, porque se produjeron sin dependencias externas a System.Web.dll y, por lo tanto, ninguna dependencia de IIS, ASP.NET Web API incluía la capacidad para ejecutarse en un host personalizado (por ejemplo, una aplicación de consola, el servicio de Windows, etcetera.)

### <a name="the-future-a-nimble-framework"></a>El futuro: Un marco de trabajo ágil

Al desacoplar componentes de marco de trabajo entre sí y, a continuación, publicarlas en NuGet, marcos de trabajo podrían ahora **recorrer en iteración más rápida y más independiente**. Además, la eficacia y flexibilidad de capacidad de autohospedaje de la API Web resultó ser muy atractivas para los desarrolladores que deseaban un **host pequeña y ligera** para sus servicios. Resultó tan atractivo, de hecho, que otros marcos también querían esta funcionalidad, y esto aparece un nuevo desafío en que cada marco de trabajo se ejecutó en su propio proceso de host en su propia dirección base y deba administrarse (iniciado, detenido, etc.) por separado. Una aplicación Web moderna suele ser compatible con servicio de archivos estáticos, generación de páginas dinámicas, API Web y más recientemente real-time o notificaciones push. Se espera que cada uno de estos servicios debe ejecutar y administrar de forma independiente era simplemente no realista.

¿Qué se necesitaba era una sola abstracción de hospedaje que permiten a un desarrollador crear una aplicación desde una variedad de diferentes componentes y marcos de trabajo y, a continuación, ejecutar dicha aplicación en un host de apoyo.

## <a name="the-open-web-interface-for-net-owin"></a>La interfaz Web abierta para .NET (OWIN)

 Inspirado en los beneficios que se logra mediante [Rack](http://rack.github.io/) en la Comunidad de Ruby, varios miembros de la Comunidad .NET establecen para crear una abstracción entre servidores Web y componentes de framework. Dos objetivos de diseño para la abstracción de OWIN eran que era sencilla y que tardó el menor número posible de dependencias en otros tipos de marco de trabajo. Estos dos objetivos ayudar a garantizar:

- Nuevos componentes se podrían desarrollados y consumen más fácilmente.
- Las aplicaciones se puedan portar más fácilmente entre los hosts y potencialmente toda las plataformas y sistemas operativos.

La abstracción resultante se compone de dos elementos principales. El primero es el diccionario del entorno. Esta estructura de datos es responsable de almacenar todo el estado necesario para procesar una solicitud HTTP y respuesta, así como cualquier estado del servidor pertinente. El diccionario del entorno se define como sigue:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Un servidor Web compatible con OWIN es responsable de rellenar el diccionario del entorno con datos, como las colecciones de encabezado para una solicitud y respuesta HTTP y secuencias de cuerpo. A continuación, es responsabilidad de los componentes de aplicación o marco para rellenar o actualizar el diccionario con los valores adicionales y escribir en la secuencia del cuerpo de respuesta.

Además de especificar el tipo para el diccionario del entorno, la especificación de OWIN define una lista de pares de clave-valor de diccionario principal. Por ejemplo, la siguiente tabla muestra las claves del diccionario requerido para una solicitud HTTP:

| Nombre de clave | Descripción del valor |
| --- | --- |
| `"owin.RequestBody"` | Un Stream con el cuerpo de solicitud, si existe. PUEDE utilizarse Stream.Null como marcador de posición si no hay ningún cuerpo de solicitud. Consulte [cuerpo de la solicitud](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | Un `IDictionary<string, string[]>` de encabezados de solicitud. Consulte [encabezados](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | Un `string` que contiene el método de solicitud HTTP de la solicitud (por ejemplo, `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | Un `string` que contiene la ruta de acceso de la solicitud. La ruta de acceso debe ser relativa a la "raíz" del delegado de la aplicación; consulte [las rutas de acceso](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | Un `string` que contiene la parte de la ruta de acceso de solicitud correspondiente a la "raíz" del delegado de la aplicación; vea [las rutas de acceso](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | Un `string` que contiene el nombre del protocolo y la versión (por ejemplo, `"HTTP/1.0"` o `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | Un `string` que contiene el componente de cadena de consulta de la solicitud HTTP URI, sin el símbolo inicial "?" (p. ej., `"foo=bar&baz=quux"`). El valor puede ser una cadena vacía. |
| `"owin.RequestScheme"` | Un `string` que contiene el esquema URI utilizado para la solicitud (por ejemplo, `"http"`, `"https"`); vea [esquema URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

El segundo elemento clave de OWIN es el delegado de la aplicación. Se trata de una firma de función que actúa como la interfaz principal entre todos los componentes de una aplicación OWIN. La definición de delegado de la aplicación es como sigue:

`Func<IDictionary<string, object>, Task>;`

Delegado de la aplicación, a continuación, es simplemente una implementación del tipo de delegado Func donde la función acepta el diccionario del entorno como entrada y devuelve una tarea. Este diseño tiene varias implicaciones para los desarrolladores:

- Hay un número muy pequeño de las dependencias de tipo necesarias para poder escribir componentes de OWIN. Esto aumenta en gran medida la accesibilidad de OWIN para los desarrolladores.
- El diseño asincrónico permite la abstracción ser eficiente con su propio control de recursos informáticos, especialmente en operaciones intensivas de E/S más.
- Dado que el delegado de la aplicación es una unidad atómica de ejecución y debido a que el diccionario del entorno se realizan como un parámetro en el delegado, se pueden encadenar fácilmente los componentes de OWIN para crear las canalizaciones de procesamiento de HTTP complejo.

Desde una perspectiva de implementación, OWIN es una especificación ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Su objetivo no es tener el siguiente marco Web, pero en su lugar una especificación de cómo interactúan los marcos Web y servidores Web.

Si ha investigado [OWIN](http://owin.org/) o [Katana](https://github.com/aspnet/AspNetKatana/wiki), es posible que también haya observado el [paquete Owin NuGet](http://nuget.org/packages/Owin) y Owin.dll. Esta biblioteca contiene una sola interfaz, [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), que formaliza y se codifica la secuencia de inicio se describe en [sección 4](http://owin.org/html/owin.html#4-application-startup) de la especificación de OWIN. Aunque no es necesario para compilar los servidores OWIN, la [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) interfaz proporciona un punto de referencia concreta y se utiliza por los componentes del proyecto Katana.

## <a name="project-katana"></a>Proyecto Katana

Mientras que tanto el [OWIN](http://owin.org/html/owin.html) especificación y *Owin.dll* son propiedad de comunidad y comunidad de ejecutar los esfuerzos de código abierto, el [Katana](https://github.com/aspnet/AspNetKatana/wiki) proyecto representa el conjunto de OWIN componentes que, mientras sigue abierto, creados y publicados por Microsoft. Estos componentes incluyen componentes de infraestructura, como los hosts y servidores, así como componentes funcionales, como los componentes de autenticación y los enlaces a marcos como [SignalR](../../../signalr/index.md) y [Web de ASP.NET API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). El proyecto tiene los siguientes tres objetivos de alto niveles: 

- **Portable** – componentes deben poder que fácilmente se sustituirá para los componentes nuevos cuando estén disponibles. Esto incluye todos los tipos de componentes, desde el marco de trabajo para el servidor y el host. La implicación de este objetivo es que marcos de terceros pueden ejecutar sin problemas en servidores de Microsoft, mientras que los marcos de Microsoft podría ejecutarse en hosts y servidores de terceros.
- **Flexible y modular**: a diferencia de muchos marcos que incluyen una gran variedad de características que están activadas de forma predeterminada, los componentes del proyecto Katana deben ser pequeño y centrado, lo que proporciona control para determinar qué componentes para el desarrollador de aplicaciones Use en su aplicación.
- **Ligero y de alto rendimiento y escalables** : dividiendo el concepto tradicional de un marco de trabajo en un conjunto de pequeños, centra los componentes que se agregan explícitamente por el desarrollador de aplicaciones, una aplicación de Katana resultante puede consumir menos informática los recursos y como resultado, controlar más carga que con otros tipos de servidores y marcos de trabajo. Como los requisitos de la aplicación exigen más características de la infraestructura subyacente, los que se pueden agregar a la canalización de OWIN, pero que debe ser una decisión explícita por parte del desarrollador de la aplicación. Además, la sustitución de componentes de nivel inferior significa que cuando estén disponibles, nuevos servidores de alto rendimiento sin problemas se pueden introducir para mejorar el rendimiento de aplicaciones OWIN sin interrumpir las aplicaciones.

## <a name="getting-started-with-katana-components"></a>Introducción a los componentes de Katana

Cuando apareció por primera vez, uno de los aspectos de la [Node.js](http://nodejs.org/) framework que dibujó inmediatamente atención de la gente estaba la simplicidad con que uno podría crear y ejecutar un servidor Web. Si los objetivos de Katana se enmarcada en luz de [Node.js](http://nodejs.org/), uno podría resumirlos diciendo que Katana ofrece muchas de las ventajas de [Node.js](http://nodejs.org/) (y marcos que le guste) sin obligar al desarrollador desechar todo lo que sabe acerca de cómo desarrollar aplicaciones Web ASP.NET. Para que esta instrucción es cierta, comenzar con el proyecto Katana debe ser igual de sencillo en la naturaleza [Node.js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Creación de "Hello World!"

Una diferencia importante entre el desarrollo de JavaScript y .NET es la presencia (o ausencia) de un compilador. Por lo tanto, el punto de partida para un servidor de Katana simple es un proyecto de Visual Studio. Sin embargo, podemos empezar con la mínima de tipos de proyecto: la aplicación Web ASP.NET vacía.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

A continuación, se instalará el [Microsoft.Owin.Host.SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) paquete NuGet al proyecto. Este paquete proporciona un servidor OWIN que se ejecuta en la canalización de solicitudes ASP.NET. Se puede encontrar en el [Galería de NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) y puede instalarse mediante el cuadro de diálogo del Administrador de paquetes de Visual Studio o la consola del Administrador de paquetes con el siguiente comando:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Instalar el `Microsoft.Owin.Host.SystemWeb` paquete instalará algunos paquetes adicionales como dependencias. Una de esas dependencias es `Microsoft.Owin`, una biblioteca que proporciona varios tipos y métodos auxiliares para el desarrollo de aplicaciones OWIN. Podemos usar estos tipos para escribir rápidamente el siguiente servidor "¡hello world".

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Ahora se puede ejecutar este servidor Web muy sencilla con Visual Studio **F5** comando e incluye compatibilidad completa con la depuración.

## <a name="switching-hosts"></a>Hosts de conmutación

De forma predeterminada, se ejecuta el ejemplo anterior de "hello world" en la canalización de solicitudes ASP.NET, que usa System.Web en el contexto de IIS. Esto puede por sí solo agregar un enorme valor tal y como nos permite beneficiarse de la flexibilidad y capacidad de composición de una canalización OWIN con las capacidades de administración y la madurez general de IIS. Sin embargo, puede haber casos en que no son necesarias las ventajas proporcionadas por IIS y el deseo es para un host pequeño y más ligero. ¿Qué es necesario, a continuación, para ejecutar nuestro servidor Web simple fuera de IIS y System.Web?

Para ilustrar el objetivo de portabilidad, mover desde un host de servidor Web a un host de la línea de comandos requiere simplemente agregar las dependencias de servidor y host nuevo a la carpeta de salida del proyecto y, a continuación, iniciar el host. En este ejemplo, se hospedarán nuestro servidor Web en un host de Katana llamado `OwinHost.exe` y usará el servidor basado en Katana HttpListener. De forma similar a otros componentes de Katana, estos se adquirirá de NuGet con el comando siguiente:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Desde la línea de comandos, podemos, a continuación, navegue hasta la carpeta raíz del proyecto y ejecutar simplemente el `OwinHost.exe` (que se instaló en la carpeta de herramientas de su paquete NuGet correspondiente). De forma predeterminada, `OwinHost.exe` está configurado para buscar el servidor basado en HttpListener y por lo que no es necesaria ninguna configuración adicional. Navegar en un explorador Web para `http://localhost:5000/` muestra la aplicación ahora se ejecuta a través de la consola.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Arquitectura de Katana

 La arquitectura de componentes de Katana divide una aplicación en cuatro capas lógicas, como se muestra a continuación: *host, el servidor de middleware,* y *aplicación*. La arquitectura de componentes está factorizada de tal manera que las implementaciones de estas capas se pueden sustituir fácilmente, en muchos casos, sin necesidad de volver a compilar la aplicación.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>administrador de flujos de trabajo

 El host es responsable de:

- Administrar el proceso subyacente.
- Coordinación de flujo de trabajo que da como resultado la selección de un servidor y la construcción de una canalización de OWIN a través de las solicitudes que se controlarán.

  En la actualidad, hay 3 opciones de hospedaje principales para las aplicaciones basadas en Katana:  
  
**IIS/ASP.NET**: mediante los tipos de HttpModule y HttpHandler estándar, pueden ejecutar canalizaciones OWIN en IIS como parte de un flujo de solicitud ASP.NET. Compatibilidad con el hospedaje de ASP.NET se habilita al instalar el paquete Microsoft.AspNet.Host.SystemWeb NuGet en un proyecto de aplicación Web. Además, dado que IIS actúa como un host y un servidor, la distinción de servidor/host OWIN está vinculada en este paquete de NuGet, lo que significa que si usa el host SystemWeb, un desarrollador no puede sustituir una implementación de servidor alternativo.  
  
**Host personalizado**: conjunto de componentes de Katana el ofrece la capacidad para hospedar aplicaciones en su propio proceso personalizado, ya sea una aplicación de consola, el servicio de Windows, etcetera. Esta funcionalidad es similar a la capacidad de autohospedaje proporcionada por la API Web. El ejemplo siguiente muestra un host personalizado de código de API Web:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

El programa de instalación de host automático para una aplicación de Katana es similar:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Una diferencia importante entre los ejemplos de autohospedaje de Web API y Katana es que el código de configuración de Web API falta en el ejemplo autohospedaje de Katana. Con el fin de habilitar la portabilidad y capacidad de composición, Katana separa el código que inicia el servidor desde el código que configura la canalización de procesamiento de solicitudes. El código que configura la API Web, a continuación, se encuentra en la clase Startup, que además se especifica como el parámetro de tipo en WebApplication.Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

La clase de inicio se tratarán en mayor detalle más adelante en este artículo. Sin embargo, el código necesario para iniciar un proceso autohospedaje es sorprendentemente similar al código que puede que esté usando hoy mismo en las aplicaciones de autohospedaje de ASP.NET Web API de Katana.

**OwinHost.exe**: mientras que algunos deseará escribir un proceso personalizado para ejecutar aplicaciones Katana Web, muchos sería preferible simplemente iniciar un ejecutable pregenerado que puede iniciar un servidor y ejecutar su aplicación. En este escenario, se incluye el conjunto de componentes de Katana `OwinHost.exe`. Cuando se ejecuta desde dentro del directorio raíz de un proyecto, este archivo ejecutable iniciará un servidor (usa el servidor de HttpListener de forma predeterminada) y usar las convenciones para la búsqueda y ejecución de la clase de inicio del usuario. Para un control más granular, el archivo ejecutable proporciona un número de parámetros de línea de comandos adicionales.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Servidor

 Mientras el host es responsable de iniciar y mantener en el que se ejecuta la aplicación, la responsabilidad del servidor de proceso abrir un socket de red, escuchar las solicitudes y enviarlos a través de la canalización de componentes OWIN especificados por el usuario (como se es posible que haya notado, esta canalización se especifica en la clase de inicio del desarrollador de la aplicación). Actualmente, el proyecto Katana incluye dos implementaciones de servidor: 

- **Microsoft.Owin.Host.SystemWeb**: como se mencionó anteriormente, IIS junto con la canalización de ASP.NET que actúa como un host y un servidor. Por lo tanto, al elegir la opción de hospedaje, IIS administra las preocupaciones de nivel de host como activación de procesos tanto escucha las solicitudes HTTP. Para aplicaciones Web de ASP.NET, a continuación, envía las solicitudes a la canalización ASP.NET. El host de Katana SystemWeb registra un ASP.NET HttpModule y HttpHandler para interceptar las solicitudes a medida que fluyen a través de la canalización HTTP y envían a través de la canalización OWIN especificada por el usuario.
- **Microsoft.Owin.Host.HttpListener**: como su nombre indica, este servidor de Katana usa la clase de .NET Framework HttpListener para abrir un socket y enviar solicitudes a una canalización OWIN especificado por el desarrollador. Esto es actualmente la selección de servidor predeterminado para la API de autohospedaje de Katana y OwinHost.exe.

## <a name="middlewareframework"></a>Middleware o marco.

 Como se mencionó anteriormente, cuando el servidor acepta una solicitud desde un cliente, es responsable de pasarlo a través de una canalización de los componentes OWIN, que se especifican mediante el código de inicio del desarrollador. Estos componentes de canalización se conocen como middleware.  
 En un nivel muy básico, un componente de middleware OWIN simplemente tiene que implementar al delegado de la aplicación OWIN para que sea que se puede llamar.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Sin embargo, con el fin de simplificar el desarrollo y la composición de los componentes de middleware, Katana es compatible con un puñado de convenciones y tipos de aplicación auxiliar para componentes de middleware. La más común de estos es el `OwinMiddleware` clase. Un componente de middleware personalizado creado con esta clase tendría un aspecto similar al siguiente: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Esta clase se deriva de `OwinMiddleware`, implementa un constructor que acepta una instancia de la siguiente middleware en la canalización como uno de sus argumentos y, a continuación, se pasa al constructor base. Argumentos adicionales que se usan para configurar el middleware también se declaran como parámetros del constructor después del parámetro de middleware siguiente.   
  
En tiempo de ejecución, el middleware se ejecuta a través de invalidado `Invoke` método. Este método toma un único argumento de tipo `OwinContext`. Este objeto de contexto proporciona la `Microsoft.Owin` paquete de NuGet se ha descrito anteriormente y proporciona acceso fuertemente tipado para el diccionario de solicitud, respuesta y entorno, junto con algunos tipos auxiliares adicionales.   
  
La clase de middleware puede agregarse fácilmente a la canalización OWIN en el código de inicio de la aplicación como sigue:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Dado que la infraestructura de Katana simplemente crea una canalización de componentes de middleware OWIN y porque los componentes simplemente es necesario admitir el delegado de la aplicación para participar en la canalización, componentes de middleware pueden abarcar desde simple los registradores todos marcos como ASP.NET, Web API, o [SignalR](../../../signalr/index.md). Por ejemplo, la adición de ASP.NET Web API a la canalización OWIN anterior requiere agregando el siguiente código de inicio:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

La infraestructura de Katana compilará la canalización de componentes de middleware basado en el orden en que se agregaron en el objeto IAppBuilder en el método de configuración. A continuación, en nuestro ejemplo, LoggerMiddleware puede controlar todas las solicitudes que fluyen a través de la canalización, independientemente de cómo finalmente se controlan esas solicitudes. Esto habilita escenarios eficaces donde un componente de middleware (por ejemplo, un componente de autenticación) pueda procesar las solicitudes de una canalización que incluye varios componentes y marcos (por ejemplo, ASP.NET Web API, SignalR y un servidor de archivos estáticos).
 
## <a name="applications"></a>Aplicaciones

Como se muestra en los ejemplos anteriores, OWIN y el proyecto Katana no se deben considerar como un nuevo modelo de programación de aplicaciones, sino como una abstracción para desacoplar los modelos de programación de aplicaciones y marcos de trabajo de servidor y la infraestructura de hospedaje. Por ejemplo, al compilar aplicaciones de API Web, el marco de trabajo de desarrollador seguirá usar el marco ASP.NET Web API, con independencia de si la aplicación se ejecuta en una canalización de OWIN mediante componentes desde el proyecto Katana. El lugar donde estará visible para el desarrollador de aplicaciones código relacionado con el OWIN será el código de inicio de la aplicación, donde el desarrollador crea la canalización de OWIN. En el código de inicio, el desarrollador registrará una serie de instrucciones UseXx, por lo general, una para cada componente de middleware que va a procesar las solicitudes entrantes. Esta experiencia tendrá el mismo efecto que el registro de módulos HTTP en el mundo actual de System.Web. Normalmente, un middleware framework más grande, como ASP.NET Web API o [SignalR](../../../signalr/index.md) se registrará al final de la canalización. Componentes de middleware transversales, como los de autenticación o almacenamiento en caché, por lo general se registran hacia el principio de la canalización para que procesará las solicitudes de todos los marcos de trabajo y componentes registrados más adelante en la canalización. Esta separación de los componentes de middleware entre sí y desde los componentes de infraestructura subyacentes habilita los componentes evolucionando a diferentes velocidades asegurándose de que el sistema general permanece estable.

## <a name="components--nuget-packages"></a>Componentes, paquetes de NuGet

Como muchas bibliotecas actuales y marcos de trabajo, los componentes del proyecto Katana se entregan como un conjunto de paquetes de NuGet. La próxima versión 2.0, el gráfico de dependencias de paquete de Katana tiene el aspecto siguiente. (Haga clic en la imagen para aumentarla).

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Casi todos los paquetes en el proyecto Katana depende, directa o indirectamente, el paquete Owin. Es posible que recuerde que este es el paquete que contiene la interfaz de IAppBuilder, que proporciona una implementación concreta de la secuencia de inicio de la aplicación que se describe en la sección 4 de la especificación de OWIN. Además, muchos de los paquetes dependen Microsoft.Owin, que proporciona un conjunto de tipos de aplicación auxiliar para trabajar con solicitudes y respuestas HTTP. El resto del paquete se puede clasificar como a los paquetes de infraestructura (servidores o hosts) o middleware de hospedaje. Paquetes y dependencias que son externas al proyecto Katana se muestran en color naranja.

La infraestructura de hospedaje de Katana 2.0 incluye tanto el SystemWeb y servidores basados en HttpListener, el paquete OwinHost para ejecutar aplicaciones OWIN mediante OwinHost.exe y el paquete Microsoft.Owin.Hosting para el autohospedaje aplicaciones OWIN en un host personalizado (por ejemplo, aplicación de consola, el servicio de Windows, etcetera.)

Katana 2.0, los componentes de middleware se centran principalmente en proporcionar diversas formas de autenticación. Se proporciona un componente de middleware adicional para el diagnóstico, que habilita la compatibilidad con una página de inicio y de error. A medida que crece OWIN en la abstracción de hospedaje de hecho, el ecosistema de componentes de middleware, tanto los desarrollados por Microsoft y terceros, también crecerá en número.

## <a name="conclusion"></a>Conclusión

 Desde su comienzo, el objetivo del proyecto Katana no ha sido crear y, por tanto, obliga a los desarrolladores para obtener información sobre otro marco Web. En su lugar, el objetivo ha sido crear una abstracción para dar a los desarrolladores de aplicaciones Web de .NET más alternativas que previamente ha sido posible. Al dividir las capas lógicas de una pila típica de aplicación Web en un conjunto de componentes reemplazables, el proyecto Katana permite a los componentes en toda la pila para mejorar la tarifa que tiene sentido para los componentes. Mediante la creación de todos los componentes en torno a la abstracción simple de OWIN, Katana permite que marcos de trabajo y las aplicaciones que se basa en ellos sea portable entre una variedad de distintos servidores y hosts. Colocando el desarrollador en el control de la pila de Katana garantiza que el programador realiza la elección más avanzada acerca de cómo ligera o cómo completo debe ser su pila Web.  
  

## <a name="for-more-information-about-katana"></a>Para obtener más información sobre Katana

- El proyecto Katana en GitHub: [ https://github.com/aspnet/AspNetKatana/ ](https://github.com/aspnet/AspNetKatana/).
- Vídeo: [al proyecto Katana - OWIN para ASP.NET](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), por Howard Dierking.

## <a name="acknowledgements"></a>Reconocimientos

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick es programadora senior de Microsoft, centrándose en Azure y MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
