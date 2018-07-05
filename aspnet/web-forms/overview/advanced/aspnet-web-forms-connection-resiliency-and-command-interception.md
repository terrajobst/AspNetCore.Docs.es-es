---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: ASP.NET Web Forms resistencia de la conexión e intercepción de comandos | Microsoft Docs
author: Erikre
description: Este tutorial describe cómo modificar una aplicación de ejemplo para admitir la resistencia de conexión e intercepción de comandos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 8b8f592c4e79cf9dbe1255c93fac2fe18f1d9409
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368326"
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>ASP.NET Web Forms resistencia de la conexión e intercepción de comandos
====================
por [Erik Reitan](https://github.com/Erikre)

En este tutorial, modificará la aplicación de ejemplo Wingtip Toys para admitir la resistencia de conexión e intercepción de comandos. Al habilitar la resistencia de conexión, la aplicación de ejemplo Wingtip Toys volverá a intentar automáticamente las llamadas de datos cuando se producen errores transitorios típicos de un entorno de nube. Además, mediante la implementación de intercepción de comandos, la aplicación de ejemplo Wingtip Toys interceptará todas las consultas SQL enviadas a la base de datos con el fin de iniciar sesión o cambiarlos.

> [!NOTE] 
> 
> Este tutorial de formularios Web Forms se basaba en el siguiente tutorial de MVC de Tom Dykstra:  
> [Resistencia de conexión e intercepción de comandos con Entity Framework en una aplicación ASP.NET MVC](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo proporcionar resistencia de conexión.
- Cómo implementar la intercepción de comandos.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que tiene el siguiente software instalado en el equipo:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework se instala automáticamente.
- El Wingtip Toys de ejemplo de proyecto, por lo que puede implementar la funcionalidad que se mencionan en este tutorial en el proyecto de Wingtip Toys. El vínculo siguiente proporciona detalles de la descarga:

    - [Introducción a ASP.NET 4.5.1 Web Forms, Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- Antes de completar este tutorial, considere la posibilidad de revisar la serie de tutoriales relacionada, [Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). La serie de tutoriales le ayudará a familiarizarse con la **WingtipToys** código y proyecto.

## <a name="connection-resiliency"></a>Resistencia de conexión

Cuando considere la posibilidad de implementar una aplicación en Windows Azure, una opción a tener en cuenta es implementar la base de datos **Windows** **Azure SQL Database**, un servicio de base de datos en la nube. Errores de conexión transitorios son normalmente más frecuentes cuando se conecta a un servicio de base de datos en la nube que cuando el servidor web y el servidor de base de datos están conectados directamente entre sí en el mismo centro de datos. Incluso si un servidor web de nube y un servicio de base de datos en la nube se hospedan en el mismo centro de datos, hay más conexiones de red entre ellos que puede tener problemas, como los equilibradores de carga.

También un servicio en la nube normalmente se comparte con otros usuarios, lo que significa que su capacidad de respuesta puede verse afectado por ellos. Y el acceso a la base de datos podría estar sujeto a limitación. Limitación significa que el servicio de base de datos produce excepciones cuando se intenta tener acceso a él con más frecuencia que se permite en su *acuerdo de nivel de servicio* (SLA).

La mayoría o muchas problemas de conexión que se producen al que está accediendo a un servicio de nube son transitorios, es decir, en el resuelven por sí mismo se encuentre en un breve período de tiempo. Por lo que cuando se intente una operación de base de datos y obtener un tipo de error que suele ser transitorio, podría intentar la operación después de una breve espera y la operación podrían ser correcta. Puede proporcionar una experiencia mucho mejor para los usuarios si controlar errores transitorios, automáticamente intentándolo de nuevo, haciendo que la mayoría de ellos sea invisible para el cliente. La característica de resistencia de conexión de Entity Framework 6 automatiza que error del proceso de volver a intentar las consultas SQL.

La característica de resistencia de conexión debe configurarse correctamente para un servicio de base de datos determinada:

1. También debe conocer las excepciones que suelen ser transitorio. Desea volver a intentar errores causados por una pérdida temporal de conectividad de red, no de los errores causados por errores de programa, por ejemplo.
2. Tiene que esperar una cantidad apropiada de tiempo entre reintentos de una operación con errores. Puede esperar más tiempo entre reintentos para un proceso por lotes que si lo hace para una página web en línea donde un usuario está esperando una respuesta.
3. Tiene que volver a intentar un número adecuado de veces antes de desistir. Es posible que desee volver a intentarlo varias veces en un proceso por lotes que lo haría en una aplicación en línea.

Puede configurar estas opciones manualmente en cualquier entorno de base de datos compatible con un proveedor de Entity Framework.

Lo único que debe hacer para habilitar la resistencia de conexión es crear una clase en el ensamblado que se deriva de la `DbConfiguration` clase y, en esa clase, establezca la estrategia de ejecución de la base de datos SQL, que en Entity Framework es otro término para directiva de reintentos.

### <a name="implementing-connection-resiliency"></a>Implementación de resistencia de conexión

1. Descargue y abra el [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) aplicación de formularios Web Forms en Visual Studio de ejemplo.
2. En el *lógica* carpeta de la **WingtipToys** aplicación, agregue un archivo de clase denominado *WingtipToysConfiguration.cs*.
3. Reemplace el código existente con el siguiente código:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Entity Framework se ejecuta automáticamente el código que se encuentra en una clase que derive de `DbConfiguration`. Puede usar el `DbConfiguration` clase para realizar tareas de configuración en el código que deberá hacer lo contrario en la *Web.config* archivo. Para obtener más información, consulte [EntityFramework configuración basada en código](https://msdn.microsoft.com/data/jj680699).

1. En el *lógica* carpeta, abra el *AddProducts.cs* archivo.
2. Agregar un `using` instrucción para `System.Data.Entity.Infrastructure` tal como se muestra resaltado en amarillo:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Agregar un `catch` bloquear a la `AddProduct` método para que el `RetryLimitExceededException` se ha señalado en amarillo:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Agregando el `RetryLimitExceededException` excepción, que puede proporcionar el mejor registro o mostrar un mensaje de error al usuario que puede elegir el proceso para volver a intentarlo. Detectando el `RetryLimitExceededException` excepción, los únicos errores posiblemente transitorio ya habrá se probaron y no se pudo varias veces. Devuelve la excepción se ajustará en el `RetryLimitExceededException` excepción. Además, también se agrega un bloque catch general. Para obtener más información sobre la `RetryLimitExceededException` excepción, vea [resistencia de conexión de Entity Framework o lógica de reintento](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Intercepción de comandos

Ahora que ha activado una directiva de reintentos, ¿cómo prueba para comprobar que funciona según lo previsto? No es tan fácil forzar un error transitorio que suceda, especialmente cuando está ejecutando localmente, y sería especialmente difícil integrar errores transitorios reales en una prueba unitaria automatizada. Para probar la característica de resistencia de conexión, necesita una manera de interceptar consultas de Entity Framework se envía a SQL Server y reemplace la respuesta del servidor de SQL con un tipo de excepción que suele ser transitorio.

También puede usar intercepción de consulta con el fin de implementar una práctica recomendada para aplicaciones en la nube: registro externo a llama a la latencia y el éxito o error de todos los servicios, como servicios de base de datos.

En esta sección del tutorial se usa Entity Framework [ *característica de intercepción* ](https://msdn.microsoft.com/data/dn469464) tanto para el registro para simular errores transitorios.

### <a name="create-a-logging-interface-and-class"></a>Crear una interfaz de registro y la clase

Una práctica recomendada para el registro es hacerlo mediante el uso de un [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx) en lugar de codificar de forma rígida las llamadas a `System.Diagnostics.Trace` o una clase de registro. Que resulta más fácil cambiar su mecanismo de registro más adelante si necesita hacerlo. Por lo que en esta sección, creará la interfaz de registro y una clase para implementarlo.

Según el procedimiento anterior, ha descargado y ha abierto el **WingtipToys** aplicación en Visual Studio de ejemplo.

1. Cree una carpeta en el **WingtipToys** del proyecto y asígnele el nombre *registro*.
2. En el *registro* carpeta, cree un archivo de clase denominado *ILogger.cs* y reemplace el código predeterminado con el código siguiente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   La interfaz proporciona tres niveles de seguimiento para indicar la importancia relativa de los registros y uno de ellos diseñado para proporcionar información de latencia de las llamadas de servicio externo, como las consultas de base de datos. Los métodos de registro tienen sobrecargas que le permiten pasar una excepción. Esto es para que la clase que implementa la interfaz, en lugar de confiar en que se realiza en cada llamada al método de registro en toda la aplicación de forma confiable registra información de excepción incluidas stack trace y las excepciones internas.  
  
   El `TraceApi` métodos permiten realizar un seguimiento de la latencia de cada llamada a un servicio externo, como SQL Database.
3. En el *registro* carpeta, cree un archivo de clase denominado *Logger.cs* y reemplace el código predeterminado con el código siguiente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

La implementación usa `System.Diagnostics` para realizar el seguimiento. Se trata de una característica integrada de .NET que facilita la tarea genere y utilice la información de seguimiento. Hay muchos &quot;los agentes de escucha&quot; puede usar con `System.Diagnostics` seguimiento, para escribir registros en archivos, por ejemplo, o para escribirlos en blob storage en Windows Azure. Algunas de las opciones y vínculos a otros recursos para obtener más información, vea en [solución de problemas de Microsoft Azure sitios Web en Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Para este tutorial, solo verá los registros en Visual Studio **salida** ventana.

En una aplicación de producción que desee considerar el uso de marcos de trabajo de seguimiento distinto `System.Diagnostics`y el `ILogger` interfaz facilita relativamente cambiar a un mecanismo de seguimiento diferente si decide hacerlo.

### <a name="create-interceptor-classes"></a>Crear clases de interceptor

A continuación, creará las clases que Entity Framework llamará a cada vez que va a enviar una consulta a la base de datos, uno para simular errores transitorios y otro para hacer el registro. Estas clases del interceptor deben derivar de la `DbCommandInterceptor` clase. En ellos, escribir los reemplazos de método que se llama automáticamente cuando la consulta se va a ejecutar. En estos métodos puede examinar o la consulta que se envían a la base de datos de registro, y puede cambiar la consulta antes de enviarlo a la base de datos o devolver algo a Entity Framework por sí mismo sin pasar incluso la consulta a la base de datos.

1. Para crear la clase de interceptor que se registrará todas las consultas SQL antes de enviarlo a la base de datos, cree un archivo de clase denominado *InterceptorLogging.cs* en el *lógica* carpeta y reemplace el valor predeterminado de código con el código siguiente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Para las consultas correctas o los comandos, este código escribe un registro de información con la información de latencia. Para las excepciones, crea un registro de errores.
2. Para crear la clase de interceptor que generará errores transitorios ficticios al escribir &quot;Throw&quot; en el **nombre** cuadro de texto en la página llamada *AdminPage.aspx*, cree una clase archivo denominado *InterceptorTransientErrors.cs* en el *lógica* carpeta y reemplace el valor predeterminado de código con el código siguiente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Este código sólo reemplaza el `ReaderExecuting` método, que se llama para las consultas que pueden devolver varias filas de datos. Si desea comprobar la resistencia de conexión para otros tipos de consultas, podría invalidar la `NonQueryExecuting` y `ScalarExecuting` métodos, como el interceptor de registro hace.  
  
   Después, inicie sesión como "Administrador" y seleccione el **Admin** vínculo en la barra de navegación superior. A continuación, en el *AdminPage.aspx* página que se va a agregar un producto denominado &quot;Throw&quot;. El código crea una excepción de base de datos SQL ficticia para el número de error 20, un tipo conocido que suele ser transitorio. Otros números de error que actualmente se reconoce como transitorio son 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 y 40613, pero éstos están sujetos a cambios en las nuevas versiones de SQL Database. El producto se cambiará a "TransientErrorExample", que puede seguir en el código de la *InterceptorTransientErrors.cs* archivo.  
  
   El código devuelve la excepción a Entity Framework en lugar de ejecutar la consulta y pasar los resultados de vuelta. La excepción transitoria se devuelve *cuatro* veces, y, a continuación, el código se vuelve al procedimiento normal de pasar la consulta a la base de datos.

    Dado que todo lo que ha iniciado sesión, podrá ver que Entity Framework intenta ejecutar la consulta cuatro veces antes de que se ejecutara correctamente y la única diferencia en la aplicación es que tarda más tiempo para representar una página con los resultados de la consulta.  
  
   El número de veces que se reintentará el Entity Framework es configurable; el código especifica cuatro veces, ya que es el valor predeterminado de la directiva de ejecución de la base de datos SQL. Si cambia la directiva de ejecución, había también cambiar el código que especifica cuántas veces se generan errores transitorios. También podría cambiar el código para generar más excepciones para que Entity Framework arrojará el `RetryLimitExceededException` excepción.
3. En *Global.asax*, agregue las siguientes instrucciones using:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. A continuación, agregue las líneas resaltadas en el `Application_Start` método:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Estas líneas de código son lo que hace que el código de interceptor que se ejecutará cuando Entity Framework envía consultas a la base de datos. Tenga en cuenta que dado que ha creado las clases de interceptor independiente para la simulación de error transitorio y registro, puede independientemente activarlos y desactivarlos.   
  
 Puede agregar interceptores mediante el `DbInterception.Add` método en cualquier parte del código; no tiene que estar en el `Application_Start` método. Otra opción, si no agregó interceptores en el `Application_Start` sería el método actualizar o agregar la clase denominada *WingtipToysConfiguration.cs* y coloque el código anterior al final del constructor de la `WingtipToysbConfiguration` clase.

Siempre que coloque este código, tenga cuidado de no ejecutar `DbInterception.Add` mismo interceptor de más de una vez, o bien obtendrá instancias adicionales de interceptor. Por ejemplo, si agrega dos veces el interceptor de registro, verá dos registros para todas las consultas SQL.

Los interceptores se ejecutan en el orden de registro (el orden en que el `DbInterception.Add` se llama al método). Podría ser importante el orden según lo que está haciendo en el interceptor. Por ejemplo, un interceptor puede cambiar el comando SQL que obtiene en la `CommandText` propiedad. Si cambia el comando SQL, el interceptor siguiente obtendrá el comando SQL modificado, no el comando SQL original.

Ha escrito el código de simulación de error transitorio de forma que le permite producir errores transitorios especificando un valor diferente en la interfaz de usuario. Como alternativa, podría escribir el código del interceptor para generar la secuencia de excepciones transitorias siempre sin comprobar si un valor de parámetro determinado. Después, puede agregar el interceptor sólo cuando desea generar errores transitorios. Si hace esto, sin embargo, no agregue el interceptor hasta que una vez completada la inicialización de la base de datos. En otras palabras, realizar la operación de al menos una base de datos como una consulta en uno de los conjuntos de entidades antes de empezar a generar errores transitorios. Entity Framework se ejecutan varias consultas durante la inicialización de la base de datos y no se ejecutan en una transacción, por lo que podrían provocar que el contexto obtener un estado incoherente errores durante la inicialización.

## <a name="test-logging-and-connection-resiliency"></a>Resistencia de conexión y registro de prueba

1. En Visual Studio, presione **F5** para ejecutar la aplicación en modo de depuración y, a continuación, inicie sesión como "Admin" con "Pa$ $word" como contraseña.
2. Seleccione **Admin** desde la barra de navegación en la parte superior.
3. Escriba un nuevo producto denominado "Producir" con el archivo de descripción, el precio y la imagen adecuado.
4. Presione el **agregar producto** botón.  
   Observará que el explorador parece no responder durante varios segundos, mientras que Entity Framework está reintentando la consulta varias veces. El primer reintento ocurre muy rápidamente, a continuación, aumenta la espera antes de cada reintento adicional. Este proceso de espera ya antes de llama a cada reintento *retroceso exponencial* .
5. Espere hasta que la página ya no atttempting para cargar.
6. Detener el proyecto y examine el Visual Studio **salida** ventana para ver los resultados del seguimiento. Puede encontrar el **salida** ventana seleccionando **depurar**  - &gt; **Windows**  - &gt;  **Salida**. Es posible que deba Desplácese más allá de varios otros registros escritos por el registrador.  
  
   Tenga en cuenta que puede ver las consultas SQL reales que se envían a la base de datos. Consulte algunas consultas iniciales y los comandos que Entity Framework realiza para comenzar, comprobación de la tabla de historial de versión y la migración de base de datos.   
    ![Resultados (Ventana)](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Tenga en cuenta que no se puede repetir esta prueba, a menos que detenga la aplicación y reiníciela. Si desea ser capaz de resistencia de conexión de prueba varias veces en una única ejecución de la aplicación, podría escribir código para restablecer el contador de errores en `InterceptorTransientErrors` .
7. Para ver la diferencia la estrategia de ejecución (directiva de reintentos) realiza, comentario el `SetExecutionStrategy` línea *WingtipToysConfiguration.cs* de archivos en el *lógica* carpeta, ejecute el **Admin**  página en modo de depuración de nuevo y agregar el producto llamado &quot;Throw&quot; nuevo.  
  
   Esta vez, el depurador se detiene en la primera excepción generada inmediatamente cuando intenta ejecutar la consulta en la primera vez.  
    ![Depuración - Ver detalle](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Quite el `SetExecutionStrategy` de línea en el *WingtipToysConfiguration.cs* archivo.

## <a name="summary"></a>Resumen

En este tutorial, ha visto cómo modificar una aplicación de ejemplo de formularios Web Forms para admitir la resistencia de conexión e intercepción de comandos.

## <a name="next-steps"></a>Pasos siguientes

Una vez haya revisado la resistencia de conexión e intercepción de comandos en ASP.NET Web Forms, consulte el tema de formularios Web Forms ASP.NET [los métodos asincrónicos en ASP.NET 4.5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). El tema le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET asincrónica mediante Visual Studio.
