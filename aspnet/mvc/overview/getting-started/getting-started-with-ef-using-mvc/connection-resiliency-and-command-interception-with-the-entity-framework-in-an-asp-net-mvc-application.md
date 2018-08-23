---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: Resistencia de conexión e intercepción de comandos con Entity Framework en una aplicación ASP.NET MVC | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 5 con Entity Framework 6 Code First y Visual Studio...
ms.author: riande
ms.date: 01/13/2015
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9b326ec22fc70a8c1746c5cd2c302c7f04fa8d3e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836337"
---
<a name="connection-resiliency-and-command-interception-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Resistencia de conexión e intercepción de comandos con Entity Framework en una aplicación ASP.NET MVC
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [descargar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 5 con Entity Framework 6 Code First y Visual Studio 2013. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Hasta ahora la aplicación se ejecutaba localmente en IIS Express en el equipo de desarrollo. Para que una aplicación real disponible para otras personas a través de Internet, tendrá que implementarlo en un proveedor de hospedaje web, y se debe implementar la base de datos a un servidor de base de datos.

En este tutorial obtendrá información sobre cómo usar dos características de Entity Framework 6 que son especialmente valiosas al que va a implementar en el entorno de nube: resistencia de conexión (reintentos automáticos de errores transitorios) e intercepción de comandos (capturar todas las consultas SQL Enviar a la base de datos con el fin de iniciar sesión o cambiarlos).

Este tutorial de intercepción de resistencia y el comando de conexión es opcional. Si omite este tutorial, algunos ajustes menores deberá realizarse en los tutoriales posteriores.

## <a name="enable-connection-resiliency"></a>Habilitar la resistencia de conexión

Al implementar la aplicación en Windows Azure, va a implementar la base de datos en Windows Azure SQL Database, un servicio de base de datos en la nube. Errores de conexión transitorios son normalmente más frecuentes cuando se conecta a un servicio de base de datos en la nube que cuando el servidor web y el servidor de base de datos están conectados directamente entre sí en el mismo centro de datos. Incluso si un servidor web de nube y un servicio de base de datos en la nube se hospedan en el mismo centro de datos, hay más conexiones de red entre ellos que puede tener problemas, como los equilibradores de carga.

También un servicio en la nube normalmente se comparte con otros usuarios, lo que significa que su capacidad de respuesta puede verse afectado por ellos. Y el acceso a la base de datos podría estar sujeto a limitación. Limitación significa que el servicio de base de datos inicia excepciones cuando se intenta tener acceso a él más frecuentemente que está permitido en su contrato de nivel de servicio (SLA).

La mayoría o muchas problemas de conexión al que está accediendo a un servicio de nube son transitorios, es decir, en el resuelven por sí mismo se encuentre en un breve período de tiempo. Por lo que cuando se intente una operación de base de datos y obtener un tipo de error que suele ser transitorio, podría intentar la operación después de una breve espera y la operación podrían ser correcta. Puede proporcionar una experiencia mucho mejor para los usuarios si controlar errores transitorios, automáticamente intentándolo de nuevo, haciendo que la mayoría de ellos sea invisible para el cliente. La característica de resistencia de conexión de Entity Framework 6 automatiza que error del proceso de volver a intentar las consultas SQL.

La característica de resistencia de conexión debe configurarse correctamente para un servicio de base de datos determinada:

- También debe conocer las excepciones que suelen ser transitorio. Desea volver a intentar errores causados por una pérdida temporal de conectividad de red, no de los errores causados por errores de programa, por ejemplo.
- Tiene que esperar una cantidad apropiada de tiempo entre reintentos de una operación con errores. Puede esperar más tiempo entre reintentos para un proceso por lotes que si lo hace para una página web en línea donde un usuario está esperando una respuesta.
- Tiene que volver a intentar un número adecuado de veces antes de desistir. Es posible que desee volver a intentarlo varias veces en un proceso por lotes que lo haría en una aplicación en línea.

Puede configurar estas opciones manualmente en cualquier entorno de base de datos compatible con un proveedor de Entity Framework, pero ya se han configurado los valores predeterminados que normalmente funcionan bien para una aplicación en línea que usa Windows Azure SQL Database, y Estos son los valores que de la aplicación Contoso University se implementa.

Lo único que debe hacer para habilitar la resistencia de conexión es crear una clase en el ensamblado que se deriva de la [DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx) clase y, en esa clase, establezca la base de datos SQL *estrategia de ejecución*, que en EF es otro término para *directiva de reintentos*.

1. En la carpeta de la capa DAL, agregue un archivo de clase denominado *SchoolConfiguration.cs*.
2. Reemplace el código de plantilla con el código siguiente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Entity Framework se ejecuta automáticamente el código que se encuentra en una clase que derive de `DbConfiguration`. Puede usar el `DbConfiguration` clase para realizar tareas de configuración en el código que deberá hacer lo contrario en la *Web.config* archivo. Para obtener más información, consulte [EntityFramework configuración basada en código](https://msdn.microsoft.com/data/jj680699).
3. En *StudentController.cs*, agregue un `using` instrucción para `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Cambiar todos los `catch` impide que se detectan `DataException` excepciones para que, a continuación `RetryLimitExceededException` excepciones en su lugar. Por ejemplo:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Usaba `DataException` para intentar identificar los errores que podrían ser transitorios con el fin de proporcionar un mensaje descriptivo "vuelva a intentarlo". Pero ahora que ha activado una directiva de reintentos, los únicos errores posiblemente transitorio ya habrá se probaron y no se pudo varias veces y devuelve la excepción se ajustará en el `RetryLimitExceededException` excepción.

Para obtener más información, consulte [resistencia de conexión de Entity Framework o lógica de reintento](https://msdn.microsoft.com/data/dn456835).

## <a name="enable-command-interception"></a>Habilitar la intercepción de comandos

Ahora que ha activado una directiva de reintentos, ¿cómo prueba para comprobar que funciona según lo previsto? No es tan fácil forzar un error transitorio que suceda, especialmente cuando está ejecutando localmente, y sería especialmente difícil integrar errores transitorios reales en una prueba unitaria automatizada. Para probar la característica de resistencia de conexión, necesita una manera de interceptar consultas de Entity Framework se envía a SQL Server y reemplace la respuesta del servidor de SQL con un tipo de excepción que suele ser transitorio.

También puede usar intercepción de consulta con el fin de implementar una práctica recomendada para aplicaciones en la nube: [registrar la latencia y el éxito o error de todas las llamadas a servicios externos](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) como los servicios de base de datos. EF6 proporciona un [dedicado de la API de registro](https://msdn.microsoft.com/data/dn469464) que puede hacer más fácil de hacer el registro, pero en esta sección del tutorial obtendrá información sobre cómo usar Entity Framework [característica de intercepción](https://msdn.microsoft.com/data/dn469464) directamente, tanto para inicio de sesión y para simular errores transitorios.

### <a name="create-a-logging-interface-and-class"></a>Crear una interfaz de registro y la clase

Un [recomendado para el registro](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) es hacerlo mediante una interfaz en lugar de codificar de forma rígida las llamadas a System.Diagnostics.Trace o a una clase de registro. Que resulta más fácil cambiar su mecanismo de registro más adelante si necesita hacerlo. Por lo que en esta sección creará la interfaz de registro y una clase para implementar lo/p > 

1. Cree una carpeta en el proyecto y denomínelo *registro*.
2. En el *registro* carpeta, cree un archivo de clase denominado *ILogger.cs*y reemplace el código de plantilla con el código siguiente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    La interfaz proporciona tres niveles de seguimiento para indicar la importancia relativa de los registros y uno de ellos diseñado para proporcionar información de latencia de las llamadas de servicio externo, como las consultas de base de datos. Los métodos de registro tienen sobrecargas que le permiten pasar una excepción. Esto es para que la clase que implementa la interfaz, en lugar de confiar en que se realiza en cada llamada al método de registro en toda la aplicación de forma confiable registra información de excepción incluidas stack trace y las excepciones internas.

    Los métodos TraceApi permiten realizar un seguimiento de la latencia de cada llamada a un servicio externo, como SQL Database.
3. En el *registro* carpeta, cree un archivo de clase denominado *Logger.cs*y reemplace el código de plantilla con el código siguiente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    La implementación usa System.Diagnostics para realizar el seguimiento. Se trata de una característica integrada de .NET que facilita la tarea genere y utilice la información de seguimiento. Hay muchos "agentes de escucha" que puede usar con el seguimiento de System.Diagnostics, para escribir registros en archivos, por ejemplo, o para escribirlos en blob storage en Azure. Algunas de las opciones y vínculos a otros recursos para obtener más información, vea en [solución de problemas de sitios Web de Azure en Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Para este tutorial echaremos un vistazo solo en los registros en Visual Studio **salida** ventana.

    En una aplicación de producción debe tener en cuenta los paquetes de seguimiento que no sean de System.Diagnostics y la interfaz ILogger hace relativamente fácil cambiar a un mecanismo de seguimiento diferente si decide hacerlo.

### <a name="create-interceptor-classes"></a>Crear clases de interceptor

A continuación creará las clases que Entity Framework llamará a cada vez que va a enviar una consulta a la base de datos, uno para simular errores transitorios y otro para hacer el registro. Estas clases del interceptor deben derivar de la `DbCommandInterceptor` clase. En ellos escribirá los reemplazos de método que se llama automáticamente cuando se consulta va a ejecutar. En estos métodos puede examinar o la consulta que se envían a la base de datos de registro, y puede cambiar la consulta antes de enviarlo a la base de datos o devolver algo a Entity Framework por sí mismo sin pasar incluso la consulta a la base de datos.

1. Para crear la clase de interceptor que se registrará todas las consultas SQL que se envían a la base de datos, cree un archivo de clase denominado *SchoolInterceptorLogging.cs* en el *DAL* carpeta y reemplace la plantilla de código con el código siguiente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    Para las consultas correctas o los comandos, este código escribe un registro de información con la información de latencia. Para las excepciones, crea un registro de errores.
2. Para crear la clase de interceptor que generará errores transitorios ficticios al escribir "Producir" en el **búsqueda** cuadro, cree un archivo de clase denominado *SchoolInterceptorTransientErrors.cs* en el *DAL* carpeta y reemplace el código de plantilla con el código siguiente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Este código sólo reemplaza el `ReaderExecuting` método, que se llama para las consultas que pueden devolver varias filas de datos. Si desea comprobar la resistencia de conexión para otros tipos de consultas, podría invalidar la `NonQueryExecuting` y `ScalarExecuting` métodos, como el interceptor de registro hace.

    Cuando se ejecuta la página de alumnos y escriba "Producir" como la cadena de búsqueda, este código crea una excepción de base de datos SQL ficticia para el número de error 20, un tipo conocido que suele ser transitorio. Otros números de error que actualmente se reconoce como transitorio son 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 y 40613, pero éstos están sujetos a cambios en las nuevas versiones de SQL Database.

    El código devuelve la excepción a Entity Framework en lugar de ejecutar la consulta y pasar los resultados de la consulta espera. La excepción transitoria se devuelve cuatro veces y, a continuación, se revierte el código para el procedimiento normal de pasar la consulta a la base de datos.

    Dado que todo lo que ha iniciado sesión, podrá ver que Entity Framework intenta ejecutar la consulta cuatro veces antes de que se ejecutara correctamente y la única diferencia en la aplicación es que tarda más tiempo para representar una página con los resultados de la consulta.

    El número de veces que se reintentará el Entity Framework es configurable; el código especifica cuatro veces, ya que es el valor predeterminado de la directiva de ejecución de la base de datos SQL. Si cambia la directiva de ejecución, había también cambiar el código que especifica cuántas veces se generan errores transitorios. También podría cambiar el código para generar más excepciones para que Entity Framework arrojará el `RetryLimitExceededException` excepción.

    El valor que especifique en el cuadro de búsqueda estará en `command.Parameters[0]` y `command.Parameters[1]` (uno se usa para el nombre y otra para el apellido). Cuando se encuentra el valor "% Throw %", "Producir" se reemplaza en esos parámetros por "un" para que algunos estudiantes se encuentra y se devolverá.

    Se trata de una manera cómoda de resistencia de conexión basado en la variable alguna entrada a la aplicación de interfaz de usuario de prueba. También puede escribir código que genera errores transitorios para todas las consultas o actualizaciones, como se explica más adelante en los comentarios sobre la *DbInterception.Add* método.
3. En *Global.asax*, agregue las siguientes `using` instrucciones:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Agregue las líneas resaltadas en el `Application_Start` método:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Estas líneas de código son lo que hace que el código de interceptor que se ejecutará cuando Entity Framework envía consultas a la base de datos. Tenga en cuenta que dado que ha creado las clases de interceptor independiente para la simulación de error transitorio y registro, puede independientemente activarlos y desactivarlos.

    Puede agregar interceptores mediante el `DbInterception.Add` método en cualquier parte del código; no tiene que estar en el `Application_Start` método. Otra opción consiste en colocar el código en la clase DbConfiguration que creó anteriormente para configurar la directiva de ejecución.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Siempre que coloque este código, tenga cuidado de no ejecutar `DbInterception.Add` mismo interceptor de más de una vez, o bien obtendrá instancias adicionales de interceptor. Por ejemplo, si agrega dos veces el interceptor de registro, verá dos registros para todas las consultas SQL.

    Los interceptores se ejecutan en el orden de registro (el orden en que el `DbInterception.Add` se llama al método). Podría ser importante el orden según lo que está haciendo en el interceptor. Por ejemplo, un interceptor puede cambiar el comando SQL que obtiene en la `CommandText` propiedad. Si cambia el comando SQL, el interceptor siguiente obtendrá el comando SQL modificado, no el comando SQL original.

    Ha escrito el código de simulación de error transitorio de forma que le permite producir errores transitorios especificando un valor diferente en la interfaz de usuario. Como alternativa, podría escribir el código del interceptor para generar la secuencia de excepciones transitorias siempre sin comprobar si un valor de parámetro determinado. Después, puede agregar el interceptor sólo cuando desea generar errores transitorios. Si hace esto, sin embargo, no agregue el interceptor hasta que una vez completada la inicialización de la base de datos. En otras palabras, realizar la operación de al menos una base de datos como una consulta en uno de los conjuntos de entidades antes de empezar a generar errores transitorios. Entity Framework se ejecutan varias consultas durante la inicialización de la base de datos y no se ejecutan en una transacción, por lo que podrían provocar que el contexto obtener un estado incoherente errores durante la inicialización.

## <a name="test-logging-and-connection-resiliency"></a>Resistencia de conexión y registro de prueba

1. Presione F5 para ejecutar la aplicación en modo de depuración y, a continuación, haga clic en el **estudiantes** ficha.
2. Examine el Visual Studio **salida** ventana para ver los resultados del seguimiento. Es posible que deba desplazarse hacia arriba, más allá de algunos errores de JavaScript para llegar a los registros escritos por el registrador.

    Tenga en cuenta que puede ver las consultas SQL reales que se envían a la base de datos. Verá algunas consultas iniciales y los comandos que Entity Framework realiza para comenzar, comprobación de la versión de la base de datos y tabla de historial de migración (obtendrá información sobre las migraciones en el tutorial siguiente). Y verá una consulta para la paginación, para averiguar cuántos alumnos hay, y, por último, verá que la consulta que obtiene los datos de estudiante.

    ![Registro para las consultas normales](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. En el **estudiantes** página, escriba "Producir" como la cadena de búsqueda y haga clic en **búsqueda**.

    ![Generar la cadena de búsqueda](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Observará que el explorador parece no responder durante varios segundos, mientras que Entity Framework está reintentando la consulta varias veces. El primer reintento ocurre muy rápidamente, a continuación, la espera antes de aumentos antes de cada reintento adicional. Este proceso de espera ya antes de llama a cada reintento *retroceso exponencial*.

    Cuando se muestre la página, los estudiantes que muestra que tienen "un" en sus nombres, examine la ventana de salida, y verá que la misma consulta se ha intentado cinco veces, los primeros cuatro veces devolver transitorios excepciones. Para cada error transitorio, verá el registro que se escribe cuando se genera el error transitorio en el `SchoolInterceptorTransientErrors` clase ("Returning error transitorio para el comando...") y verá el registro escrito cuando `SchoolInterceptorLogging` Obtiene la excepción.

    ![Salida del registro que muestra los reintentos](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Puesto que ha especificado una cadena de búsqueda, se parametriza la consulta que devuelve datos de los alumnos:

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    No está registrando el valor de los parámetros, pero puede hacerlo. Si desea ver los valores de parámetro, puede escribir el código de registro para obtener los valores de parámetro desde el `Parameters` propiedad de la `DbCommand` objeto que se obtiene en los métodos de interceptor.

    Tenga en cuenta que no se puede repetir esta prueba, a menos que detenga la aplicación y reiníciela. Si desea ser capaz de resistencia de conexión de prueba varias veces en una única ejecución de la aplicación, podría escribir código para restablecer el contador de errores en `SchoolInterceptorTransientErrors`.
4. Para ver la diferencia la estrategia de ejecución (directiva de reintentos) realiza, comentario el `SetExecutionStrategy` línea *SchoolConfiguration.cs*, vuelva a ejecutar la página Students en modo de depuración y busque "Throw" de nuevo.

    Esta vez, el depurador se detiene en la primera excepción generada inmediatamente cuando intenta ejecutar la consulta en la primera vez.

    ![Excepción ficticia](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Quite el *SetExecutionStrategy* línea *SchoolConfiguration.cs*.

## <a name="summary"></a>Resumen

En este tutorial, ha visto cómo habilitar la resistencia de conexión y los comandos SQL que Entity Framework crea y envía a la base de datos de registro. En el siguiente tutorial, va a implementar la aplicación a Internet, mediante migraciones de Code First para implementar la base de datos.

Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar. También puede solicitar nuevos temas en [mostrar Me cómo con código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Pueden encontrar vínculos a otros recursos de Entity Framework en [acceso a datos de ASP.NET - recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Siguiente](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
