---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Apéndice: La solución aplicación de ejemplo (creación de aplicaciones de nube reales con Azure) | Documentos de Microsoft'
author: MikeWasson
description: Las aplicaciones de nube de creación Real World con libros electrónicos Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas recomendadas que puede...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 9a1fa36b34c4783b101bb27bc6931241e9251e10
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876480"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Apéndice: La solución aplicación de ejemplo (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descargar la solución proyecto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> El **compilar aplicaciones de nube de mundo Real con Azure** libros electrónicos se basa en una presentación desarrollada por Scott Guthrie. Se explican los 13 patrones y prácticas recomendadas que le permitirá tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Este apéndice a las aplicaciones de nube de creación Real World con libros electrónicos Azure contiene las siguientes secciones que proporcionan información adicional sobre la repararlo aplicación de ejemplo que puede descargar:

- [Problemas conocidos](#knownissues)
- [Procedimientos recomendados](#bestpractices)
- [Cómo ejecutar la aplicación desde Visual Studio en el equipo local](#run-in-vs)
- [Cómo implementar la aplicación de base para las aplicaciones de Web de servicio de aplicación de Azure mediante el uso de las secuencias de comandos de Windows PowerShell](#deploybase)
- [Solución de problemas de las secuencias de comandos de Windows PowerShell](#troubleshooting)
- [Cómo implementar la aplicación con cola de procesamiento para aplicaciones de Web del servicio de aplicación de Azure y un servicio de nube de Azure](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Problemas conocidos

La aplicación repararlo fue desarrollada originalmente con el fin de ilustrar tan simple como sea posible algunos de los patrones presentan en este libro electrónico. Sin embargo, puesto que el libro electrónico es sobre la creación de aplicaciones reales, se somete el código repararlo para su revisión y el proceso de prueba similar a lo que hacemos de versiones de software. Se encontró una serie de problemas y al igual que con cualquier aplicación del mundo real, algunas de ellas solucionamos y algunas de ellas se aplaza hasta una versión posterior.

En la lista siguiente incluye los problemas que deben llevar a cabo en una aplicación de producción, pero por un motivo u otra que decidimos no a la dirección en la versión inicial de la aplicación de ejemplo repararlo.

### <a name="security"></a>Seguridad

- Asegúrese de que no se puede asignar una tarea a un propietario inexistente.
- Asegúrese de que sólo se puede ver y modificar las tareas que se creó o se le han asignado.
- Usar HTTPS para las páginas de inicio de sesión y las cookies de autenticación.
- Especifique un límite de tiempo para las cookies de autenticación.

### <a name="input-validation"></a>Validación de entrada

En general, una aplicación de producción haría más validación de entrada que la aplicación repararlo. Por ejemplo, el tamaño de la imagen o imágenes de tamaño de archivo permitido de carga debe ser limitada.

### <a name="administrator-functionality"></a>Funcionalidad de administrador

Un administrador debe poder cambiar la propiedad en las tareas existentes. Por ejemplo, el creador de una tarea podría dejan la empresa, dejando a nadie con autoridad para mantener la tarea a menos que se habilite el acceso administrativo.

### <a name="queue-message-processing"></a>Procesamiento de mensajes de cola

Mensaje de la cola de procesamiento en la aplicación repararlo está diseñado para ser simple para ilustrar el patrón centrada en la cola de trabajo con una cantidad mínima de código. Este código simple no sería apropiado para una aplicación de producción real.

- El código no garantiza que cada mensaje de la cola se procesará como máximo una vez. Cuando se recibe un mensaje de la cola, hay un período de tiempo de espera, durante el cual el mensaje es invisible para otros agentes de escucha de cola. Si el tiempo de espera expira antes de que el mensaje se elimina, el mensaje vuelve visible. Por lo tanto, si una instancia de rol de trabajo invierte mucho tiempo en procesar un mensaje, teóricamente, es posible para el mismo mensaje se procesaron dos veces, lo que da lugar a una tarea duplicada en la base de datos. Para obtener más información acerca de este problema, consulte [usar colas de almacenamiento de Azure](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- La lógica de sondeo de la cola podría ser más rentable, por lotes de recuperación de mensajes. Cada vez que se llama a [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), hay un costo de transacción. En su lugar, puede llamar a [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (tenga en cuenta el plural'), que obtiene varios mensajes en una sola transacción. Los costos de transacción para las colas de almacenamiento de Azure son muy bajos, por lo que el impacto en los costos no es importante en la mayoría de los escenarios.
- El bucle ajustado en el código de procesamiento de mensajes de cola hace que la afinidad de CPU, que no utilizan eficazmente las máquinas virtuales de varios núcleos. Un mejor diseño utilizaría el paralelismo de tareas para ejecutar varias tareas asincrónicas en paralelo.
- Procesamiento de mensajes de la cola tiene el control de excepciones sólo rudimentario. Por ejemplo, el código no manipula [mensajes dudosos](https://msdn.microsoft.com/library/ms789028.aspx). (Cuando el procesamiento de mensajes produce una excepción, tendrá que registrar el error y eliminar el mensaje, o el rol de trabajo intentará procesarlo de nuevo y el bucle continuará indefinidamente.)

### <a name="sql-queries-are-unbounded"></a>Las consultas SQL no están limitadas

Código de repararlo actual no coloca ningún límite en el número de filas podrían devolver las consultas para las páginas de índice. Si se especifica un gran volumen de tareas en la base de datos, el tamaño de las listas resultantes recibe podría provocar problemas de rendimiento. La solución consiste en implementar la paginación. Para obtener un ejemplo, vea [ordenación, filtrado y paginación con Entity Framework en una aplicación de MVC de ASP.NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Ver modelos recomendados

La aplicación repararlo utiliza la clase de entidad FixItTask para pasar información entre el controlador y la vista. Una práctica recomendada es usar modelos de vista. El modelo de dominio (por ejemplo, la clase de entidad FixItTask) está diseñado alrededor de los necesarios para la persistencia de datos, mientras que un modelo de vista puede diseñarse para presentar los datos. Para obtener más información, consulte [12 prácticas recomendadas de ASP.NET MVC](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Blob de imágenes seguro recomendado

Las tiendas de aplicaciones repararlo cargan imágenes como público, lo que significa que cualquier persona que se encuentra la dirección URL puede tener acceso a las imágenes. Las imágenes se pueden proteger en lugar de público.

### <a name="no-powershell-automation-scripts-for-queues"></a>No hay scripts de automatización de PowerShell para las colas

Scripts de automatización de PowerShell de ejemplo escritos sólo para la versión base de corregir, que se ejecuta completamente en aplicaciones de Web del servicio de aplicación de Azure. No hemos proporcionamos las secuencias de comandos para configurar e implementar para la aplicación web, además de entorno del servicio de nube necesaria para el procesamiento de la cola.

### <a name="special-handling-for-html-codes-in-user-input"></a>Tratamiento especial para los códigos HTML de proporcionados por el usuario

ASP.NET evita automáticamente que muchas formas en el que los usuarios malintencionados pueden tratar ataques XSS mediante la especificación de secuencia de comandos en los cuadros de texto de entrada de usuario. Y MVC `DisplayFor` auxiliar que se usa para mostrar tarea títulos y notas automáticamente valores codifica en HTML que envía al explorador. Pero en una aplicación de producción puede tomar medidas adicionales. Para obtener más información, consulte [solicitud de validación en ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Procedimientos recomendados

Los siguientes son algunos problemas que se corrigieron después de que se detectan en la revisión de código y las pruebas de la versión original de la aplicación repararlo. Algunos causados por el codificador original no es una práctica recomendada determinada, tenga en cuenta algunas simplemente porque el código se escribió rápidamente y no se ha diseñado para la versión del software. Le estamos mostrar aquí los problemas en caso de que hay algo que hemos visto desde esta revisión y pruebas pueden resultar útil para otras personas también está desarrollando aplicaciones web.

### <a name="dispose-the-database-repository"></a>Eliminar el repositorio de base de datos

El `FixItTaskRepository` clase debe desechar el Entity Framework `DbContext` instancia. Hicimos esto implementando `IDisposable` en la `FixItTaskRepository` clase:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Tenga en cuenta que AutoFac eliminará automáticamente el `FixItTaskRepository` de instancia, por lo que no es necesario disponer explícitamente de él.

Otra opción consiste en quitar el `DbContext` variable de miembro de `FixItTaskRepository`y en su lugar, cree una variable local `DbContext` variable dentro de cada método de repositorio, dentro de un `using` instrucción. Por ejemplo:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registrar como tal singletons con DI

Desde una única instancia de la `PhotoService` clase y `Logger` clase es necesario, estas clases deben ser [registrado como instancias únicas para la inyección de dependencia](https://code.google.com/p/autofac/wiki/InstanceScope) en *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Seguridad: No mostrar detalles del error a los usuarios

La original repararlo aplicación no tiene una página de error genérico y simplemente permitir todas las burbujas de excepciones hasta la interfaz de usuario, por lo que podrían dar lugar a algunas excepciones, como errores de conexión de base de datos en un seguimiento de pila completo que se muestra en el explorador. Información detallada del error a veces puede facilitar los ataques de usuarios malintencionados. La solución consiste en registrar los detalles de la excepción y mostrar una página de error al usuario que no incluya detalles del error. La aplicación repararlo ya estaba iniciando y con el fin de mostrar una página de error, hemos agregado `<customErrors mode=On>` en el archivo Web.config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

De forma predeterminada se hace *Views\Shared\Error.cshtml* que se mostrará para errores. Puede personalizar *Error.cshtml* o crear su propia vista de página de error y agregue un `defaultRedirect` atributo. También puede especificar distintas páginas de error para errores concretos.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Seguridad: Permitir solo la puede editar el creador de una tarea

La página de índice de panel solo muestra las tareas creadas por el usuario ha iniciado sesión, pero un usuario malintencionado podría crear una dirección URL con un identificador de tarea de otro usuario. Agregamos código en *DashboardController.cs* para devolver un error 404 en ese caso:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>No pasar excepciones

La original repararlo aplicación simplemente devolvió un valor null después de iniciar una excepción que dan como resultado de una consulta SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Esto sería buscar al usuario como si la consulta es correcta, pero no devolver ninguna fila. Solución consiste en volver a producir la excepción después de detectar y registro:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Detectar todas las excepciones en los roles de trabajo

Las excepciones no controladas en un rol de trabajo hará que la máquina virtual para que se recicle, por lo que va a incluir todo el contenido en un bloque try-catch y controlar todas las excepciones.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Especificar la longitud de las propiedades de cadena en las clases de entidad

Para mostrar un código sencillo, la versión original de la aplicación repararlo no especifica longitudes de los campos de la entidad FixItTask y, como resultado que se hayan definido como varchar (max) en la base de datos. Como resultado, la interfaz de usuario aceptaban casi cualquier cantidad de entrada. Especificar longitudes se establecen los límites que se aplican tanto al usuario de entrada en la página web y el tamaño de columna en la base de datos:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Marcar los miembros privados como de solo lectura que no se esperan para cambiar

Por ejemplo, en la `DashboardController` una instancia de la clase `FixItTaskRepository` se crea y no se espera para cambiar, por lo que se definieron como [readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Use la lista. Any() en lugar de la lista. Count() &gt; 0

Si se le preocupa si uno o varios elementos en una lista de ajustan a los criterios especificados, utilice la [cualquier](https://msdn.microsoft.com/library/bb534972.aspx) método, porque devuelve tan pronto como se encuentra un elemento que se cumplen los criterios, mientras que la `Count` método siempre tiene que recorrer en iteración a través de todos los elementos. El panel *Index.cshtml* archivo originalmente tenía este código:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Se cambió a esto:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generar direcciones URL en las vistas MVC utilizando aplicaciones auxiliares MVC

Para el **crear un repararlo** botón en la página de inicio, la repararlo aplicación duro codificada de un elemento delimitador:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Para obtener vínculos de acción/Vista parecido a esto es mejor utilizar el [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) aplicación auxiliar HTML, por ejemplo:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Utilice Task.Delay en lugar de Thread.Sleep en rol de trabajo

Coloca la plantilla nuevo proyecto `Thread.Sleep` en el ejemplo de código para un rol de trabajo, sino que produce el subproceso en modo de suspensión puede hacer que el grupo de subprocesos generar los subprocesos innecesarios adicionales. Se puede evitar mediante [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) en su lugar.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Evitar async void

Si un método asincrónico no tiene que devolver un valor, devolver un `Task` tipo en lugar de `void`.

Este ejemplo es de la `FixItQueueManager` clase: 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Debe usar `async void` solo para los controladores de eventos de nivel superior. Si define un método como `async void`, el llamador no puede **await** el método o detectar las excepciones que produce el método. Para obtener más información, consulte [los procedimientos recomendados de programación asincrónica](https://msdn.microsoft.com/magazine/jj991977.aspx). 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Usar un token de cancelación para interrumpir bucle de rol de trabajo

Normalmente, el **ejecutar** método en un rol de trabajo contiene un bucle infinito. Cuando se está deteniendo el rol de trabajo, el [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) se llama al método. Debe utilizar este método para cancelar el trabajo que se realiza dentro de la **ejecutar** método y salir correctamente. En caso contrario, es posible que finaliza el proceso en el medio de una operación.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>No participar en el procedimiento de examen de MIME automática

En algunos casos, Internet Explorer le informa un tipo MIME diferente que el tipo especificado por el servidor web. Por ejemplo, si Internet Explorer encuentra HTML contenido en un archivo que se entregan con el encabezado de respuesta HTTP Content-Type: texto/sin formato, Internet Explorer determina que el contenido se debe presentar como HTML. Desgraciadamente, este "MIME-examen" también puede provocar problemas de seguridad para servidores que hospedan contenido no es de confianza. Para evitar este problema, Internet Explorer 8, ha realizado una serie de cambios al código de determinación de tipo MIME y permite a los desarrolladores de aplicaciones [no participar en el examen de MIME](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). El código siguiente se agrega a la *Web.config* archivo.

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Habilitar la agrupación y minificación

Cuando Visual Studio crea un nuevo proyecto web, agrupación y minificación de archivos JavaScript no está habilitado de forma predeterminada. Se agregó una línea de código en BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Establecer un tiempo de espera de expiración para las cookies de autenticación

De forma predeterminada, las cookies de autenticación expiran dentro de dos semanas. Un tiempo más corto es más seguro. Puede cambiar esta configuración en *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Cómo ejecutar la aplicación desde Visual Studio en el equipo local

Hay dos maneras de ejecutar la aplicación repararlo:

- Ejecute la aplicación básica que escribe las nuevas tareas directamente en la base de datos SQL.
- Ejecute la aplicación utiliza una cola, además de un servicio back-end para crear tareas. El patrón de cola se describe en el capítulo [patrón centrada en la cola de trabajo](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Ejecutar la aplicación básica

1. Instalar [Visual Studio 2013 o Visual Studio 2013 Express for Web](https://www.visualstudio.com/downloads).
2. Instalar el [Azure SDK para .NET para Visual Studio 2013.](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. Descargue el archivo .zip desde el [MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. En el Explorador de archivos, haga clic en el archivo .zip, haga clic en propiedades y, a continuación, en la ventana Propiedades, haga clic en Desbloquear.
5. Descomprima el archivo.
6. Haga doble clic en el archivo .sln para iniciar Visual Studio.
7. En el menú Herramientas, haga clic en Administrador de paquetes de biblioteca y, a continuación, la consola de administrador de paquetes.
8. En la consola de administrador de paquete (PMC), haga clic en restaurar.
9. Salga de Visual Studio.
10. Iniciar el [emulador de almacenamiento de Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx).
11. Reinicie Visual Studio, abra el archivo de solución que se cerró en el paso anterior.
12. Asegúrese de que el proyecto FixIt está establecido como proyecto de inicio y, a continuación, presione CTRL + F5 para ejecutar el proyecto.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Ejecutar la aplicación con el procesamiento de la cola

1. Siga las instrucciones para [ejecutar la aplicación básica](#runbase)y, a continuación, cierre el explorador y cierre Visual Studio.
2. Inicie Visual Studio con privilegios de administrador. (Que vamos a usar el emulador de proceso de Azure, y requiere privilegios de administrador).
3. En la aplicación *Web.config* un archivo en el *MyFixIt* proyecto (el proyecto web), cambie el valor de `appSettings/UseQueues` en "true": 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Si el [emulador de almacenamiento de Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) no está todavía está ejecutando, inícielo de nuevo.
5. Ejecutar el proyecto de web FixIt y el proyecto de MyFixItCloudService simultáneamente.

    Con Visual Studio 2013:

   1. Presione F5 para ejecutar el proyecto FixIt.
   2. En **el Explorador de soluciones**, haga clic en el proyecto MyFixItCloudService y, a continuación, haga clic en **depurar** -- **Iniciar nueva instancia**.

      Uso de Visual Studio 2013 Express for Web:

   3. En el Explorador de soluciones, haga clic en la solución de FixIt y seleccione **propiedades**.
   4. Seleccione **proyectos de inicio múltiples**...
   5. En el **acción** seleccione de la lista desplegable de MyFixIt y MyFixItCloudService, **iniciar**.
   6. Haga clic en **Aceptar**.
   7. Presione F5 para ejecutar ambos proyectos.

      Al ejecutar el proyecto MyFixItCloudService, Visual Studio inicia el emulador de proceso de Azure. Dependiendo de la configuración del firewall, deberá permitir que el emulador a través del firewall.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Cómo implementar la aplicación de base para las aplicaciones de Web de servicio de aplicación de Azure mediante el uso de las secuencias de comandos de Windows PowerShell

Para ilustrar la [automatizar todo](automate-everything.md) patrón, la aplicación repararlo se suministra con secuencias de comandos que configuración un entorno de Azure e implementación el proyecto en el nuevo entorno. Las instrucciones siguientes explican cómo usar las secuencias de comandos.

Si desea ejecutar en Azure sin usar las colas y que realizó los cambios para que se ejecute localmente con colas, asegúrese de que establecer el valor de appSetting UseQueues a false antes de continuar con las instrucciones siguientes.

Estas instrucciones se supone ya ha descargado y ejecuta la solución repararlo localmente, y que tiene un Azure cuenta o tienen una suscripción de Azure que está autorizado para administrar.

1. Instalar el **Azure PowerShell** consola. Para obtener instrucciones, consulte [cómo instalar y configurar Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Esta consola personalizada está configurada para funcionar con su suscripción de Azure. El módulo de Azure está instalado en el *archivos de programa* directorio y se importa automáticamente en cada uso de la consola de PowerShell de Azure.

    Si prefiere trabajar en un programa de host diferente, como Windows PowerShell ISE, asegúrese de utilizar el [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) cmdlet para importar el módulo de Azure o use un comando del módulo de Azure para desencadenar la importación automática del módulo.
2. Inicie PowerShell de Azure con el **ejecutar como administrador** opción.
3. Ejecute el [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) cmdlet para establecer la directiva de ejecución de PowerShell de Azure en `RemoteSigned`. Escriba **Y** (para sí) para completar el cambio de directiva.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Esta configuración le permite ejecutar scripts locales que no están firmados digitalmente. (También puede establecer la directiva de ejecución en `Unrestricted`, lo que eliminaría la necesidad de que el paso de desbloqueo más adelante, pero no se recomienda por motivos de seguridad.)
4. Ejecute el `Add-AzureAccount` cmdlet PowerShell configurados con las credenciales de su cuenta.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Estas credenciales expiren tras un período de tiempo y tiene que volver a ejecutar la `Add-AzureAccount` cmdlet. Como este libro electrónico se está escribiendo, el límite de tiempo antes de que caduquen las credenciales es de 12 horas.
5. Si tiene varias suscripciones, use el cmdlet Select-AzureSubscription para especificar la suscripción que desea crear el entorno de prueba en.
6. Importar un certificado de administración para la misma suscripción de Azure mediante el `Get-AzurePublishSettingsFile` y `Import-AzurePublishSettingsFile` cmdlets. El primero de estos cmdlets descarga un archivo de certificado y, en la segunda se especifique la ubicación del archivo para importarlo. > [!IMPORTANT]
   > Guarde el archivo descargado en una ubicación segura o elimínelo cuando haya terminado con él, porque contiene un certificado que puede usarse para administrar los servicios de Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    El certificado se usa para una llamada de API de REST que detecta la dirección IP del equipo de desarrollo con el fin de establecer una regla de firewall en el servidor de base de datos SQL.
7. Ejecute el [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) cmdlet (alias son `cd`, `chdir`, y `sl`) para navegar hasta el directorio que contiene las secuencias de comandos. (Se encuentran en el *automatización* en la carpeta de solución repararlo.) Escriba la ruta de acceso entre comillas si alguno de los nombres de directorio contiene espacios. Por ejemplo, para navegar hasta el `c:\Sample Apps\FixIt\Automation` directorio puede escribir el comando siguiente:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Para permitir que Windows PowerShell que ejecute estas secuencias de comandos, utilice la [Unblock-File](https://go.microsoft.com/fwlink/p/?linkid=294021) cmdlet. (Las secuencias de comandos están bloqueadas porque se han descargado de Internet).

    > [!WARNING]
    > Seguridad: antes de ejecutar `Unblock-File` en cualquier secuencia de comandos o archivo ejecutable, abra el archivo en el Bloc de notas, examine los comandos y compruebe que no contienen ningún código malintencionado.

    Por ejemplo, el siguiente comando se ejecuta la `Unblock-File` cmdlet en todos los scripts en el directorio actual.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Para crear la aplicación web para la base (no hay colas de procesamiento) corregirlo aplicación, ejecute el script de creación del entorno.

    Requerido `Name` parámetro especifica el nombre de la base de datos y también se utiliza para la cuenta de almacenamiento que crea el script. El nombre debe ser único globalmente dentro del dominio azurewebsites.net. Si especifica un nombre que no es único, como Fixit o prueba (o incluso como en el ejemplo, fixitdemo), el `New-AzureWebsite` cmdlet produce un Error interno que indica un conflicto. La secuencia de comandos convierte el nombre en minúsculas para cumplir los requisitos de nombre para las aplicaciones web, las cuentas de almacenamiento y bases de datos.

    Requerido `SqlDatabasePassword` parámetro especifica la contraseña de la cuenta de administrador que se crea para la base de datos SQL. No incluya caracteres XML especiales en la contraseña (&amp; &lt; &gt; ;). Se trata de una limitación de la forma en que se escribieron los scripts, no una limitación de Azure.

    Por ejemplo, si desea crear una aplicación web denominada "fixitdemo" y use una contraseña de administrador de SQL Server de "Passw0rd1", puede escribir el comando siguiente:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    El nombre debe ser único en el dominio azurewebsites.net y la contraseña debe cumplir los requisitos de base de datos SQL de complejidad de contraseña. (El ejemplo Passw0rd1 cumple los requisitos).

    Tenga en cuenta que el comando comienza con ". \". Para ayudar a evitar malintencionada ejecución de secuencias de comandos, Windows PowerShell requiere que proporcione la ruta de acceso completa al archivo de script al ejecutar una secuencia de comandos. Puede utilizar un punto para indicar el directorio actual (".\") o bien, proporcione la ruta de acceso completa, como:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Para obtener más información acerca de la secuencia de comandos, utilice la `Get-Help` cmdlet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Puede usar el `Detailed`, `Full`, `Parameters`, y `Examples` parámetros del cmdlet Get-Help para filtrar la Ayuda que se devuelve.

    Si el script genera un error o errores, como "New-AzureWebsite: llamar a Set-AzureSubscription y Select-AzureSubscription en primer lugar," se podría no haber completado la configuración de PowerShell de Azure.

    Una vez finalizada la secuencia de comandos, puede usar el Portal de administración de Azure para ver los recursos que se crearon, tal y como se muestra en el [automatizar todo](automate-everything.md) capítulo.
10. Para implementar el proyecto de FixIt en el nuevo entorno de Azure, use la *AzureWebsite.ps1* secuencia de comandos. Por ejemplo:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Cuando se realice la implementación, el explorador se abre con corregir se ejecuta en Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Solución de problemas de las secuencias de comandos de Windows PowerShell

Los errores más comunes que se ha encontrado al ejecutar estos scripts están relacionados con los permisos. Asegúrese de que `Add-AzureAccount` y `Import-AzurePublishSettingsFile` se efectuó correctamente y que usó para la misma suscripción de Azure. Incluso si `Add-AzureAccount` era correcta es posible que deba volver a ejecutarla. Los permisos agregados por `Add-AzureAccount` caducan en 12 horas.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Referencia a objeto no establecida como instancia de un objeto.

Si el script devuelve errores, como "Referencia a objeto no establecida a una instancia de un objeto," lo que significa que Windows PowerShell no se puede encontrar un objeto al proceso (ésta es una excepción de referencia nula), ejecuta el `Add-AzureAccount` cmdlet e inténtelo de nuevo la secuencia de comandos.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>Error interno: El servidor detectó un error interno.

El `New-AzureWebsite` cmdlet devuelve un error interno cuando el nombre no es único en el dominio azurewebsites.net. Para resolver el error, utilice un valor diferente para el nombre, que se encuentra en el parámetro Name de *AzureWebsiteEnv.ps1 nuevo*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Reiniciar la secuencia de comandos

Si tiene que reiniciar el *AzureWebsiteEnv.ps1 New* script porque se produjo un error antes de que imprima el mensaje "La secuencia de comandos es completa", puede eliminar los recursos que el script creado antes de que se ha detenido. Por ejemplo, si la secuencia de comandos ha creado la aplicación web de ContosoFixItDemo y volver a ejecutar la secuencia de comandos con el mismo nombre, se producirá un error en la secuencia de comandos porque el nombre está en uso.

Para determinar qué recursos de la secuencia de comandos creada antes de que detenga, use los siguientes cmdlets:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Para ejecutar este cmdlet, canalice el nombre del servidor de base de datos para `Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Para eliminar estos recursos, use los siguientes comandos. Tenga en cuenta que si elimina el servidor de base de datos, se eliminan automáticamente las bases de datos asociados con el servidor.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Cómo implementar la aplicación con cola de procesamiento para aplicaciones de Web del servicio de aplicación de Azure y un servicio de nube de Azure

Para habilitar las colas, realice el siguiente cambio en el archivo MyFixIt\Web.config. En `appSettings`, cambie el valor de `UseQueues` en "true": 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

A continuación, implementar la aplicación de MVC en una aplicación web en el servicio de aplicación de Azure, tal como se describe [anterior](#deploybase).

A continuación, cree un nuevo servicio de nube de Azure. Los scripts incluidos en la aplicación repararlo no crean ni implementar el servicio de nube, lo que debe usar el portal de Azure para esto. En el portal, haga clic en **New** -- **proceso** : **servicio de nube** -- **creación rápida**y, a continuación, escriba una dirección URL y una ubicación de centro de datos. Use el mismo centro de datos en la que implementó la aplicación web.

![](the-fix-it-sample-application/_static/image1.png)

Antes de implementar el servicio de nube, debe actualizar algunos de los archivos de configuración.

En MyFixIt.WorkerRoler\app.config, en `connectionStrings`, sustituya el valor de la `appdb` cadena de conexión con la cadena de conexión real de la base de datos de SQL. Puede obtener la cadena de conexión desde el portal. En el portal, haga clic en **bases de datos SQL** - **appdb** - **cadenas de conexión de base de datos de vista SQL para ADO. NET, ODBC, PHP y JDBC**. Copie la cadena de conexión de ADO.NET y pegue el valor en el archivo app.config. Reemplace "{la\_contraseña\_aquí}" con la contraseña de la base de datos. (Suponiendo que utilizan las secuencias de comandos para implementar la aplicación MVC, especifica la contraseña de la base de datos en el `SqlDatabasePassword` parámetro de secuencia de comandos.)

El resultado debería ser similar al siguiente:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

En el mismo archivo de MyFixIt.WorkerRoler\app.config en `appSettings`, reemplace los dos valores de marcador de posición para la cuenta de almacenamiento de Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Puede obtener la clave de acceso desde el portal. Vea [cómo administrar las cuentas de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

En MyFixItCloudService\ServiceConfiguration.Cloud.cscfg, reemplace los mismos valores de dos marcadores de posición para la cuenta de almacenamiento de Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Ahora está listo para implementar el servicio de nube. En el Explorador de soluciones, haga clic en el proyecto MyFixItCloudService y seleccione **publicar**. Para obtener más información, vea "[implementar la aplicación en Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", que se encuentra en la parte 2 de [este tutorial](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Anterior](more-patterns-and-guidance.md)
