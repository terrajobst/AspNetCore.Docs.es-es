---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: "Información general del proyecto Katana | Documentos de Microsoft"
author: howarddierking
description: "El marco de trabajo de ASP.NET ha estado presente durante más de diez años, y la plataforma ha habilitado el desarrollo de innumerables sitios Web y servicios. Como aplicación Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/30/2013
ms.topic: article
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 8f28116f88f3cf5143d3d5c9821519d62c4e5452
ms.sourcegitcommit: 6541c8b11001dd617adf5eb04c814cda165070b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2017
---
<a name="an-overview-of-project-katana"></a>Información general del proyecto Katana
====================
por [Howard Dierking](https://github.com/howarddierking)

> El marco de trabajo de ASP.NET ha estado presente durante más de diez años, y la plataforma ha habilitado el desarrollo de innumerables sitios Web y servicios. Tal y como han evolucionado estrategias de desarrollo de aplicaciones Web, el marco de trabajo ha sido capaz de evolucionar en el paso con tecnologías como ASP.NET MVC y ASP.NET Web API. Como el desarrollo de aplicaciones Web tiene el siguiente paso evolutivo en el mundo de la informática en nube, proyecto [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) proporciona el conjunto de componentes para aplicaciones de ASP.NET, les permite ser flexible, portátiles, lo que subyacente ligero y proporcionar un mejor rendimiento: dicho de otro modo, proyectos [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) en la nube optimiza las aplicaciones ASP.NET.


## <a name="why-katana--why-now"></a>¿Por qué Katana: ¿por qué ahora?

 Sin tener en cuenta si uno está hablando de un producto de framework o por el usuario final de desarrollador, es importante comprender las motivaciones subyacentes para crear el producto y la parte de la que incluye saber que se creó el producto para. ASP.NET se creó originalmente con dos clientes en mente.   
  
**El primer grupo de clientes era los programadores de ASP clásicos.** En el momento, ASP era una de las principales tecnologías para crear aplicaciones y sitios Web dinámicos, orientadas a datos por cruce marcado y el script de servidor. El tiempo de ejecución ASP había proporcionado script de servidor con un conjunto de objetos que se extraen los aspectos básicos del protocolo HTTP subyacente y el servidor Web y proporciona acceso a otros servicios de este tipo administración de Estados de sesión y aplicación, almacenar en memoria caché, etcetera. Aunque eficaz, las aplicaciones ASP clásicas pasó a ser un reto para administrar tal y como ha crecido en tamaño y complejidad. Esto era en gran medida debido a la falta de estructura encontrada en entornos junto con la duplicación de código resultante de la intercalación de marcado y código de scripting. Para aprovechar las ventajas de ASP clásico al resolver algunos de sus desafíos, ASP.NET tardó ventaja de la organización del código proporcionada los lenguajes orientados a objetos de .NET Framework mientras también conserva el modelo de programación de servidor para la aplicaciones ASP clásicas creció acostumbrados a los desarrolladores.

**El segundo grupo de clientes de destino para ASP.NET era que los desarrolladores de aplicaciones de negocio de Windows.** A diferencia de los desarrolladores ASP clásicos, que estaban acostumbrados a escribir código HTML y el código para generar el marcado HTML más, los desarrolladores de formularios Windows Forms (por ejemplo, los desarrolladores VB6 anteriores) estaba acostumbrado a una experiencia en tiempo de diseño que incluyen un lienzo y un amplio conjunto de usuario controles de interfaz. La primera versión de ASP.NET: también conocida como "Formularios Web Forms" proporciona una experiencia similar en tiempo de diseño junto con un modelo de eventos de servidor para los componentes de la interfaz de usuario y un conjunto de características de infraestructura (por ejemplo, ViewState) para crear una experiencia de desarrollador sin problemas entre el cliente y la programación del lado servidor. Formularios Web Forms hid eficazmente la naturaleza sin estado de la Web en un modelo de evento con estado que le resulta familiar a los desarrolladores de formularios Windows Forms.

### <a name="challenges-raised-by-the-historical-model"></a>Desafíos generados por el modelo histórico

**El resultado neto era un consolidada, con numerosas características de tiempo de ejecución y el modelo de programación del desarrollador.** Sin embargo, con la que característica riqueza suministrada dos desafíos importantes. En primer lugar, el marco de trabajo estaba **monolítico**, con unidades lógicamente dispares de funcionalidad que se está estrechamente en el mismo ensamblado System.Web.dll (por ejemplo, los objetos de core HTTP con el marco de trabajo de formularios Web). En segundo lugar, se incluyó como parte de .NET Framework más grande, lo que significa que ASP.NET la **tiempo entre versiones estaba en el orden de los años.** Esto dificultaba ASP.NET para mantenerse al día con todos los cambios que ocurren en evolución desarrollo Web. Por último, se acopla System.Web.dll propio de varias maneras diferentes de un opción de hospedaje específico: Internet Information Services (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Pasos evolutivos: ASP.NET MVC y ASP.NET Web API

Y una gran cantidad de cambios que sucedía en el desarrollo Web. Aplicaciones Web cada vez más que se desarrollaron como una serie de pequeños, centrado componentes en lugar de marcos de trabajo de gran tamaño. Se aumenta el número de componentes, así como la frecuencia con la que se publicaron nunca más rápido. Resultaba evidente que marcos de trabajo para obtener más pequeños, desacoplado y delimitará en lugar de mayor y más con numerosas características, por lo tanto, requiere la adaptación al ritmo de la Web el **ASP.NET seguidos por varios pasos evolutivos habilitar ASP.NET como una familia de componentes Web conectables en lugar de un marco de trabajo único**.

Uno de los cambios tempranos era el aumento de la popularidad del conocido modelo de diseño model-view-controller (MVC) gracias a los marcos de desarrollo Web como Ruby sobre raíles. Este estilo de la creación de aplicaciones Web que proporcionó el desarrollador mayor control sobre el marcado de su aplicación al tiempo que se mantiene la separación de marcado y la lógica empresarial, que era uno de los puntos de venta iniciales para ASP.NET. Para satisfacer la demanda de este estilo de desarrollo de aplicaciones Web, Microsoft tuvo la oportunidad para colocarse mejor para el futuro por **desarrollo de ASP.NET MVC fuera de banda** (y no se incluye en .NET Framework). ASP.NET MVC se publicó como una descarga independiente. Esto le da al equipo de ingeniería la flexibilidad para entregar actualizaciones de mucha más frecuencia que había sido posible anteriormente.

Otro cambio importante en el desarrollo de aplicaciones Web produjo un cambio de páginas Web dinámicas generada por el servidor a estático marcado inicial con secciones de dinámicas de la página que se genera a partir de script de cliente comunicarse **con las API Web back-end a través de Las solicitudes AJAX**. Este cambio de arquitectura ayudó a impulsar el aumento de la API Web y el desarrollo del marco de trabajo de ASP.NET Web API. Como en el caso de ASP.NET MVC, la versión de ASP.NET Web API proporciona otra oportunidad para desarrollar aún más un marco de trabajo más modular ASP.NET. El equipo de ingeniería tardó aprovechar la oportunidad y **generan ASP.NET Web API de modo que no tenía ninguna dependencia en cualquiera de los tipos de framework core encontrados en System.Web.dll**. Esta opción habilita dos cosas: en primer lugar, significa que ASP.NET Web API podría evolucionar de forma totalmente independiente (y puede seguir recorrer en iteración rápidamente dado que se envía a través de NuGet). En segundo lugar, porque no había ningún dependencias externas a System.Web.dll y, por lo tanto, ninguna dependencia en IIS, ASP.NET Web API incluye la capacidad para ejecutarse en un host personalizado (por ejemplo, una aplicación de consola, servicio de Windows, etcetera.)

### <a name="the-future-a-nimble-framework"></a>El futuro: Un marco de trabajo ágil

Desacoplar componentes de framework entre sí y, a continuación, liberándolos en NuGet, marcos de trabajo podrían ahora **recorrer en iteración de forma más rápida y más independientemente**. Además, la eficacia y flexibilidad de capacidad de la API Web autohospedaje resultó muy atractivos para los desarrolladores que desearan un **host pequeña y ligera** para sus servicios. Resultó tan atractivo, de hecho, que otros marcos también querían esta capacidad, y esto aparece un nuevo desafío en que cada marco de trabajo se ejecutó en su propio proceso de host en su propia dirección base y deba administrarse (iniciado, detenido, etc.) por separado. Una aplicación Web moderna generalmente es compatible con servicios de archivos estáticos, generación de páginas dinámicas, API Web y más recientemente real-tiempo/las notificaciones de inserción. Se espera que cada uno de estos servicios debe ejecutar y administrar con independencia simplemente no es realista.

¿Qué se necesitaba era una abstracción de hospedaje única que se permiten al desarrollador crear una aplicación desde una variedad de distintos componentes y marcos de trabajo y, a continuación, ejecutar dicha aplicación en un host compatible.

## <a name="the-open-web-interface-for-net-owin"></a>La interfaz Web abierta para .NET (OWIN)

 Inspirados en las ventajas que se logra mediante [bastidor](http://rack.github.io/) en la Comunidad Ruby, varios miembros de la Comunidad de .NET establecen para crear una abstracción entre servidores Web y los componentes de framework. Dos objetivos de diseño para la abstracción de OWIN eran que ha sido sencillo y que tardó el menor número de posibles dependencias en otros tipos de framework. Estos dos objetivos garantizar:

- Nuevos componentes pudieron desarrollados o consume más fácilmente.
- Podrían migrar más fácilmente las aplicaciones entre los hosts y potencialmente todo plataformas y sistemas operativos.

La abstracción resultante se compone de dos elementos principales. El primero es el diccionario de entorno. Esta estructura de datos es responsable de almacenar todo el estado necesario para procesar una solicitud HTTP y respuesta, así como cualquier estado de servidor que corresponda. El diccionario de entorno se define como sigue:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Un servidor Web compatible con OWIN es responsable de rellenar el diccionario de entorno con datos como las secuencias de cuerpo y colecciones de encabezado de una solicitud HTTP y la respuesta. A continuación, es responsabilidad de los componentes de aplicación o marco para rellenar o actualizar el diccionario con valores adicionales y escribir en la secuencia de cuerpo de respuesta.

Además de especificar el tipo para el diccionario de entorno, la especificación de OWIN define una lista de pares de clave-valor de diccionario principal. Por ejemplo, en la tabla siguiente muestra las claves del diccionario requerido para una solicitud HTTP:

| Nombre de clave | Descripción del valor |
| --- | --- |
| `"owin.RequestBody"` | Una secuencia con el cuerpo de la solicitud, si lo hay. Stream.Null puede utilizarse como un marcador de posición si no hay ningún cuerpo de solicitud. Vea [cuerpo de la solicitud](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | Un `IDictionary<string, string[]>` de encabezados de solicitud. Vea [encabezados](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | A `string` que contiene el método de solicitud HTTP de la solicitud (p. ej., `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | Un `string` que contiene la ruta de acceso de la solicitud. La ruta de acceso debe ser relativa a la "raíz" del delegado de aplicación; vea [las rutas de acceso](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | A `string` que contiene la parte de la ruta de acceso de solicitud correspondiente a la "raíz" del delegado de aplicación; vea [las rutas de acceso](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | A `string` que contiene el nombre de protocolo y versión (por ejemplo, `"HTTP/1.0"` o `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | Un `string` que contiene el componente de cadena de consulta de la solicitud HTTP URI, sin el interlineado "?" (p. ej., `"foo=bar&baz=quux"`). El valor puede ser una cadena vacía. |
| `"owin.RequestScheme"` | A `string` que contiene el esquema URI utilizado para la solicitud (p. ej., `"http"`, `"https"`); vea [esquema URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

El segundo elemento clave de OWIN es el delegado de la aplicación. Se trata de una firma de función que actúa como la interfaz principal entre todos los componentes de una aplicación OWIN. La definición para el delegado de aplicación es como sigue:

`Func<IDictionary<string, object>, Task>;`

El delegado de la aplicación, a continuación, es simplemente una implementación del tipo de delegado de Func donde la función acepta el diccionario de entorno como entrada y devuelve una tarea. Este diseño tiene varias implicaciones para los desarrolladores:

- Hay un número muy pequeño de las dependencias del tipo necesario para escribir componentes de OWIN. Esto aumenta enormemente la accesibilidad de OWIN para los desarrolladores.
- El diseño asincrónico permite la abstracción sean eficaces con su propio control de los recursos, especialmente en operaciones intensivas de E/S más informáticos.
- Dado que el delegado de la aplicación es una unidad atómica de ejecución y debido a que el diccionario de entorno se realizan como un parámetro en el delegado, se pueden encadenar fácilmente los componentes OWIN para crear complejo HTTP las canalizaciones de procesamiento.

Desde una perspectiva de implementación, OWIN es una especificación ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Su objetivo no es tener el siguiente marco de trabajo Web, pero en su lugar una especificación de cómo interactúan los marcos de trabajo Web y servidores Web.

Si ha investigado [OWIN](http://owin.org/) o [Katana](https://github.com/aspnet/AspNetKatana/wiki), es podrán que también haya observado el [paquete Owin NuGet](http://nuget.org/packages/Owin) y Owin.dll. Esta biblioteca contiene una única interfaz [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), que formaliza y codifica la secuencia de inicio se describe en [sección 4](http://owin.org/html/owin.html#4-application-startup) de la especificación de OWIN. Si bien no es necesario para crear servidores OWIN, la [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) interfaz proporciona un punto de referencia concreta y se utiliza por los componentes del proyecto de Katana.

## <a name="project-katana"></a>Proyecto Katana

Mientras que tanto el [OWIN](http://owin.org/html/owin.html) especificación y *Owin.dll* son propiedad de comunidad y Comunidad ejecutar trabajos de código abierto, la [Katana](https://github.com/aspnet/AspNetKatana/wiki) proyecto representa el conjunto de OWIN componentes que, mientras sigue abierto, se genera y publicados por Microsoft. Estos componentes incluyen componentes de infraestructura, como hosts y servidores, así como componentes funcionales, como los componentes de autenticación y enlaces a los marcos como [SignalR](../../../signalr/index.md) y [Web de ASP.NET API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). El proyecto tiene los siguientes tres objetivos de alto niveles: 

- **Portable** : componentes deben poder sustituirá fácilmente por componentes nuevos cuando estén disponibles. Esto incluye todos los tipos de componentes, desde el marco de trabajo para el servidor y el host. La implicación de este objetivo es que marcos de trabajo de otros fabricantes pueden ejecutar sin problemas en los servidores de Microsoft mientras marcos de trabajo de Microsoft podrían ejecutarse en hosts y servidores de otros fabricantes.
- **Modulares y flexibles**: a diferencia de muchos marcos de trabajo que incluye una gran cantidad de características que se activan de forma predeterminada, deben ser componentes del proyecto de Katana pequeñas y tiene el foco, lo que proporciona control al desarrollador de la aplicación para determinar qué componentes Utilice en su aplicación.
- **Ligero/rendimiento/escalable** : dividiendo la noción tradicional de un marco de trabajo en un conjunto de pequeños, los componentes que se agregan explícitamente por el desarrollador de aplicaciones, una aplicación de Katana resultante puede consumir menos informática centrados en recursos y como resultado, controlar más la carga, que con otros tipos de servidores y marcos de trabajo. Como los requisitos de la aplicación exigen más características de la infraestructura subyacente, los que pueden agregarse a la canalización OWIN, pero que debe ser una decisión explícita por parte del desarrollador de la aplicación. Además, la sustitución de componentes de nivel inferior significa que cuando estén disponibles, nuevos servidores de alto rendimiento pueden perfectamente introducirán para mejorar el rendimiento de aplicaciones OWIN sin interrumpir las aplicaciones.

## <a name="getting-started-with-katana-components"></a>Introducción a los componentes de Katana

Cuando apareció por primera vez, un aspecto de la [Node.js](http://nodejs.org/) framework que dibujó inmediatamente atención de la gente estaba la simplicidad con que uno puede crear y ejecutar un servidor Web. Si los objetivos de Katana se encuadran en habida cuenta de [Node.js](http://nodejs.org/), uno podría resumir diciendo que Katana ofrece muchas de las ventajas de [Node.js](http://nodejs.org/) (y marcos de trabajo similares) sin forzar al desarrollador tirar todo lo que se conoce sobre el desarrollo de aplicaciones Web ASP.NET. Para que esta instrucción es cierta, cómo empezar a usar el proyecto de Katana debe ser igual de sencillo de naturaleza a [Node.js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Creación de "¡Hello World!"

Una diferencia importante entre el desarrollo de JavaScript y .NET es la presencia (o ausencia) de un compilador. Por lo tanto, el punto de partida para un servidor de Katana simple es un proyecto de Visual Studio. Sin embargo, podemos empezar con el más mínimo de tipos de proyecto: la aplicación Web ASP.NET vacía.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

A continuación, se instalará el [Microsoft.Owin.Host.SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) paquete de NuGet en el proyecto. Este paquete proporciona un servidor OWIN que se ejecuta en la canalización de solicitudes ASP.NET. Se puede encontrar en el [Galería de NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) y se puede instalar mediante el cuadro de diálogo del Administrador de paquetes de Visual Studio o la consola de administrador de paquetes con el comando siguiente:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Instalar el `Microsoft.Owin.Host.SystemWeb` paquete instalará algunos paquetes adicionales como dependencias. Una de esas dependencias es `Microsoft.Owin`, una biblioteca que proporciona varios tipos y métodos auxiliares para el desarrollo de aplicaciones OWIN. Podemos usar esos tipos para escribir rápidamente el siguiente servidor de "Hola mundo".

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Ahora pueden ejecutarse en este servidor Web muy sencilla con Visual Studio **F5** comando e incluye compatibilidad completa para la depuración.

## <a name="switching-hosts"></a>Hosts de conmutación

De forma predeterminada, el ejemplo anterior de "¡hello world" se ejecuta en la canalización de solicitud ASP.NET, que utiliza System.Web en el contexto de IIS. Esto puede por sí sola agregar valor enorme como que nos permite beneficiarse de la flexibilidad y facilidad de creación de una canalización OWIN con las capacidades de administración y madurez general de IIS. Sin embargo, puede haber casos en que no son necesarias las ventajas proporcionadas por IIS y el deseo es para un host más pequeño y más ligero. ¿Qué es necesario, a continuación, para ejecutar el servidor de Web simple fuera de IIS y System.Web?

Para ilustrar el objetivo de portabilidad, mover desde un host de servidor Web a un host de la línea de comandos requiere simplemente se agregan las nuevas dependencias de servidor y el host a la carpeta de salida del proyecto y, a continuación, iniciar el host. En este ejemplo, se podrá hospedar el servidor Web en un host de Katana denominado `OwinHost.exe` y usará el servidor basado en Katana HttpListener. De forma similar a los demás componentes de Katana, estos se adquirirá de NuGet utilizando el comando siguiente:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Desde la línea de comandos, podemos, a continuación, navegue hasta la carpeta raíz del proyecto y ejecutar simplemente el `OwinHost.exe` (que se instaló en la carpeta de herramientas de su paquete de NuGet correspondiente). De forma predeterminada, `OwinHost.exe` está configurado para buscar el servidor basado en HttpListener, por lo que se necesita ninguna configuración adicional. Navegar en un explorador Web para `http://localhost:5000/` muestra la aplicación que se ejecuta ahora a través de la consola.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Arquitectura de Katana

 La arquitectura de componentes de Katana divide una aplicación en cuatro capas lógicas, como se describe a continuación: *host, el servidor de middleware,* y *aplicación*. La arquitectura de componentes se divide de forma que las implementaciones de estas capas se pueden sustituir fácilmente, en muchos casos, sin necesidad de volver a compilar la aplicación.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Host

 El host es responsable de:

- Administrar el proceso subyacente.
- Se controlarán la coordinación de flujo de trabajo que da como resultado la selección de un servidor y la construcción de una canalización OWIN a través de las solicitudes.

 En la actualidad, hay 3 opciones de hospedaje principales para aplicaciones basadas en Katana:  
  
**IIS/ASP.NET**: con los tipos de HttpModule y HttpHandler estándares, pueden ejecutar canalizaciones OWIN en IIS como parte de un flujo de solicitud ASP.NET. Compatibilidad con el host de ASP.NET está habilitado al instalar el paquete de Microsoft.AspNet.Host.SystemWeb NuGet en un proyecto de aplicación Web. Además, debido a que IIS actúa como un host y un servidor, la distinción de servidor/host OWIN está vinculada en este paquete de NuGet, lo que significa que si utiliza el host SystemWeb, un desarrollador no puede sustituir una implementación de servidor alternativo.  
  
**Host personalizado**: Katana el conjunto de componentes permite un desarrollador para hospedar aplicaciones en su propio proceso personalizado, ya sea una aplicación de consola, servicio de Windows, etcetera. Esta funcionalidad es similar a la capacidad de autohospedaje proporcionada por la API Web. En el ejemplo siguiente se muestra un host personalizado de código de la API de Web:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

El programa de instalación de autohospedaje para una aplicación de Katana es similar:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Una diferencia importante entre los ejemplos de API Web y Katana autohospedaje es que el código de configuración de la API Web está presente en el ejemplo de autohospedaje Katana. Para habilitar la portabilidad y facilidad de creación, Katana separa el código que inicia el servidor desde el código que configura la canalización de procesamiento de solicitudes. El código que configura la API Web, a continuación, se encuentra en la clase de inicio, que además se especifica como el parámetro de tipo en WebApplication.Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

La clase de inicio se tratarán con más detalle más adelante en el artículo. Sin embargo, el código necesario para iniciar un proceso de host propio brusca parecido al código que puede que esté utilizando actualmente en las aplicaciones ASP.NET Web API de autohospedaje de Katana.

**OwinHost.exe**: aunque algunos deseará escribir un proceso personalizado para ejecutar aplicaciones Katana Web, muchas sería preferible simplemente iniciar un archivo ejecutable pregenerado que puede iniciar un servidor y ejecutar su aplicación. En este escenario, se incluye el conjunto de componentes de Katana `OwinHost.exe`. Cuando se ejecuta desde dentro del directorio raíz del proyecto, este archivo ejecutable se inicia un servidor (usa el servidor HttpListener de forma predeterminada) y utilizar las convenciones para buscar y ejecutar la clase de inicio del usuario. Para un control más detallado, el archivo ejecutable proporciona una serie de parámetros de línea de comandos adicionales.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Servidor

 Mientras el host es responsable de iniciar y mantener proceso dentro del cual se ejecuta la aplicación, la responsabilidad del servidor abrir un socket de red, escuchar las solicitudes y enviarlos a través de la canalización de componentes OWIN especificados por el usuario (como se posible que haya notado, esta canalización se especifica en la clase de inicio del desarrollador de la aplicación). Actualmente, el proyecto de Katana incluye dos implementaciones de servidor: 

- **Microsoft.Owin.Host.SystemWeb**: como se mencionó anteriormente, IIS junto con la canalización ASP.NET actúa como un host y un servidor. Por lo tanto, al elegir la opción de hospedaje, IIS administra los problemas de nivel de host, como la activación de procesos tanto escucha las solicitudes HTTP. Para aplicaciones Web de ASP.NET, a continuación, envía las solicitudes a la canalización ASP.NET. El host de Katana SystemWeb registra un HttpModule de ASP.NET y HttpHandler para interceptar las solicitudes a medida que fluyen a través de la canalización HTTP y envían a través de la canalización OWIN especificada por el usuario.
- **Microsoft.Owin.Host.HttpListener**: como su nombre indica, este servidor Katana usa HttpListener (clase) de .NET Framework para abrir un socket y enviar solicitudes a una canalización OWIN especificado por el desarrollador. Actualmente, esta es la selección de servidor predeterminada para la API de autohospedaje Katana y OwinHost.exe.

## <a name="middlewareframework"></a>Middleware/framework

 Como se mencionó anteriormente, cuando el servidor acepta una solicitud de un cliente, es responsable de pasarlo a través de una canalización de los componentes OWIN, que se especifican mediante código de inicio del desarrollador. Estos componentes de canalización se conocen como middleware.  
 En un nivel muy básico, un componente de middleware OWIN solo es necesario implementar el delegado de la aplicación OWIN para que sea accesible.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Sin embargo, para simplificar el desarrollo y la composición de los componentes de middleware, Katana admite una serie de convenciones y tipos de aplicación auxiliar para componentes de middleware. Es la más común de estos la `OwinMiddleware` clase. Un componente de middleware personalizadas creado con esta clase tendría un aspecto similar al siguiente: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Esta clase se deriva de `OwinMiddleware`, implementa un constructor que acepta una instancia de la siguiente middleware en la canalización como uno de sus argumentos y, a continuación, se pasa al constructor base. Argumentos adicionales que se usan para configurar el middleware también se declaran como parámetros del constructor después el siguiente parámetro de middleware.   
  
En tiempo de ejecución, se ejecuta el middleware a través de la invalidado `Invoke` método. Este método toma un único argumento de tipo `OwinContext`. Este objeto de contexto proporciona la `Microsoft.Owin` paquete de NuGet se ha descrito anteriormente y proporciona acceso fuertemente tipado para el diccionario de solicitud y respuesta, entorno, junto con algunos tipos auxiliares adicionales.   
  
La clase de middleware se pueden agregar con facilidad a la canalización OWIN en el código de inicio de la aplicación como se indica a continuación:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Puesto que la infraestructura de Katana simplemente crea una canalización de componentes de middleware OWIN, y los componentes basta admitir el delegado de la aplicación para participar en la canalización, componentes de software intermedio pueden abarcar desde simple registradores que se van a marcos completos como ASP.NET, Web API, o [SignalR](../../../signalr/index.md). Por ejemplo, agregar ASP.NET Web API a la canalización OWIN anterior, es necesario agregar el siguiente código de inicio:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

La infraestructura de Katana compilará la canalización de componentes de middleware según el orden en que se agregaron al objeto IAppBuilder en el método de configuración. A continuación, en nuestro ejemplo, LoggerMiddleware puede controlar todas las solicitudes que fluyen a través de la canalización, independientemente de cómo se administran en última instancia esas solicitudes. Esto habilita escenarios eficaces que un componente de middleware (por ejemplo, un componente de autenticación) puede procesar las solicitudes de una canalización que incluye varios componentes y marcos de trabajo (por ejemplo, ASP.NET Web API SignalR y un servidor de archivos estáticos).
 
## <a name="applications"></a>Aplicaciones

Tal y como se muestra en los ejemplos anteriores, OWIN y el proyecto Katana deben no considerarse como un nuevo modelo de programación de aplicaciones, sino como una abstracción para desacoplar los modelos de programación de aplicaciones y marcos de trabajo de servidor y la infraestructura de hospedaje. Por ejemplo, al compilar aplicaciones de API de Web, el marco de trabajo de desarrollador continuarán utilizando el marco de ASP.NET Web API, con independencia de si la aplicación se ejecuta en una canalización OWIN con componentes del proyecto de Katana. El único lugar donde serán visible para el desarrollador de aplicaciones relacionadas con OWIN código será el código de inicio de la aplicación, donde el desarrollador crea la canalización OWIN. En el código de inicio, el programador se registrará una serie de instrucciones de UseXx, por lo general, uno para cada componente de middleware que va a procesar las solicitudes entrantes. Esta experiencia tendrá el mismo efecto que registrar módulos HTTP en el mundo actual de System.Web. Normalmente, un middleware framework mayor, por ejemplo, ASP.NET Web API o [SignalR](../../../signalr/index.md) se registrará al final de la canalización. Componentes de middleware de corte del cruce, como los relativos a la autenticación o almacenamiento en caché, por lo general se registran hacia el principio de la canalización para que se procesan las solicitudes de todos los marcos de trabajo y los componentes registrados más adelante en la canalización. Esta separación de los componentes de software intermedio entre sí y de los componentes de la infraestructura subyacente permite los componentes que evolucionan a diferentes velocidades asegurándose de que el sistema global siga siendo estable.

## <a name="components--nuget-packages"></a>Componentes, paquetes de NuGet

Al igual que muchas bibliotecas actuales y marcos de trabajo, los componentes del proyecto Katana se entregan como un conjunto de paquetes de NuGet. En la próxima versión 2.0, el gráfico de dependencias de paquete de Katana tiene el siguiente aspecto. (Haga clic en la imagen para aumentarla.)

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Casi todos los paquetes en el proyecto Katana depende, directa o indirectamente, el paquete de Owin. Puede que recuerde que este es el paquete que contiene la interfaz de IAppBuilder, que proporciona una implementación concreta de la secuencia de inicio de la aplicación que se describe en la sección 4 de la especificación de OWIN. Además, muchos de los paquetes dependen de Microsoft.Owin, que proporciona un conjunto de tipos de aplicación auxiliar para trabajar con las solicitudes y respuestas HTTP. El resto del paquete se puede clasificar como hospedaje paquetes de infraestructura (servidores o hosts) o middleware. Paquetes y dependencias que son externas al proyecto Katana se muestran en color naranja.

La infraestructura de hospedaje para Katana 2.0 incluye tanto el SystemWeb y servidores basados en HttpListener, el paquete de OwinHost para ejecutar aplicaciones OWIN con OwinHost.exe y el paquete Microsoft.Owin.Hosting para el autohospedaje aplicaciones OWIN en un host personalizado (por ejemplo, aplicación de consola, servicio de Windows, etcetera.)

Para Katana 2.0, los componentes de middleware principalmente se centran en proporcionar distintos métodos de autenticación. Se proporciona un componente de middleware adicionales para el diagnóstico, que habilita la compatibilidad para una página de inicio y de error. A medida que crece OWIN en la abstracción de hospedaje de hecho, el ecosistema de componentes de middleware, tanto los desarrollado por Microsoft y otros fabricantes, también aumentará en número.

## <a name="conclusion"></a>Conclusión

 Desde su comienzo, el objetivo del proyecto Katana no ha sido crear y, por tanto, obligar a los programadores para obtener información sobre otro marco de trabajo Web. En su lugar, el objetivo ha sido crear una abstracción para proporcionar a los desarrolladores de aplicaciones Web de .NET más alternativas que ahora ha sido posible. Dividiendo las capas lógicas de una pila de aplicación Web típica en un conjunto de componentes reemplazables, el proyecto de Katana permite a los componentes a lo largo de la pila para mejorar en cualquier velocidad relacione con dichos componentes. Mediante la creación de todos los componentes de alrededor de la abstracción de OWIN simple, Katana permite marcos de trabajo y las aplicaciones que se basa en ellos sea portable entre una variedad de distintos servidores y hosts. Al colocar el programador en el control de la pila, Katana garantiza que el programador realiza la elección más avanzada sobre cómo ligera o cómo con numerosas características que debe ser su pila Web.  
  

## <a name="for-more-information-about-katana"></a>Para obtener más información acerca de Katana

- El proyecto Katana en GitHub: [https://github.com/aspnet/AspNetKatana/](https://github.com/aspnet/AspNetKatana/).
- Vídeo: [el proyecto de Katana - OWIN para ASP.NET](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), por Howard Dierking.

## <a name="acknowledgements"></a>Reconocimientos

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick es un escritor de programación senior de Microsoft que se centran en Azure y MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
