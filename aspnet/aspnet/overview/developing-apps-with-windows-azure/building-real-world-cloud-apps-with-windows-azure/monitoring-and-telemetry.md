---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Supervisión y telemetría (crear aplicaciones de nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with e-book de Azure se basa en una presentación desarrollada por Scott Guthrie. Explican el 13 de patrones y prácticas que puede...
ms.author: riande
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: f4dae827627103e5cfb9981b6c3b9342cdc34c13
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578008"
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Supervisión y telemetría (crear aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto corregirlo](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libro electrónico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **Building Real World Cloud Apps with Azure** eBook se basa en una presentación desarrollada por Scott Guthrie. Se explican el 13 patrones y prácticas que pueden ayudarle a tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Muchas personas dependen de los clientes que le avisa cuando su aplicación está inactiva. Que no es realmente una práctica recomendada en cualquier lugar y especialmente no en la nube. No hay ninguna garantía de notificación rápida y, cuando reciba notificaciones, es raro tener datos mínima o puede inducir a error sobre lo que sucedió. Con buena telemetría y sistemas de registro que puede saber de qué está ocurriendo con su aplicación y, cuando algo va mal, saberlo al instante y tener información útil para trabajar con.

## <a name="buy-or-rent-a-telemetry-solution"></a>Comprar o alquilar una solución de telemetría

> [!NOTE]
> En este artículo se escribió antes [Application Insights](/azure/application-insights/app-insights-overview) se publicó. Application Insights es el método preferido para las soluciones de telemetría en Azure. Consulte [configurado Application Insights para su sitio Web ASP.NET](/azure/application-insights/app-insights-asp-net) para obtener más información.


Una de las cosas excelente sobre el entorno de nube es que resulta muy fácil comprar o alquilar su camino hacia la victoria. La telemetría es un ejemplo. Sin mucho esfuerzo puede obtener un sistema de telemetría realmente bueno hasta y funcionamiento, muy rentable. Hay una serie de excelentes socios que se integran con Azure y algunos de ellos tienen niveles gratuitos: por lo que obtendrá los datos de telemetría básicos para nada. Estos son sólo algunas de las que actualmente disponibles en Azure:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

[Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) también incluye las características de supervisión.

Le guiaremos rápidamente a través de configuración de New Relic para mostrar lo fácil se puede usar un sistema de telemetría.

En el portal de administración de Azure, regístrese para el servicio. Haga clic en **New**y, a continuación, haga clic en **Store**. El **elegir un complemento** aparece el cuadro de diálogo. Desplácese hacia abajo y haga clic en **New Relic**.

![Elegir un complemento](monitoring-and-telemetry/_static/image1.png)

Haga clic en la flecha derecha y elija el nivel de servicio que desee. En esta demostración, vamos a usar el nivel gratis.

![Personalizar el complemento](monitoring-and-telemetry/_static/image2.png)

Haga clic en la flecha derecha, confirme la "compra" y New Relic se muestra ahora como un complemento en el portal.

![Revisar compra](monitoring-and-telemetry/_static/image3.png)

![Nuevo complemento Relic en el portal de administración](monitoring-and-telemetry/_static/image4.png)

Haga clic en **información de conexión**y copie la clave de licencia.

![Información de conexión](monitoring-and-telemetry/_static/image5.png)

Vaya a la **configurar** pestaña para establecer la aplicación web en el portal, **supervisión del rendimiento** a **complemento**y establezca el **elegir complemento** la lista desplegable para **New Relic**. A continuación, haga clic en **guardar**.

![New Relic en la pestaña configurar](monitoring-and-telemetry/_static/image6.png)

En Visual Studio, instale el paquete de New Relic NuGet en su aplicación.

![Análisis del desarrollador en la pestaña configurar](monitoring-and-telemetry/_static/image7.png)

Implementar la aplicación en Azure y empezar a usarlo. Crear Fix It algunas tareas para proporcionar alguna actividad New relic supervisar.

A continuación, vaya a la **New Relic** página en el **complementos** ficha del portal y haga clic en **administrar**. El portal le envía al portal de administración de New Relic, con inicio de sesión único para la autenticación para que no tenga que escribirlas de nuevo. La página información general presenta una variedad de estadísticas de rendimiento. (Haga clic en la imagen para ver el tamaño total de página de información general).

[![Nueva pestaña de supervisión Relic](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Estos son sólo algunas de las estadísticas que se puede ver:

- Tiempo medio de respuesta en distintos momentos del día.

    ![Tiempo de respuesta](monitoring-and-telemetry/_static/image10.png)
- Tasas de rendimiento (en las solicitudes por minuto) en distintos momentos del día.

    ![Rendimiento](monitoring-and-telemetry/_static/image11.png)
- Tiempo de CPU del servidor empleado en el tratamiento diferentes solicitudes HTTP.

    ![Tiempos de transacción Web](monitoring-and-telemetry/_static/image12.png)
- Tiempo de CPU empleado en distintas partes del código de aplicación:

    ![Detalles de seguimiento](monitoring-and-telemetry/_static/image13.png)
- Estadísticas de rendimiento históricos.

    ![Historial de rendimiento](monitoring-and-telemetry/_static/image14.png)
- Las llamadas a servicios externos, como el servicio de Blob y estadísticas acerca de cómo confiable y con capacidad de respuesta ha sido el servicio.

    ![Servicios externos](monitoring-and-telemetry/_static/image15.png)

    ![Servicios externos](monitoring-and-telemetry/_static/image16.png)

    ![Servicio externo](monitoring-and-telemetry/_static/image17.png)
- Obtener información sobre dónde en el mundo o en la aplicación web de EE. UU. tráfico procedencia.

    ![Geography](monitoring-and-telemetry/_static/image18.png)

También puede configurar los informes y eventos. Por ejemplo, puede decir en cualquier momento comenzar a ver errores, envíe un correo electrónico al personal de soporte de alerta para el problema.

![Informes](monitoring-and-telemetry/_static/image19.png)

New Relic es simplemente un ejemplo de un sistema de telemetría; puede obtener todo esto desde otros servicios. La belleza de la nube es que, sin tener que escribir ningún código y para mínimos o ningún gasto, repentinamente puede obtener mucha más información acerca de cómo se usa la aplicación y lo que realmente están experimentando sus clientes.

<a id="log"></a>
## <a name="log-for-insight"></a>Registro para obtener información

Un paquete de telemetría es un buen primer paso, pero aún debe instrumentar su propio código. El servicio de telemetría indica cuando hay un problema y le indica lo que experimentan los clientes, pero no podría dar un gran cantidad de información sobre lo que está ocurriendo en el código.

No desea tener que de forma remota a un servidor de producción para ver lo que hace la aplicación. Que puede resultar práctico si tienes un servidor, pero ¿qué ocurre cuando escala a cientos de servidores y no sabe cuáles necesita remota con? El registro debe proporcionar información suficiente para que nunca tendrá que de forma remota a servidores de producción para analizar y depurar problemas. Debe registrar información suficiente para que pueda aislar problemas únicamente a través de los registros.

### <a name="log-in-production"></a>Inicie sesión en producción

Mucha gente activar el seguimiento en producción solo cuando hay un problema y quiere depurar. Esto puede presentar un considerable retraso entre el momento de que hacerse una idea de un problema y la hora de que obtener información de solución de problemas útil sobre él. Y la información que se obtiene no podría ser útil para errores intermitentes.

¿Qué se recomienda en el entorno de nube donde el almacenamiento es barato es siempre deje de iniciar sesión en producción. De este modo, cuando se producen errores dispone ya de ellos se ha iniciado, y cuente con datos históricos que pueden ayudar a analizar problemas que desarrollan con el tiempo o se producen con regularidad en momentos diferentes. Puede automatizar un proceso de depuración para eliminar registros antiguos, pero es posible que esté más caro configurar este proceso de conservar los registros.

El gasto añadido de registro es trivial en comparación con la cantidad de solución de problemas de tiempo y dinero que se puede guardar con todas la información que necesita ya está disponible cuando algo va mal. A continuación, cuando alguien le dice que tenían un error aleatorio en algún momento aproximadamente 8:00 de la noche anterior, pero no recuerdan el error, fácilmente puede averiguar cuál fue el problema.

Para menos de $4 un mes puede tener 50 GB de registros a mano y el impacto de rendimiento de inicio de sesión es trivial siempre y cuando tenga una cosa en mente, con el fin de evitar cuellos de botella de rendimiento, asegúrese de que la biblioteca de registro es asincrónica.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Diferenciar los registros que informan de los registros que requieren acción

Los registros están diseñados para INFORM (quiero saber algo) o ACT (quiero hacer algo). Tenga cuidado al escribir solo registros de ACT para problemas que realmente requieren una persona o un proceso automatizado para tomar medidas. Hay demasiados registros ACT creará ruido, que requieren demasiado trabajo llevaba a través de él todo para localizar problemas originales. Y si los registros de ACT desencadenan automáticamente alguna acción como el envío de correo electrónico al personal de soporte técnico, evite permitir que miles de estas acciones se desencadena por un solo problema.

En el seguimiento de System.Diagnostics. NET, se pueden asignar a los registros de nivel de Error, advertencia, información y depuración/Verbose. Puede diferenciar ACT de INFORM registros al reservar el nivel de Error para los registros de ACT y usar los niveles inferiores para los registros INFORM.

![Niveles de registro](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Configurar los niveles de registro en tiempo de ejecución

Mientras que merece la pena tener sesión siempre en producción, otra práctica recomendada es implementar una plataforma de registro que le permite ajustar en tiempo de ejecución en el nivel de detalle que se está iniciando, sin volver a implementar o reiniciar la aplicación. Por ejemplo, cuando usa la opción de seguimiento en `System.Diagnostics` puede crear Error, advertencia, información y los registros de depuración y detallado. Se recomienda iniciar siempre Error, advertencia y registra información en producción y desea poder agregar dinámicamente el registro de depuración y detallado para solucionar problemas en el caso por caso.

Las aplicaciones Web en Azure App Service tienen compatibilidad integrada para escribir en él `System.Diagnostics` registros para el sistema de archivos, almacenamiento de tablas o almacenamiento de blobs. Puede seleccionar los niveles de registro para cada destino de almacenamiento, y puede cambiar el nivel de registro sobre la marcha sin tener que reiniciar la aplicación. La compatibilidad con almacenamiento de blobs facilita la ejecución [HDInsight](https://docs.microsoft.com/azure/hdinsight/) trabajos de análisis en los registros de aplicaciones, porque HDInsight sabe cómo trabajar directamente con el almacenamiento de blobs.

### <a name="log-exceptions"></a>Registrar excepciones

No incluya solo *excepción. ToString()* en el código de registro. Se dejan fuera de la información contextual. En el caso de errores de SQL, deja fuera el número de error SQL. Para todas las excepciones, incluir información de contexto, la propia excepción y las excepciones internas para asegurarse de que se proporciona todo lo que serán necesarios para solucionar problemas. Por ejemplo, información de contexto puede incluir el nombre del servidor, un identificador de transacción y un nombre de usuario (pero no la contraseña o los secretos).

Si depende de cada desarrollador hacer lo correcto con el registro de excepciones, algunos de ellos no. Para asegurarse de que se lleva a cabo el derecho de forma cada vez, compilar el control directamente en la interfaz del registrador de excepciones: pase el objeto de excepción para la clase de registrador y registrar los datos de la excepción correctamente en la clase de registrador.

### <a name="log-calls-to-services"></a>Registro de llamadas a servicios

Se recomienda encarecidamente que escribir un registro cada vez que la aplicación llama a un servicio, ya sea para una base de datos o una API de REST o cualquier servicio externo. Incluir en los registros no solo una indicación de éxito o error, pero el tiempo que tardó cada solicitud. En el entorno de nube a menudo verá problemas relacionados con ralentizaciones en lugar de interrupciones completadas. De repente algo que normalmente tarda 10 milisegundos podría empezar teniendo a un segundo. Cuando alguien le indique que la aplicación es lenta, desea poder buscar en New Relic o cualquier servicio de telemetría tienen y validar su experiencia y, a continuación, desea poder buscar son sus propios registros para profundizar en los detalles de por qué es lenta.

### <a name="use-an-ilogger-interface"></a>Usar una interfaz ILogger

¿Qué se recomienda hacer al crear una aplicación de producción es crear una sencilla *ILogger* interfaz y a seguir algunos métodos en ella. Esto facilita la cambiar más adelante la implementación de registro y no tiene que pasar por todo el código que lo haga. Vamos podríamos usar el `System.Diagnostics.Trace` clase a lo largo de la aplicación Fix It, pero en su lugar lo usamos en segundo plano en una clase de registro que implementa *ILogger*, y se realizarán *ILogger* método llama a lo largo de la aplicación.

De este modo, si desea que el registro sea más completo, puede reemplazar [ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) con cualquier mecanismo de registro que desee. Por ejemplo, a medida que crece la aplicación podría decidir que desea usar un paquete de registro más completo, como [NLog](http://nlog-project.org/) o [registro Application Block de Enterprise Library](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) es otra plataforma de registro populares, pero no hace registro asincrónico.)

Una posible razón para usar un marco como NLog es facilitar la división de los registro de salida en almacenes de datos de gran volumen y de gran valor independientes. Que le ayuda a almacenar eficazmente grandes volúmenes de datos INFORM que no necesita ejecutar consultas rápidas en, al tiempo que mantiene un acceso rápido a los datos de ACT.

### <a name="semantic-logging"></a>Registro semántico

Para que hacer el registro que puede generar información de diagnóstico más útil una forma relativamente nueva, consulte [Enterprise Library semántica registro aplicación bloque (cemento)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). CEMENTO utiliza [seguimiento de eventos para Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) y [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) admite .NET Framework 4.5 para que pueda crear más registros consultables y no estructurados. Definir un método diferente para cada tipo de evento que inicia sesión, que le permite personalizar la información que se escribe. Por ejemplo, para registrar un error de base de datos SQL puede llamar a un `LogSQLDatabaseError` método. Para ese tipo de excepción, sabe que una pieza clave de información es el número de error, por lo que podría incluir un parámetro de número de error en la firma del método y registre el número de error como un campo distinto en la entrada del registro que se escribe. Dado que el número se encuentra en un campo independiente más fácil y confiable obtendrá informes basados en números de error SQL, de lo que podía si simplemente se concatenar el número de error en una cadena de mensaje.

## <a name="logging-in-the-fix-it-app"></a>Registro en la corrección de aplicación

### <a name="the-ilogger-interface"></a>La interfaz ILogger

Este es el *ILogger* interfaz en la aplicación Fix It.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Estos métodos le permiten escribir registros en los mismos niveles de cuatro compatibles con *System.Diagnostics*. Los métodos TraceApi son para registrar las llamadas de servicio externo con información acerca de la latencia. También puede agregar un conjunto de métodos de nivel de depuración y detallado.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>La implementación del registrador de la interfaz ILogger

La implementación de la interfaz es muy sencilla. Básicamente, simplemente llama a la norma *System.Diagnostics* métodos. El siguiente fragmento muestra los tres de los métodos de información y uno para cada uno de los demás.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Llamar a los métodos de ILogger

Cada vez que el código en la aplicación Fix It detecta una excepción, llama a un *ILogger* método para registrar los detalles de la excepción. Y cada vez que realiza una llamada a la base de datos, el servicio Blob o una API de REST, se inicia un cronómetro antes de la llamada, se detiene el cronómetro cuando el servicio devuelve y registra el tiempo transcurrido junto con información sobre el éxito o error.

Tenga en cuenta que el mensaje del registro incluye el nombre de clase y el nombre del método. Es una buena práctica para asegurarse de que los mensajes de registro identifican los escribían en qué parte del código de aplicación.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Así que ahora para cada vez que la aplicación Fix It ha realizado una llamada a la base de datos SQL, puede ver la llamada, el método que lo llamó, y exactamente cuánto tiempo se tardó.

![Consulta de base de datos SQL en los registros](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Si se vaya a examinar los registros, puede ver que la hora de realizar llamadas de base de datos es variable. Que la información puede resultar útil: dado que la aplicación registra todo esto puede analizar las tendencias históricas en funcionamiento el servicio de base de datos con el tiempo. Por ejemplo, un servicio podría ser rápido la mayoría del tiempo pero podrían producir un error en las solicitudes o respuestas ralentizar en determinados momentos del día.

Puede hacer lo mismo para el servicio de Blob cada vez que la aplicación carga un archivo nuevo, hay un registro y puede ver exactamente cuánto tiempo ha tardado en cargar todos los archivos.

![Registro de la carga de BLOB](monitoring-and-telemetry/_static/image23.png)

Es sólo un par de líneas adicional de código para escribir cada vez que se llama a un servicio, y ahora cada vez que alguien le dice que produjo un problema, sabrá exactamente lo que el problema era, si ha producido un error, o incluso si sólo se ejecutan lentamente. Puede localizar el origen del problema sin tener que de forma remota a un servidor o activar el registro después de que el error se produce y esperamos volver a crearla.

## <a name="dependency-injection-in-the-fix-it-app"></a>Inserción de dependencias en la solución, aplicación

Tal vez se pregunte cómo el constructor de repositorio en el ejemplo anterior obtiene al registrador de la implementación de la interfaz:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Para conectar la interfaz a la implementación de la aplicación usa [inserción de dependencias](http://en.wikipedia.org/wiki/Dependency_injection)(DI) con [AutoFac](http://autofac.org/). Inserción de dependencias le permite usar un objeto basado en una interfaz en varios lugares en todo el código y solo tiene que especificar en un solo lugar, la implementación que se usa cuando se crea una instancia de la interfaz. Así resulta más fácil cambiar la implementación: por ejemplo, es posible que desea reemplazar el registrador de System.Diagnostics con un registrador de NLog. O bien, para las pruebas automatizadas puede sustituir una versión ficticia del registrador.

La aplicación Fix It utiliza DI en todos los repositorios y todos los controladores. Los constructores de las clases del controlador obtención una *ITaskRepository* la interfaz de la misma manera que el repositorio Obtiene una interfaz de registrador:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

La aplicación usa la biblioteca de AutoFac DI para proporcionar automáticamente *TaskRepository* y *registrador* instancias para estos constructores.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Básicamente, este código indica que necesita de un constructor en cualquier lugar un *ILogger* de la interfaz, pase una instancia de la *registrador* (clase), y cada vez que necesita un *IFixItTaskRepository*la interfaz, pase una instancia de la *FixItTaskRepository* clase.

[AutoFac](http://autofac.org/) es uno de los muchos marcos de inyección de dependencia que puede usar. Es otro muy popular [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), que es recomendable y compatible con Microsoft Patterns and Practices.

## <a name="built-in-logging-support-in-azure"></a>Compatibilidad de registro integrados en Azure

Azure admite los siguientes tipos de [registro para aplicaciones Web en Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- Seguimiento de System.Diagnostics (puede activar y desactivar y establecer niveles sobre la marcha sin tener que reiniciar el sitio).
- Eventos de Windows.
- Registros de IIS (HTTP/FREB).

Azure admite los siguientes tipos de [registro en los servicios de nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Seguimiento de System.Diagnostics.
- Contadores de rendimiento.
- Eventos de Windows.
- Registros de IIS (HTTP/FREB).
- Supervisar el directorio personalizado.

La aplicación Fix It utiliza el seguimiento de System.Diagnostics. Todo lo que necesita hacer para habilitar el registro en una aplicación web de System.Diagnostics es cambiar un conmutador en el portal o llamar a la API de REST. En el portal, haga clic en el **configuración** pestaña para el sitio y desplácese hacia abajo para ver el **Application Diagnostics** sección. Puede activar o desactivar el registro y seleccione el nivel de registro que desee. Puede tener Azure escribir los registros en el sistema de archivos o a una cuenta de almacenamiento.

![Diagnósticos de aplicación y de sitio en la pestaña configurar](monitoring-and-telemetry/_static/image24.png)

Después de habilitar el registro en Azure, puede ver los registros en la ventana de salida de Visual Studio cuando se crean.

![Menú de los registros de streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menú de los registros de streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

También puede tener registros escritos en la cuenta de almacenamiento y vea con cualquier herramienta que puede acceder al servicio de Azure Storage Table, como **Explorador de servidores** en Visual Studio o [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

![Registros en el Explorador de servidores](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Resumen

Es muy sencillo implementar un sistema de telemetría de fábrica, instrumentar iniciar sesión en su propio código y configurar el registro en Azure. Y cuando haya problemas de producción, la combinación de un sistema de telemetría y registros personalizados le ayudará a resolver rápidamente los problemas antes de que se conviertan en problemas importantes para los clientes.

En el [siguiente capítulo](transient-fault-handling.md) veremos cómo controlar errores transitorios, por lo que no se convierten en problemas de producción que se deben investigar.

## <a name="resources"></a>Recursos

Para obtener más información, vea los siguientes recursos.

Documentación sobre la telemetría principalmente:

- [Microsoft Patterns and Practices - Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte el patrón de reconfiguración en tiempo de ejecución, Guía de servicio de disponibilidad, patrón de supervisión del estado de punto de conexión y orientación para la instrumentación y telemetría.
- [Decimales gestos en la nube: habilitación de nuevo en Azure websites de nivel de supervisión de rendimiento de Relic](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Procedimientos recomendados para el diseño de servicios a gran escala en Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Notas del producto, Mark Simms y Michael Thomassy. Consulte la sección de telemetría y diagnósticos.
- [Desarrollo de próxima generación con Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Artículo de MSDN Magazine.

Documentación principalmente acerca del registro:

- [Bloque de aplicaciones de registro semántico (placa)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie presenta el caso de registro semántico con cemento.
- [Creación de registros significativos y no estructurados con registro semántico](https://channel9.msdn.com/Events/Build/2013/3-336). (Vídeo) Juliano Dominguez presenta el caso de registro semántico con cemento.
- [EF6 Registro de SQL: parte 1: registro Simple](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers se muestra cómo registrar las consultas ejecutadas por Entity Framework en EF 6.
- [Resistencia de conexión e intercepción de comandos con Entity Framework en una aplicación ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). En cuarto lugar en una serie de tutoriales de nueve partes, se muestra cómo usar la característica de intercepción de comandos de EF 6 que registre los comandos SQL que se envía a la base de datos mediante Entity Framework.
- [Mejorar el registro de uso de atributos de información del llamador de C# 5.0](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Cómo registrar fácilmente el nombre del método que realiza la llamada sin lo codificando en literales o usar la reflexión para obtenerlo manualmente.

Documentación principalmente sobre solución de problemas:

- [Solución de problemas de Azure &amp; depuración blog](https://blogs.msdn.com/b/kwill/).
- [AzureTools: la herramienta usa el equipo de soporte técnico de Azure para desarrolladores](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Presenta y proporciona un vínculo de descarga de una herramienta que puede usarse en una máquina virtual de Azure para descargar y ejecutar una amplia variedad de herramientas de diagnóstico y supervisión. Resulta útil cuando se necesita para diagnosticar un problema en una máquina virtual concreta.
- [Solución de problemas de una aplicación web en Azure App Service mediante Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Un tutorial paso a paso para comenzar con el seguimiento de System.Diagnostics y depuración remota.

Vídeos:

- [FailSafe: Creación de servicios de nube escalables, resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de nueve partes, Ulrich Homann, Marc Mercuri y Mark Simms. Presenta conceptos y principios de la arquitectura de una manera muy accesible e interesante, con casos procedentes de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Episodios 4 y 9 son sobre la supervisión y telemetría. Episodio 9 incluye una visión general de supervisión de servicios MetricsHub, AppDynamics, New Relic y PagerDuty.
- [Creación Big: Lecciones aprendidas de los clientes de Azure: parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms habla sobre el diseño de error y la instrumentación de todo el contenido. Similar a la serie Failsafe pero entran en obtener más información sobre procedimientos.

Ejemplo de código:

- [En la nube Fundamentos de servicio en Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicación de ejemplo creado por el equipo de asesoramiento de cliente de Microsoft Azure. Muestra datos de telemetría y prácticas recomendadas de registro, como se explica en los siguientes artículos. El ejemplo implementa el registro de aplicación mediante el uso de [NLog](http://nlog-project.org/). Para obtener documentación relacionada, consulte el [serie de cuatro artículos de wiki de TechNet sobre telemetría y el registro](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Anterior](design-to-survive-failures.md)
> [Siguiente](transient-fault-handling.md)
