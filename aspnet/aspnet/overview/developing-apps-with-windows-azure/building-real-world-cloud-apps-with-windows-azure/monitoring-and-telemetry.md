---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Supervisión y telemetría (creación de aplicaciones de nube reales con Azure) | Documentos de Microsoft
author: MikeWasson
description: Las aplicaciones de nube de creación Real World con libros electrónicos Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas recomendadas que puede...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2015
ms.topic: article
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: d58c495b3888c146a2a9bc831865cf7cc0d94c7b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Supervisión y telemetría (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descarga corregirlo proyecto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libros electrónicos](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **compilar aplicaciones de nube de mundo Real con Azure** libros electrónicos se basa en una presentación desarrollada por Scott Guthrie. Se explican los 13 patrones y prácticas recomendadas que le permitirá tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Mucha gente se basan en los clientes que les permiten saber si su aplicación está inactiva. Que no es realmente una práctica recomendada en cualquier lugar y sobre todo no está en la nube. No hay ninguna garantía de notificación rápida y, al recibir notificaciones, a menudo obtiene datos mínimo o puede inducir a error acerca de qué ha ocurrido. Con buena telemetría y sistemas de registro, puede tener en cuenta que está sucediendo con la aplicación y cuando algo sale mal averiguar inmediatamente y tener información de solución de problemas útil para trabajar con.

## <a name="buy-or-rent-a-telemetry-solution"></a>Comprar o alquilar una solución de telemetría

> [!NOTE]
> En este artículo se escribió antes de [Application Insights](https://azure.microsoft.com/services/application-insights/) se publicó. Visión de la aplicación es el método preferido para las soluciones de telemetría en Azure. Vea [configurar Application Insights para su sitio Web ASP.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) para obtener más información.


Una de las cosas que excelente sobre el entorno de nube es que resulta más fácil comprar o alquilar el camino para victoria. La telemetría es un ejemplo. Sin un gran esfuerzo puede obtener un sistema de telemetría realmente buenos hacia arriba y funcionamiento, muy económica. Hay una serie de grandes asociados que se integran con Azure, y algunos de ellos tienen niveles libres: para que puedas telemetría básico para nada. Presentamos algunos de los que está disponible en Azure:

- [New Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

A partir de marzo de 2015, [Microsoft Application Insights para Visual Studio Online](https://azure.microsoft.com/documentation/articles/app-insights-get-started/) no se ha liberado aún pero está disponible en vista previa para probarlo. [Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) también incluye características de supervisión.

Usaremos rápidamente configurando New Relic para mostrar lo fácil que puede ser utilizar un sistema de telemetría.

En el portal de administración de Azure, registrarse para el servicio. Haga clic en **New**y, a continuación, haga clic en **almacén**. El **elegir un complemento** aparece el cuadro de diálogo. Desplácese hacia abajo y haga clic en **New Relic**.

![Elegir un complemento](monitoring-and-telemetry/_static/image1.png)

Haga clic en la flecha derecha y elija el nivel de servicio que desee. En esta demostración se utilizará el nivel gratis.

![Personalizar el complemento](monitoring-and-telemetry/_static/image2.png)

Haga clic en la flecha derecha, confirme la "compra" y New Relic aparecerá ahora como un complemento en el portal.

![Revise la compra](monitoring-and-telemetry/_static/image3.png)

![Nuevo complemento Relic en el portal de administración](monitoring-and-telemetry/_static/image4.png)

Haga clic en **información de conexión**y copie la clave de licencia.

![Información de conexión](monitoring-and-telemetry/_static/image5.png)

Vaya a la **configurar** ficha para configurar la aplicación web en el portal, **supervisión del rendimiento** a **complemento**y establezca el **elegir complemento** lista desplegable para **New Relic**. A continuación, haga clic en **guardar**.

![New Relic en la pestaña configurar](monitoring-and-telemetry/_static/image6.png)

En Visual Studio, instale el paquete NuGet de Relic nueva en la aplicación.

![Análisis de desarrollador en la pestaña configurar](monitoring-and-telemetry/_static/image7.png)

Implementar la aplicación en Azure y empezar a usar. Crear repararlo algunas tareas para proporcionar alguna actividad para New Relic supervisar.

A continuación, vaya a la **New Relic** página en el **complementos** ficha del portal y haga clic en **administrar**. El portal envía en el portal de administración de New Relic, mediante el inicio de sesión único para la autenticación para que no tenga que escribirlas de nuevo. La página de introducción presenta una serie de estadísticas de rendimiento. (Haga clic en la imagen para ver el tamaño total de página de introducción).

[![Nueva pestaña supervisión Relic](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Estos son sólo algunas de las estadísticas que se puede ver:

- Tiempo medio de respuesta en distintos momentos del día.

    ![Tiempo de respuesta](monitoring-and-telemetry/_static/image10.png)
- Tasas de rendimiento (de solicitudes por minuto) en distintos momentos del día.

    ![Rendimiento](monitoring-and-telemetry/_static/image11.png)
- Tiempo de CPU del servidor dedicado a controlar las solicitudes HTTP diferentes.

    ![Tiempos de transacción Web](monitoring-and-telemetry/_static/image12.png)
- Tiempo de CPU dedicado a diferentes partes del código de aplicación:

    ![Detalles de seguimiento](monitoring-and-telemetry/_static/image13.png)
- Estadísticas de rendimiento históricos.

    ![Historial de rendimiento](monitoring-and-telemetry/_static/image14.png)
- Llamadas a servicios externos, como el servicio Blob y estadísticas acerca de cómo confiable y capacidad de respuesta ha sido el servicio.

    ![Servicios externos](monitoring-and-telemetry/_static/image15.png)

    ![Servicios externos](monitoring-and-telemetry/_static/image16.png)

    ![Servicio externo](monitoring-and-telemetry/_static/image17.png)
- Información acerca de la ubicación en el mundo o en la aplicación web de Estados Unidos tráfico procedencia.

    ![Geography](monitoring-and-telemetry/_static/image18.png)

También puede configurar informes y eventos. Por ejemplo, puede decir siempre al comenzar a ver los errores, envíe un correo electrónico al personal de soporte de alerta para el problema.

![Informes](monitoring-and-telemetry/_static/image19.png)

New Relic es simplemente un ejemplo de un sistema de telemetría; puede obtener todo esto desde otros servicios. La belleza de la nube es que, sin tener que escribir ningún código y para mínima o ninguna gastos repentinamente puede obtener mucha más información acerca de cómo se usa la aplicación y lo que los clientes están experimentando realmente.

<a id="log"></a>
## <a name="log-for-insight"></a>Registro para obtener información

Un paquete de telemetría es un buen primer paso, pero tendrá que instrumentar su propio código. El servicio de telemetría indica cuando hay un problema y le indica lo que están experimentando los clientes, pero no podría dar mucha información sobre lo que ocurre en el código.

No desea que estén a remoto en un servidor de producción para ver lo que está haciendo la aplicación. Que puede resultar práctico si tienes un servidor, pero ¿qué sucede cuando ha ajustado a cientos de servidores y no sabe cuáles necesita remoto en? El registro debe proporcionar información suficiente para que nunca tendrá que remota en servidores de producción para analizar y depurar problemas. Debe registrar información suficiente para que pueda aislar problemas únicamente a través de los registros.

### <a name="log-in-production"></a>Inicie sesión en producción

Mucha gente activar el seguimiento en producción solo cuando hay un problema y desean depurar. Esto puede presentar un retraso considerable entre el momento en que es consciente de un problema y la hora de que obtener información de solución de problemas útil sobre él. Y la información que se obtendrá no puede ser útil para los errores intermitentes.

Se recomienda en el entorno de nube donde el almacenamiento es barato es que siempre deje a iniciar sesión en producción. De este modo cuando producirán errores ya ya los ha iniciado y de que los datos históricos que pueden ayudar a analizar problemas que desarrollan con el tiempo o se producen con regularidad en momentos diferentes. Puede automatizar un proceso de purga para eliminar los registros antiguos, pero es posible que resulta más económico configurar este tipo de un proceso diferente mantener los registros.

El gasto adicional de inicio de sesión es trivial en comparación con la cantidad de solución de problemas de tiempo y dinero que puede ahorrar si tener toda la información que necesita ya están disponibles cuando algo va mal. A continuación, cuando alguien le dice que tenían un error aleatorio unos minutos aproximadamente 8:00 de la noche anterior, pero no recuerdan el error, fácilmente puede averiguar cuál fue el problema.

Para menos de 4 $ un mes puede tener 50 gigabytes de registros a mano y el impacto en el rendimiento de inicio de sesión es trivial, siempre y cuando tenga una cosa en cuenta: con el fin de evitar los cuellos de botella de rendimiento, asegúrese de que la biblioteca de registro es asincrónica.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Diferenciar los registros que informan de registros que requieran acción

Los registros están diseñados para INFORM (quiere saber algo) u otro acto (desea hacer algo). Asegúrese de escribir solo los registros de ACT para problemas que requieren realmente una persona o un proceso automatizado para tomar medidas. Hay demasiados registros de ACT creará ruido, que requieren muy trabajoso tener que deberá todo esto para encontrar problemas originales. Y si los registros de ACT desencadenan automáticamente alguna acción, como enviar correo electrónico a personal de soporte técnico, evite permitiendo miles de estas acciones se desencadena por un único problema.

En seguimiento de System.Diagnostics. NET, se pueden asignar a registros de nivel de Error, advertencia, información y depuración/detallado. Puede diferenciar ACT INFORM registros de reservar el nivel de Error para los registros de ACT y usando los niveles inferiores para los registros INFORM.

![Niveles de registro](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Configurar los niveles de registro en tiempo de ejecución

Mientras que merece la pena tener a iniciar sesión siempre en producción, otra práctica recomendada es implementar un marco de registro que permite ajustar en tiempo de ejecución en el nivel de detalle que está iniciando una sesión, sin volver a implementar o reiniciar la aplicación. Por ejemplo, cuando usa la opción de seguimiento en `System.Diagnostics` puede crear Error, advertencia, información y registros de depuración/detallado. Se recomienda siempre iniciar Error, advertencia y registra información en producción y desea poder agregar dinámicamente el registro de depuración/detallado para la solución caso por caso.

Las aplicaciones Web en el servicio de aplicación de Azure tienen compatibilidad integrada para escribir en él `System.Diagnostics` registros para el sistema de archivos, el almacenamiento de tabla o el almacenamiento de blobs. Puede seleccionar diferentes niveles de registro para cada destino de almacenamiento, y puede cambiar el nivel de registro sobre la marcha sin necesidad de reiniciar la aplicación. La compatibilidad con almacenamiento de blobs resulta más sencillo ejecutar [HDInsight](https://docs.microsoft.com/azure/hdinsight/) trabajos de análisis en los registros de aplicaciones, porque HDInsight sabe cómo trabajar directamente con el almacenamiento de blobs.

### <a name="log-exceptions"></a>Excepciones de registro

No incluya solo *excepción. ToString()* en el código de registro. Que no especifica la información contextual. En el caso de errores SQL, deja el número de error SQL. Para todas las excepciones, incluyen información de contexto, la propia excepción y las excepciones internas para asegurarse de que va a proporcionar todo lo que será necesaria para solucionar el problema. Por ejemplo, la información de contexto puede incluir el nombre del servidor, un identificador de transacción y un nombre de usuario (pero no la contraseña o los secretos!).

Si confía en cada desarrollador hacer lo correcto con registro de excepción, no tendrá algunas de ellas. Para asegurarse de que se complete el derecho de forma cada vez, crear directamente en la interfaz del registrador de control de excepciones: pasar el objeto de excepción para la clase de registrador y registrar correctamente los datos de excepción en la clase de registrador.

### <a name="log-calls-to-services"></a>Registro de llamadas a servicios

Se recomienda escribir un registro cada vez que la aplicación llama a un servicio, ya sea para una base de datos o una API de REST o ningún servicio externo. Incluir en los registros no solo una indicación de éxito o error pero cuánto tiempo tardó cada solicitud. En el entorno de nube a menudo verá problemas relacionados con las ralentizaciones en lugar de interrupciones completadas. De repente algo que normalmente tarda 10 milisegundos podría comienza a realizar a un segundo. Cuando alguien le indique que la aplicación es lenta, desea buscar en New Relic o cualquier servicio de telemetría tienen y validar su experiencia y, a continuación, desea poder buscar son sus propios registros para profundizar en los detalles de por qué es lenta.

### <a name="use-an-ilogger-interface"></a>Usar una interfaz ILogger

¿Qué se recomienda realizar cuando se crea una aplicación de producción es crear un sencillo *ILogger* interfaz y pegar algunos métodos en ella. Esto facilita la cambia la implementación del registro posterior y no tiene que pasar por todo el código para que lo haga. Se puede usar el `System.Diagnostics.Trace` clase a lo largo de la aplicación repararlo, pero en su lugar que estamos usando tras los bastidores en una clase de registro que implementa *ILogger*, y lo convertimos *ILogger* método llama a lo largo de la aplicación.

De este modo, si desea realizar el registro más completo, puede reemplazar [ `System.Diagnostics.Trace` ](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) con cualquier mecanismo de registro que desee. Por ejemplo, a medida que crece la aplicación podría decidir que desea usar un paquete de registro más completo como [NLog](http://nlog-project.org/) o [bloque de aplicación de registro de biblioteca de empresas](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4Net](http://logging.apache.org/log4net/) es otro marco de trabajo de registro popular pero no registro asincrónico.)

Una posible razón para utilizar un marco como NLog es facilitar dividir el registro de salida en almacenes de datos de gran volumen y de gran valor independiente. Que le ayuda a almacenar de forma eficaz grandes volúmenes de datos INFORM que no es necesario ejecutar las consultas rápidas con, al tiempo que mantiene un acceso rápido a los datos de ACT.

### <a name="semantic-logging"></a>Registro semántica

Para que un modo relativamente nuevo realizar el registro que puede generar información de diagnóstico más útil, vea [Enterprise Library semántica registro aplicación bloque (bloques)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Usa bloques [seguimiento de eventos para Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) y [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) admite en .NET 4.5 para permitirle crear más registros consultables y no estructurados. Definir un método diferente para cada tipo de evento que ha iniciado sesión, que le permite personalizar la información que se escribe. Por ejemplo, para registrar un error de base de datos SQL puede llamar a un `LogSQLDatabaseError` método. Para ese tipo de excepción, sabrá que una pieza clave de información es el número de error, por lo que puede incluir un parámetro de número de error en la firma del método y registre el número de error como un campo independiente en la entrada del registro que se escribe. Dado que el número está en un campo independiente puede más fácil y confiable obtener informes basados en números de error SQL que se puede realizar si se concatenan simplemente el número de error en una cadena de mensaje.

## <a name="logging-in-the-fix-it-app"></a>Registro en la corrección de aplicación

### <a name="the-ilogger-interface"></a>La interfaz ILogger

Este es el *ILogger* interfaz en la aplicación repararlo.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Estos métodos le permiten escribir registros en los mismos niveles de cuatro compatibles con *System.Diagnostics*. Los métodos de TraceApi son para el registro de llamadas de servicio externo con información acerca de la latencia. También puede agregar un conjunto de métodos de nivel de depuración/detallado.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>La implementación de registrador de la interfaz ILogger

La implementación de la interfaz es muy sencilla. Básicamente simplemente llama a la norma *System.Diagnostics* métodos. El siguiente fragmento muestra los tres de los métodos de información y uno para cada uno de los demás.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Llama a los métodos de ILogger

Cada vez que el código de la aplicación repararlo detecta una excepción, llama una *ILogger* método para registrar los detalles de la excepción. Y cada vez que realiza una llamada a la base de datos, servicio de Blob o una API de REST, inicia un cronómetro antes de la llamada, se detiene el cronómetro cuando el servicio devuelve y registra el tiempo transcurrido junto con información sobre el éxito o error.

Tenga en cuenta que el mensaje del registro incluye el nombre de clase y el nombre del método. Es una buena práctica para asegurarse de que los mensajes de registro identifican escribían en qué parte del código de aplicación.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Ahora para cada vez que la aplicación repararlo ha realizado una llamada a la base de datos SQL, puede ver la llamada, el método que llamó, y exactamente cuánto tiempo se tardó.

![Consulta de base de datos de SQL en los registros](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Si vaya examine los registros, puede ver que la hora de realizar llamadas de base de datos es variable. Que la información puede ser útil: dado que la aplicación registra todo esto se pueden analizar las tendencias históricas en el rendimiento del servicio de base de datos con el tiempo. Por ejemplo, un servicio puede ser rápido mayoría de las veces, pero podrían producir un error en las solicitudes o podrían ralentizar las respuestas a determinadas horas del día.

Puede hacer lo mismo para el servicio Blob: cada vez que la aplicación carga un archivo nuevo, hay un registro y puede ver exactamente cuánto tiempo se tardó cargue cada archivo.

![Registro de la carga de BLOB](monitoring-and-telemetry/_static/image23.png)

Es un par líneas adicionales de código para escribir cada vez que se llama a un servicio y ahora cada vez que alguien le dice que tuvieron un problema, ya sabe exactamente lo que el problema fue, tanto si se trata de un error, o incluso si solo se estaba ejecutando lentamente. Puede localizar el origen del problema sin tener que remoto en un servidor o activar el registro después de que el error se produce y esperar que volver a crearlo.

## <a name="dependency-injection-in-the-fix-it-app"></a>Inyección de dependencia en la solución, aplicación

Tal vez se pregunte cómo el constructor de repositorio en el ejemplo anterior obtiene al registrador de implementación de interfaz:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Para conectar la interfaz para la implementación de la aplicación usa [inyección de dependencia](http://en.wikipedia.org/wiki/Dependency_injection)(DI) con [AutoFac](http://autofac.org/). DI permite usar un objeto basado en una interfaz en muchos lugares en todo el código y solo tiene que especificar en un solo lugar la implementación que se utiliza cuando se crea una instancia de la interfaz. Así resulta más fácil cambiar la implementación: por ejemplo, puede reemplazar el registrador de System.Diagnostics con un registrador de NLog. O bien, para las pruebas automatizadas desee sustituir una versión ficticia del registrador.

La aplicación repararlo utiliza DI en todos los repositorios y todos los controladores de. Los constructores de las clases de controlador obtener un *ITaskRepository* interfaz del mismo modo que el repositorio Obtiene una interfaz de registrador:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

La aplicación usa la biblioteca de AutoFac DI para proporcionar automáticamente *TaskRepository* y *registrador* instancias para determinar si estos constructores.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Básicamente, este código indica que necesita de un constructor en cualquier lugar un *ILogger* de la interfaz, pase una instancia de la *registrador* (clase), y cuando sea necesario un *IFixItTaskRepository*de la interfaz, pase una instancia de la *FixItTaskRepository* clase.

[AutoFac](http://autofac.org/) es uno de los muchos marcos de trabajo de inyección de dependencia que puede usar. Otro popular es [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), que es recomendable y compatible con Microsoft Patterns and Practices.

## <a name="built-in-logging-support-in-azure"></a>Compatibilidad de registro integrados en Azure

Azure admite los siguientes tipos de [registro para las aplicaciones Web en el servicio de aplicación de Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- Seguimiento de System.Diagnostics (puede activar y desactivar y establecer niveles sobre la marcha sin necesidad de reiniciar el sitio).
- Eventos de Windows.
- Registros de IIS (HTTP/FREB).

Azure admite los siguientes tipos de [registro de servicios en la nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Seguimiento de System.Diagnostics.
- Contadores de rendimiento.
- Eventos de Windows.
- Registros de IIS (HTTP/FREB).
- Supervisión de directorios personalizados.

La aplicación repararlo usa seguimiento de System.Diagnostics. Todo lo que necesita hacer para habilitar el registro en una aplicación web de System.Diagnostics es voltear un conmutador en el portal o llamar a la API de REST. En el portal, haga clic en el **configuración** pestaña para el sitio y desplácese hacia abajo para ver el **Application Diagnostics** sección. Puede activar o desactivar el registro y seleccione el nivel de registro que desee. Puede hacer que Azure escribir los registros en el sistema de archivos o en una cuenta de almacenamiento.

![Diagnóstico de aplicaciones y diagnósticos del sitio en la ficha configurar](monitoring-and-telemetry/_static/image24.png)

Después de habilitar el registro en Azure, puede ver los registros en la ventana Resultados de Visual Studio que se crean.

![Menú de registros de streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menú de registros de streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

También puede tener registros se escriban en la cuenta de almacenamiento y la vista con cualquier herramienta que puede acceder al servicio de la tabla de almacenamiento de Azure, como **Explorador de servidores** en Visual Studio o [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

![Registros en el Explorador de servidores](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Resumen

Es realmente fácil de implementar un sistema de telemetría de fábrica, instrumentar su propio código de inicio de sesión y configurar el registro en Azure. Y cuando haya problemas de producción, la combinación de un sistema de telemetría y registros personalizados le ayudará a resolver rápidamente los problemas antes de que resulten problemas importantes para los clientes.

En el [siguiente capítulo](transient-fault-handling.md) analizaremos cómo controlar errores transitorios, por lo que no se convierten en problemas de producción que se deben investigar.

## <a name="resources"></a>Recursos

Para obtener más información, vea los siguientes recursos.

Documentación principalmente acerca de telemetría:

- [Microsoft patrones y prácticas - Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Ver instrucciones de instrumentación y telemetría, Guía de servicio de medición, patrón de supervisión de estado de punto de conexión y patrón de volver a configurar en tiempo de ejecución.
- [Decimales aprietan en la nube: habilitar la supervisión de sitios Web de Azure de rendimiento de New Relic](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Procedimientos recomendados para el diseño de servicios a gran escala en los servicios de nube de Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Notas del producto, Mark Simms y Michael Thomassy. Vea la sección telemetría y diagnósticos.
- [Desarrollo de próxima generación con Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Artículo de MSDN Magazine.

Documentación principalmente acerca del registro:

- [Bloque de aplicación de registro semántico (bloques)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie presenta el caso de registro semántica con bloques.
- [Crear registros estructurados y significativos con registro semántica](https://channel9.msdn.com/Events/Build/2013/3-336). (Vídeo) Juliano Dominguez presenta el caso de registro semántica con bloques.
- [EF6 Inicio de sesión de SQL – parte 1: registro Simple](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers muestra cómo registrar las consultas ejecutadas por Entity Framework de EF 6.
- [Resistencia de conexión y comando interceptación con Entity Framework en una aplicación ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). En cuarto lugar en una serie de tutoriales de nueve partes, muestra cómo utilizar la característica de intercepción de EF 6 comando para registrar los comandos SQL enviados a la base de datos por Entity Framework.
- [Mejorar el registro mediante los atributos de información del autor de llamada de C# 5.0](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Cómo registrar el nombre del método que realiza la llamada fácilmente sin codificar de forma rígida en literales o use la reflexión para obtener de forma manual.

Documentación principalmente acerca de la solución de problemas:

- [Solución de problemas de Azure &amp; depuración blog](https://blogs.msdn.com/b/kwill/).
- [AzureTools – el diagnóstico utilidad utilizado por el equipo de soporte técnico de Azure para desarrolladores](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Presenta y proporciona un vínculo de descarga de una herramienta que puede usarse en una máquina virtual de Azure para descargar y ejecutar una amplia variedad de herramientas de diagnóstico y supervisión. Resulta útil cuando es necesario diagnosticar un problema en una máquina virtual concreta.
- [Solucionar problemas de una aplicación web en el servicio de aplicaciones de Azure con Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Un tutorial paso a paso para comenzar a usar seguimiento de System.Diagnostics y la depuración remota.

Vídeos:

- [FailSafe: Creación de servicios en la nube escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de nueve partes por Ulrich Homann y Marc Mercuri, Mark Simms. Presenta los conceptos y principios de la arquitectura de una manera muy interesante y accesible, con casos extraídos de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Episodios 4 y 9 son sobre la supervisión y telemetría. Episodio 9 incluye una visión general de supervisión de los servicios MetricsHub, AppDynamics, New Relic y PagerDuty.
- [Creación Big: Lecciones aprendidas de clientes de Azure, parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms habla sobre el diseño de error y la instrumentación de todo el contenido. Similar a la serie Failsafe pero queda procedimientos con mayor detalle.

Ejemplo de código:

- [Fundamentos de servicio en Azure en la nube](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicación de ejemplo creada por el equipo de asesoramiento de cliente de Microsoft Azure. Muestra la telemetría y prácticas recomendadas de registro, como se explica en los siguientes artículos. El ejemplo implementa el registro de aplicaciones mediante el uso de [NLog](http://nlog-project.org/). Para obtener documentación relacionada, consulte el [serie de cuatro artículos de wiki de TechNet sobre telemetría y el registro](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Anterior](design-to-survive-failures.md)
> [Siguiente](transient-fault-handling.md)
