---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Apéndice: Corrección de la aplicación de ejemplo (creación de aplicaciones de nube reales con Azure) | Microsoft Docs'
author: MikeWasson
description: Building Real World Cloud Apps with e-book de Azure se basa en una presentación desarrollada por Scott Guthrie. Explican el 13 de patrones y prácticas que puede...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: de3c8ea29f2c271136f58d8165bb92f4ab28ce83
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912831"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Apéndice: Corrección de la aplicación de ejemplo (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Descargue la corrección el proyecto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> El **Building Real World Cloud Apps with Azure** eBook se basa en una presentación desarrollada por Scott Guthrie. Se explican el 13 patrones y prácticas que pueden ayudarle a tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).

Este apéndice a Building Real World Cloud Apps with e-book de Azure contiene las siguientes secciones que proporcionan información adicional sobre la Fix It aplicación de ejemplo que puede descargar:

- [Problemas conocidos](#knownissues)
- [Procedimientos recomendados](#bestpractices)
- [Cómo ejecutar la aplicación desde Visual Studio en el equipo local](#run-in-vs)
- [Cómo implementar la aplicación de base en Azure App Service Web Apps mediante el uso de los scripts de Windows PowerShell](#deploybase)
- [Solución de problemas de los scripts de Windows PowerShell](#troubleshooting)
- [Cómo implementar la aplicación con cola de procesamiento para Azure App Service Web Apps y Azure Cloud Services](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Problemas conocidos

La aplicación Fix It originalmente se desarrolló con el fin de ilustrar tan simple como sea posible algunos de los patrones presentan en este libro electrónico. Sin embargo, puesto que es el libro electrónico sobre la creación de aplicaciones del mundo real, se somete el código Fix It a una revisión y el proceso de prueba similar a lo que hacemos de versiones de software. Se ha encontrado un número de problemas y al igual que con cualquier aplicación del mundo real, algunos de ellos se ha corregido y algunos de ellos se aplaza hasta una versión posterior.

En la lista siguiente incluye los problemas que deben solucionarse en una aplicación de producción, pero por una razón u otra que decidimos no dirección en la versión inicial de la aplicación de ejemplo Fix It.

### <a name="security"></a>Seguridad

- Asegúrese de que no se puede asignar una tarea a un propietario que no existe.
- Asegúrese de que solo pueden ver y modificar las tareas que ha creado o que tiene asignados.
- Usar HTTPS para las páginas de inicio de sesión y cookies de autenticación.
- Especifique un límite de tiempo para las cookies de autenticación.

### <a name="input-validation"></a>Validación de entrada

En general, una aplicación de producción haría más validación de entrada que la aplicación Fix It. Por ejemplo, el tamaño de imagen / permitido para la carga se debe limitar el tamaño de archivo de imagen.

### <a name="administrator-functionality"></a>Funcionalidad de administrador

Un administrador debe poder cambiar la propiedad en las tareas existentes. Por ejemplo, el creador de una tarea podría dejar la empresa, dejando a nadie con la autoridad para mantener la tarea a menos que se habilite el acceso administrativo.

### <a name="queue-message-processing"></a>Procesamiento de mensajes de cola

Mensaje de cola de procesamiento en la aplicación Fix It se diseñó para ser simple para ilustrar el patrón de trabajo centrado en la cola con una cantidad mínima de código. Este código simple no sería adecuado para una aplicación de producción real.

- El código no garantiza que cada mensaje de cola se procesará al menos una vez. Cuando reciba un mensaje de la cola, hay un período de tiempo de espera, durante el cual el mensaje es invisible para otros agentes de escucha de cola. Si el tiempo de espera expira antes de que el mensaje se elimina, el mensaje vuelve a visible. Por lo tanto, si una instancia de rol de trabajo dedica mucho tiempo en procesar un mensaje, es teóricamente posible para el mismo mensaje que se procese dos veces, lo que resulta en una tarea en la base de datos duplicada. Para obtener más información sobre este problema, consulte [utilizando colas de Azure Storage](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- La lógica de sondeo de la cola podría ser más rentable, por lotes de recuperación de mensajes. Cada vez que se llama a [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), hay un costo de transacción. En su lugar, puede llamar a [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (tenga en cuenta el plural'), que obtiene varios mensajes en una sola transacción. Los costos de transacción para las colas de almacenamiento de Azure son muy bajos, por lo que el impacto en los costos no es relevante en la mayoría de los escenarios.
- El bucle cerrado en el código de procesamiento de mensajes de cola hace que la afinidad de CPU, que no utiliza eficazmente las máquinas virtuales de varios núcleos. Un mejor diseño usaría el paralelismo de tareas para ejecutar varias tareas asincrónicas en paralelo.
- Procesamiento de mensajes de cola tiene el control de excepciones sólo rudimentario. Por ejemplo, el código no controla [mensajes dudosos](https://msdn.microsoft.com/library/ms789028.aspx). (Cuando el procesamiento de mensajes produce una excepción, tendrá que registrar el error y eliminar el mensaje, o el rol de trabajo intentará procesarlo otra vez y el bucle continuará indefinidamente.)

### <a name="sql-queries-are-unbounded"></a>Las consultas SQL son sin enlazar

Actual Fix It código no coloca ningún límite en las consultas para las páginas de índice podrían devolver el número de filas. Si se escribe un gran volumen de tareas en la base de datos, el tamaño de las listas resultantes recibe podría provocar problemas de rendimiento. La solución consiste en implementar la paginación. Para obtener un ejemplo, vea [ordenación, filtrado y paginación con Entity Framework en una aplicación ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Modelos de vista recomendados

La aplicación Fix It utiliza la clase de entidad FixItTask para pasar información entre el controlador y la vista. Una práctica recomendada es usar los modelos de vista. El modelo de dominio (por ejemplo, la clase de entidad FixItTask) está diseñado en torno a lo que se necesita para la persistencia de datos, aunque se puede diseñar un modelo de vista para la presentación de datos. Para obtener más información, consulte [12 prácticas recomendadas de ASP.NET MVC](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Blob de imágenes seguras recomendada

Las tiendas de aplicaciones Fix It cargan imágenes como pública, lo que significa que cualquier persona que encuentre la dirección URL puede tener acceso a las imágenes. Proteger las imágenes en lugar de público.

### <a name="no-powershell-automation-scripts-for-queues"></a>No hay scripts de automatización de PowerShell para las colas

Scripts de automatización de PowerShell de ejemplo se escribieron solo para la versión base de corregir lo que se ejecuta completamente en Azure App Service Web Apps. Las secuencias de comandos que no hemos proporcionado para configurar e implementar en la aplicación web más el entorno de servicio en la nube necesaria para el procesamiento de la cola.

### <a name="special-handling-for-html-codes-in-user-input"></a>Tratamiento especial para los códigos HTML en la entrada del usuario

ASP.NET evita automáticamente muchas formas en que los usuarios malintencionados podrían intentar ataques de scripts entre sitios mediante la especificación de secuencia de comandos en los cuadros de texto de entrada de usuario. Y MVC `DisplayFor` auxiliar que se usa para mostrar tarea títulos y notas automáticamente valores codifica en HTML que envía al explorador. Pero en una aplicación de producción es posible que desee tomar medidas adicionales. Para obtener más información, consulte [validación de solicitudes en ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Procedimientos recomendados

Siguientes son algunos problemas que se corrigieron después de que se detectan en la revisión de código y las pruebas de la versión original de la aplicación Fix It. Algunos causados por el codificador original no es una práctica recomendada determinada, tenga en cuenta algunos simplemente porque el código se escribió rápidamente y no se ha diseñado de versiones de software. Nos estamos enumerar aquí los problemas en caso de que hay algo que hemos aprendido de esta revisión y pruebas pueden resultar útil para otros usuarios que también está desarrollando aplicaciones web.

### <a name="dispose-the-database-repository"></a>Eliminar el repositorio de la base de datos

El `FixItTaskRepository` clase debe desechar el Entity Framework `DbContext` instancia. Lo hicimos implementando `IDisposable` en el `FixItTaskRepository` clase:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Tenga en cuenta que AutoFac eliminará automáticamente el `FixItTaskRepository` , instancia, es necesario desecharlo de forma explícita.

Otra opción consiste en quitar el `DbContext` variable miembro de `FixItTaskRepository`y en su lugar, cree una variable local `DbContext` variable dentro de cada método del repositorio, dentro de un `using` instrucción. Por ejemplo:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registrar singletons como tales con DI

Desde una única instancia de la `PhotoService` clase y `Logger` clase es necesario, estas clases deben ser [registrado como instancias únicas de inserción de dependencias](https://code.google.com/p/autofac/wiki/InstanceScope) en *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Seguridad: No mostrar detalles del error a los usuarios

La original Fix It aplicación no tiene una página de error genérico y permitiré que todas las burbujas excepciones hasta la interfaz de usuario, por lo que podrían dar lugar algunas excepciones, como errores de conexión de base de datos en un seguimiento de pila completo que se muestra en el explorador. Información detallada del error a veces puede facilitar a los ataques de usuarios malintencionados. La solución consiste en registrar los detalles de excepción y mostrar una página de error al usuario que no incluya los detalles del error. La aplicación Fix It ya estaba iniciando y, con el fin de mostrar una página de error, hemos agregado `<customErrors mode=On>` en el archivo Web.config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Esto hace de forma predeterminada *Views\Shared\Error.cshtml* que se mostrará para los errores. Puede personalizar *Error.cshtml* o crear su propia vista de página de error y agregue un `defaultRedirect` atributo. También puede especificar distintas páginas de error para errores concretos.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Seguridad: Permitir solo una tarea para que se puede editar su creador

La página de índice del panel solo muestra las tareas creadas por el usuario ha iniciado sesión, pero un usuario malintencionado podría crear una dirección URL con un identificador para la tarea de otro usuario. Se ha agregado código en *DashboardController.cs* para devolver un error 404 en ese caso:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>No trague agua excepciones

La original Fix It aplicación simplemente devuelve null después de iniciar una excepción que dan como resultado de una consulta SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Esto haría que parezca al usuario como si la consulta se realizó correctamente, pero simplemente no devolvió ninguna fila. Solución consiste en volver a producir la excepción después de detectar y registro:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Detectar todas las excepciones en los roles de trabajo

Cualquier excepción no controlada en un rol de trabajo hará que la máquina virtual para que se recicle, por lo que desee ajustar todo en un bloque try-catch y controlar todas las excepciones.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Especificar la longitud de las propiedades de cadena en las clases de entidad

Con el fin de mostrar código simple, la versión original de la aplicación Fix It no especificó las longitudes de los campos de la entidad FixItTask y como resultado se definieron como varchar (max) en la base de datos. Como resultado, la interfaz de usuario aceptaban casi cualquier cantidad de entrada. En la página web y el tamaño de columna en la base de datos de entrada especificando las longitudes se establecen los límites que se aplican tanto al usuario:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Marcar los miembros privados como de solo lectura al que no se esperaban que cambie

Por ejemplo, en el `DashboardController` una instancia de la clase `FixItTaskRepository` se crea y no se espera que cambiar, por lo que se define como [readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Use la lista. Any() en lugar de la lista. Count() &gt; 0

Si se le preocupa si uno o varios elementos en una lista de ajustarse a los criterios especificados, utilice el [cualquier](https://msdn.microsoft.com/library/bb534972.aspx) método, porque devuelve tan pronto como se encuentra un elemento que se cumplen los criterios, mientras que el `Count` método siempre tiene que recorrer en iteración todos los elementos. El panel *Index.cshtml* archivo tenía originalmente este código:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Se cambió a esto:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generar direcciones URL en las vistas MVC usa aplicaciones auxiliares MVC

Para el **crear un Fix It** botón en la página principal, la Fix It aplicación dura codificada de un elemento delimitador:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Para obtener vínculos de acción/vista similar al siguiente es mejor usar la [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) aplicación auxiliar HTML, por ejemplo:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Use Task.Delay en lugar de Thread.Sleep en función de trabajador

Coloca la plantilla de proyecto nuevo `Thread.Sleep` en el ejemplo de código para un rol de trabajo, pero lo que hace que el subproceso entre en suspensión puede hacer que el grupo de subprocesos generar más subprocesos innecesarios. Se puede evitar mediante [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) en su lugar.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Evite async void

Si un método asincrónico no tiene que devolver un valor, devuelve un `Task` tipo en lugar de `void`.

Este ejemplo es de la `FixItQueueManager` clase:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Debe usar `async void` sólo para controladores de eventos de nivel superior. Si define un método como `async void`, el llamador no **await** el método o detectar las excepciones que produce el método. Para obtener más información, consulte [prácticas recomendadas de programación asincrónica](https://msdn.microsoft.com/magazine/jj991977.aspx).

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Usar un token de cancelación para interrumpir el bucle de rol de trabajo

Normalmente, el **ejecutar** método en un rol de trabajo contiene un bucle infinito. Cuando se está deteniendo el rol de trabajo, el [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) se llama al método. Debe utilizar este método para cancelar el trabajo que se realiza dentro de la **ejecutar** método y salir correctamente. En caso contrario, es posible que se finaliza el proceso en el medio de una operación.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Rechazar el procedimiento de examen de MIME automático

En algunos casos, Internet Explorer informa de un tipo MIME diferente que el tipo especificado por el servidor web. Por ejemplo, si Internet Explorer encuentra HTML contenido en un archivo que se entregue con el encabezado de respuesta HTTP Content-Type: texto/sin formato, Internet Explorer determina que se debe presentar el contenido como HTML. Desafortunadamente, este "rastreo de MIME" también puede provocar problemas de seguridad para servidores que hospedan contenido no seguro. Para combatir este problema, Internet Explorer 8 ha realizado una serie de cambios al código de la determinación de tipo MIME y permite a los desarrolladores de aplicaciones [rechazar el rastreo de MIME](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). El código siguiente se agregó a la *Web.config* archivo.

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Habilitar la unión y minificación

Cuando Visual Studio crea un nuevo proyecto web, unión y minificación de los archivos de JavaScript no está habilitado de forma predeterminada. Se agregó una línea de código en BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Establecer un tiempo de espera de expiración para las cookies de autenticación

De forma predeterminada, las cookies de autenticación expiren en dos semanas. Un tiempo más corto es más seguro. Puede cambiar esta configuración en *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Cómo ejecutar la aplicación desde Visual Studio en el equipo local

Hay dos maneras de ejecutar la aplicación Fix It:

- Ejecute la aplicación básica que escribe las nuevas tareas directamente en la base de datos SQL.
- Ejecute la aplicación utiliza una cola, además de un servicio back-end para crear tareas. El patrón de cola se describe en el capítulo [patrón centrado en colas de trabajo](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Ejecute la aplicación básica

1. Instalar [de Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).
2. Instalar el [Azure SDK para .NET para Visual Studio](https://azure.microsoft.com/downloads/).
3. Descargue el archivo .zip desde el [MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. En el Explorador de archivos, haga clic en el archivo .zip y propiedades, haga clic en la ventana Propiedades, haga clic en Desbloquear.
5. Descomprima el archivo.
6. Haga doble clic en el archivo .sln para iniciar Visual Studio.
7. Desde el **herramientas** menú, haga clic en **Administrador de paquetes de NuGet**, a continuación, **Package Manager Console**.
8. En la consola de administrador de paquetes (PMC), haga clic en restaurar.
9. Salga de Visual Studio.
10. Iniciar el [emulador de Azure storage](/azure/storage/common/storage-use-emulator).
11. Reinicie Visual Studio, abra el archivo de solución que se ha cerrado en el paso anterior.
12. Asegúrese de que el proyecto FixIt está establecido como proyecto de inicio y, a continuación, presione CTRL + F5 para ejecutar el proyecto.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Ejecute la aplicación con el procesamiento de cola

1. Siga las instrucciones para [ejecutar la aplicación básica](#runbase)y, a continuación, cierre el explorador y cierre Visual Studio.
2. Inicie Visual Studio con privilegios de administrador. (Que va a usar el emulador de proceso de Azure, y requiere privilegios de administrador).
3. En la aplicación *Web.config* de archivos en el *MyFixIt* proyecto (proyecto web), cambie el valor de `appSettings/UseQueues` en "true":

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Si el [emulador de Azure storage](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) no está todavía se está ejecutando, inícielo de nuevo.
5. Ejecutar el proyecto web de FixIt y el proyecto MyFixItCloudService simultáneamente.

    Con Visual Studio:

   1. Presione **F5** para ejecutar el proyecto FixIt.
   2. En **el Explorador de soluciones**, haga clic en el proyecto MyFixItCloudService y, a continuación, haga clic en **depurar** > **Iniciar nueva instancia**.

    Uso de Visual Studio 2013 Express para Web:

   3. En el Explorador de soluciones, haga clic en la solución de FixIt y seleccione **propiedades**.
   4. Seleccione **varios proyectos de inicio**.
   5. En el **acción** seleccione de la lista desplegable bajo MyFixIt y MyFixItCloudService, **iniciar**.
   6. Haga clic en **Aceptar**.
   7. Presione **F5** para ejecutar ambos proyectos.

      Al ejecutar el proyecto MyFixItCloudService, Visual Studio inicia el emulador de proceso de Azure. Según la configuración de firewall, debe permitir que el emulador a través del firewall.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Cómo implementar la aplicación de base en Azure App Service Web Apps mediante el uso de los scripts de Windows PowerShell

Para ilustrar la [automatizar todo](automate-everything.md) patrón, la aplicación Fix It se suministra con secuencias de comandos que configuración un entorno de Azure e implementación el proyecto al nuevo entorno. Las instrucciones siguientes explican cómo usar las secuencias de comandos.

Si desea ejecutar en Azure sin usar las colas y que realizó los cambios para ejecutarse localmente con colas, asegúrese de que establecer el valor de appSetting UseQueues volver en false antes de continuar con las instrucciones siguientes.

Estas instrucciones se supone ya ha descargado y ejecuta la solución Fix It localmente, y que tiene una instancia de Azure de la cuenta o tienen una suscripción de Azure que están autorizados a administrar.

1. Instalar el **Azure PowerShell** consola. Para obtener instrucciones, consulte [cómo instalar y configurar Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Esta consola personalizada está configurada para funcionar con su suscripción de Azure. El módulo de Azure está instalado en el *archivos de programa* Active y se importa automáticamente en todos los usos de la consola de PowerShell de Azure.

    Si prefiere trabajar en un programa de host diferente, como Windows PowerShell ISE, asegúrese de usar el [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) cmdlet para importar el módulo de Azure o use un comando en el módulo de Azure para desencadenar la importación automática del módulo.
2. Inicie Azure PowerShell con la **ejecutar como administrador** opción.
3. Ejecute el [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) cmdlet para establecer la directiva de ejecución de PowerShell de Azure en `RemoteSigned`. Escriba **Y** (para sí) para completar el cambio de directiva.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Esta configuración le permite ejecutar scripts locales que no están firmados digitalmente. (También se puede establecer la directiva de ejecución como `Unrestricted`, lo que eliminaría la necesidad de la etapa de desbloqueo más adelante, pero esto no se recomienda por motivos de seguridad.)
4. Ejecute el `Add-AzureAccount` cmdlet para configurar PowerShell con credenciales de su cuenta.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Estas credenciales expiran tras un período de tiempo y tendrá que volver a ejecutar el `Add-AzureAccount` cmdlet. Tal como se escribe este libro electrónico, el límite de tiempo antes de que caduquen las credenciales es de 12 horas.
5. Si tiene varias suscripciones, use el cmdlet Select-AzureSubscription para especificar la suscripción que desea crear en el entorno de prueba.
6. Importar un certificado de administración para la misma suscripción de Azure mediante el `Get-AzurePublishSettingsFile` y `Import-AzurePublishSettingsFile` cmdlets. El primero de estos cmdlets descarga un archivo de certificado y, en la segunda se especifique la ubicación de ese archivo para importarlo. > [!IMPORTANT]
   > Conservar el archivo descargado en una ubicación segura o elimínelo cuando haya terminado con él, porque contiene un certificado que puede usarse para administrar los servicios de Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    El certificado se usa para una llamada de API de REST que detecta la dirección IP del equipo de desarrollo con el fin de establecer una regla de firewall en el servidor de base de datos SQL.
7. Ejecute el [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) cmdlet (alias son `cd`, `chdir`, y `sl`) para navegar hasta el directorio que contiene las secuencias de comandos. (Se encuentran en el *automatización* en la carpeta de solución repararlo.) Escriba la ruta de acceso entre comillas si alguno de los nombres de directorio contiene espacios. Por ejemplo, para navegar hasta el `c:\Sample Apps\FixIt\Automation` directorio podría escribir el comando siguiente:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Para permitir que Windows PowerShell ejecutar estas secuencias de comandos, use el [Unblock-File](https://go.microsoft.com/fwlink/p/?linkid=294021) cmdlet. (Los scripts se bloquean debido a se han descargado de Internet).

    > [!WARNING]
    > Security - antes de ejecutar `Unblock-File` en secuencias de comandos o archivo ejecutable, abra el archivo en el Bloc de notas, examine los comandos y compruebe que no contienen ningún código malintencionado.

    Por ejemplo, el siguiente comando ejecuta el `Unblock-File` cmdlet en todos los scripts en el directorio actual.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Para crear la aplicación web para la base (no hay colas de procesamiento) corregirlo app, ejecute el script de creación del entorno.

    Necesario `Name` parámetro especifica el nombre de la base de datos y también se utiliza para la cuenta de almacenamiento que crea el script. El nombre debe ser único globalmente en el dominio azurewebsites.net. Si especifica un nombre que no es único, como la corrección o de prueba (o incluso como se muestra en el ejemplo, fixitdemo), el `New-AzureWebsite` cmdlet produce un Error interno que informa sobre un conflicto. La secuencia de comandos convierte el nombre en minúsculas para cumplir los requisitos de nombre para las aplicaciones web, las cuentas de almacenamiento y bases de datos.

    Necesario `SqlDatabasePassword` parámetro especifica la contraseña de la cuenta de administrador que se crearán para la base de datos SQL. No incluya caracteres XML especiales en la contraseña (&amp; &lt; &gt; ;). Esta es una limitación de la forma en que los scripts se escribieron, no una limitación de Azure.

    Por ejemplo, si desea crear una aplicación web denominada "fixitdemo" y usar una contraseña de administrador de SQL Server de "Passw0rd1", puede escribir el siguiente comando:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    El nombre debe ser único en el dominio azurewebsites.net y la contraseña debe cumplir los requisitos de base de datos SQL para complejidad de contraseñas. (El ejemplo Passw0rd1 cumple los requisitos).

    Tenga en cuenta que el comando comienza con ". \". Para ayudar a evitar la ejecución de secuencias de comandos malintencionada, Windows PowerShell requiere que proporcione la ruta de acceso completa al archivo de script al ejecutar una secuencia de comandos. Puede usar un punto para indicar el directorio actual (".\") o bien, proporcione la ruta de acceso completa, por ejemplo:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Para obtener más información acerca de la secuencia de comandos, utilice el `Get-Help` cmdlet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Puede usar el `Detailed`, `Full`, `Parameters`, y `Examples` parámetros del cmdlet Get-Help para filtrar la Ayuda que se devuelve.

    Si el script genera un error o errores, como "New-AzureWebsite: llamar a Set-AzureSubscription y Select-AzureSubscription primero," es posible que no ha completado la configuración de Azure PowerShell.

    Una vez finalizada la secuencia de comandos, puede usar el Portal de administración de Azure para ver los recursos que se crearon, como se muestra en el [automatizar todo](automate-everything.md) capítulo.
10. Para implementar el proyecto de corrección para el nuevo entorno de Azure, use el *AzureWebsite.ps1* secuencia de comandos. Por ejemplo:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Cuando se realiza la implementación, el explorador se abre con la corrección se ejecuta en Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Solución de problemas de los scripts de Windows PowerShell

Los errores más comunes encontrados al ejecutar estos scripts están relacionados con los permisos. Asegúrese de que `Add-AzureAccount` y `Import-AzurePublishSettingsFile` se realizaron correctamente y que usó para la misma suscripción de Azure. Incluso si `Add-AzureAccount` fue correcta podría tener para volver a ejecutarlo. Los permisos agregados por `Add-AzureAccount` expiren en 12 horas.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Referencia a objeto no establecida como instancia de un objeto.

Si el script devuelve errores, como "Referencia a objeto no establecida como instancia de un objeto," lo que significa que Windows PowerShell no puede encontrar un objeto al proceso (Esto es una excepción de referencia nula), ejecute el `Add-AzureAccount` cmdlet e inténtelo de nuevo la secuencia de comandos.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>Error interno: El servidor encontró un error interno.

El `New-AzureWebsite` cmdlet devuelve un error interno cuando el nombre no es único en el dominio azurewebsites.net. Para resolver el error, use un valor diferente para el nombre, que se encuentra en el parámetro Name del *New AzureWebsiteEnv.ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Reiniciar la secuencia de comandos

Si tiene que reiniciar el *New AzureWebsiteEnv.ps1* script porque se produjo un error antes de que imprime el mensaje "La secuencia de comandos está completa", es posible que desea eliminar los recursos que el script creado antes de que se ha detenido. Por ejemplo, si la secuencia de comandos ya ha creado la aplicación web de ContosoFixItDemo y volver a ejecutar el script con el mismo nombre, se producirá un error en la secuencia de comandos porque el nombre está en uso.

Para determinar qué recursos de la secuencia de comandos creada antes de que se detenga, use los siguientes cmdlets:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Para ejecutar este cmdlet, canalice el nombre del servidor de base de datos a `Get-AzureSqlDatabase`:   `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Para eliminar estos recursos, use los siguientes comandos. Tenga en cuenta que si elimina el servidor de base de datos, elimina las bases de datos asociadas con el servidor.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Cómo implementar la aplicación con cola de procesamiento para Azure App Service Web Apps y Azure Cloud Services

Para habilitar las colas, realice el siguiente cambio en el archivo MyFixIt\Web.config. En `appSettings`, cambie el valor de `UseQueues` en "true":

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

A continuación, implemente la aplicación de MVC en una aplicación web en Azure App Service, como se describe [anteriores](#deploybase).

A continuación, cree un nuevo servicio de nube de Azure. Los scripts incluidos con la aplicación Fix It no crear ni implementar el servicio en la nube, lo que debe usar el portal de Azure para esto. En el portal, haga clic en **New** -- **proceso** – **servicio en la nube** -- **creación rápida**y, a continuación, escriba una dirección URL y una ubicación de centro de datos. Use el mismo centro de datos en la que implementó la aplicación web.

![](the-fix-it-sample-application/_static/image1.png)

Antes de implementar el servicio en la nube, deberá actualizar algunos de los archivos de configuración.

En MyFixIt.WorkerRoler\app.config, bajo `connectionStrings`, reemplace el valor de la `appdb` cadena de conexión con la cadena de conexión real para la base de datos de SQL. Puede obtener la cadena de conexión desde el portal. En el portal, haga clic en **bases de datos SQL** - **appdb** - **cadenas de conexión de base de datos de vista SQL para ADO. NET, ODBC, PHP y JDBC**. Copie la cadena de conexión de ADO.NET y pegue el valor en el archivo app.config. Reemplace "{su\_contraseña\_aquí}" con la contraseña de la base de datos. (Suponiendo que utiliza las secuencias de comandos para implementar la aplicación MVC, especificó la contraseña de la base de datos en el `SqlDatabasePassword` parámetro de script.)

El resultado debería ser similar al siguiente:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

En el mismo archivo MyFixIt.WorkerRoler\app.config, bajo `appSettings`, reemplace los dos valores de marcador de posición para la cuenta de almacenamiento de Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Puede obtener la clave de acceso desde el portal. Consulte [cómo administrar las cuentas de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

En MyFixItCloudService\ServiceConfiguration.Cloud.cscfg, reemplace los mismos valores de dos marcadores de posición para la cuenta de almacenamiento de Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Ahora está listo para implementar el servicio en la nube. En el Explorador de soluciones, haga clic en el proyecto MyFixItCloudService y seleccione **publicar**. Para obtener más información, vea "[implementar la aplicación en Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", que es la parte 2 de [este tutorial](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Anterior](more-patterns-and-guidance.md)
